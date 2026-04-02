---
argument-hint: ""
---

# Create a New Issue

You are creating a new issue. Follow these steps:

## 1. Gather Issue Details

Ask the user: **What does this issue need to accomplish?** (description of the work, context, goals)

Wait for their response.

## 2. Load Project Context

Before generating anything, read:

1. `project/MEMORY.md` — project-specific conventions (if present)
2. `project/information.md` — cross-issue knowledge base (if present)

This ensures the new issue title, slug, and initial state reflect established project conventions and avoids duplicating work already done.

## 3. Generate Title and Slug

Based on the description provided:
- Generate a **concise title** (3-5 words)
- Generate a **slug** using naming conventions:
  - Feature work: `feat-{feature-name}` (e.g., `feat-login-oauth`)
  - Bug fixes: `fix-{bug-description}` (e.g., `fix-cart-persist`)
  - Refactoring: `refactor-{component}` (e.g., `refactor-api-client`)
  - Investigations: `investigation-{topic}` (e.g., `investigation-performance`)
  - Standard issues: `issue-{number}` (e.g., `issue-42`)

Show the user the generated **title** and **slug**, and ask for confirmation: "Is this good, or should I adjust?"

Wait for their response and adjust if needed.

## 4. Determine Issue Number

- Scan `project/` directory for existing directories matching the pattern `^\d{3}-` (e.g., `001-`, `002-`, etc.)
- Find the highest number N
- Assign the next number as (N+1), zero-padded to 3 digits (e.g., if highest is `006-`, use `007-`)
- Construct full issue directory name: `{NNN}-{slug}` (e.g., `007-feat-new-feature`)
- Use this as `{ISSUE}`

## 5. Create Directory Structure

Create the following structure for `project/{ISSUE}/`:

```
project/{ISSUE}/
  planning/          (create with .gitkeep)
  research/          (create with .gitkeep)
  tasks/             (create with .gitkeep)
  state.md           (create with initial content)
```

Create `project/{ISSUE}/state.md` with this content (replacing {ISSUE} with the full directory name and {DATE} with today's date):

```markdown
# State: {ISSUE}

**Started**: {DATE}
**Status**: In Progress

## Current Focus

(Nothing yet)

## Progress Log

- Session started
```

If `project/information.md` does not exist, create it as an empty file.

## 6. Confirm to User

Output a brief summary:

```
✓ Created issue: {ISSUE}

Ready to work. What would you like to do first?
```

Include a reminder that they can update `project/{ISSUE}/state.md` with current focus and progress log as they work.

## 7. When the User Describes the First Task

**Do not start implementing.** When the user tells you what they want done:

1. Research the relevant code (read files, explore the codebase)
2. Write a short proposed approach: what you plan to change, where, and how
3. Ask: "Does this approach work for you?"
4. Wait for explicit confirmation before touching any files

The user describing a task is **not** approval to implement it.

---

**Tip**: Run `/issue` anytime to resume work on any issue—`state.md` will be automatically loaded.
