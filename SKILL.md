# Issue-Driven Workflow — Skill Description

This skill set teaches AI coding agents how to manage structured development work using **issues**: numbered directories that hold planning notes, research, tasks, and a running state scratchpad.

## Prompts

| Prompt | File | What it does |
|--------|------|--------------|
| `issue [name-or-number]` | `prompts/issue.md` | Start or resume work on an issue |
| `new-issue` | `prompts/new-issue.md` | Interactively create a new issue |
| `list-issues` | `prompts/list-issues.md` | Show all issues as a kanban board |
| `retro` | `prompts/retro.md` | Run an open retrospective across all issues |

## How Agents Use This

When you invoke a prompt, your agent reads the corresponding markdown file and follows the instructions inside it. No plugin, no extension — just markdown instructions that tell the agent what to do.

A key behavior built into the prompts: **the agent proposes before it acts.** When you describe a task, the agent researches the code, writes a short proposed approach, and waits for your explicit confirmation before making any changes. The description is not the approval.

## Project Structure Required

```
project/
  information.md          ← cross-issue knowledge base (auto-created)
  example-issue/          ← optional reference template
  001-feat-something/
    planning/             ← design docs, approach notes
    research/             ← investigation findings, code analysis
    state.md              ← scratchpad: current focus + progress log
  002-fix-something/
    ...
```

## Installation

See the `install/` directory for agent-specific setup instructions:

- [install/claude-code.md](install/claude-code.md) — Claude Code (Anthropic)
- [install/opencode.md](install/opencode.md) — OpenCode (sst.dev)
- [install/zed.md](install/zed.md) — Zed editor

## Conventions

- Issue numbers are 3-digit zero-padded: `001`, `002`, ..., `999`
- Slug prefixes by type: `feat-`, `fix-`, `refactor-`, `investigation-`
- `state.md` has a human-readable H1 title (editable) and an `**Issue**` field (the permanent slug)
- Valid status values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`
- `state.md` is your "where did I leave off" file — Progress Log is append-only, never overwrite it
- `project/information.md` accumulates cross-issue decisions and patterns
