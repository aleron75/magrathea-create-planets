---
description: One-off migration from the old project/ layout to Magrathea
---

# Migrate to Magrathea

> *"The first ten million years were the worst. And the second ten million: they were the worst too. The third ten million I didn't enjoy at all. After that I went into a bit of a decline."* — Marvin, on running the old `project/` layout for too long.

This is a **one-off command** for projects that were set up under the previous `project/` + `information.md` layout. The Magratheans have woken up, dusted off the planet-building equipment, and want to move your existing world onto the new naming. It renames the folder, renames the molten core, and refreshes your installed prompt files. Your continents (issues) are untouched.

After it runs successfully, delete this file from your commands folder — you won't need it again. (Unlike the planet, it really is one-off.)

## What This Does

Three actions, in order:

1. Rename `project/` → `magrathea/`
2. Rename `magrathea/information.md` → `magrathea/core.md`
3. Refresh the installed prompt files (your local copies of `issue.md`, `new-issue.md`, `list-issues.md`, `retro.md`) so they reference the new layout

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
- For each of `issue.md`, `new-issue.md`, `list-issues.md`, `retro.md` in the detected commands folder: does it still reference `project/` or `information.md`? (grep for those literal strings)

### Step 2 — Fail loudly if already migrated

If **all** of the following are true, the migration has already been run:

- `magrathea/` exists
- `project/` does not exist
- `magrathea/core.md` exists (or `magrathea/information.md` does not exist)
- None of the installed prompt files contain `project/` or `information.md`

In that case, output:

```
✗ Already migrated. magrathea/ is in place and the installed prompts are up to date.
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
  - Prompts still reference project/:     issue.md, new-issue.md, list-issues.md, retro.md

Actions I will take:
  1. Rename:  project/  →  magrathea/                          (plain `mv`)
  2. Rename:  magrathea/information.md  →  magrathea/core.md   (plain `mv`)
  3. Overwrite installed prompt files in .claude/commands/ with the updated Magrathea versions:
       - issue.md
       - new-issue.md
       - list-issues.md
       - retro.md

I will NOT:
  - Edit anything inside your magrathea/{issue}/ directories (state.md, planning/, research/)
  - Touch your CLAUDE.md, OPENCODE.md, or any other project files
  - Run any git commands — staging and committing this rename is up to you

Proceed? (yes / no)
```

Adjust the report to reflect what you actually detected. If only some prompts still reference the old paths, only list the ones that need overwriting. If `information.md` doesn't exist, drop step 2.

**Wait for explicit "yes" before doing anything.** The user describing the task or saying "looks good" is not the same as "yes, proceed". No exceptions.

### Step 4 — Execute

On confirmation, run the actions **in order**, stopping immediately if any step fails. Use plain `mv` — never `git mv` or any other git command. Staging and committing is the user's responsibility, and running git commands here could interfere with in-progress work they haven't told you about.

1. **Folder rename:** `mv project magrathea`

2. **Core file rename:** (only if `magrathea/information.md` exists)
   `mv magrathea/information.md magrathea/core.md`

3. **Refresh installed prompts:** for each of `issue.md`, `new-issue.md`, `list-issues.md`, `retro.md` that still references `project/` or `information.md`, overwrite the local copy with the current Magrathea version.

   The user already has this repository checked out somewhere — they ran `cp prompts/*.md .claude/commands/` originally. Ask them where the source repo is if you cannot infer it. If they don't know or the source isn't available, write the new prompt content directly using the canonical versions in this repo's `prompts/` directory.

   Do **not** copy `migrate.md` itself into their commands folder.

### Step 5 — Report results

Output a summary of what actually happened:

```
✓ Planet rebuilt. Mostly harmlessly.

Renamed:
  - project/  →  magrathea/
  - magrathea/information.md  →  magrathea/core.md

Refreshed prompts in .claude/commands/:
  - issue.md
  - new-issue.md
  - list-issues.md
  - retro.md

Next steps:
  - Delete prompts/migrate.md from your commands folder — so long, and thanks for all the renames.
  - Review the rename with `git status` and stage / commit it yourself when you're ready.
    (I deliberately did not touch git — if you had in-progress work staged, it's untouched.)
  - Run `/list-issues` to see your board on the renamed planet.
```

If any step failed, report exactly what succeeded, what didn't, and what the user should check before re-running.

## Notes

- Never delete files. Renames only. If something is unexpected (e.g., both `project/` and `magrathea/` already exist), stop and ask — do not merge or overwrite blindly.
- Do not modify the contents of any user-authored file in `magrathea/{issue}/`. Their `state.md`, planning notes, research, and retro files are untouched by this migration.
- This is a one-off. Once it succeeds, the user should remove `migrate.md` from their installed commands folder. Mentioning this in the closing summary is part of the job.
