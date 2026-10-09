# Worked example: adopting the intermediate knowledge layer

This can only be a sketch, not a real walkthrough — see the caveat at
the top of `optional-intermediate-knowledge/README.md`: no concrete
template file for this directory was available when this documentation
was written, so there is no actual file format to walk through filling
in.

## The shape of what adopting it would probably look like

If you decide your project needs to record a structured fact about your
domain — say, "the retry queue guarantees at-most-once delivery per
message id" — without committing to the full clean-room apparatus (no
frozen snapshot, no independent reconstruction, no mechanical
isolation), this is presumably where that record would go: a short,
evidence-referenced note, lighter than an `assertions/` record, more
durable and checkable than an entry in `planning/knowledge/`.

## What to do instead, honestly, right now

Until this directory has a real template, the safer move for an actual
project is one of:

- use `planning/knowledge/`'s existing, specified format for the
  learning, or
- if the claim is important and evidence-worthy enough to deserve
  real rigor, use `optional-clean-room-workflow/assertions/TEMPLATE.md`
  directly, and simply not adopt the rest of that workflow's heavier
  machinery (snapshots, comparison, isolation) until and unless it's
  warranted.

Either is better than inventing a format for this directory that isn't
actually backed by anything in the template yet.
