---
description: Start or resume work on an issue
---

# Start or Resume Issue Work

You are starting or resuming work on an issue — a continent under construction on the Magrathea planet. Follow these steps:

## 1. Determine the Issue

- If **$ARGUMENTS** is provided (non-empty):
  - Check if it's a numeric reference (e.g., `1`, `01`, `001`, `6`) or a numbered slug (e.g., `001-research-csp-migration-modules`)
  - Match against existing issue directories in `magrathea/` matching the pattern `NNN-*` or `*-*`
  - If found, use the matched directory name as `{ISSUE}`
  - If not found as a number, treat as a slug and check for exact directory match
  - Use `{ISSUE}` as the resolved issue directory name
- Otherwise, ask the user: "Are we working on an **existing** issue or creating a **new** one?"
  - **If existing**: List all directories in `magrathea/` (excluding `core.md` and `retro/`) with their numbers and ask which one to work on.
  - **If new**:
    1. Ask for a **description** of the issue/work (what needs to be done, context, goals)
    2. Based on the description, **generate a concise title** and **slug** (e.g., `feat-login`, `fix-cart-bug`, `issue-42`)
    3. Confirm the generated title and slug with the user before proceeding
    4. Scan `magrathea/` for directories matching `^\d{3}-` to find the highest number N
    5. Assign the next number as (N+1), zero-padded to 3 digits
    6. Create the issue directory as `{NNN}-{slug}` (e.g., `007-feat-new-feature`)
    7. Use `{NNN}-{slug}` as `{ISSUE}`

## 2. Load Context

Read the following files in order — do not skip any that exist:

1. `magrathea/MEMORY.md` — project-specific entry point or conventions (if present)
2. `magrathea/core.md` — the molten core: cross-issue knowledge, patterns, decisions, lessons learned
3. `magrathea/{ISSUE}/state.md` — current focus, tasks, and progress log for this issue
4. `magrathea/{ISSUE}/planning/*.md` — any existing plans (note titles)

Then output a structured context summary before asking what to do:

```
Issue: {ISSUE}
Title: {H1 heading from state.md, or slug if not set}
Status: {status from state.md, or "New" if just created}
Last worked on: {date from state.md, or today if new}

Last focus: {current focus section from state.md, or "Not started"}
Last action: {most recent progress log entry, or "None"}

Pending tasks: {unchecked items from ## Tasks in state.md, or "None"}
```

This summary is mandatory — it confirms context was loaded and orients both you and the user.

**Move the card.** If the current status is `Todo`, update it to `In Progress` immediately — opening an issue to work on it is the signal that work has begun and the kanban board should reflect reality. Then sync `board.md` (see §Board Sync below).

## §Board Sync

Whenever you change an issue's status (or create a new one), keep `magrathea/board.md` in sync. It is the fast index that `/list-issues` reads.

**Before any update:** if `magrathea/board.md` does not exist, stop and tell the user:

```
magrathea/board.md is missing — the kanban index hasn't been built yet.
Run /rebuild-board first, then re-run this command.
```

Do not attempt to update the issue's status or create directories until the board exists. The board and `state.md` must move together.

**To update `board.md`:**
1. Locate the row with the issue's slug in whichever status group currently contains it (the row is `| NNN | Title | Started | NNN-slug |`).
2. Remove that row from its current group.
3. Insert the row into the new status group, keeping the group's sort order (ascending by number for Todo / In Progress / In Review; descending for Complete / Closed).
4. If the source group is now empty, replace its table with `(none)`.
5. If the destination group was `(none)`, add the table header before inserting.

For a new issue, append the row to the `## Todo` group's table (or replace `(none)` with a fresh table).

Keep title and date in sync with `state.md` if either changes.

## 3. Bootstrap Directory Structure (if new)

If `magrathea/{ISSUE}/` does not exist, create the following structure:

```
magrathea/{ISSUE}/
  planning/          (create with .gitkeep)
  research/          (create with .gitkeep)
  state.md           (create with initial content)
```

Valid status values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`

Create `magrathea/{ISSUE}/state.md` with (where {ISSUE} is the full numbered issue directory name and {TITLE} is the confirmed human-readable title):

```markdown
# {TITLE}

**Issue**: {ISSUE}
**Started**: {today's date}
**Status**: Todo

## Current Focus

(Nothing yet)

## Tasks

(No tasks yet)

## Progress Log

- Session started
```

For example, if creating issue `007-feat-new-feature` with title "Add New Feature", the header would be `# Add New Feature` and the Issue field `**Issue**: 007-feat-new-feature`.

After creating the directory, append the new issue's row to the `## Todo` group in `board.md` (§Board Sync).

(If `magrathea/core.md` does not exist, create it as an empty file—the molten core will be populated as issues complete.)

## 4. Confirm to User

The structured context summary from Step 2 serves as the confirmation. Follow it with:

```
What would you like to do?
```

## 5. When the User Describes a Task

**Do not start implementing.** Don't panic — but don't act either. When the user tells you what they want done:

1. Research the relevant code (read files, explore the codebase)
2. Write a structured proposed approach listing **every file you plan to touch and why**
3. Ask: "Does this approach work for you?"
4. Wait for explicit confirmation ("yes", "go ahead") before touching any files

The user describing a task is **not** approval to implement it. Neither is enthusiasm, nor the task being small or obvious. No exceptions.

Once confirmed:
- Save the confirmed plan to `magrathea/{ISSUE}/planning/plan.md` before starting work
- Update `**Status**` to `In Progress` (if not already), and sync `board.md` (§Board Sync)
- Replace `## Tasks` in `state.md` with a checkbox list derived from the plan

## 6. Guide During Work

As you work on this issue:

- **Planning**: Save plans and design docs to `magrathea/{ISSUE}/planning/*.md` — the confirmed plan always goes in `planning/plan.md`
- **Research**: Save investigation results and code analysis to `magrathea/{ISSUE}/research/*.md`
- **Tasks**: Maintain the `## Tasks` checkbox list in `state.md` — check items off as they are completed
- **State**: Keep `magrathea/{ISSUE}/state.md` updated with:
  - Current focus (what you're working on now)
  - Progress log (summary of work completed, blockers, decisions made)
  - If the scope has shifted significantly, update the H1 title to reflect what the issue actually became — the slug never changes, but the title should stay accurate

**Important**: Never erase content from `state.md` when completing an issue. The Progress Log is permanent geology — append to it, never overwrite it. If a plan was written into Current Focus, move a summary of it to the Progress Log before updating Current Focus to reflect the completed state.

## 7. When Issue is Complete

Move the card across the board: update `**Status**` to `Complete` (or `In Review` if it needs user testing first), and sync `board.md` (§Board Sync) so `/list-issues` reflects reality.

Remove any empty directories (`planning/`, `research/`) that contain only a `.gitkeep` file — they add no value if unused.

Before closing the issue, feed the molten core. Update `magrathea/core.md` with:

- **Key decisions** made in this issue
- **Patterns discovered** or conventions established
- **Architectural notes** relevant to other issues
- **Lessons learned** that affect future work
- Links to relevant issues or code locations

The core is what every future continent will be built on — be generous with what you record.

---

**Tip**: Invoke this prompt again anytime to pick up where you left off — `state.md` will be automatically loaded.
