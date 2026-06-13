# Install Magrathea: Claude Code

Claude Code discovers custom commands in `.claude/commands/` at the project root. Each `.md` file becomes a slash command.

## Steps

1. Copy the prompt files into your project:

   ```bash
   mkdir -p .claude/commands
   cp /path/to/prompts/*.md .claude/commands/
   ```

2. Create the planet (folder + empty kanban index):

   ```bash
   mkdir magrathea && cat > magrathea/board.md <<'EOF'
   # Board

   > Magrathea kanban board. Regenerate with `/board-redraw` if you suspect it's out of sync.

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

3. Start Claude Code and run `/pickup` to begin (or `/capture` to file your first issue).

## Invocation

| Prompt | Command |
|--------|---------|
| `pickup.md` | `/pickup` or `/pickup 5` or `/pickup feat-login` |
| `capture.md` | `/capture` or `/capture <description>` |
| `board.md` | `/board` |
| `board-redraw.md` | `/board-redraw` |
| `retro.md` | `/retro` |

## Argument passing

Claude Code passes everything after the command name as `$ARGUMENTS`. `/pickup` uses this to accept an issue number or slug directly. `/capture` uses it to detect in-flight invocations and skip questions the user has already answered.

## Agent context file

Create a `CLAUDE.md` at your project root to give Claude Code persistent context about your tech stack, conventions, and environment. See the [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/memory) for details.

### Tell your agent about Magrathea

The whole point of Magrathea is that **project knowledge lives in the project itself**, not in your head and not in a separate conversation. The agent only benefits from that if it knows where to look. Add this block to your `CLAUDE.md` (paste it as-is, then customize for your project):

```markdown
## Project knowledge — Magrathea

This project uses the Magrathea workflow. Accumulated project knowledge lives here:

- `magrathea/core.md` — patterns, decisions, lessons learned across all issues. Read this before suggesting architectural changes or making non-obvious choices.
- `magrathea/board.md` — the kanban index. The current state of every issue at a glance.
- `magrathea/{NNN-slug}/state.md` — the "where I left off" file for each issue, including its Progress Log.

When the user references prior work ("the cart bug we fixed last week", "the auth refactor"), check `core.md` and the relevant issue's `state.md` before asking them to re-explain.
```

Without this block the prompts still work, but the agent won't think to read `core.md` unsolicited — and that's the file that makes Magrathea worth the effort.
