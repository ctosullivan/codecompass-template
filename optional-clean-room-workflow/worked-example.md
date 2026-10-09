# Worked example: writing one assertion

A short, honest walkthrough of adopting the lightest piece of this
workflow — not the whole pipeline.

## The situation

Say you've established, by reading the code, that your payment client's
retry logic treats a specific error code as retryable when it shouldn't
be — a real, specific fact about the implementation, not a decision
you're making.

## What you actually do

1. Copy `assertions/TEMPLATE.md` to a new file with a stable id, e.g.
   `payment-retry-003.md`.
2. **Statement**: "Error code `E_TIMEOUT_UPSTREAM` is treated as
   retryable by the payment client's retry logic, even though it
   indicates a non-idempotent partial failure."
3. **Kind**: `observed_behaviour` — you watched the real system do this,
   as opposed to a rule someone stated.
4. **Basis**: `observed_behaviour`.
5. **Evidence**: the exact file and line range of the retry predicate,
   plus the specific test (or lack of one) that would have caught this.
   A citation that doesn't resolve to something real is a defect in the
   record, per the template — so this has to be checkable, not vague.
6. **Justification**: one or two sentences connecting that evidence to
   the claim.
7. **Counterexamples**: did you check for cases where this *doesn't*
   hold? If you looked and found none, say so explicitly — an empty list
   here means "checked," not "skipped."
8. **Evidence-support state**: `supported` — qualitative, not a
   percentage.
9. **Status**: `proposed` to start, or `supported` if you're confident
   in the evidence as written. Not `verified` — per the template's own
   note, that status is reserved for a later, separate, deliberate check
   against primary evidence, which this single record doesn't do by
   itself.

## Where it's honest to stop

That's a complete, useful artifact on its own. Freezing it into a
`snapshots/` bundle, building a `coding-context-selection/` packet from
it, running it through `implementation-comparison/` against an
independently-reconstructed version, or standing up the mechanical
isolation described in `mechanical-isolation.md` — all of that is real,
specified, and available in this workflow, but none of it is required
to get value from writing assertions like this one. Adopt further
pieces only when the verification they provide is actually worth their
real cost.
