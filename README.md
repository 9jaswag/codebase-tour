# Onboarding to a Codebase

A skill that walks you through an unfamiliar part of a codebase — one real execution path, hop by hop, with evidence — instead of handing you a file dump.

```bash
npx skills add 9jaswag/onboarding-to-a-codebase
```

Then ask your agent something like:

> I'm new to this repo. Help me understand how billing works end-to-end, and where I'd change a payment method.

## What it does

The agent traces one path through the section you name, citing `file:line`, in five chunks:

| # | Chunk | What you get |
|---|-------|--------------|
| 1 | Frame | What's in and out, the canonical flow, and a calibration question |
| 2 | Entry | The real entry points and the first hops |
| 3 | Spine | The core path, hop by hop, with inputs and outputs |
| 4 | Edges | Branches, async work, and failures that change behavior |
| 5 | Change map + playback | Where changes go, then you retell the flow and place a change |

It stops after each chunk for questions, so you build the model instead of receiving a wall of text. The session ends only when you can retell the flow and say where a change would land.

## Why it's different

- **Trace, not inventory.** It follows one path that actually runs, not every file near the feature.
- **Evidence.** Behavioral claims cite `file:line`; anything inferred rather than verified is labelled.
- **Playback.** The agent checks your understanding and corrects the gaps before calling it done.
- **Durable map.** It can save the result as `docs/onboarding/<section>.md`.

## When it triggers

Ask to understand an unfamiliar flow, feature, service, module, or subsystem end-to-end, or ask where a change to it would be made. It is stack-agnostic — tested on Next.js and Rails.

## Install

```bash
# Project
npx skills add 9jaswag/onboarding-to-a-codebase

# Global
npx skills add 9jaswag/onboarding-to-a-codebase -g

# One agent
npx skills add 9jaswag/onboarding-to-a-codebase -a claude-code
```

Requires an agent with skills support — OpenCode, Claude Code, Codex, Cursor, and others — and Node.js 22.20 or later for the CLI.

Like any skill, it only runs if your agent loads it. If your agent supports explicit invocation, name it: `onboarding-to-a-codebase`.

## License

MIT © Chuks Opia
