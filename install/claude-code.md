# Install Magrathea: Claude Code

Claude Code discovers custom commands in `.claude/commands/` at the project root. Each `.md` file becomes a slash command.

## Steps

1. Copy the prompt files into your project:

   ```bash
   mkdir -p .claude/commands
   cp /path/to/prompts/*.md .claude/commands/
   ```

2. Create the planet:

   ```bash
   mkdir magrathea
   ```

3. Start Claude Code and run `/issue` to begin.

## Invocation

| Prompt | Command |
|--------|---------|
| `issue.md` | `/issue` or `/issue 5` or `/issue feat-login` |
| `new-issue.md` | `/new-issue` |
| `list-issues.md` | `/list-issues` |
| `retro.md` | `/retro` |

## Argument passing

Claude Code passes everything after the command name as `$ARGUMENTS`. The `issue` prompt uses this to accept an issue number or slug directly.

## Agent context file

Create a `CLAUDE.md` at your project root to give Claude Code persistent context about your tech stack, conventions, and environment. See the [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/memory) for details.
