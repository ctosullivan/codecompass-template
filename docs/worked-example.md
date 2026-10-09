# Worked example: filing your first ADR

A short, honest walkthrough of adopting one piece of this scaffold —
not a tutorial for the whole template.

## The situation

Say your project needs to pick a serialization format for an internal
cache, and you've gone with something less obvious than the default
(maybe a binary format instead of JSON, for a reason specific to your
constraints). `decisions/README.md`'s own test applies: could someone
later look at the code and reasonably ask "why not the obvious thing?"
Here, yes — so it's worth a record.

## What you actually do

1. Copy `decisions/TEMPLATE.md` to `decisions/0001-cache-serialization-format.md`.
   Numbering starts at `0001`; the filename names the actual decision,
   not a generic label like "decision 1."
2. Fill in **Status**: `Accepted (2026-10-09).`
3. Fill in **Context**: the real constraint that forced a choice — e.g.
   the cache has to survive a process restart, JSON's parse cost showed
   up in profiling, and you're already committed to a particular binary
   library elsewhere in the project.
4. Fill in **Decision**: name the actual mechanism — "use MessagePack via
   library X," not "use an efficient binary format."
5. Fill in **Alternatives considered**: JSON (ruled out: parse cost),
   Protocol Buffers (ruled out: schema-compiler dependency felt like
   overkill for one internal cache). This section is where the record
   earns its keep later.
6. Fill in **Consequences**: what this commits you to (a dependency on
   library X, a migration cost if you ever reverse it), stated plainly.
7. Commit it alongside the actual code change it justifies, not as a
   separate housekeeping pass.

## What happens later, if this decision is reversed

Per `decisions/README.md`'s append-only rule: you don't edit `0001`.
You write `0007` (or whatever the next number is), and `0007` says
explicitly that it supersedes `0001`. A short status note gets added to
`0001` itself ("superseded by `decisions/0007`") — the rest of `0001`'s
original content stays exactly as written, now readable as history
rather than current truth.

That's the whole mechanism. There's no tooling enforcing any of this —
it's a convention, held up by the template's own README and by whoever
is doing the writing.
