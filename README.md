# Issue-Driven Workflow for Claude Code

> Work with Claude like a senior dev who never forgets what you talked about yesterday.

---

## The Problem It Solves

Every Claude Code conversation starts fresh. You explain the context, Claude helps, you close the tab. Next day: explain it all again.

This workflow fixes that by giving Claude a **filing cabinet** — a `project/` directory where every piece of work lives in its own folder with notes, plans, and a "where I left off" file. Open a new conversation, type `/issue 5`, and Claude reads the state, loads the context, and picks up exactly where you stopped.

---

## The Three Commands

Think of these like sticky notes on your desk, except Claude can read them.

### `/issue`

> "Hey Claude, let's work on something."

- **With a number**: `/issue 5` or `/issue 005` — resumes that issue, loads its state
- **With a name**: `/issue feat-login` — finds it by slug and loads it
- **Alone**: `/issue` — Claude asks if you're resuming something or starting fresh

### `/new-issue`

> "I have a new thing I want to track."

Claude asks what you want to build/fix/investigate, generates a title and slug for you to confirm, then creates the folder structure automatically. You start with a clean slate.

### `/list-issues`

> "What are we working on again?"

Shows a table of all issues, their status (In Progress / Completed), and when they were started. Quick orientation at the start of a session.

---

## What Lives Where

```
your-project/
│
├── project/                        ← the filing cabinet
│   ├── information.md              ← shared knowledge across all issues
│   │                                  (patterns, decisions, lessons learned)
│   │
│   └── 001-feat-user-login/        ← one folder per issue
│       ├── planning/               ← design docs, approach notes
│       ├── research/               ← what you investigated and found
│       ├── tasks/                  ← checklists and progress tracking
│       └── state.md                ← "where I left off" scratchpad
│
└── .claude/
    └── commands/                   ← Claude reads these when you type /issue etc.
        ├── issue.md
        ├── list-issues.md
        └── new-issue.md
```

---

## A Day in the Life

**Monday morning — you want to add a dark mode toggle:**

```
You:    /new-issue
Claude: What does this issue need to accomplish?
You:    Add a dark mode toggle to the header
Claude: Title: "Dark Mode Toggle" — slug: feat-dark-mode-toggle. Good?
You:    yes
Claude: ✓ Created issue: 007-feat-dark-mode-toggle. What would you like to do first?
You:    Let's plan it out
Claude: [writes a plan to project/007-feat-dark-mode-toggle/planning/approach.md]
```

**You get pulled into meetings. Come back Tuesday:**

```
You:    /issue 7
Claude: Resuming 007-feat-dark-mode-toggle.
        Last focus: planning the toggle implementation.
        Progress: approach.md written, CSS variables identified.
        What would you like to do?
You:    Let's write the code
```

No re-explaining. No "so last time we were...". Just work.

**When the issue is done:**

Claude updates `project/information.md` with anything useful for future issues — patterns discovered, decisions made, gotchas to avoid. That knowledge stays available forever.

---

## Installation

1. **Copy the commands** into your project's `.claude/commands/` folder:
   ```
   your-project/.claude/commands/issue.md
   your-project/.claude/commands/list-issues.md
   your-project/.claude/commands/new-issue.md
   ```

2. **Create the project folder** at your project root:
   ```bash
   mkdir project
   ```

3. **That's it.** Start Claude Code and run `/issue`.

> Claude Code automatically discovers commands in `.claude/commands/` — no configuration needed.

---

## Tips

- **Update `state.md` often.** At the end of a session, ask Claude: *"Update the state with what we did today."* Your future self will thank you.
- **`information.md` is gold.** When closing an issue, ask Claude: *"What should we add to information.md from this work?"*
- **Issue numbers are for humans.** `/issue 3` is easier to type than `/issue 003-feat-dark-mode-toggle`. Both work.
- **Keep issues focused.** One clear goal per issue. If scope creeps, create a new issue and link them in `state.md`.
- **You control the workflow.** These are markdown files — read them, edit them, adapt them to your team's needs.

---

## Adapting for Your Project

The commands are fully generic. The only project-specific thing is your `CLAUDE.md` — that's where you describe your tech stack, environment, and development conventions to Claude. See [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code) for how to write one.

The `project/information.md` file is where accumulated project knowledge lives. Start it empty; it grows as you work.

---

*Built with love during the Hyva Developers Paradise workshop. Shared because good workflows should travel.*
