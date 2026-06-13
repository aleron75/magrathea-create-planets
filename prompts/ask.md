---
description: Ask Magrathea a question — query the planet's accumulated knowledge
---

# Ask Magrathea

> *"I checked it very thoroughly," said the computer, "and that quite definitely is the answer. I think the problem, to be quite honest with you, is that you've never actually known what the question is."* — Deep Thought.

Query the planet's memory. `/ask` answers from the *curated* knowledge: `magrathea/core.md` and the retro notes in `magrathea/retro/`. It does **not** scan every issue's `state.md` (too noisy, too session-specific) or your project source code. Use it like a favorite mixtape — the things you keep wanting to refer back to.

## Workflow

### 1. Get the question

- If `$ARGUMENTS` is non-empty, that's the question. Use it.
- Otherwise, ask:

  > "What do you want to know?"

  Wait for the response.

### 2. Read the planet's memory

Read these files (silently — don't narrate the reads):

1. `magrathea/core.md` — the molten core. Patterns, decisions, lessons learned.
2. All files in `magrathea/retro/` — sorted by filename, all of them. Retros surface meta-insights that don't always land in core.md.

If `magrathea/core.md` does not exist (or is empty) **and** `magrathea/retro/` is empty or missing, output:

```
The planet's memory is empty. Nothing has been written to magrathea/core.md and no retros have been recorded yet.

Once you complete an issue, the /pickup workflow prompts you to add what you learned to core.md. After a few issues, /ask will have something to draw from.
```

Stop there. Do not invent answers from the codebase or training knowledge.

### 3. Answer with citations

Synthesize a focused answer to the user's question from what you read. Rules:

- **Stay grounded.** Every substantive claim must trace back to something you actually read. Do not pad with general best-practices.
- **Cite as you go.** Use inline citations: `[core.md > Auth Decisions]`, `[retro/20260301-retro-velocity.md > Conclusions]`. Citations point at the *section* (or top-level heading) so the user can verify.
- **Quote when it matters.** If the answer hinges on a specific sentence, quote it. Don't paraphrase what's already crisp.
- **Be honest about partial answers.** If the planet has *some* relevant material but not a full answer, say so explicitly: "core.md mentions X but doesn't say Y."

### 4. Handle the empty case honestly

If the question was specific and nothing in `core.md` or the retros is actually relevant, do not fabricate. Output:

```
Nothing in the planet's memory about that yet.

Closest material found:
- {bullet — the nearest relevant section, or "no related entries"}

You can:
- Capture this as an issue with /capture if it's something worth tracking
- Add knowledge to magrathea/core.md if you already have an answer in your head
- Ask a more specific question if you think the answer might be in there
```

### 5. Offer follow-ups (when natural)

After the answer, if the question opens obvious threads — a related decision, a half-finished line of investigation, a contradiction between two retros — surface one or two of them as questions the user could ask next. Don't force this. If the answer is complete, end clean.

## Notes

- `/ask` is read-only. It never modifies `core.md`, retros, or any issue files.
- The scope is deliberately narrow: curated knowledge only. If you want broader research (reading state.md files, planning notes, or project source), tell the user to ask directly outside of `/ask` — that's a different mode of work.
- The quality of `/ask` is the quality of `core.md`. Treat the "feed the molten core" step in `/pickup` seriously — what goes in is what comes back out.
