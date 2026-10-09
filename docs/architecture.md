# Scaffold structure

This describes codecompass-template's *own* layout — not a running
system, since this repository has no runtime behaviour of its own.

```
decisions/
  README.md        — what an ADR is for, when to write one, append-only rule
  TEMPLATE.md       — the ADR shape: Status / Context / Decision /
                      Alternatives considered / Consequences

planning/
  CONTEXT.md        — current state only; overwritten at each stopping point
  ROADMAP.md        — an at-a-glance status table, updated as scope changes
  retros/
    TEMPLATE.md     — a short after-action record per piece of work
  knowledge/
    README.md       — informal learnings log: candidate → reviewed →
                      promoted / retained / discarded
  context-gaps/
    README.md       — log of context that should have existed for a real
                      task and didn't (narrower than knowledge/: specifically
                      about missing/misleading context, not "how we should work")

optional-intermediate-knowledge/
  — a lightweight knowledge-layer add-on; see its own README for how much
    of this is actually specified versus still open

optional-clean-room-workflow/
  README.md                       — what the workflow is for, as a whole
  conceptual-documentation-guide.md — writing a fresh doc draft from a snapshot
  mechanical-isolation.md          — the isolation mechanism behind the
                                     independent-reconstruction step
  assertions/TEMPLATE.md           — one record per established fact, with
                                     evidence, basis, status, dependencies
  snapshots/TEMPLATE.md            — a frozen, versioned TOML bundle of
                                     reviewed assertions for one topic
  coding-context-selection/TEMPLATE.md — a task-scoped packet assembled
                                     from one snapshot
  documentation-verification/TEMPLATE.md — checking published docs against
                                     reality, independently of their author
  implementation-comparison/TEMPLATE.md — comparing a snapshot against an
                                     independent, isolated reconstruction
  legacy-reconciliation/TEMPLATE.md — reconciling old narrative docs against
                                     a fresh evidence-backed draft
  propagation/TEMPLATE.md          — demonstrating that a source change
                                     actually reaches every dependent artifact

.gitignore           — keeps a *separate* tool's (CodeCompass CLI's)
                       generated state out of version control, if you pair
                       this scaffold with that tool: vendor/, context-graph.db,
                       its Claude skill/command files
vendor.toml          — that same optional tool's dependency-tracking config,
                       starts empty
LICENSE              — MIT
```

## Why the split between `decisions/` and `planning/`

`decisions/README.md` is explicit that a decision record is not a design
doc — it exists specifically to preserve the *reasoning* behind a
non-obvious tradeoff, append-only, so later readers see what was true
and believed at each point in time. `planning/` is the opposite in
character: `CONTEXT.md` is explicitly meant to be overwritten, not kept
as history — history belongs in git log and `planning/retros/` instead.
The two directories are deliberately asymmetric: one preserves a
permanent trail, the other reflects only the present.

## Why two separate optional add-ons rather than one

`optional-intermediate-knowledge/` and `optional-clean-room-workflow/`
sit at different weights. The clean-room workflow's own templates
(`assertions/`, `snapshots/`, `implementation-comparison/`, etc.)
describe a fairly heavy apparatus: cited evidence, frozen versioned
bundles, independent reconstruction, and a documented isolation
mechanism to keep that reconstruction honest. Nothing in the evidence
available for this template suggests the intermediate-knowledge layer
carries that same weight — see its own README for what's actually
grounded versus inferred from its name and position in the scaffold.
