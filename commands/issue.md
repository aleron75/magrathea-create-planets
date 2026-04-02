---
argument-hint: "[issue-name]"
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
3. `project/{ISSUE}/state.md` — current focus and progress log for this issue
4. `project/{ISSUE}/planning/*.md` — any existing plans (note titles)
5. `project/{ISSUE}/tasks/*.md` — any pending task lists

Then output a structured context summary before asking what to do:

```
Issue: {ISSUE}
Status: {status from state.md, or "New" if just created}
Last worked on: {date from state.md, or today if new}

Last focus: {current focus section from state.md, or "Not started"}
Last action: {most recent progress log entry, or "None"}

Pending tasks: {list open tasks, or "None"}
```

This summary is mandatory — it confirms context was loaded and orients both you and the user.

## 3. Bootstrap Directory Structure (if new)

If `project/{ISSUE}/` does not exist, create the following structure:

```
project/{ISSUE}/
  planning/          (create empty or with .gitkeep)
  research/          (create empty or with .gitkeep)
  tasks/             (create empty or with .gitkeep)
  state.md           (create with initial content)
```

Create `project/{ISSUE}/state.md` with (where {ISSUE} is the full numbered issue directory name):

```markdown
# State: {ISSUE}

**Started**: {today's date}
**Status**: In Progress

## Current Focus

(Nothing yet)

## Progress Log

- Session started
```

For example, if creating issue `007-feat-new-feature`, the header would be `# State: 007-feat-new-feature`

(If `project/information.md` does not exist, create it as an empty file—it will be populated as issues complete.)

## 4. Confirm to User

The structured context summary from Step 2 serves as the confirmation. Follow it with:

```
What would you like to do?
```

## 5. Guide During Work

As you work on this issue:

- **Planning**: Save plans, design docs, and approach notes to `project/{ISSUE}/planning/*.md`
- **Research**: Save investigation results, code analysis, and findings to `project/{ISSUE}/research/*.md`
- **Tasks**: Save task lists and progress tracking to `project/{ISSUE}/tasks/*.md`
- **State**: Keep `project/{ISSUE}/state.md` updated with:
  - Current focus (what you're working on now)
  - Progress log (summary of work completed, blockers, decisions made)

## 6. When Issue is Complete

Before closing the issue, update `project/information.md` with:

- **Key decisions** made in this issue
- **Patterns discovered** or conventions established
- **Architectural notes** relevant to other issues
- **Lessons learned** that affect future work
- Links to relevant issues or code locations

---

**Tip**: Run `/issue` again anytime to pick up where you left off—`state.md` will be automatically loaded.
