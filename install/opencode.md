# Install: OpenCode

OpenCode discovers custom commands in `.opencode/commands/` at the project root (or `~/.config/opencode/commands/` for global commands). Each `.md` file becomes a slash command.

## Steps

1. Copy the prompt files into your project:

   ```bash
   mkdir -p .opencode/commands
   cp /path/to/prompts/*.md .opencode/commands/
   ```

2. Create the `project/` directory:

   ```bash
   mkdir project
   ```

3. Start OpenCode and run `/issue` to begin.

## Invocation

| Prompt | Command |
|--------|---------|
| `issue.md` | `/issue` or `/issue 5` or `/issue feat-login` |
| `new-issue.md` | `/new-issue` |
| `list-issues.md` | `/list-issues` |
| `retro.md` | `/retro` |

## Argument passing

OpenCode passes everything after the command name as `$ARGUMENTS`. The `issue` prompt uses this to accept an issue number or slug directly.

## Agent context file

Create an `OPENCODE.md` or add an `instructions` key in `.opencode/config.json` to give OpenCode persistent context about your project. See the [OpenCode docs](https://opencode.ai/docs) for details.
