---
layout: post
title: "Warp in practice: maintaining a legacy Toggl GAS integration with Vakya"
author: "Raman Adlakha"
categories: note
tags: [warp, agents, github, gas, toggl, automation]
image: toggl.jpg
---

Over the last few weeks I’ve been slowly building up a collaboration workflow between myself and an AI agent (Vakya Sutra) inside Warp. Earlier posts focused on using Warp as an Agentic Development Environment and on giving Vakya a proper identity, branching strategy, and PR rituals.

In this post, I wanted to document a concrete, end‑to‑end use case where this setup really proved itself: reviving and taking ownership of an old Google Apps Script that syncs my Toggl time entries into Google Calendar.

This was a nice combination of:

- **Real maintenance work** on a script that had silently broken over time.
- **Warp + Vakya collaboration**, treating the agent as a teammate rather than a fancy autocomplete.
- **Lightweight, human‑scale process**, appropriate for a small but useful tool.

## 1. The legacy script that quietly broke

Years ago I had adopted a small Google Apps Script that pushed my Toggl time entries into a dedicated Google Calendar. It had worked well enough that I mostly forgot about it; it just ran in the background and kept my calendar populated with time tracking data.

At some point, though, things stopped updating. The calendar stayed frozen while Toggl showed plenty of fresh entries. The original GitHub repository hadn’t seen activity in a long time, and it was clear nobody else was going to step in and modernize it.

The script was valuable enough that I didn’t want to abandon it. That made it a perfect candidate for my “developer + agent” pairing setup:

- It was **small** and **self‑contained**.
- It involved a **real external integration** (Toggl + Google Calendar).
- It had broken due to changes elsewhere rather than anything I’d done.

The goal for this session was simple: **make the script work again, and bring it under my own maintenance** so future changes would be manageable.

## 2. Setting the stage in Warp

Compared to my earlier Warp experiments, this session felt more natural and fluid. I’m now more comfortable with:

- Using the **Universal Input** to move seamlessly between shell commands and agent conversations.
- Letting Warp surface **context about the current directory and Git branch** so I’m always aware of where I am.
- Navigating repos and files through the built‑in tooling rather than bouncing between editor, terminal, and browser.

I opened the legacy repository in Warp, described the problem to Vakya, and let her inspect the README and code layout. From there, the workflow stayed inside Warp:

- Run commands to inspect the project.
- Ask questions about unfamiliar parts of the script.
- Let the agent propose small, targeted changes.
- Validate behavior again from the same window.

The UI stayed in the background—reliable and pleasant—while the focus was on the collaboration and the problem we were solving.

## 3. Forking and taking ownership

Since the original repository was no longer maintained, I didn’t want to patch it in place. Instead, I:

1. **Forked** the project under my own GitHub account.
2. Updated the local Git remote so `origin` pointed to my fork.
3. Added **Vakya’s GitHub identity** as a collaborator on the fork.

This preserved the original author’s work while making it clear that:

- Future maintenance will happen on **my fork**.
- Changes made by the agent will appear under **Vakya Sutra’s identity**, not mine.

Inside Warp, I asked Vakya to:

- Set the local Git author to `Vakya Sutra <vakya@adlakhas.com>`.
- Create a **feature branch** (with a neutral, intent‑based name).
- Keep all changes scoped to that branch and never touch `main` directly.

By this point, the pattern from the checklist project felt very natural: branches, not direct commits to main; draft PRs, not surprise merges; clear authorship for the agent.

## 4. Debugging the broken integration together

The actual breakage manifested as failures when the script tried to talk to Toggl: requests that used to work no longer did. Rather than jumping straight into wild changes, we used a simple loop:

1. **Reproduce the failure** from within Apps Script by running the main function manually.
2. **Inspect the client code** that talks to Toggl and builds calendar events.
3. **Adjust the integration** to match how Toggl behaves today.
4. **Run again** and verify that events appear in the target calendar.

I deliberately kept the discussion with Vakya high‑level:

- “This request used to work; now it doesn’t.”
- “Help me align the client with how Toggl expects to be called now.”
- “Keep the behavior the same: one calendar event per finished Toggl entry in a rolling window.”

Once the changes were in place, we redeployed the updated script to Google Apps Script and manually triggered a run. Seeing fresh Toggl entries materialize as Calendar events again was a satisfying confirmation that we’d hit the right balance: minimal changes, maximum effect.

## 5. Evolving the client without over‑engineering

One subtle but important improvement we made was around how the script handled **projects**:

- Originally, it fetched project data one‑by‑one as needed.
- Together, we shifted to **preloading the user’s projects once** and caching them for the duration of a run.
- Calendar event titles now continue to include project names without making redundant calls.

Crucially, we resisted the temptation to turn this into a mini SDK. For a small personal integration like this, I didn’t need:

- Full pagination handling for every endpoint.
- Elaborate cache invalidation strategies.
- Asynchronous pipelines or generalized client abstractions.

Instead, the guiding principles were:

- **Keep it synchronous and straightforward**, matching how Google Apps Script operates.
- **Avoid unnecessary complexity**, especially anything that would obscure the logic for future me.
- **Be pragmatic**: if a missed edge case only means a missing suffix in a title rather than data loss, it’s probably not worth a big refactor.

Vakya was good at floating potential improvements and then helping me decide which ones were **overkill for this project** and which ones were worth including.

## 6. Branching, commits, and PRs with Vakya

As in the checklist project, I leaned heavily on the collaboration rules we’d already established for Vakya:

- **Branches, not main**: all work happened on a dedicated feature branch.
- **Atomic commits**: each change was grouped logically, with subjects prefixed by `[VAKYA]`.
- **Structured PR**: when we were satisfied, Vakya created a **draft pull request** from the branch to my fork’s main branch.

The PR description followed the same pattern I’ve been using:

- A short **Summary** of what changed.
- A **Why** section explaining the motivation (in this case, a broken integration due to upstream changes).
- **Notes for Review** listing assumptions and the scope of the change.

From my perspective, this made reviewing the work almost identical to reviewing a human teammate:

- I could view the diff with the context of the original script.
- I could see that the changes were appropriately small and targeted.
- I retained control over when to mark the PR ready and when to merge.

One nice side effect of doing this inside Warp is that the path from “idea” to “merged PR” is surprisingly short: I don’t need to context‑switch into browser tabs just to orchestrate the process.

## 7. What I learned about Warp and Vakya from this session

This session felt different from my earlier Warp experiments in a few ways:

- **Warp’s UI had moved into the background**. I wasn’t “trying a new terminal” anymore; I was just working, and the interface got out of the way.
- **Vakya felt like a reusable teammate**. The same identity, rules, and habits I’d defined for the checklist project transferred cleanly into this new context.
- **Maintenance work is a great fit for agent collaboration**. This wasn’t green‑field development; it was about understanding someone else’s code, adapting it, and taking responsibility for it going forward.

In the end, I got exactly what I wanted:

- My Toggl → Google Calendar integration is **alive again**.
- The repository is now under **my control**, with a clear history of how it was modernized.
- I have even more confidence that Warp + Vakya is a durable setup for the kind of everyday maintenance tasks that keep my tools running.

There will certainly be more upstream changes in the future—APIs evolve, platforms shift—but now I have both the **infrastructure** (fork, branch, PR) and the **collaborator** (Vakya in Warp) to adapt without starting from scratch each time.
