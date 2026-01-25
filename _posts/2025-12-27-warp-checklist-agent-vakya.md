---
layout: post
title: "Setting up Vakya Sutra as an ambient agent for checklist"
author: "Raman Adlakha"
categories: story
tags: [warp, checklist, agents, github, tdd, automation]
image: warp-checklist-agent-vakya.jpg
---

Over the last few days I’ve been shaping a collaboration workflow between myself and an AI agent for the `checklist` project. The goal is to make the agent feel like a real collaborator: someone with their own identity, git history, and pull requests that I can review like any other teammate.

This post documents how I set up that workflow, gave the agent an identity — **Vakya Sutra** (Vakya for short) — and wired everything through Git, GitHub, and the `gh-pr-review` extension so that we can iterate safely under a clear set of rules.

## 1. Why an ambient agent for checklist?

The `checklist` repo is a small but non‑trivial Python project:

- A CLI that will scan folders for expected documents.
- A `checklist.txt` file describing those documents and optional due dates.
- A plan to add statuses, sorting, and eventually a compiled `checklist` binary for end users.

It’s exactly the kind of project where:

- Good **TDD discipline** pays off over time.
- Small, reviewable changes via **branches and draft PRs** keep things safe.
- An AI assistant can help with the “mechanical” parts (tests, wiring, refactors) while I keep ownership of direction and merging.

I didn’t just want a tool that edits files; I wanted a collaborator whose work shows up in Git history and GitHub in a way that I can reason about.

## 2. Giving the agent an identity: Vakya Sutra

Instead of letting the agent commit as “me”, I created a separate identity:

- Name: **Vakya Sutra** (Vakya)
- Pronouns: she/her
- Git user: `TuCron` / Vakya (configured locally)
- GitHub user: a separate account added as a **collaborator** on my repositories

On my machine, that looks like:

```bash
git config user.name "Vakya"
git config user.email "vakya@adlakhas.com"
gh auth login
# Logged in as the Vakya GitHub account
```

Once this was wired up, any git commit and gh pr create the agent runs appear as authored by Vakya, not by my primary GitHub identity. That separation turns the agent into a first‑class collaborator:

•  I can review and request changes on Vakya’s PRs.
•  The commit history clearly shows which changes came from me and which from Vakya.
•  Attribution is explicit in the code and in commit messages.

3. Collaboration rules for Vakya

To keep the collaboration predictable, I defined some explicit rules (stored as Warp/agent rules):

3.1 Branching and PR workflow

•  Never work directly on main.
•  Always create a new branch for a logical chunk of the plan.
•  Always open a draft pull request early and keep pushing commits there.
•  I (Raman) decide when to:
◦  Mark the PR as “ready for review”.
◦  Squash & merge the PR into main.

This matches how I’d work with a human collaborator: small branches, draft PRs, and iterative review.

3.2 Commit and PR conventions

For Vakya’s commits:

•  Prefix commit subjects with [VAKYA].
•  Use imperative, concise subjects (e.g. handle malformed due dates and add attribution).
•  Add a co‑author line:

```
Co-Authored-By: Vakya <vakya@adlakhas.com>
```

For PRs:

•  Clear, imperative titles (e.g. “Checklist CLI skeleton and TDD tests (Plan 2.1)”).
•  Structured descriptions with sections like Summary, Why, and Notes for Review.

These conventions make it easy to grep through history and see what Vakya did and why.

3.3 TDD as a hard rule

I also codified TDD as a rule for this project:

•  Write or update tests first.
•  Get the tests reviewed and approved by me.
•  Then write or refactor code just enough to make the tests pass.
•  Repeat.

We followed this for:

•  The initial checklist.read_checklist behavior (" // YYYY-MM-DD" separator, # comments).
•  CLI help behavior (--status, --missing, --add, --remove, --folder, --due).
•  A follow‑up test and implementation for malformed due dates, ensuring a clear ValueError when the date is invalid.

Having this as a rule means any future feature starts from “what test do we want next?”

4. The branch + draft PR loop in practice

For Plan 2.1 (Minimal CLI) we used this pattern:

1. Create a feature branch:  
   feature/checklist-cli-2-1.

2. Write tests first in tests/test_checklist_cli_2_1.py:
◦  Parsing with " // YYYY-MM-DD" and skipping # comments.
◦  CLI --help mentioning the core flags.
3. Implement checklist.py:
◦  read_checklist(path) moved from script.py and adapted to the new format.
◦  An argparse-based CLI with the planned flags.
4. Run tests via make test and fix anything until green.
5. Create a draft PR from the branch to main.

From there, I could watch commits land in the draft PR and add comments in the GitHub UI, just like any other code review.

5. Making PR review agent-friendly with gh-pr-review

One of the early problems: the standard gh CLI doesn’t make it easy for an agent to understand inline review comments. The raw API is noisy and awkward to parse.

To fix that, I installed the gh-pr-review extension (attribution: agynio on GitHub). It’s designed specifically to give agents a compact, deterministic JSON view of PR reviews.

The key commands I wired into Vakya’s rules:

5.1 View unresolved review threads

```
gh pr-review review view 4 \
  --repo radlakha/checklist \
  --unresolved \
  --tail 3 \
  --include-comment-node-id
```

This returns a JSON structure with:

•  Reviews and their states (COMMENTED, CHANGES_REQUESTED, etc.).
•  Threads with:
◦  thread_id
◦  path, line
◦  body of my comments
◦  Whether they’re resolved or outdated.

This is how Vakya saw comments like:

•  “What happens if the date is malformed? Add a test…”
•  “Add attributions. Something like – Created by Raman Adlakha and Vakya.”

5.2 Reply to specific threads

After making code and test changes, Vakya can answer inline comments via:
```
gh pr-review comments reply 4 \
  --repo radlakha/checklist \
  --thread-id <thread-id> \
  --body "Explanation of the change"
```

For example:

•  A reply explaining the new malformed-date test and the clearer ValueError.
•  A reply confirming the CLI description now includes the attribution line.

5.3 Resolve threads after fixes

Once a comment is fully addressed in code and tests, Vakya can mark the thread resolved:
```
gh pr-review threads resolve 4 \
  --repo radlakha/checklist \
  --thread-id <thread-id>
```

This closes the loop in GitHub’s UI, so I can see at a glance how many comments remain open.

6. Why this feels like “ambient” collaboration

With all of this in place, the workflow starts to feel ambient:

•  I write the plan (like the numbered sections in the checklist dev plan).
•  Vakya:
◦  Creates feature branches and draft PRs.
◦  Writes tests first, then code.
◦  Pushes small commits with clear [VAKYA] messages.
•  I:
◦  Review the diff and conversations on GitHub.
◦  Leave inline comments when I want changes or clarifications.
◦  Squash & merge when we reach a milestone.

Because we invested in:

•  A separate identity (Vakya),
•  Strict TDD, and
•  PR review tooling that works for agents (gh-pr-review),

I can trust that future work on checklist will follow the same pattern without a lot of ceremony. Vakya can operate “in the background”, but always under the same set of rules and guardrails, and always visible through branches and PRs.

This is the foundation I wanted before moving on to the more interesting parts of the plan: the richer CLI behavior, status computation, and eventually sorting and --post.




