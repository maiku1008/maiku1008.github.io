---
title: "Using Jujutsu in a colocated git repository"
date: 2025-03-15T08:00:00+01:00
description: "git"
keywords: [git, jujutsu, colocated, remote, repository]
draft: false
tags: [git, jujutsu, colocated, remote, repository]
math: false
toc: false
---

[Jujutsu](https://github.com/jj-vcs/jj) (`jj`) is a Git-compatible version control system that offers a different approach to managing your code history. 
I have spent the last few weeks using Jujutsu (or `jj`) daily on personal and work git repositories.
One of its advantages is that you can use it locally alongside your team without them being aware that you're managing your code history differently.

Really, Jujutsu is a frontend for git, using git as its backend.
Your work is still based on a commit history, but the way you evolve your commits is different.

In this post, we'll explore how to use Jujutsu alongside an existing Git repository.

## Setting Up Jujutsu with an Existing Git Repository

### Cloning the repository
We will be working with an existing repository.
I'll be using https://github.com/maiku1008/jj-demo as an example. Feel free to fork it or create a new repository.

Let's start with cloning it:
```bash
$ git clone git@github.com:maiku1008/jj-demo.git
```
This is how the directory tree looks:
```bash
$ cd jj-demo/
$ tree
.
├── Cargo.toml
├── README.md
└── src
    └── main.rs
```

### Initializing Jujutsu
Use the following command to initialize Jujutsu in the git repository:
```bash
$ jj git init --colocate
Done importing changes from the underlying Git repo.
Setting the revset alias "trunk()" to "main@origin"
Hint: The following remote bookmarks aren't associated with the existing local bookmarks:
  main@origin
Hint: Run `jj bookmark track main@origin` to keep local bookmarks updated on future pulls.
Initialized repo in "."
```
This creates a `.jj/` directory at the root of the project, which is ignored by git by default.
As mentioned before, Jujutsu uses `git` as a backend.
We can now inspect the repository:
```bash
$ jj status
The working copy is clean
Working copy : ppmklykz a0c618e4 (empty) (no description set)
Parent commit: qwppmxos bdc4d64c main main@origin | initial commit
```
This shows us the working copy commit (where our changes will go) and its parent commit. Unlike Git, Jujutsu always has a working copy commit.
Every time a change is made, the equivalent of a `git commit --amend` is made to save that change.

If you're following along with this how-to, note that the ID hashes shown here will be different from the ones you will have locally.

## Basic Jujutsu Workflows

### Creating and Managing Commits

Let's make some changes to our code and see how Jujutsu handles them:

```bash
$ echo "// Add a comment" >> src/main.rs
$ jj status
Working copy changes:
M src/main.rs
Working copy : ppmklykz ab604fa8 (no description set)
Parent commit: qwppmxos bdc4d64c main main@origin | initial commit
```

Our working copy (with the change ID `ppmklykz`) has some changes but has no description.
Let's fix that:
```bash
$ jj describe -m "add a comment to main.rs"
Working copy now at: ppmklykz 6a839da5 add a comment to main.rs
Parent commit      : qwppmxos bdc4d64c main main@origin | initial commit
```

Note how the change ID `ppmklykz` of our working copy stayed the same, but the actual commit ID has changed.
Now let's create a new commit:
```bash
$ jj new
Working copy now at: vvrwrmys a8b00dce (empty) (no description set)
Parent commit      : ppmklykz 6a839da5 add a comment to main.rs
```

Jujutsu creates a new change ID and commit ID, and automatically moves our working copy forward. Let's make another change:
```bash
$ echo "// Add another comment" >> src/main.rs
$ jj describe -m "add another comment"
Working copy now at: vvrwrmys afce1a3d add another comment
Parent commit      : ppmklykz 6a839da5 add a comment to main.rs
```

Let's squash the changes:
```bash
$ jj squash
```
This will open our default editor to allow us to edit our desired commit message:
```
JJ: Enter a description for the combined commit.
JJ: Description from the destination commit:
add a comment to main.rs

JJ: Description from source commit:
add another comment

JJ: Lines starting with "JJ:" (like this one) will be removed.
```

Let's change the commit message to "add comments to main.rs"

```
JJ: Enter a description for the combined commit.
JJ: Description from the destination commit:
add comments to main.rs

JJ: Description from source commit:
JJ: Lines starting with "JJ:" (like this one) will be removed.
```
Once we exit the editor, we will be presented with the new state:
```bash
Working copy now at: ozummmpz d45e738b (empty) (no description set)
Parent commit      : ppmklykz e8ee5ac1 add comments to main.rs
```

This combines the changes from both commits into a single commit.

### Working with Branches and Bookmarks

In Jujutsu, [bookmarks](https://github.com/jj-vcs/jj/blob/main/docs/bookmarks.md) map to actual branches in Git:
```bash
$ jj bookmark create feature-x
Created 1 bookmarks pointing to ozummmpz d45e738b feature-x | (empty) (no description set)
$ jj status
The working copy is clean
Working copy : ozummmpz d45e738b feature-x | (empty) (no description set)
Parent commit: ppmklykz e8ee5ac1 add comments to main.rs
```

By default the new bookmark is pointing to our working copy

Let's set our current working copy to our `ppmklykz` change.
```bash
$ jj edit ppmklykz
Working copy now at: ppmklykz e8ee5ac1 add comments to main.rs
Parent commit      : qwppmxos bdc4d64c main main@origin | initial commit
```

Let's create a new bookmark
```bash
$ jj bookmark create add-comments
Created 1 bookmarks pointing to ppmklykz e8ee5ac1 add-comments | add comments to main.rs
```

To see all bookmarks:

```bash
$ jj branch list
feature-x: ozummmpz d45e738b (empty) (no description set)
add-comments: ppmklykz e8ee5ac1 add comments to main.rs
main: qwppmxos bdc4d64c initial commit
```

## Synchronizing with Remote Repositories

### Pushing Changes

To push your changes in the current working copy to the remote Git repository:

```bash
$ jj git push --allow-new
Changes to push to origin:
  Add bookmark add-comments to e8ee5ac1c954
```

Jujutsu automatically maps your bookmarks to Git branches when pushing.

To push a specific bookmark:
```bash
$ jj git push --bookmark feature-y
```

### Fetching and Tracking Remote Changes

To fetch changes from the remote repository:
```bash
$ jj git fetch
Fetching from origin (git@github.com:maiku1008/jj-demo.git)
```

To track a remote branch:
```bash
$ jj bookmark track main@origin
Started tracking 1 remote bookmarks.
```

To update your local repository with remote changes:
```bash
$ jj rebase -d main
Rebased 1 commits onto destination
```

To see what changes came in from the remote:
```bash
$ jj log -r "remote_branches(origin)"
```

## Wrapping up

Hopefully this short guide provides a clear introduction to using Jujutsu in an existing repository without disrupting your teammates' workflow.

I believe this is the most common use case for most developers, yet as of March 2025, straightforward documentation on this specific scenario remains surprisingly scarce.
I hope you've found this helpful for your own version control journey with Jujutsu!
