# Install: Zed

Zed does not support markdown-based custom slash commands directly — slash commands require building a Zed extension. However, you can use the prompts in Zed's **Prompt Library** or paste them manually into the AI panel.

## Option A: Prompt Library (recommended)

Zed has a built-in Prompt Library accessible from the AI panel. You can add the prompts there as saved prompts and invoke them via the `/prompt` slash command.

1. Open the AI panel in Zed
2. Open the Prompt Library (click the book icon or use the command palette)
3. Create a new prompt for each file in `prompts/`:
   - Name: `issue`, `new-issue`, `list-issues`, `retro`
   - Body: paste the contents of the corresponding `.md` file
4. Create the `project/` directory at your project root:
   ```bash
   mkdir project
   ```
5. Invoke via `/prompt issue` in the AI panel

## Option B: Inline paste

For occasional use, copy the contents of any prompt file and paste it directly into the Zed AI chat input.

## Argument passing

Zed's Prompt Library does not support `$ARGUMENTS` substitution. When using the `issue` prompt, simply type the issue number or slug as part of your message after invoking the prompt — the agent will pick it up from context.

## Agent context file

Use Zed's **Rules** feature (`.zed/rules.md` or the Rules section in Settings) to give the AI persistent context about your project's tech stack and conventions.

## Note

For a fully integrated slash command experience in Zed, a Zed extension would need to be built. This is outside the scope of this workflow, but the prompt files in `prompts/` contain everything needed to implement one.
