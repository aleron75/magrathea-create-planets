---
---

# List All Issues

Display all available issues in the `project/` directory with their current status.

## Implementation

1. Check if `project/` directory exists
   - If not, output: "No issues found. Run `/issue` to create one."
   - Stop here.

2. List all directories in `project/` (excluding `information.md`, `example-issue`)

3. For each issue directory found:
   - Extract the **number** from the directory name if it matches `^\d{3}-(.+)$`
   - For non-numbered directories (like `example-issue`), mark as "template"
   - Read the issue's `state.md` file
   - Extract the **Status** field (should be "In Progress", "Completed", or similar)
   - Extract the **Started** date
   - Display in a formatted table

4. Output format (sorted by number ascending):

   ```
   Available Issues:

   | # | Issue | Status | Started |
   |---|-------|--------|---------|
   | 001 | research-csp-migration-modules | In Progress | 2026-03-11 |
   | 002 | feat-hyva-paradise-packages | Completed | 2026-03-11 |
   | 003 | impl-frontend-learning-hub | In Progress | 2026-03-11 |
   | 004 | test-frontend-and-helper | In Progress | 2026-03-11 |
   | 005 | improve-quiz-ux | Completed | 2026-03-12 |
   | 006 | fix-frontend-urls | Completed | 2026-03-12 |
   ```

5. At the end, prompt: "Run `/issue {number}` (e.g., `/issue 3`), `/issue {full-name}` (e.g., `/issue 003-impl-frontend-learning-hub`), or `/issue` to create a new one."

## Notes

- Sort issues by numeric ID (ascending)
- Gracefully handle missing `state.md` files (show "Unknown" for status/date)
- Include both numbered issues and template directories
- Make the output easy to scan and reference
