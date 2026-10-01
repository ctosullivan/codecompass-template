# Worked example: one small change through the everyday loop

A short, concrete walkthrough of `CLAUDE.md`'s six conventions against a
trivial, invented change — adding a `--verbose` flag to a CLI tool — to
show what each step actually produces, not a complete specification.

## 1. Plan before implementing

A few lines, not a document: "Add `--verbose` to `mytool run`. Scope:
just the flag and one extra log line per processed item; not a general
logging-framework change. Done when: flag exists, tested, documented."

## 2. Keep documentation in sync, same change

`docs/architecture.md`'s CLI-surface description (if it has one) gets
the new flag added in the same commit as the code change — not a
follow-up.

## 3. Record real decisions

This change is too small for a decision record — it's not a tradeoff,
just an addition. A genuine tradeoff (e.g. "log to stderr, not stdout,
so `--verbose` output doesn't pollute piped output") would get a short
file under `decisions/`, one per decision, using `decisions/TEMPLATE.md`'s
shape.

## 4. Keep a running context file

`planning/CONTEXT.md`'s current-state section gets overwritten (not
appended to): "Added `--verbose` to `mytool run` (`a1b2c3d`). Next: none
pending."

## 5. Close the loop

A few lines in `planning/retros/`: "Goal: add `--verbose`. What happened:
straightforward, done in one pass. Nothing to add to the knowledge log
this time" — closing the loop doesn't require finding something
noteworthy; most small changes won't produce a real learnings-log entry,
and that's fine.

If something *had* gone wrong in a way worth remembering — say, the
first attempt broke a test that assumed `mytool run`'s output was always
exactly one line — that becomes a candidate entry in
`planning/knowledge/` per its own `README.md`: what happened, the
evidence, why it's worth keeping.

## 6. Commits

One commit: `feat: add --verbose flag to mytool run`, covering the code
change, its test, and the `docs/architecture.md` update together — not
split across several commits, and not bundled with an unrelated change.
