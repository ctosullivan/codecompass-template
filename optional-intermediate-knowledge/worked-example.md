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

## 4. `codecompass knowledge select-candidates retry-policy`

Writes a manifest entry proposing a new Claim from that text, at
`status: proposed` — **not** an Observation, and not yet confirmed,
since no one has actually verified this against the real client code
through this mechanism yet.

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
