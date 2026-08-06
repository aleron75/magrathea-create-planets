---
description: Export an issue as a self-contained, portable document for another Magrathea instance to import
---

# Export an Issue

> *"Share and Enjoy."* — the motto stamped on everything Sirius Cybernetics ever shipped, including, apparently, this planet's exports.

You are exporting an issue off this planet so it can be handed to a **different Magrathea instance** — a different repo, a different project, possibly a different organisation. That instance has no access to this repo, this `magrathea/` directory, or anything in it. The export must stand completely on its own.

This is a one-way handoff: you produce a single file. You are not setting up sync, you are not expecting the other side to write back, and you are not touching the other instance yourself — you're producing something a human can carry over and hand to it.

## 1. Determine the Issue

- If **$ARGUMENTS** is provided (non-empty): resolve it the same way `/pickup` does — numeric reference or numbered slug, matched against `magrathea/` directories.
- Otherwise, if the current conversation is already working an issue (a `/pickup` happened earlier in this session, or it's otherwise unambiguous), use that issue.
- Otherwise, list the issues on the board and ask which one to export.

If the resolved issue directory doesn't exist, say so and stop.

## 2. Load Everything the Export Might Need

Read, in order:

1. `magrathea/{ISSUE}/state.md` — the issue's own record
2. `magrathea/{ISSUE}/planning/*.md` and `magrathea/{ISSUE}/research/*.md` — all of it
3. `magrathea/core.md` — scan for sections relevant to this issue (they'll typically be tagged with `[{ISSUE}]` or a related issue's tag, but also pull in anything a reader would need to understand the decisions referenced in state.md even if untagged)
4. Any **other issue** referenced from this issue's `state.md` (wikilinks, "see 002", wording like "discovered during X") — read enough of that issue's `state.md` to extract the specific fact being referenced. You are not exporting those issues wholesale; you are inlining the one or two facts this issue actually depends on.

Build a mental list of every external dependency this issue's understanding rests on: a decision made elsewhere, a pattern from `core.md`, a lesson from another issue's Progress Log. Each one needs to end up **inlined as prose** in the export — not referenced.

## 3. Write the Export Document

Produce ONE self-contained Markdown file. Do not split it across multiple files — the importing side is told to drop this into its `research/` directory unmodified, so it needs to be one complete artifact.

### Required frontmatter-style header

```markdown
# {Issue Title}

> Exported from another Magrathea planet on {export date, today}. Originally opened {original **Started** date from state.md}.
> This document is self-contained — everything needed to pick this issue up is inline below. It has no dependencies on the planet it came from.

## Summary

{2-4 sentences: what this issue is, why it exists, what state it's in}

## Background

{Inlined context — the "why" a new reader needs. This is where you fold in anything pulled from core.md or sibling issues: write it as plain explanatory prose ("The established pattern elsewhere in this project is: ..."), never as a pointer.}

## Current State / Progress So Far

{What's been done, what's been decided, what's still open. Derived from state.md's Current Focus + Progress Log + Tasks, rewritten as narrative/list rather than dumped verbatim — the original Progress Log is a session-by-session transcript with internal references; the export is a clean digest of what a new reader needs to know, not that transcript.}

## Open Tasks

{Checkbox list of what remains, adapted from state.md's ## Tasks}

## Relevant Decisions & Patterns

{Any core.md material that's load-bearing for understanding this issue, inlined as standalone statements with their rationale — not "see core.md", the actual content.}

## Suggested Next Steps

{What the importing instance should probably do first, if that's inferable}

## How to Import This

This section is the only place the import instructions live — the document travels alone, so the instructions must travel with it, not be handed over separately.

To bring this into a Magrathea planet:

1. Copy this file into the target repo, anywhere reachable (its own `magrathea/` directory, or a scratch path).
2. Ask the assistant working that planet to import it: point it at this file and ask it to create a new issue from it.
3. The target instance should create a new issue directory with a fresh, locally-chosen number (this export carries no numbering that needs to be preserved), write that issue's `state.md` from the Summary / Background / Open Tasks sections above, and copy this file unmodified into that issue's `research/` directory (e.g. `research/imported-{date}.md`) as the source record.

---

*End of export.*
```

Adjust section names/order if the issue's shape doesn't fit cleanly (e.g. an investigation issue vs. a feature issue) — the goal is completeness and readability, not slavish template adherence.

### Sanitization — mandatory, do this as a final pass

This document leaves the repo. The rule: an export can carry the substance of the work, but not this planet's bookkeeping or anyone's identity.

- **No issue numbers or slugs** (`002`, `[[011-refactor-...]]`, "see 007", "discovered during 002 patch work"). Rewrite each as inline rationale: *why* the fact is true, not *where* it was learned. Example: not "the pattern from 002" but "the established pattern is to use the repository's own management interface rather than mutating a cached object directly, because that interface enforces business rules the direct mutation skips."
- **No `magrathea/` paths, `../` links, or wikilinks** (`[[...]]`) — meaningless outside this repo.
- **No personal names** — rewrite as roles ("the lead developer for this area", "the project owner", "the reporting user"). No email addresses. If this project's `core.md` has a People & Responsibilities (or similar) section, check every name in it and rewrite by role, not just the obvious ones.
- **No internal tool/product names that only make sense inside this org** if they're incidental — but DO keep genuinely necessary technical facts (class names, method names, file paths **within the actual codebase being worked on**), since those are real and the importing instance will need them if it's working on the same codebase. The line: strip references to *this planet's bookkeeping*, keep references to *the actual software*.
- Do a final sweep for the tells: `issue [0-9]{3}`, `magrathea`, `\[\[`, `\.\./`, and any personal name this project's `core.md` mentions. Resolve every hit before finishing.

If the issue isn't sensitive beyond the ordinary (the common case — exporting for reasons like "wrong repo", "spinning off a sub-project", "sharing with a partner team"), there's no need to redact technical detail beyond the above — only the *provenance* (this repo, this planet, these people) needs stripping, not the substance.

## 4. Save the Export

Write the file to `magrathea/{ISSUE}/research/export-{YYYY-MM-DD}.md` (today's date). This keeps a record on this planet of what was exported and when — it does not get deleted or moved.

Log the export in `magrathea/{ISSUE}/state.md`'s Progress Log: a one-line entry noting the issue was exported on this date and (briefly) why/where to, if the user said. Do not change the issue's status or board position — exporting is not completing it here; the same issue may still be worked on both planets, or this planet may close it separately later.

## 5. Hand It Off

Tell the user:

```
✓ Exported {ISSUE} → magrathea/{ISSUE}/research/export-{DATE}.md

This file is self-contained and carries its own "How to Import This" section at the
bottom, so it's ready to hand to another Magrathea instance as-is — copy it into the
target repo and ask that instance to import it from there.
```

Do not attempt to perform the import yourself, even if the target repo happens to be reachable in this session — this command's job ends at producing the file.

## Notes

- This is different from `/capture` in the other direction: `/capture` starts a NEW issue on THIS planet from scratch. `/magrathea-export` takes an EXISTING issue on this planet and prepares it to become a new issue on ANOTHER planet.
- If the issue references ongoing work that only makes sense in this repo's context (e.g. "wait for the other release to ship") and that context doesn't transfer, say so plainly in the export rather than silently dropping it — a gap the importing reader can see is better than one they can't.
- Re-running this command on the same issue on a later date produces a new dated export file; it does not overwrite the previous one. Each export is a point-in-time snapshot.
