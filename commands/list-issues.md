---
---

# List All Issues

Display all available issues in the `project/` directory as a kanban-style board.

## Implementation

1. Check if `project/` directory exists
   - If not, output: "No issues found. Run `/issue` to create one."
   - Stop here.

2. List all directories in `project/` (excluding `information.md`, `MEMORY.md`, `example-issue`)

3. For each issue directory found:
   - Extract the **number** from the directory name if it matches `^\d{3}-(.+)$`
   - Read only the **first 6 lines** of the issue's `state.md` — that contains the Title (H1), Issue, Started, and Status fields
   - Extract the **Title** from the H1 heading (first line starting with `# `)
   - Extract the **Status** field (valid values: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`)
   - Extract the **Started** date
   - If `state.md` is missing, use "Unknown" for all fields

4. Group issues by status. Output in this order (least urgent first, most actionable last — optimized for terminal where the bottom is closest to the prompt):

   1. **Closed** — show all, sorted by number descending
   2. **Complete** — show 3 most recent (highest number), then "X more not shown"
   3. **In Review** — show all, sorted by number ascending
   4. **Todo** — show all, sorted by number ascending
   5. **In Progress** — show all, sorted by number ascending

5. Format each group as:

   ```
   ## Closed
   (none)

   ## Complete
   | # | Title | Started | Slug |
   |---|-------|---------|------|
   | 003 | Research Auth Patterns | 2026-01-01 | 003-research-auth-patterns |
   | 002 | Add Dark Mode Toggle | 2026-01-05 | 002-feat-dark-mode-toggle |
   | 001 | Fix Cart Persistence Bug | 2026-01-08 | 001-fix-cart-persistence-bug |
   2 more not shown.

   ## In Review
   (none)

   ## Todo
   | # | Title | Started | Slug |
   |---|-------|---------|------|
   | 006 | Fix Something | 2026-01-10 | 006-fix-something |

   ## In Progress
   | # | Title | Started | Slug |
   |---|-------|---------|------|
   | 005 | Add Login Feature | 2026-01-09 | 005-feat-add-login |
   ```

6. End with: "Run `/issue {number}` to resume, or `/new-issue` to create one."

## Notes

- Always show all groups, even if empty (show `(none)`)
- In Progress is last — it's the most actionable and closest to the prompt
- Complete and Closed are capped at 3 most recent to keep the view focused
- Sort ascending for active groups (lowest number = oldest = most likely to need attention)
- Sort descending for Complete/Closed (most recent first)
