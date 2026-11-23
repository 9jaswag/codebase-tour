---
name: codebase-tour
description: Use when someone asks to understand an unfamiliar flow, feature, service, module, or subsystem in a codebase end-to-end, including how its functions, components, and services interact, or asks where a change to it would be made.
license: MIT
---

# Codebase Tour

## Overview

Understanding a codebase section means tracing one real execution path and placing a change in it. Build it in chunks, with evidence, and prove it by playback — not with a file inventory.

**Core principle:** one flow, hop by hop, one chunk per turn, until the person can retell it and place a change.

## The Contract

Five chunks, in order. Each is one response ending in its checkpoint; 3 and 4 may repeat. The session ends only after playback.

| # | Chunk | Ends with |
|---|-------|-----------|
| 1 | Frame | Boundary, canonical flow, assumed level; ask which flow if the term is ambiguous |
| 2 | Entry | Entry points and first hops; "is this the flow you mean?" |
| 3 | Spine | Core path, 3–6 hops; "what's unclear?" |
| 4 | Edges | Branches, async, failures; "what's unclear?" |
| 5 | Change map + playback | Change → location; retell and place a change |

## 1. Frame

Name the section in the person's words. State what is inside and outside. If the term means two things (for example "onboarding" as signup-plus-wallet or the `/onboarding` wizard), name both and ask. Pick the canonical flow — the user-triggered path that creates, reads, or mutates the core object — and state its trigger in one line. State your read of their familiarity — repo, stack, and this area — and invite correction. Ask at most two questions, then stop.

## 2. Entry

Find real entry points: routes and pages, server handlers, CLI commands, queue consumers, UI actions, and their tests. Show the first hop or two — trigger → entry → bootstrap → state source — with `file:line`, naming the mechanism (SSR, client fetch, queue, cron). Checkpoint: "Is this the flow you mean, or the other one?"

## 3–4. Spine and Edges

Trace caller → callee with `file:line`, inputs and outputs, and each participant's responsibility. Label framework and library edges, and network, database, queue, and async boundaries. When the flow crosses services, name the contract and continue in the other codebase if present.

Cover only behavior that changes this flow: outcome-changing branches, async continuations and polling, and failure or retry paths. Leave out participants not on the flow.

## 5. Change Map and Playback

Give three to five changes:

| To change | Edit | Also update |
|---|---|---|
| Add a required field | the form or schema definition | types, messages, tests |

Then verify:

1. "Walk me through what happens when <trigger>, starting at the entry point."
2. "If you wanted to <change>, where would you start?"

Correct gaps by pointing at the missed hop or file. Then offer the map: "Want this saved as `docs/onboarding/<section>.md`?"

## Evidence Rules

- Open the file before describing it; never infer behavior from a name.
- Cite `file:line` for every behavioral claim.
- Label the unverified ("backend, not in this repo"; "inferred from client handling").
- Prefer a test or run to settle a doubt; say which you used.

## Example Frame

> "Onboarding" could mean `/signup` account creation or the `/onboarding` wizard. I'll trace the wizard; say if you meant the whole journey.
>
> In: `/onboarding` → email → financials → personal and user data → account application and legitimation. Out: `/signup`, goals, dashboard.
>
> Canonical flow: open `/onboarding` with a session → SSR fetches state → the client resumes at the first incomplete step.
>
> I'll assume you know React, not this repo or this area; correct me if that's wrong. Start at the entry and step engine, or jump to the personal-details step?

## Red Flags

- More than one chunk in a response: a dump.
- Boundary unstated: scope guessed.
- No checkpoint: a report, not a conversation.
- Playback skipped: understanding assumed.
- Assumed background contradicts what they told you: recalibrate.
- File inventory without execution order: not a trace.
- No change map: the person cannot act.

## Not For

Architecture review, debugging a failure, or implementing the change. Finish onboarding or hand off to the matching skill.
