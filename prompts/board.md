---
description: Show the kanban board
---

# Show the Board

Display the current state of the planet at a glance. Reads from `magrathea/board.md` — the fast index — rather than walking every `state.md`.

## Implementation

1. **Check `magrathea/` exists.** If not, output: "No planet yet. Use `/capture` to file your first issue." and stop.

2. **Check `magrathea/board.md` exists.** If not, output:

   ```
   No board index yet. Run /board-redraw to generate magrathea/board.md from your existing issues, then try again.
   ```

   Stop. Do not attempt to walk state.md files — that's `/board-redraw`'s job. Keeping this command fast is the point.

3. **Read `magrathea/board.md`.** Parse the five (or six, if Unknown is present) status sections.

4. **Render the board** in this order (least urgent first, most actionable last — optimized for terminal where the bottom is closest to the prompt):

   1. **Closed** — show all, sorted by number descending
   2. **Complete** — show 3 most recent (highest number), then "X more not shown" if there are more
   3. **In Review** — show all, sorted by number ascending
   4. **Todo** — show all, sorted by number ascending
   5. **In Progress** — show all, sorted by number ascending
   6. **Unknown** (only if present) — show all

5. **Use the same table format already in board.md.** Print empty groups as `(none)`.

6. End with: "Use `/pickup <number>` to resume, `/capture` to file a new one, or `/board-redraw` if the board looks out of date."

## Notes

- Always show all groups (even empty ones) — the board is always the full board.
- In Progress is last — most actionable, closest to the prompt.
- Complete and Closed are capped at 3 most recent to keep the view focused.
- Truncation happens at render time. `board.md` itself stores everything.
- If `board.md` looks suspicious (e.g. issues you remember creating are missing, or status doesn't match what you just changed), run `/board-redraw`.
