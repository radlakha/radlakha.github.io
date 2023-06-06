---
layout: post
title: "Working with Repos on GitHub"
author: "Raman Adlakha"
categories: story
tags: [tag1,tag2]
image: boy-in-park.jpg
---

# Working with repos on GitHub

Follow the below instructions to 

- access the repo
- contribute code to it.
- review the code of your teammates

---

- **Setting up your dev env and downloading the code**
    
    - I prefer to use codespaces, though, you can use a local dev env too
    - Refer to Readme in the repo for instructions to setup your dev env

    
- **Working on your feature/refactor/bug**
    - If this is the first time accessing the repo you can clone it to your dev env
    ```bash
    git clone <url>
    ```

    - Start with the latest and greatest codebase
    
    ```bash
    git checkout main
    git pull origin main
    ```
    
    - Create your own branch now
    
    ```bash
    git checkout -b this_is_the_branch_i_will_be_working_on
    git status
    ```
    
    - Do your thing by adding/modifying code
    - Keep committing to the local branch you created as often as you think you are at a logical point in your work
    - Push your work to remote once ready to create a PR (Pull Request) for review. You can otherwise also keep pushing the code if you want others to download and look before the formal PR
    
    ```bash
    git status
    git add *
    git commit -m 'Do this and that'
    # the first time
    git push origin this_is_the_branch_i_will_be_working_on
    # subsequently you can just say
    git push
    ```
    
    - Ideally, you should be doing this frequently - like, at least, once in 48 hours
    - But if you get much behind, or otherwise too, good practice to pull the latest from the main. If auto-merge fails, resolve merge conflicts with your branch just before creating a PR
    
    ```bash
    # get the latest main in the local rep
    git checkout main
    git pull origin main
    # Then switch back to feature branch and merge in main
    git checkout this_is_the_branch_i_will_be_working_on
    git merge main
    git commit -m 'Merge latest from main. Now ready to create PR'
    ```
    
    - Now head over to the web interface and create a PR from your branch. Refer your branch for review and propose it be merged into the main
- **Review process**
    - A reviewer will receive your PR and will start the reviewing process by reading your PR request. Make sure you did a good job of describing changes. If your team uses a PR Template it will show up automatically when drafting PR.
    - Reviewers will add comments to your PR overall and/or against the code you submitted by going to the ‘Files changed’ tab. Respond to these comments.
    - If you are required to take action per the review, head over to your local repo and make changes. Once done push your code up to remote again. The PR will be automatically updated
    
    ```bash
    # switch to your branch
    git checkout this_is_the_branch_i_will_be_working_on
    # just in case reviewer made any code changes on the fly (usually they do not)
    git pull origin this_is_the_branch_i_will_be_working_on
    # make your changes and once done
    git status
    git add *
    git commit -m 'add change or whatever per the review comments'
    git push
    ```
    
    - The reviewer will do the final checks. As a reviewer you may add some test cases, build the code and run locally before finally approving the PR.
    - _Look further down for more tips for reviewers._
    ```bash
    # get the latest main in the local rep
    git checkout main
    git pull origin main
    # Then switch back to feature branch and merge in main
    git checkout this_is_the_branch_i_will_be_working_on
    git merge main

    # now review, run the test cases etc to satisfy yourself this PR is ready to be approved
    ```
    
- **Time to merge with the main!**
    - You or the reviewer can now merge your branch with main
    
    ```bash
    git checkout main
    git merge --no-ff this_is_the_branch_i_will_be_working_on
    ```
    - OR, prefer squash. Read [this](https://blog.dnsimple.com/2019/01/two-years-of-squash-merge/) to understand why
    
    ```bash
    git merge --squash this_is_the_branch_i_will_be_working_on
    git push origin main
    ```
    
- **Delete the branch and clean-up**
    
    ```bash
    # delete the branch from local repo
    git branch -d this_is_the_branch_i_will_be_working_on
    # delete the remote branch. You can use web interface too to delete remote branch
    git push origin --delete this_is_the_branch_i_will_be_working_on
    ```
    
- **Helpful tips for thorough reviewers**
    - Here is a look at the workflow of reviewers who want to be thorough by downloading the code and inspecting it before approving
    - If a reviewer wants to do more than a visual check of code on a web interface keep reading
    - If you are in the middle of your own code changes when a PR request lands at your door here are things you can do
        - Check-in your code into your local branch. You are now ready to download the proposed code from referred branch. You can resume your work by checking out your branch again once the review is done.
        - Stash your code. If you are a stickler and do not like to do commits unless there is a logical stop you may set aside (stash) your code
    - Let us look at these two options:
    - Check-in your code into your local branch
    
    ```bash
    # check where are you
    git status
    # if you forgot to create your feature branch and are working off of main now is the time
    git checkout -b this_is_my_work
    git commit -am 'brb'
    ```
    
    - Stash your code
    
    ```bash
    # Stash all your local, uncommitted changes since the last commit 
    git stash
    ```
    
    - Now you are ready to pull the PR code from the remote using the command `git fetch origin pull/<ID>/head:<branchname_of_your_choice>. ID is the pull request ID
    
    ```bash
    git fetch origin pull/7834/head:PR_7834
    git checkout PR_7834
    ```
    
    - Or, you could simply pull down the referred remote branch proposed in the PR
    
    ```bash
    git checkout -b this_is_the_branch_i_am_working_on origin/this_is_the_branch_i_am_working_on
    # :-) if you are going to be sharing your code, pick your branch names so they make sense universally and not just in your context
    ```
    - Or, super simple is the new git command - switch

    ```bash
    git switch this_is_the_branch_i_am_working_on
    ```

    - When it is time to get back to what you (the reviewer) were doing, simply switch back to your branch
    
    ```bash
    git checkout this_is_my_work
    # if you stashed your code time to pop
    git stash pop
    ```
    
- **Finally, the below commands are helpful for cleaning up your working directory in case there is a need**
    
    ```bash
    # to make your branch match the remote main
    git branch this_is_the_branch_i_will_be_working_on
    # retrieve the latest changes from the remote repository named "origin" without modifying your local branches
    git fetch origin
    # reset your current branch to the state of the "origin/main". 
    # discard any local commits and changes you made in your working directory and makes your branch's history and code match the "origin/main" branch
    git reset —hard origin/main
    # remove untracked files and directories from your working directory
    git clean -xdf
    ```
    

---

*Works Cited*

1. Jim Vallandingham [*Feature Branches and Pull Requests: Walkthrough*](https://gist.github.com/vlandham/3b2b79c40bc7353ae95a)
