# The Planet

> *"The factory floor of Magrathea was vast beyond measure, a place where bespoke planets were built to order for the discerning hyperintelligent client."* — The Guide, paraphrased.

Welcome, weary traveller, to the `magrathea/` directory. If you've stumbled in here looking for source code, you've taken a wrong turn at Betelgeuse — this is where the project does its *thinking*, not its compiling.

## What is this place?

This directory is the **planet** for the [Magrathea](https://github.com/) workflow — a kanban-driven system that turns the project around it into something AI coding agents can actually remember between conversations. Every issue is a continent under construction. Every retro is a geological survey. The molten core (`core.md`) accumulates everything the project has learned about itself.

Without this directory, every AI agent conversation starts from zero. With it, the agent reads where you left off and gets back to work. The planet keeps spinning whether you're paying attention or not.

## What's in here?

```
magrathea/
├── readme.md            ← you are here (mostly harmless)
├── core.md              ← molten core: shared knowledge across every issue
│                          (patterns, decisions, lessons learned)
├── board.md             ← fast kanban index — what `/board` reads
│
├── 001-feat-something/  ← one continent per issue
│   ├── planning/        ← design docs and approach notes
│   ├── research/        ← what was investigated and what was found
│   └── state.md         ← "where I left off" scratchpad + Progress Log
│
├── 002-fix-something/
│   └── ...
│
└── retro/               ← retrospective notes (appears after the first /retro)
```

## How is this used?

Through prompts that the AI agent already knows about. The headline acts:

| Prompt | What it does |
|--------|--------------|
| `/magrathea` | One-screen orientation tour — *"so… where were we?"* solved |
| `/pickup` | Pick up a card and start (or resume) work |
| `/capture` | File a new issue before you forget it |
| `/board` | Show the kanban board |
| `/ask` | Query the planet's memory with citations |
| `/retro` | Step back, look at the planet from orbit |

Cards move across these lanes:

```
Todo  →  In Progress  →  In Review  →  Complete
                                          ↓
                                       Closed
```

Status moves are explicit acts — `/pickup` opens a card and moves it to *In Progress*, finishing moves it to *Complete*. The board is always the source of truth.

## Rules of the planet

- **Don't panic — but do confirm.** The agent will propose a plan and wait for your explicit "yes" before changing anything. Describing a task is not approval.
- **Progress Log is append-only.** It's the geology of the planet — a record, not a draft. Never overwrite earlier entries. Marvin would approve, if Marvin approved of anything.
- **`core.md` is gold.** When closing an issue, ask the agent what should be added to `core.md`. The molten core feeds every future continent — and powers `/ask`.
- **Slugs are forever, titles are not.** The `**Issue**:` field in `state.md` is the permanent slug. The H1 title can evolve. The Vogons don't tolerate revisionism on slugs.

## For humans reading directory listings

If you've found this directory and you're not an AI agent: this is project memory. Browse `core.md` for the project's accumulated decisions and patterns. Browse `board.md` for what's in flight. Pick any `NNN-slug/` directory to see the planning, research, and progress log for a specific piece of work.

Nothing in here is generated at build time. Nothing in here is required to run the project. It's all just markdown — sticky notes for your desk, except your agent can read them.

---

*So long, and thanks for all the context.*
