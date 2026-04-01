---
argument-hint: ""
---

# Create a New Issue

You are creating a new issue. Follow these steps:

## 1. Gather Issue Details

Ask the user: **What does this issue need to accomplish?** (description of the work, context, goals)

Wait for their response.

## 2. Generate Title and Slug

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

## 3. Determine Issue Number

- Scan `project/` directory for existing directories matching the pattern `^\d{3}-` (e.g., `001-`, `002-`, etc.)
- Find the highest number N
- Assign the next number as (N+1), zero-padded to 3 digits (e.g., if highest is `006-`, use `007-`)
- Construct full issue directory name: `{NNN}-{slug}` (e.g., `007-feat-new-feature`)
- Use this as `{ISSUE}`

## 4. Create Directory Structure

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

## 5. Confirm to User

Output a brief summary:

```
✓ Created issue: {ISSUE}

Ready to work. What would you like to do first?
```

Include a reminder that they can update `project/{ISSUE}/state.md` with current focus and progress log as they work.

---

**Tip**: Run `/issue` anytime to resume work on any issue—`state.md` will be automatically loaded.
