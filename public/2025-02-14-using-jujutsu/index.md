# Using Jujutsu in a colocated git repository


Jujutsu (jj) is a Git-compatible version control system that offers a different approach to managing your code history. 
I have spent a few weeks using Jujutsu (or `jj`) daily on personal and work git repositories.
One of the advantages of Jujutsu is that you can use it locally alongside your team without them being aware that you're managing your code history differently.

Really Jujutsu is a frontend for git, using git as its backend.
So we are still based on a commit history, but the way we evolve our commit is different.

In this post, we'll explore how to use Jujutsu alongside an existing Git repository.

## Setting Up Jujutsu with an Existing Git Repository

1. Cloning the repository
   - Using standard git clone
   - Repository structure after cloning

2. Initializing Jujutsu
   - Using jj init --colocated
   - Understanding the colocated setup
   - Initial repository state

## Basic Jujutsu Workflows

1. Creating and Managing Commits
   - Using `jj new` to create changes
   - Describing commits with `jj describe`
   - Squashing commits using `jj squash`

2. Working with Branches and Bookmarks
   - Understanding Jujutsu bookmarks
   - Creating bookmarks with `jj set bookmark`
   - Mapping bookmarks to Git branches

## Synchronizing with Remote Repositories

1. Pushing Changes
   - Using `jj git push`
   - Understanding bookmark to branch mapping
   - Common push scenarios

2. Fetching and Tracking Remote Changes
   - Fetching with `jj git fetch`
   - Setting up branch tracking
   - Using `jj bookmark track`
   - Managing remote branches

## Conclusion

Summary of key benefits and workflows when using Jujutsu with Git repositories.

[Note: Advanced rebasing workflows will be covered in a future post]

