# Worked example: one fact through the whole loop

A short, invented, deliberately trivial walkthrough. Use this to
pattern-match against, not to copy verbatim.

## 1. The canonical record

`planning/knowledge/retry-policy/CL-RETRY-001.yaml`:

```yaml
id: CL-RETRY-001
kind: claim
statement: >
  The HTTP client retries a failed request up to 3 times with
  exponential backoff, but only for 5xx responses -- a 4xx is never
  retried.
derivation: DE-RETRY-001
supporting_evidence: [EV-RETRY-001]
contradicting_evidence: []
derived_by: "direct code read"
repository_revision: "abc1234"
timestamp: "2026-01-01T00:00:00Z"
status: supported
supersedes: null
basis: observed_behaviour
```

(Plus its own `DE-RETRY-001.yaml` derivation and `EV-RETRY-001.yaml`
evidence record, each a similarly small flat file.)

## 2. `codecompass knowledge render retry-policy`

Produces `planning/knowledge/retry-policy/intermediate/overview.md`,
containing an anchored block:

```markdown
<!-- codecompass-knowledge: CL-RETRY-001 semantic-sha256:... projection-sha256:... -->
### CL-RETRY-001

The HTTP client retries a failed request up to 3 times with exponential
backoff, but only for 5xx responses -- a 4xx is never retried.

Supporting evidence: [EV-RETRY-001]
Status: status=supported
Provenance: OBSERVED
<!-- /codecompass-knowledge -->

## Candidate additions

<!-- codecompass-candidates:start -->
<!-- codecompass-candidates:end -->
```

## 3. An external tool edits the projection

Someone — a teammate, or an AI tool reviewing the code — adds, inside
the candidate region:

```
A 429 (rate limited) response is also never retried today, even though
it arguably should be -- worth a follow-up decision.
```

Plain prose like this — no `Type:` header — stays an **unclassified
Claim**. If instead the team wanted to assert this as a deliberate
policy going forward (not just an observation about current behaviour),
they would write:

```
Type: Intent
A 429 (rate limited) response should also never be retried, on the same
reasoning as a 4xx.
```

...which becomes a Claim with `basis: proposed_policy` once applied — the
`Type: Intent` header itself is stripped and never leaks into the
record's own `statement` field. And if the team already had an approved
`DEC-RETRY-002` authorising a specific, testable rule, they could instead
write a `Type: Requirement` block (`Decision:`/`Statement:`/`Example:`
lines) to get a real Requirement rather than a Claim — merely mentioning
`DEC-RETRY-002` in prose would not be enough on its own.

## 4. `codecompass knowledge select-candidates retry-policy`

Writes a manifest entry proposing a new Claim from that text, at
`status: proposed` — **not** an Observation, and not yet confirmed,
since no one has actually verified this against the real client code
through this mechanism yet. This step only ever reads; nothing canonical
changes yet, and running it again before anyone reviews the manifest just
reports the same pending candidate.

## 5. Review

A human (or an agent) checks the real code, confirms it's accurate, and
annotates the manifest item `accept`.

## 6. `codecompass knowledge apply <manifest>`

Writes a new `CL-RETRY-00N.yaml`, `status: proposed` — reconciliation
never auto-promotes a brand-new external claim straight to `verified`.
A follow-up Observation/Evidence pass (a human or an agent actually
checking the code and recording what they found) is what would move it
further along the same status lifecycle every other Claim already
follows.

## 7. `codecompass knowledge render retry-policy` again

The projection now shows both claims — nothing was lost, nothing was
silently trusted.

## Idempotency and concurrency, briefly

Running `apply` on this same manifest a second time is a safe no-op — it
recognises its own prior success and does nothing further. Running
`select-candidates` again after step 3 but before step 6 would keep
reporting the same pending candidate, not silently drop it or duplicate
it. And if someone had changed the candidate text (or deleted it) in the
live projection between steps 4 and 6, `apply` would refuse rather than
guess — a stale manifest is never quietly treated as either "already
done" or "fine to apply anyway."
