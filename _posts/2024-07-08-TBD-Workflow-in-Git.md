---
author: Raman Adlakha
categories: note
image: camden-friend.jpg
layout: post
tags:
- github
- git
- workflow
- coding
title: Trunk-based Development in Git
---

# TBD Workflow in Git

Trunk-based development is the most suitable branching strategy if a
team is aspiring to achieve high velocity and quality and also reduce
failure rates and the mean time to recover.

Here is a typical few hours in life of a developer who is coding in a
repo using TBD. \## Create or clone a repo

``` bash
git clone the-repo ~\repos\the-repo
```

## Switch to branch

Switch to or create branch you are interested in

``` bash
git switch JIR-9999
```

## Fetch often

If you are part of the team where TBD is followed passionately, you can
expect many commits to the remote branch. Therefore, you should be
downloading those changes frequently. Be the judge of the frequency but
remember to do at least once before you are ready to commit and/or
create a pull request.

``` bash
git fetch --all -p
```

## Changes coming down?

You can use merge, or rebase. I prefer rebase.

``` bash
git rebase -i origin/main
```

This command allows to interactively replay your changes on top of
changes you got handed down.

Edit the file opened in the editor. You can choose to: - Keep a commit
as is (by leaving it as “pick”). - Reorder commits by changing their
order in the list. - Squash commits (combine them into one) by changing
“pick” to “squash” or “s.” - Edit commit messages by changing the text
after “pick.”

## Uncommitted changes of your own

If you are in the middle of your code changes and wanting to do a fetch,
it is possible you have uncommitted changes. You should not be forced to
commit your changes because you are about to fetch. This is the time to
stash your changes.

``` bash
git stash push
git rebase -i origin/main
git stash pop
git add *
git commit -m "Commit message"
```

## Shortcuts

Now, if you understood the steps above — there is good news. You can do
stash, rebase, and pop in one command.
`git rebase -i origin/main --autostash`

## Push your commits

Remember all commits stay on your laptop unless you push it up to remote
Repo. If you have to create a pull request or if you want to informally
get feedback on your code you need to push it. Generally, it is not a
good idea to push the code if it will break the build — even if it is on
your branch.

``` bash
git push
```

## Create a Pull Request

Once you think the job is done, you should follow your team guidelines
to create a pull request. Your team may require you to ensure certain
amount of testing, linting etc. and also the use of certain feature
flags. Make sure you follow those. A pull request could be to merge
changes to main or to include your changes back into repo from where you
forked.

GitHub has a web interface to help you create a pull request. IDEs such
as VS Code now allow creating pull request from within the IDE. CLI is
also available to create a pull request.
