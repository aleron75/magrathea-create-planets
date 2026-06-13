---
description: One-off migration from the old project/ layout to Magrathea
---

# Migrate to Magrathea

> *"The first ten million years were the worst. And the second ten million: they were the worst too. The third ten million I didn't enjoy at all. After that I went into a bit of a decline."* — Marvin, on running the old `project/` layout for too long.

This is a **one-off command** for projects that were set up under the previous `project/` + `information.md` layout. The Magratheans have woken up, dusted off the planet-building equipment, and want to move your existing world onto the new naming. It renames the folder, renames the molten core, swaps out your installed command files for the new ones (commands have been renamed too — see the map below), and tells you how to finish the migration.

After it runs successfully, delete this file from your commands folder — you won't need it again. (Unlike the planet, it really is one-off.)

## Command Rename Map

The commands themselves have new names in this version. The old prompt files will be **removed** from your commands folder and the new ones installed in their place.

| Old command | New command | Why |
|---|---|---|
| `/issue` | `/pickup` | "Pick up a card from the board" reads better as the verb for resuming work. |
| `/new-issue` | `/capture` | Works for both deliberate filing *and* grabbing something mid-conversation before it's forgotten. |
| `/list-issues` | `/board` | Shorter; matches the file name (`magrathea/board.md`); kanban-native. |
| `/rebuild-board` | `/board-redraw` | Shorter; pairs visually with `/board`. |
| `/retro` | `/retro` | Unchanged. |

In addition, two brand-new commands are installed (no old equivalent):

- `/ask` — query the planet's memory (`core.md` + retros) with citations.
- `/magrathea` — orientation tour: reads README, `core.md`, the board, and the latest retro, then prints a one-screen briefing on the state of the planet.

The noun "issue" stays everywhere it already was — directories (`001-feat-foo`), the `**Issue**:` field in `state.md`, prose. Only the *commands* moved.

## What This Does

Four actions, in order:

1. Rename `project/` → `magrathea/`
2. Rename `magrathea/information.md` → `magrathea/core.md`
3. Replace your installed command files: delete the old ones (`issue.md`, `new-issue.md`, `list-issues.md`), install the new ones (`pickup.md`, `capture.md`, `board.md`, `board-redraw.md`, `ask.md`, `magrathea.md`), and refresh `retro.md` to the new layout
4. Tell you to run `/board-redraw` afterward to generate the new kanban index (`magrathea/board.md`) from your existing issues — this command doesn't do it automatically; it's safer to keep migrate focused on the rename and leave the index build as an explicit second step

## Workflow

You must **plan first, confirm, then act**. Do not start renaming anything until the user has explicitly approved.

### Step 1 — Detect current state

Run all checks before saying anything. Report findings as a single summary.

Check the following and record the result of each:

- Does `project/` exist?
- Does `magrathea/` already exist?
- Does `project/information.md` exist?
- Does `magrathea/core.md` already exist?
- Where are the installed prompts located? Check in this order and pick the first that exists:
  - `.claude/commands/`
  - `.opencode/commands/`
  - (if neither, report "no installed prompts detected — you may be using Zed or a manual setup")
- Which old-name command files are present in the detected commands folder? (Check for: `issue.md`, `new-issue.md`, `list-issues.md`, `rebuild-board.md`)
- Which new-name command files are already present? (Check for: `pickup.md`, `capture.md`, `board.md`, `board-redraw.md`, `ask.md`, `magrathea.md`)
- For each prompt file present in the commands folder, does it still reference `project/` or `information.md`? (grep for those literal strings)

### Step 2 — Fail loudly if already migrated

If **all** of the following are true, the migration has already been run:

- `magrathea/` exists
- `project/` does not exist
- `magrathea/core.md` exists (or `magrathea/information.md` does not exist)
- None of the old-name command files are present (`issue.md`, `new-issue.md`, `list-issues.md`, `rebuild-board.md`)
- The new-name command files are all present (`pickup.md`, `capture.md`, `board.md`, `board-redraw.md`, `ask.md`, `magrathea.md`)

In that case, output:

```
✗ Already migrated. magrathea/ is in place and your commands have been renamed.
  Nothing to do. You can safely delete this migrate command from your commands folder.
```

Stop here. Do not proceed.

If only **some** of those conditions hold, the project is in a partially-migrated state. Do not assume — report exactly what you found and ask the user how to proceed before touching anything.

### Step 3 — Report the plan

Output a structured report. Be specific. Example:

```
Migration plan for: {absolute path of working directory}

Detected:
  - project/ exists                       ✓
  - magrathea/ does not exist             ✓
  - project/information.md exists         ✓
  - Installed prompts: .claude/commands/  ✓
  - Old command files present:            issue.md, new-issue.md, list-issues.md, retro.md
  - New command files present:            (none)

Actions I will take:
  1. Rename:  project/  →  magrathea/                          (plain `mv`)
  2. Rename:  magrathea/information.md  →  magrathea/core.md   (plain `mv`)
  3. Replace command files in .claude/commands/:
       - Delete:  issue.md, new-issue.md, list-issues.md
                  (and rebuild-board.md if present — superseded by board-redraw.md)
       - Install: pickup.md, capture.md, board.md, board-redraw.md, ask.md, magrathea.md
       - Refresh: retro.md (re-copied with updated layout references)
     See the Command Rename Map above for what changed. (`ask.md` and `magrathea.md` are new —
     `/ask` queries `core.md` and the retros with citations; `/magrathea` prints a one-screen
     orientation tour of the planet.)

I will NOT:
  - Edit anything inside your magrathea/{issue}/ directories (state.md, planning/, research/)
  - Touch your CLAUDE.md, OPENCODE.md, or any other project files
  - Run any git commands — staging and committing this rename is up to you

Proceed? (yes / no)
```

Adjust the report to reflect what you actually detected. If `information.md` doesn't exist, drop the core file rename. If some new-name files are already in place (partial migration), only list the deletes/installs that are still needed.

**Wait for explicit "yes" before doing anything.** The user describing the task or saying "looks good" is not the same as "yes, proceed". No exceptions.

### Step 4 — Execute

On confirmation, run the actions **in order**, stopping immediately if any step fails. Use plain `mv` and plain `rm` — never `git mv`, `git rm`, or any other git command. Staging and committing is the user's responsibility, and running git commands here could interfere with in-progress work they haven't told you about.

1. **Folder rename:** `mv project magrathea`

2. **Core file rename:** (only if `magrathea/information.md` exists)
   `mv magrathea/information.md magrathea/core.md`

3. **Replace command files in the detected commands folder:**

   - **Delete the old command files** that are present: `issue.md`, `new-issue.md`, `list-issues.md`, `rebuild-board.md` (only delete the ones that actually exist).
   - **Install the new command files**: `pickup.md`, `capture.md`, `board.md`, `board-redraw.md`, `ask.md`, `magrathea.md`. Use the canonical versions from this repo's `prompts/` directory. Ask the user where the source repo is if you can't infer it; if they don't know or the source isn't available, write the new prompt content directly.
   - **Refresh `retro.md`** to the new layout if it still references `project/` or `information.md`.
   - Do **not** copy `migrate.md` into the commands folder (it's already there — that's how you got invoked).

### Step 5 — Report results

Output a summary of what actually happened. Surface the command renames prominently — that's the most disruptive part for the user's muscle memory:

```
✓ Planet rebuilt. Mostly harmlessly.

Renamed on disk:
  - project/  →  magrathea/
  - magrathea/information.md  →  magrathea/core.md

Command renames (your muscle memory will fight this for a day or two):
  - /issue          →  /pickup
  - /new-issue      →  /capture
  - /list-issues    →  /board
  - /rebuild-board  →  /board-redraw
  - /retro          →  unchanged

Command files in .claude/commands/:
  - Removed: issue.md, new-issue.md, list-issues.md
  - Installed: pickup.md, capture.md, board.md, board-redraw.md, ask.md, magrathea.md
  - Refreshed: retro.md

New capabilities in this version:
  /ask       — query the planet's memory (core.md + retros) with citations.
               Try it: /ask what did we decide about <something>?
  /magrathea — one-screen orientation tour of the planet. Run it at the start
               of a fresh conversation to skip the "so… where were we?" dance.

Next steps:
  - Run `/board-redraw` first — it generates `magrathea/board.md`, the new fast kanban index.
    The other commands (`/pickup`, `/capture`, `/board`) refuse to run until it exists.
  - Then delete prompts/migrate.md from your commands folder — so long, and thanks for all the renames.
  - Review the rename with `git status` and stage / commit it yourself when you're ready.
    (I deliberately did not touch git — if you had in-progress work staged, it's untouched.)
  - Run `/board` to see your board on the renamed planet.
```

If any step failed, report exactly what succeeded, what didn't, and what the user should check before re-running.

## Notes

- The folder and `information.md` renames are reversible (the destination didn't exist before — `mv` it back). The command-file replacement is *also* reversible if the user has the old `prompts/` directory checked out, since it's just copies.
- If something is unexpected (e.g., both `project/` and `magrathea/` already exist, or some of the new command files exist but others don't), stop and ask — do not merge or overwrite blindly.
- Do not modify the contents of any user-authored file in `magrathea/{issue}/`. Their `state.md`, planning notes, research, and retro files are untouched by this migration.
- This is a one-off. Once it succeeds, the user should remove `migrate.md` from their installed commands folder. Mentioning this in the closing summary is part of the job.
