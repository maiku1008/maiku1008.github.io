---
title: "Pitfalls I encountered while learning Go"
date: 2019-05-08T20:56:19+02:00
keywords: [go, junior]
draft: false
tags: [go, junior]
math: false
toc: false
---

I started learning Go on and off out of curiosity, late last year.

Being mainly a Python developer, I figured learning a different programming language would help to broaden my horizons.

Recently though, we adopted the language for some work projects, therefore I took the plunge and got to really hacking around with it.

The following are some noobie gotchas that I bumped into during development of my latest project (link below).
Mostly these came around while trying to figure out how to use mocks and asserts in unit tests with the [testify](https://github.com/stretchr/testify) collection of packages.

## Validate if an interface is satisfied by a struct

Here is the first snippet:
```
package requests

import (
    "github.com/stretchr/testify/mock"
    "net/http"
)

// ClientInterface defines a common interface to
// *http.Client and our *MockClient
type ClientInterface interface {
    Do(req *http.Request) (*http.Response, error)
}

// Validate satisfaction of interface
var _ ClientInterface = (*MockClient)(nil)
var _ ClientInterface = (*http.Client)(nil)

// MockClient embeds mock.Mock to reuse its methods
type MockClient struct {
    mock.Mock
}

// Do is a mock of http.Client's method Do
func (mockClient *MockClient) Do(req *http.Request) (*http.Response, error) {
    args := mockClient.Called(req)
    return args.Get(0).(*http.Response), args.Error(1)
}
```

Zooming into the interesting part:
```
// Validate satisfaction of interface
var _ ClientInterface = (*MockClient)(nil)
var _ ClientInterface = (*http.Client)(nil)
```

We assign a `MockClient` nil pointer to an empty var of type `ClientInterface`.
In the second line we do the same, but with an `http.Client`.

This is a fun little trick we use in order to validate whether or not both structs satisfy our `ClientInterface`.

If they don't, the program will complain at build time.

We will be using our custom interface in the next step.

## Use interfaces instead of structs to pass data around

A key function of my app is the following:
```
import "net/http"

// GetResponse gets a response
func GetResponse(req *http.Request, c *http.Client) {

    resp, err := c.Do(req)
    if err != nil {
        fmt.Println(err)
    }
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(body)
}
```

I wanted to write tests for this, which would verify its functionality.

I also did not wish to test this against a live environment, since unit tests should be isolated,
so I had to create my own mock version of `*http.Client`, and use it as an argument to my GetResponse function.

It turns out that Go is pretty anal about its types (no real surprise there!).

```
./requests_test.go:47:28: cannot use mockClient (type *MockClient) as type *http.Client in argument to GetResponse
```

So I resolved instead to pass it my interface, `ClientInterface`, which implements `Do()`, as described above.

```
import "net/http"

// GetResponse gets a response
func GetResponse(req *http.Request, c ClientInterface) {

    resp, err := c.Do(req)
    if err != nil {
        fmt.Println(err)
    }
    defer resp.Body.Close()

    body, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(body)
}
```

## Dereferencing nil pointers is bad

At this point, I just wanted to see the test work, so in a hurry I loaded a couple nil pointers into the arguments I needed, as such:

```
    // Initialize mock object
    mockClient := &MockClient{}

    // Set return value
    returnValue := (*http.Response)(nil)
    req := (*http.Request)(nil)

    // Set expectations
    mockClient.On("Do", req).Return(returnValue, nil)

    GetResponse(req, mockClient)
```

This returned a nasty error:
```
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0xffffffff addr=0x0 pc=0x3a6626]
```

Dereferencing means reading the memory value at a given address.
So when we have a pointer to something, to dereference the pointer means to read or write the data that the pointer points to.

If that pointer points to nothing we will get the above error.

Time to fix that by creating a request object with some dummy content.

```
func main() {

    // Initialize mock object
    mockClient := &MockClient{}

    // Set return value
    body := ioutil.NopCloser(strings.NewReader("hello world"))
    returnValue := &http.Response{Status: "200 OK", Body: body}
    req := (*http.Request)(nil)

    // Set expectations
    mockClient.On("Do", req).Return(returnValue, nil)

    GetResponse(req, mockClient)
}
```

## Parametrize tests with a struct

Finally, I parametrized different calls by using a struct, like described in the [official docs](https://github.com/golang/go/wiki/TableDrivenTests).

```
// A struct for testing the different calls
var responseTests = []struct {
    method       string
    endpoint     string
    auth         string
    requestBody  string
    responseBody string
}{
    {"POST", TokenEndPoint, authString, OauthTokenBody, tokenEndpointResponse},
    {"POST", ContactEndpoint, bearerToken, CreateContactBody, contactEndpointResponse},
    {"GET", checkEndpoint, bearerToken, "", checkEndpointResponse},
    {"POST", DomainEndpoint, bearerToken, buyDomainBody, buyDomainEndpointResponse},
}

func TestGetResponse(t *testing.T) {
    assert := assert.New(t)

    // Initialize mock object
    mockClient := &MockClient{}

    for _, rt := range responseTests {
        // Setup request
        req := SetRequest(rt.method, rt.endpoint, rt.auth, rt.requestBody)

        // Setup expected response
        body := ioutil.NopCloser(strings.NewReader(rt.responseBody))
        returnValue := &http.Response{Status: "200 OK", Body: body}

        // Set expectations
        mockClient.On("Do", req).Once().Return(returnValue, nil)

        // Run the thing!
        GetResponse(req, mockClient)

        // Assert expectations
        mockClient.AssertExpectations(t)
    }
}
```

Looping these this way seems like a much cleaner way to do things, for readbility, and also for being able to add more calls easily if needed.

## To Conclude

The above can all be found in my [checkdomain](https://github.com/micuffaro/checkdomain) project.

Go is awesome, and I'm looking forward to mess around with it more.
