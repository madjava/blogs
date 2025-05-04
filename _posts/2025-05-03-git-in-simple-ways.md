---
layout: post
author: Felix Eyetan
title: Simple ways to use Git
level: Beginner
is_blog: true
---

### Assumptions

- You have Git [installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) on your local machine
- You have either a [GitHub](https://github.com/) or [BitBucket](https://bitbucket.org/) account.
- You have a terminal tool or an [IDE](https://www.geeksforgeeks.org/what-is-ide/) that provided one
  - [iTerm](https://bitbucket.org/) is nice if using a Mac

### What is Git

[Git](https://git-scm.com/) is a fast and modern implementation of version control. It provides a history of content
changes and facilitates collaborative changes to files.

### Why do I need Git### 

- Fast and easy to setup and learn
- Locally enable and distributed
- Good history tracking features
- Good for collaboration
- Lots of tools, features and documentation
- A large community of users

### Git is not GitHub

👉🏽 **Git** is a version control system that let's you manage and keep track of your source code history.

👉🏽 **GitHub** is a cloud-based hosting service that let's you manage Git repositories. It has additional capabilities around tooling, pipelines, security and a bunch of others useful features.

If you have open-source projects that use Git, then GitHub is designed to help you better manage them.

### The Basics - Setting up

#### Creating a Git repository

```json
git init 
```

The name `main` will be used by default.

```json
git init -b master
```

If using another branch name, you can replace `master` with a name of your preference, but `main` (modern) or `master` (legacy) will suffice.

For your day-to-day projects or if working in collaboration with others, you will want to store content in a more central location such as GitHub or BitBucket best to create a new repository (repo) and the clone to your local machine.

To do so, from your GitHub account for example, create a new repo, then clone that i.e. make a copy of it on your local machine using the commands below in your terminal of choice.

```json
git clone <REMOTE_URL>
```

You will notice a new folder in the location you executed the command from.

<details>
   <summary>🏋🏽‍♀️ Setting up exercises</summary>
   <strong>Exercise 1</strong>

   Create a folder called `simple-git` and navigate into it

   ```json
   mkdir simple-git && cd simple-git
   ```

   ```json
   > mkdir lab1 && cd lab1
   > git init # to setup a new repo
   > echo "First file and content" >> first.txt
   > git add .
   > git status #To see what's to be committed
   > git commit -m "First commit on main"
   ```
   
   In the `simple-git` folder, create a new repo on your GitHub account called `lab2`. The from another folder execute the below commands


   ```json
   > git clone git@github.com:<your-github-account>/lab2.git
   ```

   if you have setup a git token and ssl or

   ```json
   > git clone https://github.com/<your-github-account>/lab2.git
   ```

At this point you should have the following structure:

<pre>
simple-git
├── lab1
   └── first.txt
└── lab2
</pre>

</details>


#### Synching your git repo

If you already have a local git repo by [initialing](#creating-a-git-repository) git locally, you can point it (connect it) to an existing remote repo.

A remote repo is a repo that exists in another location that uses Git e.g. [GitHub](https://github.com/) or [BitBucket](https://bitbucket.org/product/).

The below command can help you achieve this.

```json
git remote add origin <REMOTE_URL>
```

```json
git checkout <REMOTE_URL>
```

Your local repo will now point to your remove repo, meaning when you push changes the remote repo will get those changes.

<details>
   <strong>Exercise 2</strong>
   <summary>🏋🏽‍♀️ Synching your git repo</summary>

   You could try this out by deleting the `lab2` folder from [exercise 1](#exercise-1)

   The run the following commands from the `simple-git` folder
   
   ```json
   mkdir lab2 && git init
   ```

   This will initialise a `git` locally then point that to the remove repo using commands above.

   ```json
   git remote add origin https://github.com/<your-github-account>/lab2.git
   ```
</details>


_**Note:** Watch out for discrepancies aka `merge` conflicts if files already exists in either repositories. You may have to take further [actions](#merging-rebasing-and-reverting) to resolve them_

### Branching and Switching

A branch represents an independent line of development, like a silo. When working in collaboration with others, you create a branch, a copy of the project, where you can experiment with your ideas make change and not affect the main body of work.

You may also invite collaborators to review your work on your branch where contributions will be added to it and reviews received.

#### Creating a branch

```json
git branch <branch-name> 
```

_Note: You don’t really have a branch until you add or commit a file to the new branch_

```json
git checkout -b <new-branch>
```

```json
git checkout -b <new-branch> <existing-branch>
```

#### Switching branches

```json
git fetch –all
```

_Optional, but sometimes useful, especially in large projects. This updates your local repo with any new branches that may have been created by others_

```json
git checkout <branch-name>
```

#### Renaming a branch

While on the branch you can change it's name like so,

```json
git branch -m <new-branch-name>
```

#### Deleting branches

There are various ways to delete a branch, both a local copy and a remote copy.

```json
git branch -d <branch-name>
```

```json
git branch -D <branch-name>
```

```json
git push origin --delete <branch-name>
```

```json
git push origin :<branch-name>
```

_Note: Deleting it locally does not "delete" the branch, it still exists on the remote server until you `push` your changes out, thread with caution._

<details>

   <strong>Exercise 3</strong>
   <summary>🏋🏽‍♀️ Branching, Switching and Deleting</summary>

   Navigate to `lab1` folder as described in [exercise 1](#exercise-1)

   Execute the following commands.

   ```json
   > git branch branch-a
   > git checkout branch-a
   > echo "branch-a first file and content" >> file1-a.txt
   > git add .
   > git commit -m "My first commit on branch a"
   ```

  Create and checkout at the same time
  
  ```json
  > git checkout -b branch-b
  > echo "branch-b first file and content" >> file1-b.txt
  > git add .
  ```

  Create a branch from an existing branch

  ```json
  > git checkout -b branch-a branch-c
  ```

  Delete the new branch created

  ```json
  > git branch -d branch-c
  > git branch -d branch-b 
  ```

  Deleting `branch-b` will fail, to force delete

  ```json
  > git branch -D branch-b
  ```

</details>   


### Committing, Amending and Pushing changes

Commits can be thought of as snapshots or milestones along the timeline of a Git project.

Used when you want to capture the state of changes to the project or mark milestones as the work evolves.

#### Adding new files

Before there is anything to commit, you need to add the any changes or files to git. Essentially what this does is that it notifies git to "keep track" of changes to the file.

```json
git add <file-name>
```

Add's a single file by name

```json
git add . 
```

This adds every file change in current directory.

Key word here is `current` directory. If you have made a repo that has many parent and child folders and you make changes in multiple places then you either have to jump to each folder and run the above command or jump to the parent directory and add all.

#### Committing changes

When you have changes or have reached a point you want to "mark", then its a good time to `commit` your changes after you have [added](#adding-new-files) them.

```json
git status
```

Will show you all changes ready to be committed

```json
git commit –m “your descriptive but brief commit message”
```

```json
git commit –am “your descriptive but brief commit message” 
```

If file has already been [staged](#adding-new-files), you can skip the `add` commands and just use the `-am` flag, this is a short cut to both `add` the file and add a commit message as above.

#### Amending commits

There sometime is the need to amend a most recent commit e.g. You had committed your changes but added a new change that you want to reflect as part of the previous commit set. To do so you can:

```json
git commit --amend
```

To add the new change to the last commit

```json
git commit --amend -m "an updated commit message” 
```

To both add the new change and also update the commit message. For example you notice a typo, happens to the best of us. 🙃

### Changing committed files

```json
git add <the-file>
```

```json
git commit --amend --no-edit
```

> 🔥 Don’t amend public commits, avoid amending a commit that other developers have based their work on, do so only on your local branch/commits.

### Pushing changes to remote

When happy with your changes, you can make it public or visible to other collaborators by placing it in the central location with the below commands.

```json
git push
```

<details>

   <strong>Exercise 4</strong>
   <summary>🏋🏽‍♀️ Committing, amending and pushing changes</summary>
   
   
   ```json
   > git checkout branch-a
   > echo "branch-a #2 edit" >> file1-a.txt
   > git add .
   > git commit -m "second commit"
   ```
  
   Amend the typo in the commit message

   ```json
   > git commit --amend -m "second commit"
   ```
  
   Add a file to the recent commit

   ```json
   > echo "The amend file" >> amended.txt
   > git add amended.txt
   > git commit --amend --no-edit
   ```

</details>   

### Merging, Rebasing and Reverting

There are time when you need to lump things together, combining changes from more than one branch.

Git merge is used to combine changes from two or more branches into a single branch.

Git rebase is used to incorporate changes from one branch into another by rewriting the commit history.

These feature can be very helpful in keeping things organised or help you separate/chunk your work in more that one branch then bring them all together in one branch.

#### Merging

```json
git checkout <branch name>
```

```json
git merge main
```

```json
git merge <branch name> main
```

Options include `--squash`, `--abort`, `--quit`, `-s [our]` etc.

#### Rebasing

```json
git checkout <branchname>
```

```json
git rebase main
```

> The golden rule of git rebase is to never use it on public branches

You can look at a `merge` as combining two repos together, sorting out the difference between them and a `rebase` as adding one repo right on top of the other.

Git has a nice documentation called [Git Branching - Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) that breaks it down in detail. Check it out if you need more clarity.

#### Squashing

This is another feature that allows you group multiple commits into one. If you have multiple small changes committed and want to push them out together then `squash` them.

```json
git log --oneline
```

```json
git rebase -i HEAD~N
```

```json
git merge --squash <branch name> (then commit)
```

> ℹ️ Very useful for tidying up local commits before a milestone push.

_🔥 Note: Try to avoid squashing too many changes into one push that it become one large commit when its review time. Everything in moderation._

#### Reverting

```json
git revert --<hard|soft|mixed> <commit-id>
```

### Pulling, Searching and Aliases

Git pull is used to fetch and download content from a remote repository, updating the local repository to match that content.

Git aliases can shorten common commands and make it easy for you to remember, just try not to go overboard.

#### Pulling

```json
git pull <remote>
```

```json
git pull --rebase <remote>
```

> Used to ensure a linear history by preventing unnecessary merge commits.

#### Searching

You can look into your git repo or history to search for information about past commits, branches etc.

```json
git grep <text> 
```

This will look through files

```json
git log <options> 
```

This will show your commits

<details>
   
   <strong>Exercise 5</strong>

   <summary>🏋🏽‍♀️ Merging and rebasing changes</summary>

   ```json
   > git checkout main
   > echo "from main" >> main-branch.txt
   > git add . && git commit -m "from main branch"
   > git checkout -b branch-c
   > echo "from branch-b" >> branch-b.txt
   > git add .
   > git commit -m "from branch c"
   > ll #to list dir
   > git merge main
   > ll #to list dir
   ```
   
   Rebase with the `main` branch from another branch

   ```json
   > git checkout -b branch-d
   > git rebase main
   ```

   Merge all changes to main branch

   ```json
   > git checkout main
   > git merge branch-a main --squash
   ```
   
   Commit milestone changes before push

   ```json
   > git checkout main
   > echo "change #1" >> first.txt
   > git commit -am "change #1"
   > echo "change #2" >> first.txt
   > git commit -am "change #2"
   > echo "change #3" >> first.txt
   > git commit -am "change #3"
   > git log --oneline
   > git rebase -i HEAD~3
   ```
   
   Reverting a recent changes
   
   ```json
   > git checkout -b revert-b
   > echo "change to revert #1" >> revert.txt
   > git add . && git commit -m "revert change 1"
   > echo "change to revert #2" >> revert.txt
   > git add .
   > git commit -m "revert change 2"

   > git log --oneline
   > cat revert.txt
   > git revert -e <commitid>
   ```

</details>   

#### Aliases

```json
git config --global alias.<name> ‘<git subcommand options>’
```

```json
git config –e 
```

To open your default editor

```json
git config --list
```

To show your git configuration and all the crazy aliases you have set, and the one you had forgotten about 🙃.

<details>
   
   <strong>Exercise 6</strong>

   <summary>🏋🏽‍♀️ Pulling, searching and aliases</summary>
   
   Pulling in changes from a remote repo

   ```json
   > git pull --rebase <remote>
   ```
   
   To search your repository
   
   ```json
   > git grep <text> - will look
   > git log --committer felix  --pretty=format:"%h - %an, %ar : %s" --no-merges
   > git log --grep=ukw --pretty=format:"%h - %an, %ar : %s"
   ```
   
   Working with aliases and git config

   ```json
   > git config --list
   > git config --global
   ```

   _Below are just examples, you don't have to execute them as they will update your git config file. The aliases may not be relevant to you_

   ```json
   > git config --global alias.onlinegraph 'log --oneline --graph --decorate'
   > git config --global alias.expirenow 'reflog expire --expire-unreachable=now --all'
   ```
   
   To use your aliases e.g.

   ```json
   git onlinegraph
   ```

</details>   

### Next Steps

Git is really powerful and has lots of features, it can sometimes feel overwhelming but practicing one feature at a time is the way to go, you can try out most commands locally.

### For More Adventures

Any of the below commands in your terminal will provide you with lots of git related information. Comes in handy when you quickly want to verify a command or look up a concept.

```json
git –help 
git help -a
git help -g
```

These will show you lots of other options you can have a play with.

### Practice, practice, practice

Some great places to look to for deeper learning
- [Git documentation](https://git-scm.com/doc)
- [Git In The Trenches](https://cbx33.github.io/gitt/intro.html)
- [For More Adventures](#for-more-adventures) section commands

FYI don't get budged down with too much git detail, majority of the time the common commands will be more than enough for your daily work, having good knowledge of git, or what it can do, comes in handy when those edge cases crop up, usually when working with large teams or on a very active repo with many developers pushing changes near simultaneously.

These days however, most of our IDE's come baked with lots of git  capabilities via plugins and extensions. You just need to install one if not already and you're good-to-go.
