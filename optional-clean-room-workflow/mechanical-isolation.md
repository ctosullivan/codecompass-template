# Mechanical isolation

This page replaces an older narrative version of itself. That older
page is reported to have committed to adding "a short pointer to a
working mechanical-isolation approach, once one exists." One now does —
verified, not merely designed, in a real, independent project
(CodeCompass itself, Phase 81B Amendment 4). This page describes it
honestly, including its one real limitation, rather than presenting it
as simpler than it is.

**Scope note:** this template does not ship the scripts or broker
process described below. This page documents a verified *approach*,
for a project that decides it needs `implementation-comparison/`'s
independent-reconstruction step to be genuinely independent. Building
or adapting the actual mechanism is work the adopting project does
itself.

## Why this exists

`implementation-comparison/TEMPLATE.md` requires the reconstruction it
compares against to be produced "model-blind, no access to this
snapshot at the time it was written." That requirement is only real if
there's an actual, verified way to enforce it — not just an instruction
to the writer to not peek.

## What was actually verified

A Linux namespace mechanism — `unshare --user --map-root-user --mount
--net --pid --fork` combined with `pivot_root` into a freshly-built
root containing only explicitly allow-listed content — was built and
**actively, adversarially tested** against a real project, not merely
described. Direct attempts confirmed it closed every one of these
routes:

- parent-path traversal to the original repository or any sibling
  checkout,
- the repository's own `.git` history, branches, or remotes,
- public network access,
- credentials on disk and in the environment — including a real leak
  that was found and fixed along the way: an inherited session token
  reached a sandboxed subprocess's environment unless that environment
  was rebuilt from an empty base, not merely filtered,
- MCP/connector configuration,
- the orchestrating agent's own prior conversation or context —
  confirmed with a canary test: a secret phrase mentioned only in the
  orchestrator's own live session was unreachable from inside the
  sandbox.

## The part that's genuinely harder: two distinct halves, not one recipe

It is tempting to describe this as "run `unshare`, you're done." That
would be dishonest. There are two separable problems here, solved by
two separable mechanisms:

**Half one — filesystem and process isolation.** Simple and
self-contained: the namespace + `pivot_root` mechanism above. This part
really is close to "build an allow-listed root, unshare into it, go."

**Half two — giving an AI writer model access without giving it
reusable credentials.** Isolating what a *human or deterministic*
writer can see is the problem half one solves. Giving an **AI** writer
the ability to actually call a model from inside that sandbox, without
handing it a credential that could be exfiltrated or reused outside the
sandbox's intended scope, is a separate and harder problem — and it is
not solved by the namespace mechanism at all.

CodeCompass's own solution to half two is a narrowly-scoped **broker
process** that stays *outside* the sandbox, holds the real credentials,
and exposes only a fixed, narrow inference capability across a local
Unix-socket IPC channel: one request in, one response out, with no file
paths, no shell commands, and no tool access anywhere in the protocol.
The sandbox can't use that channel for anything else.

## The one real, named limitation

**Do not describe this as a single simple mechanism.** Isolating the
*evidence a writer can see* is a solved problem — half one, above.
Isolating *model access for an AI writer* without reusable credentials
required building an entirely additional component — the broker — to
solve. A project adopting this approach for a deterministic or
human-only reconstruction step only needs half one. A project that
wants an AI agent to do the reconstruction itself needs both halves,
and the broker is real added complexity, not a footnote.
