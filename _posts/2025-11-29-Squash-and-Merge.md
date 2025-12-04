---
author: Raman Adlakha
categories: note
image: draft-pr-2.jpg
layout: post
tags: git github workflow pr squash
title: Draft PRs + Squash & Merge
---

Here’s the exact workflow I’ve settled on after years of trying everything else. It gives me:

- Rich daily collaboration and WIP visibility  
- Zero noise in main’s history  
- One perfect commit per logical feature

## The full flow

```bash
# 1. Start the feature
git switch -C feature/add-dark-mode

# 2. Make your first commit (anything at all)
git add .
git commit -m "start dark mode toggle"

# 3. Create a draft PR immediately (it pushes for you)
gh pr create --draft --title "Add dark mode support" --body "WIP – iterating on design"

# 4. Work normally for hours/days/weeks
git add .
git commit -m "add prefers-color-scheme detection"
git push

git commit -m "fix contrast on buttons"
git push

git commit -m "try tailwind dark: classes"
git push
# → teammates can see everything, comment, react, etc.
```

If you need to pull from main as you go, you can do so with:
   ```bash
   git stash
   git fetch origin
   git rebase origin/main
   git stash apply
   git stash clear 
   ```

### Repeat step 4 as many times as you want.
When the feature is actually done

```bash
# 5. Mark ready + final polish
gh pr ready

# 6. (Optional) leave a comment for reviewers
gh pr comment --body "@team ready for final review – tests passing on all browsers"

# 7. Merge using the only correct button
→ On GitHub UI: choose "Squash and merge"
→ Edit the commit message (see template below)
→ Confirm squash and merge
```

### My go-to squashed commit message template

```text
Add dark mode support with user toggle and auto-detect

- Implement prefers-color-scheme media query
- Add manual toggle in settings with persistent cookie
- Update all components to respect dark: variants
- Add e2e tests for both light/dark flows

Closes #123
```

Result in main: one clean, readable, bisectable commit. All the messy daily commits and PR discussion live forever in the PR thread.

I wrote this as a permanent reminder to myself. Draft PRs for collaboration, squash for history. Nothing else comes close.

Hope it helps you too.