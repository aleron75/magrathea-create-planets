# Install Magrathea: OpenCode

OpenCode discovers custom commands in `.opencode/commands/` at the project root (or `~/.config/opencode/commands/` for global commands). Each `.md` file becomes a slash command.

## Steps

1. Copy the prompt files into your project:

   ```bash
   mkdir -p .opencode/commands
   cp /path/to/prompts/*.md .opencode/commands/
   ```

2. Create the planet (folder + empty kanban index):

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

3. Start OpenCode and run `/issue` to begin.

## Invocation

| Prompt | Command |
|--------|---------|
| `issue.md` | `/issue` or `/issue 5` or `/issue feat-login` |
| `new-issue.md` | `/new-issue` |
| `list-issues.md` | `/list-issues` |
| `rebuild-board.md` | `/rebuild-board` |
| `retro.md` | `/retro` |

## Argument passing

OpenCode passes everything after the command name as `$ARGUMENTS`. The `issue` prompt uses this to accept an issue number or slug directly.

## Agent context file

Create an `AGENTS.md` (or `OPENCODE.md`) at your project root, or add an `instructions` key in `.opencode/config.json` to give OpenCode persistent context about your project. See the [OpenCode docs](https://opencode.ai/docs) for details.

### Tell your agent about Magrathea

The whole point of Magrathea is that **project knowledge lives in the project itself**, not in your head and not in a separate conversation. The agent only benefits from that if it knows where to look. Add this block to your `AGENTS.md` (paste it as-is, then customize for your project):

```markdown
## Project knowledge — Magrathea

This project uses the Magrathea workflow. Accumulated project knowledge lives here:

- `magrathea/core.md` — patterns, decisions, lessons learned across all issues. Read this before suggesting architectural changes or making non-obvious choices.
- `magrathea/board.md` — the kanban index. The current state of every issue at a glance.
- `magrathea/{NNN-slug}/state.md` — the "where I left off" file for each issue, including its Progress Log.

When the user references prior work ("the cart bug we fixed last week", "the auth refactor"), check `core.md` and the relevant issue's `state.md` before asking them to re-explain.
```

Without this block the prompts still work, but the agent won't think to read `core.md` unsolicited — and that's the file that makes Magrathea worth the effort.
