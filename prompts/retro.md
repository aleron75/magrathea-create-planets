---
description: Facilitate an open retrospective
---

# Run a Retrospective

You are facilitating an open retrospective — a chance for the Magratheans to step back and look at the planet's geography. Follow these steps:

## 1. Load Context

Read the following — do not skip any that exist:

1. `magrathea/core.md` — the molten core: cross-issue knowledge, patterns, decisions, lessons learned
2. First 6 lines of all `magrathea/*/state.md` — issue titles, statuses, and dates (lean read)
3. All files in `magrathea/retro/` sorted by filename ascending — extract:
   - `## Open Threads` from the most recent retro — these are carried forward
   - `## Conclusions` from all previous retros — do not reopen these

If `magrathea/retro/` does not exist, create it.

## 2. Prepare Observations

Before opening the floor, synthesize what you see in the data:

- **Board health**: counts by status (Todo / In Progress / In Review / Complete / Closed), sense of pace and momentum
- **Recurring themes**: patterns spotted in `core.md` (what keeps coming up?)
- **Scope and drift**: issues where title or progress log suggest scope changed
- **Open threads**: unresolved points carried from previous retro (if any)

Do not structure this as guided questions. Present it as "here is what I see" and then open the floor:

> "That's what the planet looks like from orbit. What stands out to you?"

## 3. Facilitate the Conversation

- Follow where the conversation goes — do not steer with prepared questions
- Capture key points, decisions, and insights into the retro file as the discussion progresses (append, don't wait until the end)
- If an open thread from a previous retro comes up naturally, address it — if it doesn't, surface it near the end: "One thing from last time we didn't resolve yet: ..."

## 4. Close the Retro

When the conversation feels complete, summarize:

1. **Conclusions** — what was recognized or decided
2. **Open Threads** — things raised but not resolved, to carry forward
3. **Action Items** — concrete follow-ups

For each action item:
- Suggest a title and slug following standard issue conventions
- Note it was created from this retro in the issue's Progress Log
- Create all action item issues in batch (confirm titles/slugs before creating) — they land in the `Todo` lane
- Record the issue numbers in the retro file: `- Improve confirmation flow → #050`

## 5. Write the Retro File

Save the completed retro to `magrathea/retro/YYYYMMDD-retro-{slug}.md` where the slug reflects the main theme of the session (decided at close, not upfront).

Use this structure:

```markdown
# Retro: {Theme}

**Date**: {YYYY-MM-DD}
**Issues reviewed**: {count} issues ({N} complete, {N} in progress, {N} todo)

## Observations

(what was surfaced from the data)

## Discussion

(key points from the conversation)

## Conclusions

(what was recognized or decided — these will not be reopened in future retros)

## Action Items

- {Action item description} → #{issue number}

## Open Threads

(things raised but not resolved — carried forward to next retro)
```

## 6. Offer Shareable Summary

Ask: "Would you like a shareable version of this retro?"

If yes, produce a clean version of the retro file — same structure but written for a technical audience outside the project. Remove any internal rough notes or project-specific context that wouldn't make sense to an outside reader. This can be copied directly to a blog post, team wiki, or shared document.

---

**Tip**: Run this prompt at any cadence that feels right — after a batch of issues, at a project milestone, or whenever you want to step back and look at the planet from orbit.
