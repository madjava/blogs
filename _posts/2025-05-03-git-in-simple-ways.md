---
layout: post
author: Felix Eyetan
title: Simple ways to use Git
level: Beginner
is_blog: true
---
# Simple ways to use Git

## Assumptions

- You have Git [installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) on your local machine
- You have either a [GitHub](https://github.com/) or [BitBucket](https://bitbucket.org/) account.
- You have a terminal tool or an [IDE](https://www.geeksforgeeks.org/what-is-ide/) that provided one
  - [iTerm](https://bitbucket.org/) is nice if using a Mac

## What is Git

[Git](https://git-scm.com/) is a fast and modern implementation of version control. It provides a history of content
changes and facilitates collaborative changes to files.

## Why do I need Git

- Fast and easy to setup and learn
- Locally enable and distributed
- Good history tracking features
- Good for collaboration
- Lots of tools, features and documentation
- A large community of users

## Git is not GitHub

👉🏽 **Git** is a version control system that let's you manage and keep track of your source code history.

👉🏽 **GitHub** is a cloud-based hosting service that let's you manage Git repositories. It has additional capabilities around tooling, pipelines, security and a bunch of others useful features.

If you have open-source projects that use Git, then GitHub is designed to help you better manage them.

## The Basics - Setting up

### Creating a Git repository

```cmd
git init 
```

The name `main` will be used by default.

```cmd
git init -b master
```

If using another branch name, you can replace `master` with a name of your preference, but `main` (modern) or `master` (legacy) will suffice.

For your day-to-day projects or if working in collaboration with others, you will want to store content in a more central location such as GitHub or BitBucket best to create a new repository (repo) and the clone to your local machine.

To do so, from your GitHub account for example, create a new repo, then clone that i.e. make a copy of it on your local machine using the commands below in your terminal of choice.

```cmd
git clone <REMOTE_URL>
```

You will notice a new folder in the location you executed the command from.

### Synching your git repo

If you already have a local git repo by [initialing](#creating-a-git-repository) git locally, you can point it (connect it) to an existing remote repo.

A remote repo is a repo that exists in another location that uses Git e.g. [GitHub](https://github.com/) or [BitBucket](https://bitbucket.org/product/).

The below command can help you achieve this.

```cmd
git remote add origin <REMOTE_URL>
```

```cmd
git checkout <REMOTE_URL>
```

Your local repo will now point to your remove repo, meaning when you push changes the remote repo will get those changes.

_**Note:** Watch out for discrepancies aka `merge` conflicts if files already exists in either repositories. You may have to take further [actions](#merging-rebasing-and-reverting) to resolve them_

## Branching and Switching

A branch represents an independent line of development, like a silo. When working in collaboration with others, you create a branch, a copy of the project, where you can experiment with your ideas make change and not affect the main body of work.

You may also invite collaborators to review your work on your branch where contributions will be added to it and reviews received.

### Creating a branch

```cmd
git branch <branch-name> 
```

_Note: You don’t really have a branch until you add or commit a file to the new branch_

```cmd
git checkout -b <new-branch>
```

```cmd
git checkout -b <new-branch> <existing-branch>
```

### Switching branches

```cmd
git fetch –all
```

_Optional, but sometimes useful, especially in large projects. This updates your local repo with any new branches that may have been created by others_

```cmd
git checkout <branch-name>
```

### Renaming a branch

While on the branch you can change it's name like so,

```cmd
git branch -m <new-branch-name>
```

### Deleting branches

There are various ways to delete a branch, both a local copy and a remote copy.

```cmd
git branch -d <branch-name>
```

```cmd
git branch -D <branch-name>
```

```cmd
git push origin --delete <branch-name>
```

```cmd
git push origin :<branch-name>
```

_Note: Deleting it locally does not "delete" the branch, it still exists on the remote server until you `push` your changes out, thread with caution._


## Committing, Amending and Pushing changes

Commits can be thought of as snapshots or milestones along the timeline of a Git project.

Used when you want to capture the state of changes to the project or mark milestones as the work evolves.

### Adding new files

Before there is anything to commit, you need to add the any changes or files to git. Essentially what this does is that it notifies git to "keep track" of changes to the file.

```cmd
git add <file-name>
```
Add's a single file by name

```cmd
git add . 
```

This adds every file change in current directory. 

Key word here is `current` directory. If you have made a repo that has many parent and child folders and you make changes in multiple places then you either have to jump to each folder and run the above command or jump to the parent directory and add all.

### Committing changes
When you have changes or have reached a point you want to "mark", then its a good time to `commit` your changes after you have [added](#adding-new-files) them.

```cmd
git status
```
Will show you all changes ready to be committed

```cmd
git commit –m “your descriptive but brief commit message”
```

```cmd
git commit –am “your descriptive but brief commit message” 
```

If file has already been [staged](#adding-new-files), you can skip the `add` commands and just use the `-am` flag, this is a short cut to both `add` the file and add a commit message as above.

### Amending commits

There sometime is the need to amend a most recent commit e.g. You had committed your changes but added a new change that you want to reflect as part of the previous commit set. To do so you can:

```cmd
git commit --amend
```

To add the new change to the last commit

```cmd
git commit --amend -m "an updated commit message” 
```

To both add the new change and also update the commit message. For example you notice a typo, happens to the best of us. 🙃

### Changing committed files

```cmd
git add <the-file>
```

```cmd
git commit --amend --no-edit
```

> 🔥 Don’t amend public commits, avoid amending a commit that other developers have based their work on, do so only on your local branch/commits.

### Pushing changes to remote

When happy with your changes, you can make it public or visible to other collaborators by placing it in the central location with the below commands.

```cmd
git push
```

## Merging, Rebasing and Reverting

There are time when you need to lump things together, combining changes from more than one branch.

Git merge is used to combine changes from two or more branches into a single branch.

Git rebase is used to incorporate changes from one branch into another by rewriting the commit history.

These feature can be very helpful in keeping things organised or help you separate/chunk your work in more that one branch then bring them all together in one branch.

### Merging

```cmd
git checkout <branchname>
```

```cmd
git merge main
```

```cmd
git merge <branchname> main
```

Options include `--squash`, `--abort`, `--quit`, `-s [our]` etc.

### Rebasing

```cmd
git checkout <branchname>
```

```cmd
git rebase main
```

> The golden rule of git rebase is to never use it on public branches

You can look at a `merge` as combining two repos together, sorting out the difference between them and a `rebase` as adding one repo right on top of the other.

Git has a nice documentation called [Git Branching - Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) that breaks it down in detail. Check it out if you need more clarity.

### Squashing

This is another feature that allows you group multiple commits into one. If you have multiple small changes committed and want to push them out together then `squash` them.

```cmd
git log --oneline
```

```cmd
git rebase -i HEAD~N
```

```cmd
git merge --squash <branchname> (then commit)
```

> ℹ️ Very useful for tidying up local commits before a milestone push.

_🔥 Note: Try to avoid squashing too many changes into one push that it become one large commit when its review time. Everything in moderation._

### Reverting

```cmd
git revert --<hard|soft|mixed> <commit-id>
```

## Pulling, Searching and Aliases

Git pull is used to fetch and download content from a remote repository, updating the local repository to match that content.

Git aliases can shorten common commands and make it easy for you to remember, just try not to go overboard.

### Pulling

```cmd
git pull <remote>
```

```cmd
git pull --rebase <remote>
```

> Used to ensure a linear history by preventing unnecessary merge commits.

### Searching

You can look into your git repo or history to search for information about past commits, branches etc.

```cmd
git grep <text> 
```

This will look through files

```cmd
git log <options> 
```
This will show your commits

### Aliases

```cmd
git config --global alias.<name> ‘<git subcommand options>’
```

```cmd
git config –e 
```

To open your default editor

```cmd
git config --list
```

To show your git configuration and all the crazy aliases you have set, and the one you had forgotten about 🙃.

## Next Steps

Git is really powerful and has lots of features, it can sometimes feel overwhelming but practicing one feature at a time is the way to go, you can try out most commands locally.

### For More Adventures

```cmd
git –help, 
git help -a or 
git help -g
```

These will show you lots of other options you can have a play with.

## Practice, practice, practice

Follow my [Git hands on work through](#) and practice some of the above commands with me.
