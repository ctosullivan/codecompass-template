# Optional: persistent, human/tool-editable knowledge layer

**This directory is not part of the everyday adoption path.** It
describes an optional capability shipped by `codecompass` itself
(`codecompass knowledge render|select-candidates|apply|status`) — there
is nothing here to copy into your own `src/`. If you don't need a
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

## How to adopt it

1. Pick (or create) a `planning/knowledge/<slug>/` directory holding
   your own records in that shape.
2. Run `codecompass knowledge render <slug>` — this writes
   `planning/knowledge/<slug>/intermediate/*.md`, an editable projection.
3. Edit the projection directly (by hand, or via any AI tool), writing
   genuinely new material inside each file's own `## Candidate
   additions` region.
4. Run `codecompass knowledge select-candidates <slug>` — mechanical
   detection only, writes a reconciliation manifest, touches nothing
   canonical.
5. Review the manifest (a human, or an AI agent dispatch) and annotate
   each item `accept` or `reject`.
6. Run `codecompass knowledge apply <manifest>` — the only step that
   writes your canonical records, and only after re-checking everything
   mechanically.
7. Optionally, cite a record from your own README/CONTRIBUTING with a
   `<!-- codecompass-grounded-by: CL-... -->` marker so
   `codecompass knowledge status` can tell you when that documentation
   region's own grounding record has moved.

Full design rationale: see CodeCompass's own
`planning/phase-81-intermediate-knowledge-layer.md` and
`docs/codecompass-knowledge-workflow.md`.
