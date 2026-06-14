---
description: Orient yourself — what this project is, where the planet is at, and how Magrathea works
---

# Welcome to Magrathea

> *"Hello," said the Vogon, "you have to understand the work before you can argue with it."* — Roughly speaking.

You are giving the user a tour of the planet. They typed `/magrathea` — they want to know what this project is, what state the work is in, and (if they're new) how the workflow operates. Lead with state, then a short workflow recap, and end with a clear "what next."

Do not ask the user a question before producing the tour. The whole point is that they didn't have to ask.

## 1. Read the Planet

Read these silently — do not narrate each read:

1. **`README.md`** at the project root (if present) — the human description of what this project is and why it exists. Pull the opening tagline / one-paragraph elevator pitch.
2. **`magrathea/core.md`** — the molten core. Decisions, patterns, lessons learned. The "what we already know" of the project.
3. **`magrathea/board.md`** — the kanban index. Used for the state snapshot.
4. **Most recent retro** in `magrathea/retro/` — sorted by filename descending, take the first one. Look at `## Conclusions` and `## Open Threads`.

If `magrathea/` doesn't exist yet, skip to §First-Run below. The tour is different when the planet hasn't been built.

If `magrathea/board.md` is missing but `magrathea/` exists, tell the user:

```
The planet exists, but magrathea/board.md is missing — the kanban index hasn't been built yet.
Run /board-redraw first, then re-run /magrathea.
```

Stop there.

## 2. Produce the Tour

Output exactly this structure. Keep each section tight — this is a briefing, not a novel. Marvin would already be bored.

```
# {Project name from README H1, or the directory name as a fallback}

{One-paragraph elevator pitch from the README — the tagline or opening lines. If README is absent or silent, say "No README at the project root — describe what this project does in core.md or the README so future tours know."}

## State of the Planet

- **In Progress**: {N} — {list slugs, or "(none)"}
- **In Review**: {N} — {list slugs, or "(none)"}
- **Todo**: {N}
- **Complete**: {N} (closed: {N})

{If there is a card In Progress, name it and its current focus from state.md. One line — this is the "you were here" pointer.}

## What the Planet Remembers

{2–4 bullets synthesized from core.md — the key decisions, patterns, or constraints already established. Cite section names: "[core.md > Auth Decisions]". If core.md is empty, say "core.md is still empty — the molten core will fill as issues complete."}

## Latest Retro

{One or two lines from the most recent retro's Conclusions and any Open Threads still carried forward. Include the retro filename. If no retros exist, say "No retros yet — run /retro after a batch of issues to step back and look at the planet from orbit."}
```

After the tour, add a short workflow recap (heading: `## How Magrathea Works`). Keep it to about 8 lines — this is for someone who's new or rusty, not a manual:

> Magrathea is a kanban-driven workflow. The board lives in `magrathea/board.md` and moves cards across these lanes:
>
> **Todo → In Progress → In Review → Complete → Closed**
>
> Each issue is a continent on the planet: a numbered directory with planning notes, research, and a `state.md` scratchpad. The molten core (`magrathea/core.md`) accumulates shared knowledge across every issue — that's what `/ask` queries, and what the next continent is built on top of.
>
> The agent **proposes before it acts**: when you describe work, it researches the code, lists every file it plans to touch, and waits for your explicit "yes." Don't panic — but don't act either.

Then `## What Next`. Make this responsive to the state you just read — don't list every command, suggest the one or two that actually fit:

- If there is a card In Progress: `/pickup {NNN}` to resume it.
- If In Review is non-empty: mention they may want to verify or close it.
- If only Todo cards exist: `/pickup {NNN}` to start one, or `/pickup` to pick interactively.
- If the planet is empty: `/capture` to file the first issue.
- Mention `/board` for the full kanban view and `/ask` if they want to query the planet's memory.

End with one Hitchhiker's-flavored sign-off line. Vary it — pick whichever fits the state of the planet. A few you can draw from (rotate, don't reuse the same one every time):

- "The Guide says: *Don't panic.* Then it suggests `/pickup`."
- "The planet is mostly harmless. Carry on."
- "42 things are technically still possible. Pick one."
- "Time is an illusion. Lunchtime doubly so. Issue {NNN} is right there, though."
- "So long, and thanks for all the context."

Pick one. Don't list them all.

## §First-Run

If `magrathea/` does not exist, the planet hasn't been built yet. Skip the tour and output:

```
# {Project name from README H1, or directory name}

{Elevator pitch from README, if any.}

## No planet yet

This project doesn't have a `magrathea/` directory — the workflow hasn't been initialized here. Magrathea is a kanban-driven workflow for AI coding agents: every issue is a numbered directory with planning notes, research, and a "where I left off" file, and a shared `core.md` accumulates project knowledge across all of them.

To build the planet, copy the four starter templates that ship with the Magrathea source repo:

       mkdir magrathea
       cp <path-to-magrathea-repo>/templates/readme.md  magrathea/readme.md
       cp <path-to-magrathea-repo>/templates/board.md   magrathea/board.md
       cp <path-to-magrathea-repo>/templates/core.md    magrathea/core.md
       cp <path-to-magrathea-repo>/templates/state.md   magrathea/example-state.md

   - `readme.md` — playful tour of the directory for humans browsing.
   - `board.md` — empty kanban index that `/board` reads.
   - `core.md` — molten core, pre-stubbed with the section headings the prompts and `/ask` know how to navigate.
   - `example-state.md` — populated reference showing the canonical shape of a `state.md`. Lives in the planet so nobody has to look outside `magrathea/` to find it; `/board-redraw` ignores it (regex matches `^\d{3}-`).

   (If you don't have the Magrathea source repo handy, I can write all four directly from the canonical templates — just say the word.)

Then run `/capture` to file your first issue, or `/pickup` to start working.

The Guide says: *Don't panic.* It also suggests bringing a towel.
```

Then stop.

## Notes

- `/magrathea` is read-only. It never writes files, moves cards, or modifies `core.md` / `board.md` / `state.md`.
- Keep the whole tour to roughly a screen of output. A user who wanted more would have asked `/ask` or `/board`.
- The references are flavor, not the meal. If the state is busy, lean into the state and leave the jokes shorter.
- The point of this command is that running it costs the user nothing. They should feel oriented in seconds, not lectured.
