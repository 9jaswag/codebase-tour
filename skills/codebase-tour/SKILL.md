---
name: codebase-tour
description: Use when someone asks to understand an unfamiliar flow, feature, service, module, or subsystem in a codebase end-to-end, including how its functions, components, and services interact, or asks where a change to it would be made.
license: MIT
---

# Codebase Tour

## Overview

Understanding a codebase section means tracing a real execution path and, when useful, showing where a change would land. Use evidence rather than a file inventory. Adjust the depth and pace to what the person asks for.

**Core principle:** follow the flow hop by hop, making the system easier to understand without turning the explanation into a required lesson or quiz.

## Guided Tour

For an open-ended tour, use one stage per response and end at a useful checkpoint. Do not add the next stage to the same response. When they respond, follow their lead: answer a question, take a correction, or continue the tour. Combine or skip stages when they ask for a concise answer, want to jump to a particular part, or already know a layer. Clarify only when an ambiguity would change which flow you trace. Otherwise state a reasonable assumption and proceed.

| # | Stage | What to cover | Checkpoint if needed |
|---|-------|--------------|---------------------|
| 1 | Frame | Boundary, likely flow, assumed level | Clarify only if ambiguity changes the flow |
| 2 | Entry | Entry point and first hops | Confirm the flow if it remains ambiguous |
| 3 | Spine | The 3–6 main hops on the core path | Ask what is unclear before tracing further |
| 4 | Edges | Branches, async work, and failures that affect the path | Ask what needs more detail |
| 5 | Change map | The location of a requested change, or a useful map when relevant | Recap the flow and invite a question |

## 1. Frame

Name the section in the person's words. State what is inside and outside when that boundary helps. If the term could mean different flows, name the likely interpretations and ask which one matters. Pick a representative flow, often the user-triggered path that creates, reads, or mutates the core object, and state its trigger. Briefly state assumptions about familiarity when they affect the explanation. Ask only questions whose answers would change what you trace; otherwise proceed and invite correction.

## 2. Entry

Find real entry points: routes and pages, server handlers, CLI commands, queue consumers, or UI actions. Show the first hop or two (trigger → entry → bootstrap → state source) with `file:line`, naming the mechanism (SSR, client fetch, queue, cron). If the intended flow remains uncertain, ask whether this is the right one before tracing further.

## 3–4. Spine and Edges

Trace caller → callee with `file:line`, inputs and outputs, and each participant's responsibility. Cover 3–6 main hops in a response; split a longer flow at a natural boundary and continue in another response. Label framework and library edges, and network, database, queue, and async boundaries. If the flow crosses services, name the contract and continue into another codebase when it is available and within the request's scope; otherwise mark where evidence ends.

Cover only behavior that changes this flow: outcome-changing branches, async continuations and polling, and failure or retry paths. Leave out participants not on the flow.

## 5. Change Map and Recap

When the person asks where to make a change, show where that change belongs and any closely related files that need updates. For explanation-only requests, include a change map only when it adds useful context. Ground each suggestion in the inspected code:

| To change | Edit | Also update |
|---|---|---|
| Add a required field | the form or schema definition | types, messages, tests |

End the tour with a short recap of the trigger, main hops, and outcome. Once per tour, you may offer a low-pressure check: "Want to explain it back? I can fill in any gaps." If they decline or move on, finish without testing their recall.

Create or save documentation only when the person asks for it. Follow the repository's existing documentation conventions for the destination.

## Evidence Rules

- Open the file before describing it; never infer behavior from a name.
- Cite `file:line` for behavioral claims about the traced flow.
- Label the unverified ("backend, not in this repo"; "inferred from client handling").
- Use a targeted test, run, or other available evidence to settle a material doubt when it is useful, safe, and within scope; say what evidence you checked. Do not run unrelated or side-effecting commands just to complete a tour.

## Example Frame

> "Onboarding" could mean `/signup` account creation or the `/onboarding` wizard. I'll trace the wizard; say if you meant the whole journey.
>
> In: `/onboarding` → email → financials → personal and user data → account application and legitimation. Out: `/signup`, goals, dashboard.
>
> Canonical flow: open `/onboarding` with a session → SSR fetches state → the client resumes at the first incomplete step.
>
> I'll assume you know React, not this repo or this area; correct me if that's wrong. I can trace the full wizard from its entry, or focus on the personal-details step if that's what you need.

## Red Flags

- Multiple stages in one response on an open-ended tour.
- A long dump when the person asked for a concise answer.
- Boundary unclear when it matters.
- No chance for the person to steer when uncertainty matters.
- Playback treated as a required quiz or repeated after the person declines.
- Assumed background contradicts what they told you: recalibrate.
- File inventory without execution order: not a trace.
- A change map omitted after the person asks where to make a change.
- A change map included when it does not help answer an explanation-only request.

## Not For

Architecture review, debugging a failure, or implementing the change. Finish the tour or hand off to the matching skill.
