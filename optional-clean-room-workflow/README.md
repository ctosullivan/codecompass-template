# Optional: clean-room documentation workflow

A heavier, opt-in add-on for projects that want their documentation and
internal knowledge base to be **evidence-backed and independently
checked**, rather than trusted on the author's word.

## The pieces, and how they fit together

1. **`assertions/`** — one record per established fact about your
   project's subject matter (a definition, a rule, an invariant, a
   boundary), each with a required evidence citation, a `basis`
   classification (directly stated / inferred / proposed policy /
   observed behaviour), and a status lifecycle that distinguishes
   `supported` from the much higher bar of `verified`.
2. **`snapshots/`** — once a set of assertions is reviewed, freeze them
   into a versioned, hash-checked TOML bundle for one topic. Everything
   downstream cites the frozen snapshot, not the live, still-changing
   assertions — so "the doc said X, and X was true when this was
   written" stays a checkable claim even after the knowledge base moves
   on.
3. **`coding-context-selection/`** — a task-scoped packet assembled from
   one snapshot, deliberately narrow: only the assertions a specific
   bounded coding task actually needs, with what was left out recorded
   explicitly.
4. **`documentation-verification/`** — a published doc gets checked two
   separate ways: a reader with no other access answers real questions
   from the doc alone, and those answers get checked against the real
   system; separately, if the doc is meant to help with coding, it gets
   checked for whether it actually gives a coding-task advantage.
5. **`implementation-comparison/`** — a frozen snapshot is compared,
   assertion by assertion, against an **independent** reconstruction of
   the same topic built from the implementation alone — see
   `mechanical-isolation.md` for how that reconstruction has to be kept
   genuinely isolated from the snapshot for this comparison to mean
   anything. Critically: agreement alone never promotes an assertion to
   `verified` — that needs a separate, assertion-specific check.
6. **`legacy-reconciliation/`** — once a fresh draft exists from a
   snapshot, old pre-existing narrative docs get reconciled against it
   claim by claim, as a genuinely separate later step, not folded into
   the first draft.
7. **`propagation/`** — a disposable-fixture demonstration that a change
   to a real *source* actually gets discovered and propagates all the
   way through evidence → assertions → dependents → snapshots → both
   documentation and coding-context packets, including correct handling
   of a dependency cycle.

## Why "clean room"

The comparison in step 5 only means something if the reconstruction
being compared against wasn't written with access to the thing it's
being compared against — otherwise you're just checking a document
against itself. `mechanical-isolation.md` describes the real, verified
mechanism for enforcing that separation, and its one real limitation.

## Adopting this incrementally

Nothing requires adopting all seven pieces at once. `assertions/` alone,
with evidence citations, is already more rigor than most projects have.
The independent-reconstruction and mechanical-isolation machinery is
the heaviest part of this workflow and is reasonable to defer — or skip
— for a project that doesn't need that level of verification.
