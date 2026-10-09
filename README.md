# codecompass-template

A small, MIT-licensed scaffold of directories and templates for adopting
CodeCompass-style conventions in a project that is not CodeCompass
itself.

## What this is

CodeCompass-style discipline has a few recurring pieces: a written
record of non-obvious decisions (ADRs), a lightweight planning cadence
(current state, roadmap, retros, a running knowledge log), and —
optionally — a more structured way of capturing what you know about a
codebase and verifying your own documentation against reality.

This repository is **just the scaffold**: empty-but-structured
directories, README files explaining what belongs in each one, and
`TEMPLATE.md` files to copy. There is no CLI, no tooling, no generated
state of its own. Everything here is meant to be copied into a
downstream project and then filled in by hand (or by an agent working
on that project).

## Why use this instead of adopting CodeCompass's own conventions directly

Some evidence of the relationship between this template and CodeCompass
itself survives in the plumbing: `.gitignore` and `vendor.toml` both
reference a `codecompass` CLI, a `context-graph.db`, a `vendor/`
directory, and `.claude/skills` / `.claude/commands` integration — all
of which belong to CodeCompass as a running tool with its own sync
behaviour and governance. That's a heavier commitment than most projects
need just to get the *documentation discipline* benefit.

This template exists for the case where you want:

- ADRs and a planning cadence, without running CodeCompass's own tooling
  at all, or
- those things plus one or both optional add-on layers below, again
  without CodeCompass's own governance overhead,
- in a project that may or may not ever pair with the actual
  CodeCompass CLI (the `.gitignore` and `vendor.toml` entries are kept
  dormant-but-ready for that case, but nothing here requires it).

If you're already working inside CodeCompass itself, you don't need
this template — it's for everyone else.

## What's actually in here

- **`decisions/`** — one Architecture Decision Record (ADR) per
  significant, non-obvious tradeoff, numbered from `0001`, using
  `decisions/TEMPLATE.md` as the shape (Status, Context, Decision,
  Alternatives considered, Consequences). Append-only: a reversed
  decision gets a new, higher-numbered record, not an edit to the old
  one.
- **`planning/`** — a minimal planning scaffold: `CONTEXT.md` (current
  state only, overwritten not appended to), `ROADMAP.md` (a status
  table), `retros/TEMPLATE.md` (a short after-action record),
  `knowledge/README.md` (a candidate → reviewed → promoted/retained/
  discarded log of things learned), and `context-gaps/README.md` (a log
  of context that should have existed for a task and didn't).
- **`optional-intermediate-knowledge/`** *(optional)* — a lighter-weight
  knowledge layer, for projects that want more structure than
  `planning/knowledge/`'s informal log but don't want the full
  verification apparatus below. See that directory's own README for an
  honest note on how much of it is actually specified here.
- **`optional-clean-room-workflow/`** *(optional, heavier)* — a workflow
  for building a cited, evidence-backed knowledge base (`assertions/`,
  `snapshots/`), using it to write documentation and coding-context
  packets, and independently checking both the documentation
  (`documentation-verification/`) and the knowledge base itself
  (`implementation-comparison/`, `legacy-reconciliation/`,
  `propagation/`) against reality, with a documented isolation
  mechanism (`mechanical-isolation.md`) for keeping the independent
  check genuinely independent.

Both add-on directories are opt-in. Most projects adopting this
template will want `decisions/` and `planning/` and nothing else.

## License

MIT — see `LICENSE`.
