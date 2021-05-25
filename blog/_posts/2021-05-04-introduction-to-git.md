---
layout: post
title: Introduction to Git
category: Blog
comments: true
---

This post is the note on the [DataCamp](https://www.datacamp.com/) [course](https://learn.datacamp.com/courses/introduction-to-git) led by Greg Wilson, the Co-founder of [Software Carpentry](https://software-carpentry.org/).

## Basic workflow

Git is a modern version control tool created by Linus Torvalds in 2005, now it is very popular with data scientists and software developers. It can keep track of changes to files, notice conflicts between changes made by different people, and synchronize files between different computers.

A repository is the combination of two parts: the files and directories, and their historical information that Git records which are called `.git` located in the root directory.

- `git status` — check the status of your repository
- `git diff filename` — show you the changes
- `git add filename` — add a file to the staging area
- `git diff -r HEAD path/to/file` — compare the state of your files with those in the staging area, the `-r` flag means "compare to a particular revision", and `HEAD` means "the most recent commit"
- `nano filename` — use Nano to edit `filename`
- `git commit -m "some message in quotes"` — commit the changes in the staging area with a log message, `git commit  --amend -m "new message"` change a commit message
- `git log` — view a repository's history, `git log -3 filename` show the last three commits involving a specific file

## Repositories

Git uses a three-level structure for information stored by each commit.

1. A **commit** contains metadata such as the author, the commit message, and the time the commit happened.
2. A **tree** tracks the names and locations in the repository when that commit happened.
3. A **blob** (short for *binary large object*) contains a compressed snapshot of the contents of the file when the commit happened.

![Git Structure](https://assets.datacamp.com/production/repositories/1545/datasets/1bb404075fe1164d8b3fd78f4065b0bf3d86bc16/gds_2_1_SVG.svg)

Looking at the diagram `SVG` (zoom for better clarity), first in the oldest (top) commit, there were two files tracked by the repository, then `report.md` and `draft.md` were changed in the middle commit, so the blobs are shown next to that commit. `data/northern.csv` didn't change in that commit, so the tree links to the blob from the previous commit. Reusing blobs between commits help make common operations fast and minimize storage space.

- A hash is a unique identifier for every commit, which enables Git to share data efficiently between repositories.

- The special label `HEAD` is another way to identify a specific commit. It always refers to the most recent commit. The label`HEAD~1` then refers to the commit before it, while `HEAD~2` refers to the commit before that, and so on.

- `git annotate file` — show who made the last change to each line of a file and when.
- `git diff ID1..ID2` — show the changes between two commits, `..` is a pair of dots.
- A `.gitignore` file in the root directory tells Git to ignore certain files.
- `git clean` — only works on untracked files, `git clean -n` show a list of files whose history Git is not currently tracking, `git clean - f` delete those files for good.
- `git config --list` — see what the settings are with one of three additional options:
  + `--system` — every user on this computer
  + `--global` — every one of your projects
  + `--local` — one specific project
- `git config -- global setting value` — change a configuration value for all of your projects on a particular computer.

## Undo

- `git reset HEAD` — unstage the additions, `git reset` unstage everything.

- `git checkout -- filename` — discard the changes that have not yet been staged, `git checkout -- .` revert all files in the current directory.

- ```bash
  git reset HEAD path/to/file
  git checkout -- path/to/file
  ```

  By combining `git reset` with `git checkout`, you can undo changes to  a file that you staged changes to.

- You can think of committing as saving your work, and checking out as loading that saved version. `git checkout ID filename` would replace the current version of a file with the version that `ID` identified. Notice that this is the same syntax that you used to undo the unstaged changes, except `--` has been replaced by `ID`.

## Working with branches

Branches allow you to have multiple versions of your work and let you track each version systematically. A commit will have two parents when branches are being merged, that's why Git needs both trees and commits.

- `git branch` — list all of the branches in a repository
- `git diff branch-1..branch-2` — show the difference between two branches
- `git checkout branch-name` — switch to a branch
- `git checkout -b branch-name` — create a branch
- `git merge source destination` — merge one branch (source) to another (destination)

## Collaborating

- `git init project-name` — create a repository for a new project in the current working directory
- `git init /path/to/project` — convert existing projects into repositories
- `git clone URL` — clone a repository, `git clone /existing/project` use a path, `git clone /existing/project newprojectname` call the clone something else
- `git remote` — list the names of its remotes, `git remote -v` ("v" for "verbose") show the remote's URLs
- `git remote add remote-name URL` — add more remotes
- `git remote rm remote-name` — remove existing ones
- `git pull remote branch` — get everything in `branch` in the remote repository identified by `remote` and merges it into the current branch of your local repository
- `git push remote-name branch-name` — push the changes you have made locally into a remote repository

## Useful links

- [Step-by-step guide to contributing on GitHub](https://www.dataschool.io/how-to-contribute-on-github/)
- [Pro Git book](https://git-scm.com/book/en/v2)
- <http://git.io/sheet> — a list of cool features of Git and GitHub
- [Learn Git Branching](https://learngitbranching.js.org/) — the visual and interactive way