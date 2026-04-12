# Portotify Public Demo Script

This public demo is a static walkthrough of Portotify's governance model.

Portotify does not treat model output as decision authority. The governance boundary decides whether a governed decision is persisted as committed or rejected, and whether a separate human review workflow is required.

Use the artifacts in this order.

## 1. Low-risk allow path

Open `examples/execute_request.json` and the paired low-risk evidence.

Narrative:

- This request is ordinary and low risk.
- The governance boundary returns `allow`.
- The decision can be committed because the request does not trigger elevated controls.

What to emphasize:

- Low-risk work can proceed.
- Governance still records a decision artifact.
- The artifact is immutable and versioned.

## 2. Prompt or policy manipulation blocked path

Open `examples/injection_test.json` and `examples/injection_test_2.json`.

Narrative:

- These requests attempt to override policy or reveal hidden instructions.
- Portotify blocks them before they can become valid governed decisions.
- The system fails closed instead of trying to recover silently.

What to emphasize:

- Policy manipulation does not become a normal execution path.
- The blocked outcome is deterministic and auditable.
- The model is not the authority.

## 3. High-risk review-required path

Open `examples/governance_high_risk.json`, `evidence/03_governance_high_risk_execute.json`, and `data/boundary_high_result.json`.

Narrative:

- This request proposes a high-risk external financial action.
- The execution artifact is only a candidate summary.
- The governance boundary persists the decision as committed.
- Human review happens in a separate workflow.

What to emphasize:

- `review_required` is a governance verdict, not a lifecycle state.
- Persisted high-risk decision truth is `committed` plus `review_required`.
- Review does not introduce a separate persisted lifecycle state.

## 4. Critical fail-closed blocked path

Open `data/boundary_critical_result.json`.

Narrative:

- This request includes an external write condition without required controls.
- Portotify rejects it at the boundary.
- No commit occurs and no fallback "best effort" path is allowed.

What to emphasize:

- External write conditions must fail closed.
- Blocked decisions are rejected explicitly.
- Safety comes from deterministic boundary control, not post-hoc explanation.

## 5. Immutable decision artifact and lineage

Open `evidence/04_capsule_detail.json` and `evidence/06_review_flow_lineage.json`.

Narrative:

- Each decision artifact represents one immutable version.
- New information does not mutate an old decision.
- Review activity is tracked separately from persisted decision state.

What to emphasize:

- Decisions are immutable and versioned.
- Human review is a separate workflow.
- Review does not rewrite persisted decision state into `candidate` or `pending_review`.

## 6. Governance health and trust signals

Open `evidence/05_governance_health.json`, `data/demo_governance_signals.json`, and `data/demo_trust_signals.json`.

Narrative:

- These artifacts summarize the public demo model.
- They reinforce the immutable boundary rules without implying any live integration.

Close with:

Portotify does not promise perfect model output. It ensures that governed decisions are versioned, reviewable when necessary, and blocked fail-closed when required conditions are not met.
