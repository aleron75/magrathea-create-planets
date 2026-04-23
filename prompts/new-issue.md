---
description: Create a new issue
---

# Create a New Issue

You are creating a new issue. Follow these steps:

## 1. Set Mode

First, ask the user:

> "Are you planning to create **multiple issues** (planning session), or do you want to **start working** on this one right away?"

- **Multiple issues** — after each issue is created, ask "Describe the next issue, or say `done` to finish."
- **Start working** — after creating the issue, automatically continue into the `issue` workflow (from Step 4: Load Context onward)

## 2. Gather Issue Details

Ask the user: **What does this issue need to accomplish?** (description of the work, context, goals)

Wait for their response.

## 3. Load Project Context

Before generating anything, read:

1. `project/MEMORY.md` — project-specific conventions (if present)
2. `project/information.md` — cross-issue knowledge base (if present)

This ensures the new issue title, slug, and initial state reflect established project conventions and avoids duplicating work already done.

## 4. Generate Title and Slug

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

## 5. Determine Issue Number

- Scan `project/` directory for existing directories matching the pattern `^\d{3}-` (e.g., `001-`, `002-`, etc.)
- Find the highest number N
- Assign the next number as (N+1), zero-padded to 3 digits (e.g., if highest is `006-`, use `007-`)
- Construct full issue directory name: `{NNN}-{slug}` (e.g., `007-feat-new-feature`)
- Use this as `{ISSUE}`

## 6. Create Directory Structure

Create the following structure for `project/{ISSUE}/`:

```
project/{ISSUE}/
  planning/          (create with .gitkeep)
  research/          (create with .gitkeep)
  state.md           (create with initial content)
```

Valid status values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`

Create `project/{ISSUE}/state.md` with this content (replacing {ISSUE} with the full directory name, {TITLE} with the confirmed human-readable title, and {DATE} with today's date):

```markdown
# {TITLE}

**Issue**: {ISSUE}
**Started**: {DATE}
**Status**: Todo

## Current Focus

(Nothing yet)

## Tasks

(No tasks yet)

## Progress Log

- Session started
```

If `project/information.md` does not exist, create it as an empty file.

## 7. Confirm to User

Output a brief summary:

```
✓ Created issue: {ISSUE}
```

Then branch based on the mode set in Step 1:

- **Multiple issues mode**: Ask "Describe the next issue, or say `done` to finish." Repeat from Step 2 for each additional issue. When done, list all created issues and remind the user to invoke the `issue` prompt with the issue number to start working.
- **Start working mode**: Automatically continue as if the user invoked the `issue` prompt for `{ISSUE}` — proceed from the Load Context step, loading `project/information.md`, the new `state.md`, and outputting the context summary before asking "What would you like to do?"

---

**Tip**: Invoke the `issue` prompt anytime to resume work on any issue — `state.md` will be automatically loaded.
