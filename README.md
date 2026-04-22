# Issue-Driven Workflow for AI Coding Agents

> Work with your AI coding agent like a senior dev who never forgets what you talked about yesterday.

---

## The Problem It Solves

Every AI coding agent conversation starts fresh. You explain the context, the agent helps, you close the tab. Next day: explain it all again.

This workflow fixes that by giving your agent a **filing cabinet** — a `project/` directory where every piece of work lives in its own folder with notes, plans, and a "where I left off" file. Open a new conversation, say "start issue 5", and the agent reads the state, loads the context, and picks up exactly where you stopped.

---

## The Four Prompts

Think of these like sticky notes on your desk, except your agent can read them.

### `issue`

> "Hey, let's work on something."

- **With a number**: `issue 5` or `issue 005` — resumes that issue, loads its state
- **With a name**: `issue feat-login` — finds it by slug and loads it
- **Alone**: `issue` — the agent asks if you're resuming something or starting fresh

### `new-issue`

> "I have a new thing I want to track."

The agent asks whether you're planning multiple issues or starting work immediately, then generates a title and slug for you to confirm. In "start working" mode it flows directly into the issue workflow.

### `list-issues`

> "What are we working on again?"

Shows all issues as a kanban board grouped by status (Closed → Complete → In Review → Todo → In Progress), with the most actionable work closest to your prompt. Complete issues are capped at 3 most recent to keep the view focused.

### `retro`

> "Let's step back and reflect."

Reads all issue data and previous retros, surfaces patterns and open threads, then facilitates an open conversation — no guided questions, you take it where it needs to go. Action items automatically become tracked issues. Produces a shareable summary for your team or community.

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
│       └── state.md                ← "where I left off" scratchpad
│
└── prompts/                        ← the four prompt files (this repo)
    ├── issue.md
    ├── list-issues.md
    ├── new-issue.md
    └── retro.md
```

Where you install the prompts depends on your agent — see [Installation](#installation) below.

---

## A Day in the Life

**Monday morning — you want to add a dark mode toggle:**

```
You:    /new-issue
Agent:  What does this issue need to accomplish?
You:    Add a dark mode toggle to the header
Agent:  Title: "Dark Mode Toggle" — slug: feat-dark-mode-toggle. Good?
You:    yes
Agent:  ✓ Created issue: 007-feat-dark-mode-toggle. What would you like to do first?
You:    Let's plan it out
Agent:  [writes a plan to project/007-feat-dark-mode-toggle/planning/approach.md]
```

**You get pulled into meetings. Come back Tuesday:**

```
You:    /issue 7
Agent:  Resuming 007-feat-dark-mode-toggle.
        Last focus: planning the toggle implementation.
        Progress: approach.md written, CSS variables identified.
        What would you like to do?
You:    Let's write the code
```

No re-explaining. No "so last time we were...". Just work.

**When the issue is done:**

The agent updates `project/information.md` with anything useful for future issues — patterns discovered, decisions made, gotchas to avoid. That knowledge stays available forever.

---

## Installation

### Supported agents

| Agent | Guide |
|-------|-------|
| Claude Code | [install/claude-code.md](install/claude-code.md) |
| OpenCode | [install/opencode.md](install/opencode.md) |
| Zed | [install/zed.md](install/zed.md) |

### Quick start (Claude Code or OpenCode)

1. **Copy the prompts** into your agent's commands folder:

   ```bash
   # Claude Code
   mkdir -p .claude/commands && cp prompts/*.md .claude/commands/

   # OpenCode
   mkdir -p .opencode/commands && cp prompts/*.md .opencode/commands/
   ```

2. **Create the project folder** at your project root:

   ```bash
   mkdir project
   ```

3. **That's it.** Start your agent and invoke `/issue`.

---

## Tips

- **The agent proposes before it acts.** When you describe a task, the agent researches the code, writes a structured plan listing every file it intends to touch, and waits for your explicit "yes" or "go ahead". Describing a task — or it being small and obvious — is not approval. No exceptions.
- **Titles are editable, slugs are not.** The H1 in `state.md` is the human-readable title and can evolve as the issue does. The `**Issue**` field is the permanent slug reference — never change it.
- **Status follows the kanban:** `Todo` → `In Progress` → `In Review` → `Complete` (or `Closed` if abandoned).
- **Progress Log is permanent.** It only ever gets appended to — the agent should never erase earlier entries when closing an issue.
- **Update `state.md` often.** At the end of a session, ask your agent: *"Update the state with what we did today."* Your future self will thank you.
- **`information.md` is gold.** When closing an issue, ask: *"What should we add to information.md from this work?"*
- **Issue numbers are for humans.** `issue 3` is easier to type than `issue 003-feat-dark-mode-toggle`. Both work.
- **Keep issues focused.** One clear goal per issue. If scope creeps, create a new issue and link them in `state.md`.
- **You control the workflow.** These are markdown files — read them, edit them, adapt them to your team's needs.

---

## Adapting for Your Project

The prompts are fully generic. The only project-specific thing is your agent's context file (e.g., `CLAUDE.md` for Claude Code) — that's where you describe your tech stack, environment, and development conventions. See your agent's documentation for how to set this up.

The `project/information.md` file is where accumulated project knowledge lives. Start it empty; it grows as you work.

---

*Built with love during the Hyva Developers Paradise workshop. Shared because good workflows should travel.*
