# codecompass-template

A minimal, MIT-licensed starting point for a project that wants to
adopt [CodeCompass](https://github.com/ctosullivan/codecompass) for
dependency and first-party source context, plus a lightweight,
documentation-first way of working — without inheriting CodeCompass's
own much larger internal governance history.

**License note:** this template is MIT-licensed, independently authored,
and designed to be adopted by projects under any license — GPL,
permissive, or proprietary/internal. It is maintained alongside
CodeCompass (which is itself GPL-3.0-or-later) but is not a
redistribution of CodeCompass's own source or documentation; nothing in
this repository is copied from CodeCompass's own text. See
[`docs/architecture.md`](docs/architecture.md) for more on how the two
repositories relate.

## What's here

- `CLAUDE.md` — minimal working conventions: plan before you code, keep
  docs in sync with the change that needs them, write a short retro when
  something finishes.
- `vendor.toml` — CodeCompass's own dependency-tracking config. Starts
  empty; CodeCompass can populate it for you (see below).
- `docs/architecture.md` — a skeleton for describing your own project's
  current architecture, to be filled in as the project takes shape.
- `decisions/` — a place for append-only architecture decision records
  (ADRs), one file per significant decision, numbered sequentially. See
  `decisions/README.md`.
- `planning/` — the reusable working-state layer:
  - `ROADMAP.md` — an at-a-glance table of what's done, in progress, or
    planned.
  - `CONTEXT.md` — a single running file describing where things stand
    right now, overwritten (not appended to) at each stopping point.
  - `retros/` — a short write-up template for closing out a piece of
    work: what was the goal, what actually happened, what you'd do
    differently.
  - `knowledge/` — a lightweight "things we learned" log: a candidate
    observation, reviewed, then either promoted into a permanent
    artifact (a test, a doc, a rule) or set aside.
  - `context-gaps/` — a log of places where you notice CodeCompass's (or
    any tool's) context is missing or wrong, kept separate from "how we
    work" observations so the two don't get mixed together.
  - `knowledge/assertions/`, `knowledge/snapshots/`,
    `knowledge/coding-context-selection/`, `knowledge/implementation-comparison/`,
    `knowledge/propagation/`, `knowledge/legacy-reconciliation/`,
    `knowledge/documentation-verification/` — templates for a heavier-
    weight, optional workflow: building evidence-backed conceptual
    understanding of your own project independently of its existing
    narrative documentation, freezing it into a versioned snapshot, and
    checking it against both an independent implementation review and
    real documentation/coding-context usefulness. See
    `docs/conceptual-documentation-guide.md` and
    `docs/mechanical-isolation.md` for how the pieces fit together, and
    when the isolation this workflow relies on is genuinely achievable
    versus best-effort. Most projects won't need this until documentation
    drift or onboarding cost becomes a real, recurring problem.

## Adopting this template

1. Use this repository as a template for your own new project (or copy
   its contents into an existing one). **If you're copying into an
   existing project, don't blindly overwrite two files**: this
   repository's own `README.md` describes the *template*, not your
   project — copying it over your project's existing `README.md` would
   replace your project's own identity with a description of this
   template instead (fold in whatever parts of "What's here" are useful
   to your own readers, don't copy the file verbatim); and `LICENSE` is a
   real per-project choice this template won't make for you — only copy
   it if you actually intend your project to be MIT-licensed.
2. Install CodeCompass (`pip install codecompass-context`, or however
   your own project's ecosystem prefers) and run it once — `codecompass`
   with no arguments will discover your project's actual dependencies
   and offer to populate `vendor.toml` for you.
3. Run `codecompass sync` whenever your dependencies or first-party
   source change. This produces a local `context-graph.db` and, for each
   tracked dependency, a `vendor/<name>/` reference digest — neither is
   committed (see `.gitignore`); both are regenerated from your project's
   own current state, not hand-maintained.
4. Use `codecompass query source <path>` / `query source-symbol <name>`
   to explore your own project's source, and `codecompass query vendor
   <name>` / `query symbol <name>` to explore a tracked dependency's —
   both work whether or not you've tracked any dependencies yet.
5. Adapt `CLAUDE.md`, `docs/architecture.md`, and the `planning/`
   skeleton to your own project as it grows. Nothing here is meant to be
   followed rigidly — it's a starting shape, not a mandate.

## The reusable workflow shape

This template packages one specific, low-ceremony way of working, not a
prescribed toolchain or agent roster:

```
understand
  → research / evidence
  → plan
  → design
  → implement
  → verify
  → retro
  → update knowledge
```

At small scale, "understand → plan → implement → verify" is often
enough; the fuller shape becomes useful once a project has enough
history that documenting *why* a decision was made saves more time than
it costs. `planning/knowledge/README.md` and `planning/context-gaps/README.md`
describe two small, specific habits (a learnings log, a context-gaps
log) that make the "update knowledge" step concrete rather than aspirational.

## What this template deliberately does not include

- A specialist multi-agent roster, phase-numbering scheme, or
  milestone-gate machinery — CodeCompass's own internal development
  process is shaped by its own history and scale; this template gives
  you the underlying habits, not that specific apparatus.
- Any of CodeCompass's own accumulated learnings, decisions, or project
  history — `planning/knowledge/`, `planning/context-gaps/`, and
  `decisions/` all start genuinely empty here.
- Any generated CodeCompass artifact (`context-graph.db`, `vendor/`,
  generated Skills or slash commands). These are reproducible outputs of
  running CodeCompass against your own project, not something a template
  should ship — see `.gitignore`.
