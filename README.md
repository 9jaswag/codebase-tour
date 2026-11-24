# Codebase Tour

You're new to a repo, or you've just been dropped into a service you've never touched. You ask your agent how it works and get a folder-by-folder summary that tells you what exists, not what happens.

Codebase Tour follows one real path through the part you care about: from the button, request, or job that starts it to the data it writes or the response it returns. It names each function and service on the way, cites the file and line, and can pause at useful checkpoints so you can steer the explanation.

By the end you can explain the flow to a teammate and, when useful, point at where a change would land. It's not an architecture review, a debugger, or a documentation generator.

## Install

You need an agent that supports skills. The install command needs Node.js 22.20 or later.

Install it for the current project:

```bash
npx skills add 9jaswag/codebase-tour
```

For every project on your machine:

```bash
npx skills add 9jaswag/codebase-tour -g
```

For one agent only:

```bash
npx skills add 9jaswag/codebase-tour -a claude-code
```

Check that it's there with `npx skills list`, then start a new session in the repo you want to understand.

## Use it

Ask your agent something like:

> I'm new to this repo. Help me understand how billing works end-to-end, and where I'd change a payment method.

The skill only runs if your agent loads it. If your agent supports explicit invocation, name it: `codebase-tour`.

## How it works

For open-ended questions, it runs a short guided session; if you ask for a concise answer, it gives one.

1. **Scope.** It identifies the relevant section and path, clarifying ambiguity only when it changes what to trace.
2. **Entry.** It finds where that path starts: a route, handler, command, queue consumer, or UI action.
3. **Path.** It follows the call chain, naming what each function or service does and citing the file and line.
4. **Branches.** It covers the conditions that change the outcome: permissions, flags, async work, retries, failures.
5. **Change map.** When you ask where a change goes, it maps that location. For broader explanations, it may include likely change points when useful.

For an open-ended tour, it covers one stage per response and pauses at useful checkpoints. It can combine stages or skip ahead when you ask for a concise answer or already know a layer. Each tour ends with a short recap; an optional check-back is offered once. Documentation is created only when you ask for it, using the repository's existing conventions.

## When it fits

Use it when you're new to a repo, or new to a part of one, and need a working mental model before you change something. That could be a feature you have to modify, a service you're taking over, or a flow you need to explain to someone else.

It's not a code review, a debugging session, or an implementation tool. Use it to understand the ground before one of those starts.

It's been tested on codebases written in different programming languages.

## Updating

Skills don't update themselves. Run `npx skills check` to see if there's a newer version, then `npx skills update` to install it. New installs always get the current version.

## License

MIT © Chuks Opia
