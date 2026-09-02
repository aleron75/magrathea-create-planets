---
description: Babel fish — prepare a draft for an audience outside this planet — replace local-only references with something they can actually open
---

# Babel Fish — Prepare External Communication

> *"The Babel fish is small, yellow, leech-like, and probably the oddest thing in the universe. It feeds on brainwave energy... The practical upshot of all this is that if you stick a Babel fish in your ear you can instantly understand anything said to you in any form of language."* — The Guide.

You are preparing a piece of writing — an email, a handover brief, a report, a blog draft, a comment on someone else's ticket, anything — to go to a reader who does **not** have access to this repo, this `magrathea/` planet, or this machine. The test is not "is this customer-facing" or "is this sensitive" — it's simpler: **can the reader actually resolve every reference in this document?** If not, fix it before it goes out.

This is primarily about reachability, not anonymity — but the two aren't fully separable, so don't assume either default silently.

When this prompt starts doing its work — writing or reviewing a passage — say so in one short line before diving in, e.g. "Getting the Babel fish out — translating this for someone off-planet." It's a fitting enough image for the job (make the internal understandable to an outsider) that it's worth the one-liner; don't belabor it beyond that single line per invocation.

## 1. Get or Write the Draft

`$ARGUMENTS` is usually a task description, not a file path — treat it that way first. Note the actual value once and reason about it as plain text from here on; don't repeat the literal `$ARGUMENTS` token in your own output or in the checks below, since your agent's runtime has already substituted it with whatever was typed:

- **It points at an existing file** (a real path that resolves): read it as the draft, skip straight to step 2.
- **The conversation already has a draft on the table** (something just written this session, or pasted by the user): use that as the draft, skip straight to step 2.
- **It's a prose task description** (the common case — e.g. "a handover to Ford Prefect to pick this up further", "email to the researcher about the current process", "a report about the work done on the XSS issue"): this is a request to *write* the draft, not just clean up an existing one. Before writing:
  1. Pull out what the description already tells you for free — don't ask about anything it already answers:
     - **Recipient/audience** ("to Ford Prefect", "to the researcher") — who this is for.
     - **Platform**, if named or obvious from the recipient/context (email, Slack, a handover doc, a ticket comment). If genuinely ambiguous, ask.
     - **Subject/scope** ("the current process", "the work done on the XSS issue") — what it needs to cover.
     - **Issue context** — a named issue ("the XSS issue"), an implicit one (matches the issue already active this session), or clearly none (a cross-cutting report, a generic status update). Resolve this the same way `/pickup` resolves an issue reference; if a name/topic is given but ambiguous between two issues, ask which. **A topic name matching an issue's slug is not enough on its own** — check that issue's `state.md` for a "moved to" / "continues under" pointer to a successor issue (e.g. an investigation issue that closed into a separate release-execution issue) and resolve to whichever issue is actually the live one for this correspondence, not just the first slug match.
  2. Gather the actual content, cheapest source first — same principle as `/ask`: `core.md` and the curated layer exist precisely so most questions don't need a full-planet read.
     - Start with the relevant issue's `state.md` (if an issue was identified) — it's the "where things stand" scratchpad and usually has what a status update needs on its own.
     - Check `magrathea/core.md` and, if the topic looks retro-adjacent, `magrathea/retro/` for anything that fills a gap `state.md` didn't cover — a decision, a pattern, a piece of established framing relevant to this draft.
     - Only open `planning/` or `research/` files for a *specific* fact `state.md`/`core.md` didn't have — e.g. `state.md` points at a particular research doc by name for exactly the detail this draft needs. Don't read a whole `research/` directory speculatively; that's the expensive path and most drafts don't need it.
     - No issue identified: pull together whatever this session already knows about the topic, checking `core.md` for anything relevant before assuming nothing's there.
  3. Write a full first draft addressing the recipient and covering the subject, in a shape appropriate to the platform (an email has a subject line and greeting; a handover brief doesn't). Writing it in the reader's own voice from the underlying facts, rather than paraphrasing or copying phrasing from `state.md`/`core.md`, avoids most of what step 3 would otherwise have to catch — treat step 1's drafting and step 3's sweep as one continuous habit, not "write freely, then clean up after."
  4. Continue into step 2 with this draft — the rest of the prompt applies to it exactly as it would to a pasted draft.
- **It's empty and there's no draft on the table**: this isn't a request to prepare one specific document — it's a request to switch the rest of the session into "external comms mode." Skip to **Step 1a** instead of continuing below.

### Step 1a — Activate for the Session (no-argument invocation)

There's nothing to sanitize yet, so don't ask "what's the draft" — that question doesn't fit what was invoked. Instead:

1. Ask the naming question from step 2 once, up front, for the session: "Should real names stay in, or should anything written from here on be genericized to roles/orgs?" This is the same question step 2 would ask per-draft; asking it here just settles it once instead of re-asking every time something gets written.
2. Confirm activation plainly and briefly, e.g.: "Babel fish in — external comms guidelines are active for this session (names: {kept/genericized}). I'll apply the reachability rules from here on for anything meant to leave the planet." Don't restate the full rule list back at the user; they don't need a recap of a prompt they just invoked.
3. Continue the conversation exactly where it was — whatever issue is already loaded (from an earlier `/pickup`, or otherwise in progress) keeps going normally. Don't ask follow-up questions this step didn't need, and don't require a draft to exist right now.
4. From this point on in the session, apply steps 3–8 (reachability check, code/file handling, tone rules, final sweep, filing) as a standing habit to anything written for an outside reader, using the name-policy answer from step 1, without needing `/babelfish` invoked again for each one. If the user later runs `/babelfish` again with a task description or file in the same session, treat the name-policy answer as already settled — don't re-ask it in step 2.

This mode ends naturally at the end of the session (or if the user says to drop it) — there's no explicit "deactivate" step to define here.

## 2. Ask Whether Names Can Stay

If this session already activated external comms mode (Step 1a) and settled this question, use that answer and skip straight to step 3 — don't re-ask.

Otherwise, before touching the draft, ask explicitly:

> "Should real names stay in — your name, colleagues', the company's — or should this be genericized to roles/orgs (e.g. 'the release owner', 'the vendor') instead?"

Don't infer this from the content or guess based on how sensitive the topic feels. The right answer depends on the relationship with the reader and the nature of the disclosure, not on anything visible in the draft itself — a bug report and a partnership email can call for opposite answers, and it's the human's call, not a default this prompt should assume. If the user already stated a preference earlier in the conversation (e.g. while describing the task), confirm it rather than re-asking from scratch.

- **Names stay**: proceed normally — company names, product names, and people's names pass through untouched. The reader presumably already knows who they're dealing with; only local-only references get rewritten.
- **Genericize**: rewrite personal names as roles ("the lead developer for this area", "the project owner", "the reporting user") and, if asked, company/product names as neutral descriptions too. Apply this consistently — check the whole draft, not just the obvious first mention of a name.

Keep this answer in mind through the rest of the steps; it doesn't get revisited per-reference.

## 3. Find Every Local-Only Reference

Read through the draft and flag anything that only resolves inside this repo:

- **Magrathea bookkeeping**: `magrathea/` paths, issue numbers (`issue 021`, `007-fix-...`), wikilinks (`[[...]]`), `state.md`/`core.md`/`board.md` mentions, phrases like "see the retro" or "per core.md's rule."
- **Local filesystem paths**: anything under this machine's working directory that the reader can't open — a path into a local clone kept for reference, a scratch directory, wherever this planet happens to keep things. The specific layout is whatever this project chose (this prompt doesn't assume any particular directory convention); the test is just "is this a path on this machine," not whether it matches a known pattern.
- **Confirmation/decision-process narration**: "this is now CONFIRMED, not provisional," "gate PASSED," "corrected 2026-09-01, this replaces an earlier version." The reader never saw the earlier state — narrating how the document came to be written doesn't help them. (Exception: state a decision plainly when the reader genuinely needs to know it's settled, e.g. "we will not be backporting this" — that's a fact, not process narration.)
- **Project-internal jargon and codenames**: shorthand invented for this project's own bookkeeping — an internal bug-class label, a codename for a batch of files, an internal-only severity/process term — that reads as a real term to someone inside the project but means nothing to an outside reader. These don't match any fixed pattern, so they won't show up in the step 6 grep; catch them by reading, not by search.

For each hit, do **not** just delete it — replace it with the substance: the fact, the rule, the reasoning it was standing in for. "The established pattern is X, because Y" beats "see 007". If a referenced fact lives in `core.md` or another issue, pull the actual content in as plain prose.

## 4. Handle Code and File References

A reference to a specific file or piece of code needs different treatment depending on where that code lives. Figure out the actual origin rather than guessing from the local directory it happens to sit in — a local clone's path tells you nothing about visibility on its own, but its git remote does:

- **Find the real URL first.** If the reference points into a local git checkout, run `git remote get-url origin` (or read `.git/config`) in that checkout rather than assuming anything from the path it's cloned into — the same host can serve both public and private repos, and a path alone (whatever this planet's own clone-directory convention happens to be) doesn't tell you which.
  - `git remote get-url origin` on an SSH-form remote returns something like `git@github.com:group/repo.git` — that's not a browsable URL yet. Normalize it: `git@{host}:{path}.git` → `https://{host}/{path}` (drop the `.git` suffix). A remote that's already `https://...` needs no rewriting. A remote that's a local filesystem path (`/home/...`, `file:///...`) or otherwise has no real host has no browsable URL at all — treat it the same as private/internal (inline or attach), there's nothing to link to.
  - **The remote's namespace can be a personal fork, not the canonical repo** — e.g. `git@host:someones-username/project.git` where the actual project lives at `host/the-real-org/project`. Rewriting the remote mechanically would link the reader to somebody's personal fork instead of the real thing. If the remote's path looks like an individual's username rather than an org/team name, or doesn't match the project's obvious canonical name, check before using it — don't trust the raw rewrite blindly.
- **Public repo** (the resulting, corrected URL is one the reader can actually browse without credentials — e.g. a public GitHub/GitLab project): link it. Use a permalink to the specific commit/line where possible, not a branch reference that will drift.
- **Private/internal repo** (the reader has no access — an internal company host, a private repo on an otherwise-public host, anything behind a login): don't link it, the URL will 404 or prompt a login for them.
  - **Short, specific reference** (a function, a few lines, a single config value): inline the actual snippet in the document.
  - **Whole file, diff, or commit**: check whether it's sitting in a public repo first (e.g. a diff against a public upstream, a commit on a public mirror) — if so, link it instead of attaching anything, same as any other public reference. Only fall back to a companion attachment when there's genuinely no public URL to point at.

### Attachments Get the Same Reachability Check, Not a Pass

A companion attachment is still a document leaving the planet — it doesn't skip the sweep just because it's a file instead of prose in the main draft.

- **Check before copying.** Read the file first. If it's already self-contained — a standalone test fixture, a generic config file, a snippet with nothing project-specific in it — attach the original as-is. Don't reflexively copy every attachment "to be safe"; that's wasted work when there's nothing to fix.
- **If it has any local-only reference** (per step 3's categories — a `magrathea/` mention, an internal hostname, a local file path, a code comment naming an issue number, anything else that only resolves inside this repo): don't attach the original. Make a copy, resolve every reference in the copy the same way step 3 resolves them in the main draft (inline the substance, not the pointer), and attach the corrected copy instead. Say so plainly when handing the draft back — "attached a cleaned copy of `X`, the original had an internal reference to `Y`" — so the substitution doesn't go unnoticed.
- **This applies recursively.** If the cleaned attachment itself references yet another file the reader can't reach, that reference needs the same treatment again — checked, then either linked (if public), inlined, or turned into its own cleaned attachment. Don't stop at one level deep because the first file turned out fine.

If you can't tell whether a repo is public or private (no remote configured, unfamiliar host, ambiguous visibility even after checking), ask rather than guessing — a wrong guess here either leaks something that should have stayed inline, or sends the reader a dead link.

## 5. Apply This Planet's Own Tone Rules, If It Has Any

This prompt is generic and travels across projects — it doesn't hardcode anyone's house style. But *this* planet's `magrathea/core.md` may have its own "Writing Standards for Customer-Facing Content" (or similarly named) section: tone, spelling convention, branding, formatting rules specific to this project's audience.

- If `magrathea/core.md` has such a section, read it and apply it.
- If it points at a further tone guide elsewhere in the repo (e.g. a docs-site skill), read that too.
- If neither exists, skip this step — don't invent house style rules that weren't asked for.

## 6. Do a Final Sweep

Before calling it done, grep the draft for the tells:

```
issue [0-9]{3}|magrathea|\[\[|\.\./|state\.md|core\.md|board\.md
```

Resolve every hit. **This grep is a floor, not the check** — it only catches the fixed patterns it was written for. It will not catch a project-specific codename, jargon term, or a name that should have been genericized, because those don't share a common pattern across projects. So after the grep comes back clean, still do the read: if genericizing was requested, re-check every proper name found in step 2 got rewritten — it's easy to catch the first mention and miss a later casual reference — and separately scan for any term that would mean nothing to someone outside this project even though it reads as an ordinary word to someone inside it. Then re-read the document once as if you were the recipient with none of this repo's context — does every reference in it actually go somewhere you could open, and does every claim stand on its own without a pointer back here? A clean grep result is not a finished sweep.

## 7. File the Correspondence

This is outbound (or inbound) correspondence, so it gets a record on the planet, same as email already does:

- **If an issue is already known** — resolved in step 1 from the task description (e.g. "the XSS issue"), from an earlier `/pickup` this session, or otherwise unambiguous: just file it, no need to ask again. Write it to `{ISSUE}/research/{platform}/NN-{direction}-{YYYY-MM-DD}.md` per the correspondence convention in `core.md`:
  - `{platform}` is the lowercase channel name (`email`, `slack`, `linkedin`, `ticket`, ...) — infer it from context, ask only if genuinely unclear.
  - `NN` is the next number in the **global** sequence, shared across every issue and every platform — find the current highest via `ls magrathea/*/research/*/[0-9]*.md` (or equivalent) across the whole planet, not just this issue or this platform, and increment.
  - `{direction}` is `incoming-report` / `reporter-reply` / `reply`, or a similar pair that fits the platform.
  - Use the correspondence file format from `core.md`: heading, frontmatter (`From`/`To`/`Subject` or `Channel`), `---`, body verbatim.
- **If there's no active issue** (this is standalone/generic comms, not tied to work already in flight): don't force a `/capture` just to file one message. Instead:
  1. Check `magrathea/core.md` (and the board, if useful) for an existing issue this correspondence is plausibly related to.
  2. Draft and save the correspondence file regardless — write it somewhere sensible even before the issue question is settled (e.g. a `magrathea/_unfiled/` staging spot, or directly under a likely-related issue's `research/{platform}/` if one clearly matches).
  3. Decide the final home *after* the draft exists: either file it under the related existing issue, or flag that a new issue may be worth creating — but don't block finishing the draft on that decision.

## 8. Hand It Back

Show the user the prepared version and where it was filed (or staged). If you made non-trivial rewrites (inlining a fact, replacing a private-repo link with a snippet, flagging an attachment, genericizing names), briefly note what changed and why — not as a wall of diffs, just enough that they can sanity-check the substitutions before sending.

If anything needed a companion attachment, list it explicitly — and say whether it's the original file or a cleaned copy: "Attach `path/to/file` alongside this" vs. "Attach the cleaned copy of `path/to/file` alongside this (original had an internal reference to `Y`, resolved in the copy)."

## Notes

- This is not the same job as `/magrathea-export`. `/magrathea-export` hands a whole *issue* to another Magrathea instance and unconditionally strips identity/provenance so the issue can be picked up fresh elsewhere, no asking. `/babelfish` takes *any* piece of writing to *any* outside reader, where the right call on names depends on who's reading — so it asks once up front instead of assuming either "keep everything" or "strip everything."
- Three ways to invoke: a file or pasted draft → sanitize it (steps 2–8); a task description ("email to Ford Prefect about X") → write the first draft, then sanitize it (step 1 → 2–8); no arguments at all → don't demand a draft, just settle the naming question once and keep applying the reachability rules for the rest of the session as things get written (step 1a).
