---
author: Raman Adlakha
categories: note
image: draft-pr-1.jpg
layout: post
tags: git github pr workflow
title: Workflow for Draft PRs with Multiple Commits
---

Here are step-by-step instructions for a low-friction Git workflow using GitHub CLI to build features across multiple logical commits, all feeding into a single draft PR:

1. **Create and switch to a new branch.** Start fresh from your main branch:
   ```bash
   git switch -C feature/login-redesign
   ```

2. **Make your first commit.** Work until you hit a natural stopping point (e.g., a component works), or if you need to push a partial commit for your teammates to look at, then commit:
   ```bash
   git add .
   git commit -m "start login redesign"
   ```

3. **Create the draft PR (pushes the branch automatically).** Use GitHub CLI to set it up as a draft right away:
   ```bash
   gh pr create --draft --title "Login redesign" --body "WIP: adding new form"
   ```
   This creates the PR in draft mode. Your teammates can look and share thoughts as you go, and you can keep working on the feature without any pressure. 

4. **Keep committing and pushing as needed.** Continue developing—commit logically (e.g., after each feature slice or refactor), and push normally:
   ```bash
   git add .
   git commit -m "add form validation"
   git push  # Automatically adds to the existing draft PR
   ```
   Repeat for as many commits as make sense. Each push updates the PR without extra commands.

If you need to pull from main as you go, you can do so with:
   ```bash
   git stash
   git fetch origin
   git rebase origin/main
   git stash apply
   git stash clear 
   ```
5. **Add comments as necessary and mark ready when done.** When the work feels solid, loop in the team:
   ```bash
   gh pr comment --body "Hey team, just added validation—thoughts on the error messaging?"
   gh pr ready  # Converts draft to ready for review
   ```

This keeps your branch history clean and atomic while letting you iterate privately in the draft. No more one-and-done commits. When merging you may choose to squash or rebase the commits. More on that in the [next post](Squash-and-Merge).

I wrote this as a quick reminder to myself after tweaking my own flow. Hope it helps you too. 