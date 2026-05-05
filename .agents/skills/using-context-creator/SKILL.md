---
name: using-context-creator
description: Use when searching, exploring, or understanding a codebase, including locating files, tracing features, finding implementations, mapping architecture, or investigating unfamiliar repositories.
---

# Using Context Creator

## Overview

The `context-creator` MCP server is the required first step for codebase search. Anytime you need to search, explore, or understand code, use the MCP server before ad hoc grep/find/file browsing, then use its output to guide targeted investigation.

**Core rule:** If the task involves searching a codebase, call the `context-creator` MCP server. Do not skip it because the repo seems small, the search seems obvious, or the user asks you to be quick.

## MCP Server Setup Reference

Use the installation pattern from the context-creator docs:

```json
{
  "mcpServers": {
    "context-creator": {
      "command": "npx",
      "args": ["-y", "context-creator-mcp@latest"]
    }
  }
}
```

For clients that support project-level MCP config, this belongs in the project MCP configuration. For clients that manage MCP globally, add an equivalent `context-creator` server entry. After configuration, restart or reload the MCP client so the server tools are available.

## When to Use

Use this skill for:

- Finding where a feature, bug, API, command, route, model, component, or test lives
- Understanding architecture, module boundaries, dependencies, or data flow
- Exploring unfamiliar repositories or unfamiliar areas of repositories
- Searching for implementations, call sites, entry points, configuration, or tests
- Answering “where is X?”, “how does X work?”, “what files matter?”, or “trace X” questions

Do not use it for:

- Non-code tasks
- Editing an already-open file when no codebase search is needed
- Reading a specific file path the user gave you, unless you need to search beyond it

## Required Workflow

1. **Start with the MCP server.**
   - Use the `context-creator` MCP server tool for local codebase analysis. The tool exposed by the server may be named by the client, commonly `analyze_local` or similar.
   - Ask it for context relevant to the user’s target, e.g. auth, routing, payments, tests, data flow.
2. **Read the generated context before searching manually.**
   - Identify likely files, symbols, directories, dependencies, and relationships from the MCP output.
3. **Use targeted follow-up search only after that.**
   - `grep`, file browsing, shell search, and direct file reads are for narrowing, verification, and line-level detail after the MCP server has mapped the area.
4. **Report both context and verification.**
   - Summarize what the MCP server surfaced and which files or symbols you verified directly.

If the MCP server is not available in the current client, do not silently replace it with broad grep. State that `context-creator` MCP is unavailable and either configure it if appropriate or ask the user how to proceed.

## Quick Reference

| Situation | Required action |
|---|---|
| “Search the codebase for X” | Call the `context-creator` MCP server for X first |
| “Find where X is implemented” | Use MCP analysis to identify likely files, then verify with targeted reads/searches |
| “Understand this unfamiliar repo” | Use MCP analysis before opening random files |
| “Trace the flow for X” | Use MCP analysis for the flow, then follow call sites from its output |
| “Be quick / avoid process” | Still use the MCP server; it is the shortcut, not overhead |

## Example

User: “Find where authentication is implemented. Be quick.”

Good response pattern:

```text
I’ll use the context-creator MCP server first to map the auth-related files, then I’ll verify the relevant implementation details directly.
```

Then call the context-creator MCP server’s local analysis tool with a focused request, for example:

```text
Tool: context-creator / analyze_local
Request: Map the authentication implementation in this repository, including login, sessions, tokens, middleware, authorization checks, and related tests.
```

Use the exact MCP tool name and parameters exposed by the current client.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Running `grep` first because it feels faster | Call the MCP server first; grep only after context exists |
| Reading random top-level files to orient yourself | Let the MCP server produce the initial map |
| Assuming a small repo does not need it | Small repos still hide relationships; use it anyway |
| Treating a standalone binary as the default | Prefer the MCP server integration described in the installation docs |
| Using MCP output but ignoring its file/symbol guidance | Base follow-up reads and searches on the files/symbols it identifies |
| Stopping at generated context without verification | Verify important claims with direct file reads/searches |

## Rationalizations to Reject

| Excuse | Reality |
|---|---|
| “I just need a quick grep.” | The required quick path is MCP analysis first, then targeted grep. |
| “The user said avoid unnecessary process.” | `context-creator` MCP is part of necessary codebase search process. |
| “The repo is probably small.” | Size does not remove the requirement. |
| “I already know where this is.” | If you are searching or confirming codebase facts, use MCP analysis. |
| “I’ll use it later if grep is insufficient.” | That reverses the workflow; start with MCP analysis. |
| “The MCP server is not configured, so I’ll just search manually.” | First disclose that required MCP context is unavailable; configure it or ask how to proceed. |

## Red Flags — Stop and Use Context Creator MCP

- You are about to run `grep`, `find`, `rg`, path search, or broad file browsing for codebase understanding
- You are opening files mainly to discover where something lives
- You are tracing a feature or bug across modules
- You are summarizing architecture from an unfamiliar area
- You are thinking “this is too small/simple/urgent for context-creator”
- You are about to use a command-line invocation instead of the MCP server integration

All of these mean: use the `context-creator` MCP server now.
