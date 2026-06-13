# Install Magrathea: Zed

Zed does not support markdown-based custom slash commands directly — slash commands require building a Zed extension. However, you can use the prompts in Zed's **Prompt Library** or paste them manually into the AI panel.

## Option A: Prompt Library (recommended)

Zed has a built-in Prompt Library accessible from the AI panel. You can add the prompts there as saved prompts and invoke them via the `/prompt` slash command.

1. Open the AI panel in Zed
2. Open the Prompt Library (click the book icon or use the command palette)
3. Create a new prompt for each file in `prompts/`:
   - Name: `issue`, `new-issue`, `list-issues`, `rebuild-board`, `retro`
   - Body: paste the contents of the corresponding `.md` file
4. Create the planet at your project root (folder + empty kanban index):
   ```bash
   mkdir magrathea && cat > magrathea/board.md <<'EOF'
   # Board

   > Magrathea kanban board. Regenerate with `/rebuild-board` if you suspect it's out of sync.

   ## Closed
   (none)

   ## Complete
   (none)

   ## In Review
   (none)

   ## Todo
   (none)

   ## In Progress
   (none)
   EOF
   ```
5. Invoke via `/prompt issue` in the AI panel

## Option B: Inline paste

For occasional use, copy the contents of any prompt file and paste it directly into the Zed AI chat input.

## Argument passing

Zed's Prompt Library does not support `$ARGUMENTS` substitution. When using the `issue` prompt, simply type the issue number or slug as part of your message after invoking the prompt — the agent will pick it up from context.

## Agent context file

Use Zed's **Rules** feature (`.zed/rules.md` or the Rules section in Settings) to give the AI persistent context about your project's tech stack and conventions.

### Tell your agent about Magrathea

The whole point of Magrathea is that **project knowledge lives in the project itself**, not in your head and not in a separate conversation. The agent only benefits from that if it knows where to look. Add this block to your `.zed/rules.md` (paste it as-is, then customize for your project):

```markdown
## Project knowledge — Magrathea

This project uses the Magrathea workflow. Accumulated project knowledge lives here:

- `magrathea/core.md` — patterns, decisions, lessons learned across all issues. Read this before suggesting architectural changes or making non-obvious choices.
- `magrathea/board.md` — the kanban index. The current state of every issue at a glance.
- `magrathea/{NNN-slug}/state.md` — the "where I left off" file for each issue, including its Progress Log.

When the user references prior work ("the cart bug we fixed last week", "the auth refactor"), check `core.md` and the relevant issue's `state.md` before asking them to re-explain.
```

Without this block the prompts still work, but the agent won't think to read `core.md` unsolicited — and that's the file that makes Magrathea worth the effort.

## Note

For a fully integrated slash command experience in Zed, a Zed extension would need to be built. This is outside the scope of this workflow, but the prompt files in `prompts/` contain everything needed to implement one.
