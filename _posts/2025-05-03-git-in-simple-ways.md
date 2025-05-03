---
layout: post
author: Felix Eyetan
title: Simple ways to use Git
level: Beginner
is_blog: true
---
# Simple ways to use Git

## Introductions
xxx

## What is Git
Git is a fast and modern implementation of version control. It provides a history of content 
changes and facilitates collaborative changes to files.

## Why do I need Git

- Fast and easy to setup and learn
- Locally enable and distributed
- Good history tracking features
- Good for collaboration
- Lots of tools, features and documentation
- A large community of users

## Git is not GitHub
**Git** is a version control system that lets you manage and keep track of your source code history.

**GitHub** is a cloud-based hosting service that lets you manage Git repositories. 

If you have open-source projects that use Git, then GitHub is designed to help you better manage them.

## Terminologies to note
xxx

## The Basics - Setting up

### Creating a Git repository

```cmd
git init 
```

The name `main` will be used by default.

```cmd
git init -b master
```

If using another branch name, you can replace `master` with a name of your preference.

For your daily work or if working on a project you want to collaborate on or store in a more central location (Git) best to create a new repository (repo) on GitHub and the clone to local machine.

To do so, from your GitHub account, create a new repo, then clone that (make a copy off it) on your local nmachine.

```cmd
> git clone <REMOTE_URL>
```

### Synching your git repo
If you already have a local git repo by initialing git locally, you can point it (connect it) to an existing remote repo. 

A remote repo is a repo that exists in another location that use Git e.g. GitHub or BitBucket.

The below command can help yu achive this.

```cmd
git remote add origin <REMOTE_URL>
```

```cmd
git checkout <REMOTE_URL>
```

## Branching and Switching
A branch represents an independent line of development, like a silo. When working in collaboration with others, you create a branch, a copy of the project, where you can experiment with your ideas make change and not affect the main body of work. You will also invite co-collaborator to review your work on your branch and controbutions will be added to it.

### Creating a branch

```cmd
git branch <branch-name> 
```
Note: You don’t realy have a branch until you add/commit a file

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
Optional, if you want to make sure git is aware of all branches to be able to switch to it
```cmd
git checkout <branch-name>
```

### Renaming a branch
While on the branch you can change its name like so,
```cmd
git branch -m <new-branch-name>
```

### Deleting branches
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

## Committing, Amending and Pushing changes
Commits can be thought of as snapshots or milestones along the timeline of a Git project. 
Used when you want to capture the state of changes to the project.

### Adding new files
```cmd
git add <file-name>
```
```cmd
git add . 
```
This adds every file change in current directory.

### Committing changes
```cmd
git commit –m “your descriptive but brief commit message”
```

```cmd
git commit –am “your descriptive but brief commit message” 
```
If file has already been staged, you can skip the `add` command above and use the `-am` flag

### Amending commits
There sometime is the need to amend a most recent commit. To do so you can:

```cmd
git commit --amend
```
```cmd
git commit --amend -m "an updated commit message” 
```

### Changing committed files
```cmd
git add <the-file>
```
```cmd
git commit --amend --no-edit
```

> 🔥 Don’t amend public commits, Avoid amending a commit that other developers have based their work on, do so only on your local branch/commits.

### Pushing changes to remote
```cmd
git push
```

## Merging, Rebasing and Reverting
Git merge is used to combine changes from two or more branches into a single branch. 

Git rebase is used to incorporate changes from one branch into another by rewriting the commit history.

### Merging
```cmd
git checkout <branchname> | git merge main
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

### Squashing
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

### Reverting
```cmd
git revert --<hard|soft|mixed> <commit-id>
```

## Pulling, Searching and Aliases
Git pull is used to fetch and download content from a remote repository, updating the local repository to match that content.

Git aliases can shorten common commands and make it easy to remember.

### Pulling
```cmd
git pull <remote>
```
```cmd
git pull --rebase <remote>
```
> Used to ensure a linear history by preventing unnecessary merge commits.

### Searching
```cmd
git grep <text> (will look through files)
```
```cmd
git log <options> (will look through commits)
```

### Aliases
```cmd
git config --global alias.<name> ‘<git subcommand options>’
```
```cmd
git config –e (to open default editor)
```
```cmd
git config --list
```

## Next Steps
Git is really powerful and has lots of features, it can sometimes feel overwhelming but practicing one feature at a time really helps and you can try out most commands locally.

### For More Adventures
```cmd
git –help, 
git help -a or 
git help -g
```

> Will show you lots of other options you can have a play with.