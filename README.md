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
this repository is copied from CodeCompass's own text. The *shape* of
the working conventions it packages (plan before you code, keep docs in
sync, a running context file, a lightweight learnings log) reflects
general, widely-used development practice, freely reusable regardless of
what license governs the tool that happens to consume `vendor.toml`. A
project using this template may itself be licensed however its own
owner chooses — GPL, a permissive license, or kept entirely proprietary
— independent of both CodeCompass's license and this template's own.
This note describes *this template repository itself* — it is
deliberately not duplicated into `docs/architecture.md`, since that file
is copied into your own project and should describe only your project,
never this template.

## What's here

- `CLAUDE.md` — minimal working conventions: plan before you code, keep
  docs in sync with the change that needs them, write a short retro when
  something finishes.
- `vendor.toml` — CodeCompass's own dependency-tracking config. Starts
  empty; CodeCompass can populate it for you (see below).
- `docs/architecture.md` — a skeleton for describing your own project's
  current architecture, to be filled in as the project takes shape.
- `docs/worked-example.md` — a short, concrete walkthrough of
  `CLAUDE.md`'s six conventions against one trivial invented change, for
  pattern-matching against before you try the real thing.
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
- `optional-clean-room-workflow/` — **not part of the everyday adoption
  path; do not copy this directory unless you've read its own `README.md`
  and decided you actually need it.** A heavier, optional workflow for
  building evidence-backed conceptual understanding of your own project
  independently of its existing narrative documentation, freezing it
  into a versioned snapshot, and checking it against both an independent
  implementation review and real documentation/coding-context
  usefulness, including a short worked example. Most projects won't need
  this until documentation drift or onboarding cost becomes a real,
  recurring problem — and unlike everything else in this list, it lives
  entirely outside `planning/` and `docs/` specifically so adopting the
  everyday path never drags it in by accident.

## Adopting this template

1. **Starting a new project**: use this repository as a GitHub template,
   then **replace this file's own content** with a description of your
   actual project — a new repository seeded from this template inherits
   this very `README.md` by construction, and it describes the template,
   not your project (fold in whatever parts of "What's here" are useful
   to your own readers; don't leave this text in place). **Adding to an
   existing project**: copy in whatever you want from "What's here"
   above. Either way, **three files need care, not a blind copy**:
   - `README.md` — never copy this file's own content over an existing
     project's README; write your own (see above).
   - `LICENSE` — a real per-project choice this template won't make for
     you. Only copy it if you actually intend your project to be
     MIT-licensed.
   - `CLAUDE.md` — if your project already has one with real rules in
     it, don't overwrite it either. Keep your existing rules (verbatim,
     under their own heading), append this template's conventions below
     them, and only call out an explicit reconciliation where the two
     genuinely conflict (most of the time they won't — "write a short
     plan first" and "squash-merge your PRs," for instance, aren't in
     tension and don't need one). A brand-new project with no existing
     `CLAUDE.md` can simply copy this template's version as-is.
2. Install CodeCompass (`pip install codecompass-context`, or however
   your own project's ecosystem prefers) and run it once — bare
   `codecompass` will discover your project's actual dependencies (from
   `package.json`, `pyproject.toml`, `Cargo.toml`, `package.yaml`, etc.)
   and offer to populate `vendor.toml` for you. **If your project has no
   dependency manifest yet** (a very early-stage project, or one with no
   third-party dependencies), there's simply nothing to discover yet —
   that's fine, not an error; this step becomes useful once you have
   something to track. **If you don't have network access** to install
   or run CodeCompass right now, leave `vendor.toml` empty exactly as
   shipped and note the commands above as a next step in
   `planning/CONTEXT.md` rather than guessing at what they'd produce.
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
