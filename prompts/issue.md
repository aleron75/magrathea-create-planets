---
description: Start or resume work on an issue
---

# Start or Resume Issue Work

You are starting or resuming work on an issue. Follow these steps:

## 1. Determine the Issue

- If **$ARGUMENTS** is provided (non-empty):
  - Check if it's a numeric reference (e.g., `1`, `01`, `001`, `6`) or a numbered slug (e.g., `001-research-csp-migration-modules`)
  - Match against existing issue directories in `project/` matching the pattern `NNN-*` or `*-*`
  - If found, use the matched directory name as `{ISSUE}`
  - If not found as a number, treat as a slug and check for exact directory match
  - Use `{ISSUE}` as the resolved issue directory name
- Otherwise, ask the user: "Are we working on an **existing** issue or creating a **new** one?"
  - **If existing**: List all directories in `project/` (excluding `information.md`) with their numbers and ask which one to work on.
  - **If new**:
    1. Ask for a **description** of the issue/work (what needs to be done, context, goals)
    2. Based on the description, **generate a concise title** and **slug** (e.g., `feat-login`, `fix-cart-bug`, `issue-42`)
    3. Confirm the generated title and slug with the user before proceeding
    4. Scan `project/` for directories matching `^\d{3}-` to find the highest number N
    5. Assign the next number as (N+1), zero-padded to 3 digits
    6. Create the issue directory as `{NNN}-{slug}` (e.g., `007-feat-new-feature`)
    7. Use `{NNN}-{slug}` as `{ISSUE}`

## 2. Load Context

Read the following files in order — do not skip any that exist:

1. `project/MEMORY.md` — project-specific entry point or conventions (if present)
2. `project/information.md` — cross-issue knowledge base: patterns, decisions, lessons learned
3. `project/{ISSUE}/state.md` — current focus, tasks, and progress log for this issue
4. `project/{ISSUE}/planning/*.md` — any existing plans (note titles)

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

If the current status is `Todo`, update it to `In Progress` immediately — opening an issue to work on it is the signal that work has begun.

## 3. Bootstrap Directory Structure (if new)

If `project/{ISSUE}/` does not exist, create the following structure:

```
project/{ISSUE}/
  planning/          (create with .gitkeep)
  research/          (create with .gitkeep)
  state.md           (create with initial content)
```

Valid status values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`

Create `project/{ISSUE}/state.md` with (where {ISSUE} is the full numbered issue directory name and {TITLE} is the confirmed human-readable title):

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

(If `project/information.md` does not exist, create it as an empty file—it will be populated as issues complete.)

## 4. Confirm to User

The structured context summary from Step 2 serves as the confirmation. Follow it with:

```
What would you like to do?
```

## 5. When the User Describes a Task

**Do not start implementing.** When the user tells you what they want done:

1. Research the relevant code (read files, explore the codebase)
2. Write a structured proposed approach listing **every file you plan to touch and why**
3. Ask: "Does this approach work for you?"
4. Wait for explicit confirmation ("yes", "go ahead") before touching any files

The user describing a task is **not** approval to implement it. Neither is enthusiasm, nor the task being small or obvious. No exceptions.

Once confirmed:
- Save the confirmed plan to `project/{ISSUE}/planning/plan.md` before starting work
- Update `**Status**` to `In Progress`
- Replace `## Tasks` in `state.md` with a checkbox list derived from the plan

## 6. Guide During Work

As you work on this issue:

- **Planning**: Save plans and design docs to `project/{ISSUE}/planning/*.md` — the confirmed plan always goes in `planning/plan.md`
- **Research**: Save investigation results and code analysis to `project/{ISSUE}/research/*.md`
- **Tasks**: Maintain the `## Tasks` checkbox list in `state.md` — check items off as they are completed
- **State**: Keep `project/{ISSUE}/state.md` updated with:
  - Current focus (what you're working on now)
  - Progress log (summary of work completed, blockers, decisions made)
  - If the scope has shifted significantly, update the H1 title to reflect what the issue actually became — the slug never changes, but the title should stay accurate

**Important**: Never erase content from `state.md` when completing an issue. The Progress Log is a permanent record — append to it, never overwrite it. If a plan was written into Current Focus, move a summary of it to the Progress Log before updating Current Focus to reflect the completed state.

## 7. When Issue is Complete

Update `**Status**` to `Complete` (or `In Review` if it needs user testing first).

Remove any empty directories (`planning/`, `research/`) that contain only a `.gitkeep` file — they add no value if unused.

Before closing the issue, update `project/information.md` with:

- **Key decisions** made in this issue
- **Patterns discovered** or conventions established
- **Architectural notes** relevant to other issues
- **Lessons learned** that affect future work
- Links to relevant issues or code locations

---

**Tip**: Invoke this prompt again anytime to pick up where you left off — `state.md` will be automatically loaded.
