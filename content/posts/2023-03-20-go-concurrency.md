---
title: "Speeding up a Go cli application with concurrency"
date: 2023-03-20T08:00:47+02:00
description: ""
keywords: [go]
draft: false
tags: [go]
math: false
toc: true
---

## Premise

A few months ago I worked with a company which provided fully functional backend for online multiplayer games.
As part of their product portfolio, they provided their customers with a cli utility, called `ds-uploader` (dedicated server uploader).

Written in Go, this CLI helped the customer:
- Process all files and assets in a directory of their choice containing their game server.
- Synchronize each file to a remote object storage bucket (meaning to upload only files that are new or modified).
- From those uploaded files, build a container image.
- Pass that container image to another service, which would then be responsible to run it.

Most of the implementation details involved calling different services, both belonging to the service provider, and to the cloud provider of choice (in this case, AWS). 

These details were mostly hidden from the end user; in fact all they really needed to worry about, was to run the application and point it to the directory with their game server files, along with a few other options.

```bash
$ ds-uploader sync --path /path/to/game/server/ ... <more flags here>
```

## The Problem

The client mentioned that in some cases, the ds-uploader would take a long time to run end to end. 
In particular, the part of the application dealing with the file uploads would take a very long time, whether the files were new or to be modified.

Upon testing, we found that this mostly happened with game server directories with multiple thousands of files, distributed across multiple levels of nested directories.

In the client's case, where they had a game server directory of more than 2000 files, nested across at least 5 levels of directories, for a total of around 4GB, the `ds-uploader` would spend more than *6 hours* to complete.

## Show me the code!

### Before

In the application, we used an interface exposed by the cloud provider's API.

```go
// BatchUploadIterator is an interface that uses the scanner pattern to
// iterate through what needs to be uploaded.
type BatchUploadIterator interface {
	Next() bool
	Err() error
	UploadObject() BatchUploadObject
}
```

By creating a custom type satisfying such an interface, we could basically let the cloud provider's API do all the heavy lifting for us, by leveraging an `Uploader` type providing an `UploadWithIterator` method.

```go
type Uploader struct {
    // A few exported fields
}

func (u Uploader) UploadWithIterator(
	ctx context.Context, 
	iter BatchUploadIterator, 
	opts ...func(*Uploader)) error {}
```

So we decided to do exactly that and wrote our own `BatchUploadIterator`, which roughly looks like this.

```go
// Custom FileIterator
type FileIterator struct {
	BucketName string
	FileInfos  []FileInfo
	err        error
	Size       int64
}

func NewFileIterator(directoryPath string, auth *session.Session) *FileIterator {
	// Traverse the entire directory structure
	// and save metadata from each file if it is not a directory.
}

func (iterator *FileIterator) Next() bool {
	// return whether or not we are at the end of the list of files to upload.
}

func (iterator *FileIterator) Err() error {
	// return any error
}

func (iterator *FileIterator) UploadObject() BatchUploadObject {
	// return a single UploadObject representing the file we want to upload
}
```

Next we passed this iterator to AWS API's `UploadWithIterator`, and called it a day.

With smaller directories, this worked without issues, and upload times were acceptable.

With larger, nested directories we started seeing slower speeds.

This is what `UploadWithIterator` looks like:
```go
func (u Uploader) UploadWithIterator(
	ctx context.Context, 
	iter BatchUploadIterator, 
	opts ...func(*Uploader)) error {
	
	var errs []Error
	for iter.Next() {
		// Get the object from our custom iterator
		object := iter.UploadObject()
		// Attempt to upload the object
		if _, err := u.UploadWithContext(ctx, object.Object, opts...); err != nil {
			errs = append(errs, err)
		}
		// omitted code
	}

	if len(errs) > 0 {
		// handle the errors
	}
	return nil
}
```

As we can see, the method iterates through the list of objects _sequentially_, and calls `u.UploadWithContext` for each, which is where the upload happens under the hood.

The problem is that with this approach, the program actually waits until a file has been uploaded with success or not, before moving on to the next.

### After

This problem is a great candidate for solving with Go's concurrency features.

We have some I/O operation, on different objects which are not dependant on each other, that can be uploaded in no particular order.

We know from the API documentation that `UploadWithContext` is safe to be called across multiple goroutines.

The API doesn't natively provide methods for concurrent uploads, so we decided to extend the `Uploader` type with a custom type:

```go
// ConcurrentUploader wraps an Uploader and extends its capabilities.
type ConcurrentUploader struct {
	uploader Uploader
}

func NewConcurrentUploader(uploader Uploader) ConcurrentUploader {
	return ConcurrentUploader{
		uploader: uploader,
	}
}
```

Such a type will still call `Uploader.UploadWithContext`, but from our own version of `UploadWithIterator`, which we also rename for good measure.

Here it is, with comments at each step to help understand what it does

```go
func (cu ConcurrentUploader) UploadConcurrentlyWithIterator(
	ctx aws.Context, 
	iter s3manager.BatchUploadIterator) error {

	var errs []error
    
	// We use a waitgroup because we plan to have several goroutines, the number of which
	// is determined by the concurrency value, uploading the processed objects in batches
	var wg sync.WaitGroup
	var concurrency = 10

	objects := make(chan BatchUploadObject, concurrency)
	errors := make(chan error, concurrency)

	// Spawn a number of concurrent upload workers equal to the concurrency value.
	for w := 0; w < concurrency; w++ {
		go cu.uploadWorker(ctx, &wg, objects, errors)
		wg.Add(1)
	}

	// In a separate goroutine, start iterating through the FileIterator.
	// This contains metadata of all dedicated server files, and is responsible for
	// creating an Upload Object for the API to consume.
	// Send all found upload objects to the objects channel.
	// When we're done, meaning we sent all the objects, we close the channel.
	go func() {
		for iter.Next() {
			object := iter.UploadObject()
			objects <- object
		}
		close(objects)
	}()

	// In a separate goroutine, gather all errors from the errors channel
	go func() {
		for err := range errors {
			errs = append(errs, err)
		}
	}()

	// Wait for all the uploadWorkers to be done working, 
	// and then close the errors channel.
	wg.Wait()
	close(errors)
	if len(errs) > 0 {
		// handle the errors
	}
	return nil
}

```

Lastly, we have the `uploadWorker` that looks like this:
```go
// uploadWorker takes care to process each object and errors if any.
func (cu ConcurrentUploader) uploadWorker(
	ctx context.Context, 
	wg *sync.WaitGroup, 
	objects <-chan BatchUploadObject, 
	errors chan<- error) {

	defer wg.Done()
	// receive objects from the objects channel
	for o := range objects {
		// upload the object (see below)
		err := cu.uploadObject(ctx, o)
		if err != nil {
			errors <- err
		}
	}
}

// uploadObject simply calls the Uploader's UploadWithContext.
func (cu ConcurrentUploader) uploadObject(
	ctx context.Context, 
	object BatchUploadObject) error {

	err := cu.uploader.UploadWithContext(ctx, object.Object)
	if err != nil {
		return err
	}

	if object.After == nil {
		return nil
	}

	err = object.After()
	if err != nil {
		return err
	}

	return nil
}
```

## Conclusion

- We kept using our custom `FileIterator` and the API's `UploadWithContext` method. This allowed us to use what was already there and not refactor too much.
- We however did so while applying a relatively basic concurrency pattern based on a worker pool and waitgroup.
- Workers run concurrently, and process the objects (files) to upload in batches

As a result, the upload of the client's game server (which I repeat, was a nested directory of more than 2000 files, for a total of around 4GB ) went from *6 hours* to a much better *30 minutes* end to end.
