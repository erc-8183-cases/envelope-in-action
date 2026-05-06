# The Envelope in Action: Co-Evaluator Composition in an ERC-8183 Job Lifecycle

_Draft v0.1 — May 2026_
_Fourth case, contributed by: AHM, ThoughtProof_

_Contributed under the case format proposed in "The Envelope in Action". This draft is intentionally scoped to Sections 4 and 5 of the planned joint write-up; Sections 1, 2, 3, 6, and 7 will be built around the clean cycle-4 joint job once the assignment lands._

## Section 4 — Why this is composition, not redundancy

The clean cycle-4 joint job is meant to show a different Envelope surface than the earlier cases. Case 1 showed action-time composition at an agent-to-MCP-server invocation. Case 2 showed wallet-bound aggregation. Case 3 showed pre-commit composition at transaction-composition time. This case exercises the ERC-8183 job lifecycle itself: two independent evaluators look at the same job from different surfaces and preserve different kinds of evidence.

At this surface, AHM and ThoughtProof are not duplicate judges. They answer different questions:

- **AHM evaluates the agent / behavioral / operational layer.** The relevant question is whether the participating agent looks healthy enough to trust in the job context: observed history, behavioral signals, operational posture, and confidence boundaries around sparse evidence.
- **ThoughtProof evaluates the reasoning / process layer.** The relevant question is whether the job deliverable, rationale, or trace is defensible: whether the reasoning preserves uncertainty, avoids overclaiming, and supports the action taken in the job lifecycle.

The ERC-8183 settlement layer remains intentionally binary. A job is completed or rejected. The Envelope case is not trying to turn ERC-8183 settlement into a multi-score dispute engine. Instead, it demonstrates how richer evaluator evidence can sit around that binary settlement path without overloading the protocol primitive.

That distinction matters because evaluator evidence can be insufficient without being negative. Job #3 already exposed the shape of this problem: zero-history or sparse-history signals may make a behavioral verdict low-confidence, but low-confidence is not the same as evidence of failure. A clean cycle-4 artefact should make this explicit rather than retrofitting it after the fact.

The composition point is therefore:

1. **Same job lifecycle, different evidentiary surfaces.** AHM can see behavioral and operational signals that ThoughtProof does not claim to see. ThoughtProof can see reasoning-process failures that AHM does not claim to score.
2. **Same settlement primitive, richer evaluator middleware.** ERC-8183 stays binary at completion/rejection. Confidence, routing, dissent, insufficiency, and audit narrative live in evaluator middleware and in the case artefact.
3. **No evaluator speaks for the other.** AHM's result does not become ThoughtProof's result; ThoughtProof's verdict does not become AHM's health score. Each issuer preserves its own vocabulary and confidence semantics.
4. **The consumer composes the evidence.** The job manager or relying party can treat either evaluator as a gate, require both, or use one as an escalation trigger. The Envelope makes those independent attestations legible without forcing a single blended score.

A monolithic evaluator score would blur exactly the information the job lifecycle needs. If AHM returns an insufficient-confidence behavioral assessment and ThoughtProof returns a reasoning-faithfulness allow, an average score hides the operational reality: the reasoning may be sound while the agent's sparse history still warrants caution. Conversely, if AHM sees a healthy agent but ThoughtProof sees unsupported reasoning, the behavioral score should not wash out the reasoning failure. Composition preserves the distinction.

## Section 5 — What this demonstrates about the Envelope as a primitive

This case demonstrates the Envelope as a coordination primitive for independent evaluator evidence inside an agentic-commerce job lifecycle.

The important property is not that two evaluators can both issue a verdict. That is trivial. The important property is that they can issue **different kinds** of verdicts, on different evidence surfaces, without collapsing into one another.

A clean cycle-4 co-produced artefact should demonstrate five invariants.

### 1. Evaluators can compose around one job without sharing an internal model

AHM does not need to adopt ThoughtProof's reasoning-verification schema. ThoughtProof does not need to adopt AHM's behavioral scoring model. Each evaluator signs or records evidence in its own native form. The shared object is the job lifecycle and the artefact boundary, not a shared scoring ontology.

### 2. Binary settlement does not preclude multi-dimensional evidence

ERC-8183 should not become responsible for every nuance of evaluator confidence. Settlement remains complete/reject. But the evidence around settlement can still carry richer semantics: confidence, insufficiency, dissent, reproducibility metadata, trace hashes, or receipts.

This is the clean separation:

- **Protocol layer:** binary lifecycle transition.
- **Evaluator layer:** structured evidence and confidence.
- **Case-library layer:** narrative explanation of how independent evidence composed.

### 3. Insufficient evidence remains first-class

A sparse behavioral history should not be silently converted into a negative behavioral claim. Likewise, a reasoning verifier should not invent requirements beyond the scope of the job just to avoid uncertainty. The case should preserve the distinction between:

- evidence of failure,
- absence of evidence,
- low-confidence sufficiency,
- and positive support.

This is where the AHM x ThoughtProof pairing is useful: it gives the case a concrete way to show that evaluator middleware can route uncertainty without pretending it is the same as rejection.

### 4. Reproducibility is optional, not the floor

The floor is reasoning verification: does the evaluator preserve uncertainty, avoid overclaiming, and take a defensible lifecycle action from the evidence it has?

A reproducibility layer can be added for stronger auditability cases: permanent artefacts, rerunnable traces, stable hashes, or receipts that let a third party replay or inspect the evaluation path. But that is an auditability upgrade, not a prerequisite for every ERC-8183 job.

The taxonomy for Section 7 should therefore be:

- **Reasoning verification as the floor.** Every evaluator should be able to explain why its lifecycle action follows from its evidence.
- **Reproducibility as an optional auditability layer.** Some jobs require stronger artefact permanence or replayability; others only need a defensible signed verdict and confidence boundary.

### 5. The Envelope case can document composition without changing ERC-8183

The clean joint job should not require a protocol amendment. It should show that ERC-8183 already has enough surface for evaluator composition, provided the issuers are clear about what their attestations mean.

The case-library contribution should therefore avoid claiming that ERC-8183 needs new semantics for confidence or multi-score aggregation. The better conclusion is narrower and stronger: the existing binary lifecycle can carry richer evaluator evidence around it, and the Envelope makes that evidence independently interpretable.

## Section 3 placeholder — clean cycle-4 artefact

_To be filled after the new joint job lands._

Preferred shape:

1. A new ERC-8183 job is created on the current contracts.
2. AHM and ThoughtProof agree on paired test deliverables or a naturally fitting Bakugo32 test job.
3. AHM evaluates the agent / behavioral / operational layer.
4. ThoughtProof evaluates the reasoning / process layer.
5. The case records both outputs and shows how they informed the binary job lifecycle without collapsing the two evidence surfaces.

If Bakugo32's next test job naturally fits this structure, it can become the Section 3 artefact. Otherwise, the cleaner path is bespoke paired test deliverables that AHM and ThoughtProof evaluate for each other.
