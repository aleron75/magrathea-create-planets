---
description: Redraw magrathea/board.md from scratch by scanning every issue's state.md
---

# Redraw the Board

Regenerate `magrathea/board.md` — the fast kanban index used by `/board` — by walking every issue's `state.md`. Use this when:

- `board.md` doesn't exist yet (e.g. after migration, or first-time setup of an existing planet)
- You suspect `board.md` has drifted from reality (manual edits to state.md outside the workflow, abandoned conversations, etc.)
- `/board`, `/pickup`, or `/capture` told you to run it

This command is idempotent and safe to re-run.

## Steps

1. **Check `magrathea/` exists.** If not, output: "No planet yet. Use `/capture` to file your first issue." and stop.

2. **List issue directories** in `magrathea/` matching `^\d{3}-*`. The pattern already excludes `readme.md`, `core.md`, `board.md`, `example-state.md`, `MEMORY.md`, and `retro/`.

3. **For each issue directory**, read the first 6 lines of `state.md` and extract:
   - **Title** — from the H1 (`# {Title}`)
   - **Issue slug** — from the `**Issue**:` field (or the directory name if absent)
   - **Started** — from the `**Started**:` field
   - **Status** — from the `**Status**:` field (must be one of: `Todo`, `In Progress`, `In Review`, `Complete`, `Closed`)
   - **Number** — the leading `NNN` from the directory name

   If `state.md` is missing, record the issue with title "Unknown", status "Unknown", date "Unknown". Do not skip — surface it so the user knows it exists.

4. **Group by status** in this order (least urgent first, most actionable last — same order `/board` prints):

   1. Closed (sorted by number descending)
   2. Complete (sorted by number descending — show all in board.md; truncation happens at render time in /board)
   3. In Review (sorted by number ascending)
   4. Todo (sorted by number ascending)
   5. In Progress (sorted by number ascending)

5. **Write `magrathea/board.md`** with this exact structure:

   ```markdown
   # Board

   > Magrathea kanban board. Regenerate with `/board-redraw` if you suspect it's out of sync.

   ## Closed
   (none)

   ## Complete
   | # | Title | Started | Slug |
   |---|-------|---------|------|
   | 003 | Research Auth Patterns | 2026-01-01 | 003-research-auth-patterns |

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

   - Always render all five groups, even if empty (use `(none)`).
   - Groups with at least one issue render the full table.
   - Issues with status "Unknown" go under a sixth `## Unknown` group at the very bottom — surface them so they don't vanish.

6. **Report.** Output a one-line summary:

   ```
   ✓ Redrew magrathea/board.md ({N} issues: {N todo} todo, {N in-progress} in progress, {N in-review} in review, {N complete} complete, {N closed} closed)
   ```

   If any unknown-status issues were found, add a second line:

   ```
   ⚠ {N} issue(s) with missing/invalid status — see ## Unknown in board.md.
   ```

## Notes

- This command never modifies `state.md` or anything inside issue directories. It only writes `board.md`.
- `board.md` is the index. `state.md` is the truth. When they disagree, run `/board-redraw` and `state.md` wins.
- Subsequent invocations of `/pickup` and `/capture` will keep `board.md` in sync incrementally — you only need this command for cold starts and recovery.
