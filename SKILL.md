# Issue-Driven Workflow — Skill Description

This skill set teaches Claude Code how to manage structured development work using **issues**: numbered directories that hold planning notes, research, tasks, and a running state scratchpad.

## Commands

| Command | File | What it does |
|---------|------|--------------|
| `/issue [name-or-number]` | `commands/issue.md` | Start or resume work on an issue |
| `/new-issue` | `commands/new-issue.md` | Interactively create a new issue |
| `/list-issues` | `commands/list-issues.md` | Show all issues with status |

## How Claude Uses This

When you invoke a command, Claude Code reads the corresponding markdown file from `.claude/commands/` and follows the instructions inside it. No plugin, no extension — just markdown instructions that tell Claude what to do.

A key behavior built into the commands: **Claude proposes before it acts.** When you describe a task, Claude researches the code, writes a short proposed approach, and waits for your explicit confirmation before making any changes. The description is not the approval.

## Project Structure Required

```
project/
  information.md          ← cross-issue knowledge base (auto-created)
  example-issue/          ← optional reference template
  001-feat-something/
    planning/             ← design docs, approach notes
    research/             ← investigation findings, code analysis
    tasks/                ← task lists, progress tracking
    state.md              ← scratchpad: current focus + progress log
  002-fix-something/
    ...
.claude/
  commands/
    issue.md
    list-issues.md
    new-issue.md
```

## Installation

1. Copy `commands/*.md` → `.claude/commands/` in your project
2. Create `project/` directory at your project root
3. Optionally copy `example/state.md` → `project/example-issue/state.md`
4. Start Claude Code and run `/issue` to begin

## Conventions

- Issue numbers are 3-digit zero-padded: `001`, `002`, ..., `999`
- Slug prefixes by type: `feat-`, `fix-`, `refactor-`, `investigation-`
- `state.md` has a human-readable H1 title (editable) and an `**Issue**` field (the permanent slug)
- Valid status values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`
- `state.md` is your "where did I leave off" file — Progress Log is append-only, never overwrite it
- `project/information.md` accumulates cross-issue decisions and patterns
