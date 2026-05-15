# The Envelope in Action: Co-Evaluator Composition in an ERC-8183 Job Lifecycle

_Draft v0.2 — May 2026_
_Fourth case, contributed by: AHM, ThoughtProof_

_Contributed under the case format proposed in "The Envelope in Action". v0.2 adds Section 3 (the clean cycle-4 joint artefact from ASP Job #4), completing the core evidentiary record. Sections 1, 2, 6, and 7 remain forthcoming._

## Section 4 — Why this is composition, not redundancy

The clean cycle-4 joint job is meant to show a different Envelope surface than the earlier cases. Case 1 showed action-time composition at an agent-to-MCP-server invocation. Case 2 showed wallet-bound aggregation. Case 3 showed pre-commit composition at transaction-composition time. This case exercises the ERC-8183 job lifecycle itself: two independent evaluators look at the same job from different surfaces and preserve different kinds of evidence.

At this surface, AHM and ThoughtProof are not duplicate judges. They answer different questions:

- **AHM evaluates the agent / behavioral / operational layer.** The relevant question is whether the participating agent looks healthy enough to trust in the job context: observed history, behavioral signals, operational posture, and confidence boundaries around sparse evidence.
- **ThoughtProof evaluates the reasoning / process layer.** ThoughtProof operates two distinct verification products that compose into a single evaluator position:
  - **PoT/RV (Proof of Thought / Reasoning Verification):** A binary faithfulness check against a submitted claim, rationale, and evidence record. The question is whether the reasoning preserves uncertainty, avoids overclaiming, and supports the action taken — without requiring a structured trace or plan decomposition. This is the floor: every evaluator call produces at minimum a defensible reasoning verdict.
  - **PLV (Plan-Level Verification):** A structured reproducibility layer that decomposes the agent's task into gold plan steps with criticality weightings, evaluates per-step trace evidence, enforces source-verification rules, and produces tiered confidence bands. PLV adds auditability and replayability on top of reasoning verification for cases that require stronger evidence trails.

  In the context of an ERC-8183 job, ThoughtProof's evaluator middleware routes to PoT/RV or PLV based on the available evidence surface: if the job provides only a claim and rationale, PoT/RV produces the verdict; if the job provides a structured trace with tool calls and plan steps, PLV produces a richer assessment with per-step accountability. Both collapse into the same binary settlement action (complete/reject).

The ERC-8183 settlement layer remains intentionally binary. A job is completed or rejected. The Envelope case is not trying to turn ERC-8183 settlement into a multi-score dispute engine. Instead, it demonstrates how richer evaluator evidence can sit around that binary settlement path without overloading the protocol primitive.

That distinction matters because evaluator evidence can be insufficient without being negative. Job #3 already exposed the shape of this problem: zero-history or sparse-history signals may make a behavioral verdict low-confidence, but low-confidence is not the same as evidence of failure. A clean cycle-4 artefact should make this explicit rather than retrofitting it after the fact.

The composition point is therefore:

1. **Same job lifecycle, different evidentiary surfaces.** AHM can see behavioral and operational signals that ThoughtProof does not claim to see. ThoughtProof can see reasoning-process failures that AHM does not claim to score.
2. **Same settlement primitive, richer evaluator middleware.** ERC-8183 stays binary at completion/rejection. Confidence, routing, dissent, insufficiency, and audit narrative live in evaluator middleware and in the case artefact.
3. **No evaluator speaks for the other.** AHM's result does not become ThoughtProof's result; ThoughtProof's verdict does not become AHM's health score. Each issuer preserves its own vocabulary and confidence semantics.
4. **The consumer composes the evidence.** The job manager or relying party can treat either evaluator as a gate, require both, or use one as an escalation trigger. The Envelope makes those independent attestations legible without forcing a single blended score.

A monolithic evaluator score would blur exactly the information the job lifecycle needs. If AHM returns an insufficient-confidence behavioral assessment and ThoughtProof returns a reasoning-verification allow (whether via PoT/RV binary check or PLV plan-step analysis), an average score hides the operational reality: the reasoning may be sound while the agent's sparse history still warrants caution. Conversely, if AHM sees a healthy agent but ThoughtProof sees unsupported reasoning, the behavioral score should not wash out the reasoning failure. Composition preserves the distinction.

## Section 5 — What this demonstrates about the Envelope as a primitive

This case demonstrates the Envelope as a coordination primitive for independent evaluator evidence inside an agentic-commerce job lifecycle.

The important property is not that two evaluators can both issue a verdict. That is trivial. The important property is that they can issue **different kinds** of verdicts, on different evidence surfaces, without collapsing into one another.

A clean cycle-4 co-produced artefact should demonstrate five invariants.

### 1. Evaluators can compose around one job without sharing an internal model

AHM does not need to adopt ThoughtProof's verification schemas (neither the PoT/RV verdict vocabulary nor the PLV plan-step decomposition). ThoughtProof does not need to adopt AHM's behavioral scoring model. Each evaluator signs or records evidence in its own native form. The shared object is the job lifecycle and the artefact boundary, not a shared scoring ontology.

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

- **PoT/RV (Reasoning Verification) as the floor.** Every evaluator should be able to explain why its lifecycle action follows from its evidence. In ThoughtProof's implementation, this is Proof of Thought: a binary faithfulness check (ALLOW / UNCERTAIN / BLOCK) against a claim, rationale, and evidence record. It requires no structured trace decomposition and produces a defensible verdict at minimal cost and latency.
- **PLV (Plan-Level Verification) as the reproducibility / auditability layer.** Some jobs require stronger evidence trails: per-step trace accountability, source-verification rules, criticality-weighted plan decomposition, and tiered confidence bands. PLV adds this structured layer on top of reasoning verification. It is rerunnable, inspectable, and produces artefacts that a third party can replay. But it requires richer input (a structured trace with tool calls and plan steps) and is not required for every ERC-8183 job.

The composition point: PoT/RV and PLV are not competing approaches. They are layers in the same evaluator stack. A job with minimal evidence gets a defensible reasoning verdict (PoT/RV). A job with a rich structured trace gets full plan-level accountability (PLV). Both produce a binary settlement action.

### 5. The Envelope case can document composition without changing ERC-8183

The clean joint job should not require a protocol amendment. It should show that ERC-8183 already has enough surface for evaluator composition, provided the issuers are clear about what their attestations mean.

The case-library contribution should therefore avoid claiming that ERC-8183 needs new semantics for confidence or multi-score aggregation. The better conclusion is narrower and stronger: the existing binary lifecycle can carry richer evaluator evidence around it, and the Envelope makes that evidence independently interpretable.

## Section 3 — The clean cycle-4 artefact: ASP Job #4

### Job setup

| Field | Value |
|-------|-------|
| Contract | AgentJobManager `0xC07CE789206CBEEC3A41D5CedBdA93B1024aaDdd` |
| Chain | Base Sepolia (84532) |
| Job ID | 4 |
| Creator | Bakugo32 / Demsys (deployer wallet) |
| Provider | AHM — `0x35eeDdcbE5E1AE01396Cb93Fc8606cE4C713d7BC` |
| Evaluator | ThoughtProof — `0x118B1E5A47658D20046bC874cB34E469d472c0C2` |

### Provider deliverable (AHM)

AHM submitted an Agent Health Score attestation for ACP #2624 ("Jeff CEO"), wallet `0x92ebf2f83e9981f2ee08187794e12dabfde953e9`.

**Submit TX:** `0x392ef97c1d9bed0318cf512f9300d710752dfb2b041ef4c641cda872200a4331`

**Deliverable content:** [ahm-job4-deliverable.json](https://gist.githubusercontent.com/moonshot-cyber/7418ac58eb26eaf2fdca932462ea3c88/raw/57a25319c7360fd953fde3e8aa15daefe24dd51c/ahm-job4-deliverable.json)

Summary of AHM's assessment:

- **AHS:** 57 / Grade D / HIGH confidence
- **Dimensions:** D1 (transaction-pattern) = 70, D2 (behavioural-diversity) = 51, D3 (infrastructure) = null (2D mode)
- **Observation window:** 9 scans across 49 days, 146 transactions
- **Temporal pattern:** 8 of 9 observations at 57/D, one transient C-grade spike (64/C) on 2026-04-15 driven by a D2 dimensional burst that subsequently reverted
- **Reasoning chain included:** claim, behavioural signals, confidence basis, grade justification

The deliverable contains AHM's full reasoning chain — not just a score, but the evidentiary path from wallet signals to composite grade to confidence assignment. This is the substrate that PoT/RV evaluates.

### Evaluator verification (ThoughtProof — PoT/RV)

ThoughtProof evaluated via **PoT/RV (Proof of Thought / Reasoning Verification)** — the binary faithfulness check product. PLV was not applicable: AHM's deliverable provides claim + rationale + evidence, not a structured agent trace with tool calls and plan steps. PoT/RV is the correct product for this evidence surface.

**Pipeline:** 4 generator perspectives (Technical, Epistemology, Contrarian, Statistical) → adversarial red-team → synthesis by orchestrator.

**Key verification findings:**

1. **Arithmetic:** Weight renormalization (D3 excluded, D1+D2 renormalized to sum 1.0) produces 0.375×70 + 0.625×51 = 58.125. EMA temporal smoothing (α=0.6) across the observation sequence converges to 57. The arithmetic is defensible.
2. **Scope:** AHM claims wallet health, not agent reasoning quality. No overclaim beyond stated methodology.
3. **Evidence chain:** Complete — claim → dimensional scores → behavioural signals → confidence basis → grade justification. Each step follows from the prior.
4. **Surviving dissent:** HIGH confidence at n=9 observations is aggressive by general epistemological standards. The red-team assessed this as a calibration question internal to AHM's published methodology, not a defect in the reasoning chain.

**Verdict:** ALLOW (confidence 0.72)

**Settlement TX:** `0x4ab25466f2e790bd134ca68dd5c1a483b3e81171ed6066e9f45f3f50983a4c88` (block 41529925)

**Settlement action:** `complete(4, 0xa998c81e150e02d9eb94a62109a73b6be8a9ad67eb3b31d195e702fdedd93716)`

**Full epistemic block:** [ar://-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY](https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY) — permanently archived, hash-linked on-chain as the reason parameter.

### What the joint artefact shows

The binary settlement (`complete()`) is the protocol-layer outcome. But the evidentiary record around that binary includes:

- **AHM side:** a full scoring attestation with dimensional breakdown, temporal pattern analysis, confidence calibration, and methodology reference — all in a self-contained JSON deliverable.
- **ThoughtProof side:** a multi-perspective epistemic verification block with generator proposals, adversarial critique, mathematical validation, and synthesized verdict — permanently archived and hash-linked to the settlement transaction.

Neither evaluator adopted the other's vocabulary. AHM speaks in AHS grades and dimensional scores. ThoughtProof speaks in ALLOW/UNCERTAIN/BLOCK verdicts and epistemic confidence. The protocol composed them through the binary lifecycle without requiring a shared scoring ontology.
