# Optional: persistent, human/tool-editable knowledge layer

**This directory is not part of the everyday adoption path.** It
describes an optional capability shipped by `codecompass` itself
(`codecompass knowledge render|select-candidates|doc-select-candidates|
apply|status`, plus `doc-acknowledge-stale`/`doc-acknowledge-chunks`) —
there is nothing here to copy into your own `src/`. If you don't need a
reconciled, editable knowledge layer, ignore this directory entirely.

This is a different thing from `planning/knowledge/README.md` (the
lightweight candidate/reviewed/promoted lessons log already part of this
template's own everyday path). That log stays exactly as it is — keep
using it for "things learned while working on this project." What's
described here is a heavier capability that only makes sense once a
project has adopted a more structured knowledge model.

## What this is, and when it earns its weight

`codecompass` can project a project's own structured knowledge records —
concepts, invariants, behaviours, decisions, requirements, each with its
own cited evidence — into **editable Markdown**, detect when a human or
an AI tool (ChatGPT, Copilot, Claude Code, a plain Git PR) edits that
Markdown, and reconcile the edit back into the structured records only
after it's been checked — never bypassing that check, and never letting
an external tool's own assertion become "confirmed" just because it was
written down.

Reach for this when:

- You already keep (or want to start keeping) project knowledge as
  discrete, evidence-cited facts rather than narrative prose, and you
  want that knowledge to be safely editable by more than one person or
  tool without losing concurrent edits or fabricating confirmation.
- You want your project documentation (README, CONTRIBUTING, etc.) to be
  traceably grounded in the same reconciled facts, without requiring
  every sentence to carry a citation.

Most projects, most of the time, are well served by the plain lessons
log instead. This is for a project that has already outgrown "a
paragraph in a Markdown file" as its own source of truth.

## The record shape this requires

The reconciliation CLI expects records shaped like CodeCompass's own
`planning/knowledge/<slug>/*.yaml` corpus — six kinds (`Observation`,
`Evidence`, `Claim`, `Derivation`, `Decision`, `Requirement`), each a
flat `key: value` file. See `worked-example.md` for one real, minimal
Claim and what it looks like once rendered and reconciled. The full
schema (required fields, status values) is documented in
CodeCompass's own `scripts/check_knowledge_base.py` module docstring and
`docs/domain/concepts/*.md` — reuse it rather than inventing your own
shape, since the shipped CLI is already written against it.

## Canonical knowledge vs. the Markdown projection

`planning/knowledge/<slug>/*.yaml` is always the canonical source of
truth. `planning/knowledge/<slug>/intermediate/*.md` is a disposable,
regeneratable **projection** of it — safe to edit, safe to delete and
re-render, never itself authoritative. Detecting that the projection (or
a grounded document region) has drifted from canonical knowledge is not
the same as *acknowledging* that drift: nothing about running a detection
command ever changes what's canonical, or clears a flagged change, on its
own. Only an explicit `apply` (for a real candidate) or an explicit
acknowledge command (for an advisory finding with nothing to apply)
does that.

## How to adopt it

1. Pick (or create) a `planning/knowledge/<slug>/` directory holding
   your own records in that shape.
2. Run `codecompass knowledge render <slug>` — this writes
   `planning/knowledge/<slug>/intermediate/*.md`, an editable projection.
3. Edit the projection directly (by hand, or via any AI tool), writing
   genuinely new material inside each file's own `## Candidate
   additions` region. Inside that region, how you write it decides what
   it becomes:
   - Plain prose becomes an **unclassified Claim** (a factual hypothesis,
     not yet evidence-backed) — this is the default and the common case.
     Merely mentioning a Decision's id anywhere in your prose does
     **not** make it a Requirement, and words like "should"/"must" do
     **not** make it declared intent.
   - A block starting with a line reading exactly `Type: Requirement`,
     followed by `Decision: <id of an existing, approved Decision>`,
     `Statement: <the requirement, one line>`, and
     `Example: <a Given/When/Then acceptance example>`, becomes a real
     **Requirement** — but only if the cited Decision genuinely exists
     and is approved, and the Example structurally reads as
     Given/When/Then. Otherwise it quietly falls back to an ordinary
     Claim using your Statement text; CodeCompass never fabricates a
     placeholder example to paper over a gap.
   - A block starting with a line reading exactly `Type: Intent`,
     followed by the intended behaviour as plain prose, becomes a Claim
     with `basis: proposed_policy` — a declared policy/intent, not yet a
     fact about the system.
4. Run `codecompass knowledge select-candidates <slug>` — mechanical
   detection only, writes a reconciliation manifest, touches nothing
   canonical.
5. Review the manifest (a human, or an AI agent dispatch) and annotate
   each item `accept` or `reject`.
6. Run `codecompass knowledge apply <manifest>` — the only step that
   writes your canonical records. It re-validates everything
   mechanically immediately before writing (not just at detection time):
   if the projection's own text, or anything the edit depends on, has
   changed since detection, it refuses rather than risk clobbering a
   concurrent edit. Applying the same manifest twice, or a different
   manifest proposing already-applied content, is always a safe no-op —
   it is never re-applied, and a candidate's raw text is removed from the
   live projection once it's genuinely promoted, so it can't be
   rediscovered as "new" on the next detection pass.
7. Optionally, cite a record from your own README/CONTRIBUTING with a
   `<!-- codecompass-grounded-by: CL-... region:<a-stable-id> -->` /
   `<!-- /codecompass-grounded-by -->` pair of markers. The `region:<id>`
   token gives that grounded region a stable identity that survives you
   inserting or reordering content around it later — recommended for
   any marker you intend to keep long-term; a marker with no `region:`
   token still works, just less robustly under reordering.

   With at least one grounded region in place:
   - `codecompass knowledge doc-select-candidates` detects drift in both
     directions: the document's own prose changing (a candidate
     documentation edit, reviewed like any other candidate — see below),
     a cited record changing on its own (reported as "potentially
     stale, worth a review", never auto-resolved), or both at once (an
     explicit, unresolved conflict). None of these are acknowledged
     merely by running detection; a region seen for the very first time
     is the one exception, since there's no prior baseline to lose by
     establishing one.
   - Reviewing a documentation-edit candidate requires one more explicit
     judgment call than an ordinary candidate: is this edit purely
     presentational (`semantic_change = false` — rewording, typo fixes;
     canonical knowledge is untouched, nothing new is created, the
     baseline just advances once you've looked at it) or does it assert
     something new about the system (`semantic_change = true` — creates
     a new candidate Claim, exactly like any other proposed addition,
     never auto-promoted to verified truth)? This distinction is always
     a reviewer's own judgment — CodeCompass never tries to mechanically
     prove two wordings mean the same thing.
   - When a semantic documentation edit is applied, the region's own
     marker is updated to also cite the newly created Claim (e.g.
     `grounded-by: CL-OLD, CL-NEW`) — so the connection between the
     document and the new knowledge stays traceable going forward.
   - `codecompass knowledge doc-acknowledge-stale <doc> <region-id-or-index>`
     explicitly dismisses a "potentially stale" finding once you've
     reviewed it and decided the document's current wording is still
     accurate.
   - `codecompass knowledge doc-acknowledge-chunks` explicitly dismisses
     the separate, purely advisory tracker for *ungrounded* document
     changes (`knowledge status` surfaces these) — useful for noticing
     "this part of the README changed and isn't grounded in anything;
     maybe it should be," with zero canonical-knowledge effect either way.
   - `codecompass knowledge status` reports everything above at a
     glance, plus records needing review and any unresolved concurrent-
     change conflict.

Full design rationale: see CodeCompass's own
`planning/phase-81-intermediate-knowledge-layer.md`,
`decisions/0073-knowledge-layer-corrective-pass.md`,
`decisions/0074-knowledge-layer-second-corrective-pass.md`, and
`docs/codecompass-knowledge-workflow.md`.
