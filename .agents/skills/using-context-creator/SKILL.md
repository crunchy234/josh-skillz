---
name: using-context-creator
description: Use when searching, exploring, or understanding a codebase, including locating files, tracing features, finding implementations, mapping architecture, or investigating unfamiliar repositories.
---

# Using Context Creator

## Overview

`context-creator` is the required first step for codebase search. Anytime you need to search, explore, or understand code, use `context-creator` before ad hoc grep/find/file browsing, then use its output to guide targeted investigation.

**Core rule:** If the task involves searching a codebase, run `context-creator`. Do not skip it because the repo seems small, the search seems obvious, or the user asks you to be quick.

## When to Use

Use this skill for:

- Finding where a feature, bug, API, command, route, model, component, or test lives
- Understanding architecture, module boundaries, dependencies, or data flow
- Exploring unfamiliar repositories or parts of repositories
- Searching for implementations, call sites, entry points, configuration, or tests
- Answering “where is X?”, “how does X work?”, “what files matter?”, or “trace X” questions

Do not use it for:

- Non-code tasks
- Editing an already-open file when no codebase search is needed
- Reading a specific file path the user gave you, unless you need to search beyond it

## Required Workflow

1. **Start with `context-creator`.**
   - If command syntax is unknown, run `context-creator --help` first.
   - Ask it for context relevant to the user’s target, e.g. auth, routing, payments, tests, data flow.
2. **Read the generated context before searching manually.**
   - Identify likely files, symbols, directories, and relationships from its output.
3. **Use targeted follow-up search only after that.**
   - `grep`, file browsing, and shell search are for narrowing, verification, and line-level detail after `context-creator` has mapped the area.
4. **Report both context and verification.**
   - Summarize what `context-creator` surfaced and which files or symbols you verified directly.

## Quick Reference

| Situation | Required action |
|---|---|
| “Search the codebase for X” | Run `context-creator` for X first |
| “Find where X is implemented” | Use `context-creator` to identify likely files, then verify with targeted reads/searches |
| “Understand this unfamiliar repo” | Use `context-creator` before opening random files |
| “Trace the flow for X” | Use `context-creator` for the flow, then follow call sites from its output |
| “Be quick / avoid process” | Still use `context-creator`; it is the shortcut, not overhead |

## Example

User: “Find where authentication is implemented. Be quick.”

Good response pattern:

```text
I’ll use context-creator first to map the auth-related files, then I’ll verify the relevant implementation details directly.
```

Then run the appropriate command for the installed CLI, for example:

```bash
context-creator --help
context-creator "authentication implementation auth login session token middleware tests"
```

If the local CLI uses different syntax, follow `context-creator --help` and use the equivalent query/options.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Running `grep` first because it feels faster | Run `context-creator` first; grep only after context exists |
| Reading random top-level files to orient yourself | Let `context-creator` produce the initial map |
| Assuming a small repo does not need it | Small repos still hide relationships; use it anyway |
| Using `context-creator` but ignoring its output | Base follow-up reads and searches on the files/symbols it identifies |
| Stopping at generated context without verification | Verify important claims with direct file reads/searches |

## Rationalizations to Reject

| Excuse | Reality |
|---|---|
| “I just need a quick grep.” | The required quick path is `context-creator` first, then targeted grep. |
| “The user said avoid unnecessary process.” | `context-creator` is part of necessary codebase search process. |
| “The repo is probably small.” | Size does not remove the requirement. |
| “I already know where this is.” | If you are searching or confirming codebase facts, use `context-creator`. |
| “I’ll use it later if grep is insufficient.” | That reverses the workflow; start with `context-creator`. |

## Red Flags — Stop and Use Context Creator

- You are about to run `grep`, `find`, `rg`, path search, or broad file browsing for codebase understanding
- You are opening files mainly to discover where something lives
- You are tracing a feature or bug across modules
- You are summarizing architecture from an unfamiliar area
- You are thinking “this is too small/simple/urgent for context-creator”

All of these mean: use `context-creator` now.
