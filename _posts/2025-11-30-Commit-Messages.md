---
author: Raman Adlakha
categories: note
image: draft-pr-3.jpg
layout: post
tags: git github commit-message conventional-commits
title: Commit Messages for Draft PRs + Squash Workflow
---
This is part three of my tiny Git trilogy.  
[Part 1: Draft PRs](Draft-PR-Workflow)
[Part 2: Always Squash & Merge](Squash-and-Merge)

Now that every PR ends with exactly one commit in main, the commit message suddenly matters a lot — and becomes incredibly easy to get right.

### Rule 1 – During the draft PR (the messy days)
Write whatever helps you and your teammates right now. Seriously.

```shell
git commit -m "wip: start payment flow"
git commit -m "try stripe elements"
git commit -m "fix tests on safari"
git commit -m "debug why amount is null"
git commit -m "add console.log"
```

These messages will be deleted forever when we squash. Zero guilt.

### Rule 2 – The only message that lives forever
When you click “Squash and merge”, replace everything with this template:

```text
type(optional-scope): short summary in imperative mood

Longer explanation of WHAT and WHY (not how the code works).
Wrap lines at 72 characters.

Bullet points for non-obvious bits
Mention migrations, breaking changes, performance impact

Closes #123 or Fixes #456 at the bottom if needed
```

### Rule 3 – Types I actually use every week

```shell
feat:      → new feature for the user
fix:       → bug fix for the user
refactor:  → code change that is neither feat nor fix
style:     → formatting, missing semi-colons, etc.
test:      → adding or correcting tests
chore:     → tooling, config, CI, dependencies
docs:      → only documentation changes
```

### For example

```shell
feat(payments): add Stripe Elements with SCA support

Replace old card fields with Stripe Elements
Add proper error handling and loading states
Store payment method on customer instead of creating charges directly
Add webhook handler for payment_intent.succeeded/failed

Closes #891
```

### Rule 4 – One PR = one logical change
If your summary starts with “Add X and also Y”, split it into two PRs.

Hope this completes the trilogy for you too. Happy committing!

*Previous posts in this trilogy:*

- [Workflow for Draft PRs with Multiple Commits](Draft-PR-Workflow)
- [Always Squash & Merge](Squash-and-Merge)	