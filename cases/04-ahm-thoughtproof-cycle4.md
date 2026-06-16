# The Envelope in Action: Co-Evaluator Composition in an ERC-8183 Job Lifecycle

_Draft v0.3 — May 2026_
_Fourth case, contributed by: AHM, ThoughtProof_

_Contributed under the case format proposed in "The Envelope in Action". v0.3 adds Sections 1, 2, 3, 6, and 7, completing the case structure with the paired-jobs worked example (Jobs #5 and #4 on cycle 4) and the envelope-invariants application. Section 3 has been restructured around the paired-jobs framing endorsed in coordination. Sections 4 and 5 unchanged._

## Section 1 — Why this case exists

"The Envelope in Action" (ThoughtProof, April 2026) proposes a four-section format for supplementary case documentation. Three earlier cases have exercised the format on different topologies: Case 1 on agent-to-MCP-server invocations where the two dimensions cover orthogonal threat models (RNWY × ThoughtProof, server-side quality and reasoning integrity); Case 2 on wallet-bound aggregation at SkyeMeta where all dimensions are consulted in a single call and the relying party chooses thresholds at consumption time; Case 3 on transaction-composition time where pre-commit wallet-state and reasoning integrity gate the same action from different sides (InsumerAPI × ThoughtProof).

This document contributes a fourth case: composed-evaluator settlement under ERC-8183, where two independent evaluators with distinct methodologies attest against the same job lifecycle. The composition surface here is neither action-time (cases 1 and 3) nor pre-action aggregated (case 2) — it is at *settlement time*, where the protocol's binary verdict at job completion is backed by two independent evaluator attestations covering complementary dimensions of agent quality.

This case is co-authored to demonstrate that composition at settlement time preserves binary protocol semantics while permitting richer evaluator-layer signal at the consumption boundary. ThoughtProof's reasoning-verification products (PoT/RV and PLV) and AHM's behavioural-diagnostics scoring are independently issued, independently verifiable, and complementary in failure-mode coverage — properties consistent with the four Envelope invariants established across the prior cases.

## Section 2 — The surface or decision class

ERC-8183 ("Agentic Commerce") specifies a binary settlement protocol for agent-to-agent job lifecycles on-chain. A client funds a job, a provider submits a deliverable, an assigned evaluator attests `complete` or `reject`, and the protocol settles funds: provider receives the budget minus protocol fee on `complete`, client receives a refund minus fee on `reject`, and the fee is distributed between the evaluator and treasury under a published share ratio (currently 80/20 in favour of the evaluator, on both verdict paths, to prevent perverse incentives toward always-complete).

The settlement primitive is intentionally binary. The protocol does not aggregate multi-evaluator scores, does not encode confidence enums, and does not specify methodology requirements. These are evaluator-layer concerns. The protocol's single requirement at this surface is: one assigned evaluator, one binary verdict, one settlement.

The composition opportunity emerges in the *evaluator layer*. Two evaluators operating against the same protocol surface can:

- Attest in parallel against the same deliverable (composition by sampling).
- Attest in different role pairs against semantically related deliverables (composition by lifecycle pairing).
- Route between methodologies based on the deliverable's evidence surface (composition by evidence-driven routing).

This case documents the second of these: two evaluators in mirrored role pairs across a deliberately asymmetric job pair. The asymmetry — different subject types, different evaluator products applied — is what allows the methodological independence of each side to be demonstrated.

At this surface, two dimensions meet with shared protocol mechanics but independent attestation surfaces:

- **Behavioural diagnostics (AHM).** Produced over an agent or wallet subject using infrastructure-health behavioural scoring (D-dimensional analysis, confidence enum, observation density). Verdict is grounded in observed on-chain behaviour and infrastructure signals, not in the reasoning of any single deliverable.

- **Reasoning verification (ThoughtProof).** Produced over a claim + rationale + evidence chain using PoT/RV (binary faithfulness check, the floor) or PLV (structured reproducibility, the auditability layer). The routing between PoT/RV and PLV depends on the available evidence surface of the deliverable.

The composition is settlement-time: both attestations are produced independently, settle independently against their respective binary protocol calls, and remain interpretable in isolation. The consumer composes; the protocol does not.

## Section 3 — The worked example: two paired jobs at cycle 4

This case documents two on-chain ERC-8183 jobs run on the same AgentJobManager deployment (Base Sepolia, contract `0xC07CE789206CBEEC3A41D5CedBdA93B1024aaDdd`, subsequently redeployed as `0x27E64c0180b1c9D860561C423479492f25ff7bE3` on 2026-05-19), with the AHM and ThoughtProof evaluator wallets in mirrored roles across the pair:

| Job | Provider | Evaluator | Subject |
| --- | --- | --- | --- |
| #5 | ThoughtProof | AHM | A composition claim, verified by PoT/RV 4-model consensus |
| #4 | AHM | ThoughtProof | A real ACP agent, evaluated under AHM's behavioural scoring methodology |

Both jobs settled binary under the same lifecycle (`submit` → `complete`/`reject`, 80/20 fee split, evaluator-fee distribution regardless of verdict). The pair is deliberately asymmetric in *subject* but symmetric in *protocol mechanics*. The asymmetry is the point: the two jobs together demonstrate that the composition pattern is independent of which evaluator runs against which deliverable type.

### Job #5 — composition-pattern demonstration

ThoughtProof submitted a PoT/RV EpistemicBlock — a 4-model consensus verification artefact addressing a stated claim about composed-evaluator settlement (model diversity index 0.85, synthesis audit score 0.92, hash-chained attestation, per-model reasoning logged). AHM evaluated the deliverable as provider-fulfilment of the job spec and attested `complete` on-chain.

The settlement was clean: 80% of the gross fee to AHM (evaluator), 20% to treasury, 4.975 USDC payout to ThoughtProof (provider). The `JobCompleted` and `FeeDistributed` events fired with the verdict reason recorded on-chain.

What this job demonstrates is *procedural*: the composition pattern works end-to-end against the cycle-4 contracts. Two evaluators with distinct methodologies can settle a single job lifecycle with binary verdicts, no inter-issuer coordination, and clean fee distribution. The on-chain artefacts verify against off-chain content via byte-exact preimage handling — a non-trivial detail that surfaced as a real-world deliverable-contract gap during this exercise (initial submission lacked a persisted content URI, requiring a re-run as Job #5 with explicit Arweave-anchored content).

What this job does *not* demonstrate is AHM's behavioural diagnostics surface. The deliverable was a verification artefact about a claim — not an agent or wallet subject. AHM's evaluation here exercised the procedural attestation path (read, validate, attest) but did not invoke AHM's cross-registry scoring, confidence-band reasoning, or D-dimensional behavioural analysis. The job validated the composition mechanics; it did not exercise the composition substantively.

### Job #4 — substantive demonstration

The second job pairs AHM's behavioural scoring methodology with ThoughtProof's PoT/RV verification, in their respective native forms. AHM submits a behavioural attestation; ThoughtProof verifies the reasoning chain. Both sides produce native attestations under their own methodologies.

**Subject.** ACP agent #2624 (label "Jeff CEO", wallet `0x92ebf2f83e9981f2ee08187794e12dabfde953e9`), a Base mainnet agent active across the ACP proactive scan and AHM's rescan sources, with 49 days of observed activity and 146 transactions.

**Scoring.** AHM attests AHS 57 / Grade D with HIGH confidence based on 10 cumulative scans. The behavioural pattern is mid-range stability: the agent scored 57/D in 9 of 10 scans, with one transient transition to 64/C on 2026-04-15 driven by a D2 dimensional spike (51 → 62, computed from token-transfer activity). The D2 spike subsided in the following cycle and the agent returned to its baseline grade.

**Confidence basis.** The HIGH confidence derives from observation density (10 scans over 49 days), transaction count (146), and D-dimensional internal consistency. AHM's canonical methodological principle — *limited evidence does not equal adverse evidence* — does not apply in this case. Evidence is sufficient for high-conviction attestation, distinguishing this verdict from the INSUFFICIENT-confidence outputs AHM produces against sparse-history subjects.

**Composition outcome.** ThoughtProof evaluated the deliverable under PoT/RV and settled `complete()` with verdict ALLOW at 0.72 confidence, mined in block 41529925. The full deliverable content is published as a [gist](https://gist.githubusercontent.com/moonshot-cyber/7418ac58eb26eaf2fdca932462ea3c88/raw/57a25319c7360fd953fde3e8aa15daefe24dd51c/ahm-job4-deliverable.json), with the keccak256 hash anchored on-chain via the `JobSubmitted` event. The full PoT/RV epistemic block is archived at `https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY` with the reason hash anchored on-chain.

### What PoT/RV's evaluation actually verified

The PoT/RV pipeline ran four generator perspectives — Technical/Engineering, Epistemology/Philosophy, Contrarian/Adversarial, and Data Science/Statistical — against AHM's deliverable, followed by adversarial red-teaming and synthesis. The initial generator distribution recorded in the epistemic block was 3 BLOCK and 1 UNCERTAIN, which would have been a hard-rejection signal in a naive majority rule. The Data Science generator's BLOCK was the most analytically rigorous of the three (its critique survived red-team adjudication as a calibration concern rather than a reasoning defect, and became the surviving dissent in the synthesis); the Technical and Contrarian BLOCKs were identified as resting on critical domain misunderstandings:

- The Technical generator flagged a real inconsistency between the deliverable's assumed weights and AHM's actual 2D-mode weights. In 2D mode, AHM applies weights directly — D1 × 0.30 + D2 × 0.70 — with no renormalization logic. The generator assumed wrong weights and resolved the resulting arithmetic gap with an invented renormalization pathway. Red-team identified the weight assumption as the error, not AHM's arithmetic.

- The Contrarian generator framed AHM as committing a "category error" by measuring agent quality through wallet-health signals. Red-team identified this as a misreading of this deliverable's stated scope — the Jeff CEO deliverable ran in 2D mode (D1 + D2), measuring wallet behavioural health specifically, not the agent's task-execution quality. The deliverable said so explicitly. AHM's broader product surface includes AHM Verify — a standalone post-transaction output verification service that evaluates whether an AI agent delivered what it was paid to do, via a 6-role Claude-only adjudication pipeline. AHM Verify is a separate product surface at a different lifecycle point, not a dimension that could be invoked for pre-transaction behavioural scoring.

The arithmetic chain was independently verified: D1 × 0.30 + D2 × 0.70 = 0.30 × 70 + 0.70 × 51 = 56.7, rounded to 57. EMA temporal smoothing (α=0.6) across 9 of 10 cumulative scans showing AHS 57 (one transient observation at 64/C from an Apr 15 D2 spike) confirms the score. The synthesis verdict landed at ALLOW (confidence 0.72, medium-high), with one surviving dissenting concern carried explicitly into the record: HIGH confidence at this observation density is aggressive against general epistemological standards.

That surviving concern is acknowledged as a legitimate methodology question. AHM's confidence schema was originally calibrated against the high-volume scoring regime (hundreds of observations) and the threshold mapping has not been recalibrated for the mid-density regime (9–50 observations) this deliverable sits in. The verdict is not affected — the reasoning chain is internally consistent under the methodology as published — but the calibration question is a real one and worth separate methodology work.

### What the pair establishes

Together, the two jobs demonstrate the four Envelope invariants applied to a composed-evaluator setting:

1. **Dimensions compose without coordination between issuers.** AHM and ThoughtProof produced their attestations independently. Neither evaluator needed knowledge of the other's methodology, scoring schema, or attestation format. The deliverable contracts (what each side produces and consumes) are native to each evaluator's product surface.

2. **Composition is additive for trust, not multiplicative for friction.** Settlement remained binary at the protocol layer in both directions. The PoT/RV pipeline's internal richness (4 generators, red-team, synthesis, surviving dissent) operated entirely above the protocol surface, which saw only `submit` and `complete` calls. Evaluator-layer signal scales without protocol-layer change.

3. **Each dimension's verdict remains interpretable in isolation.** AHM's scoring attestation reads as AHM's product output, with its own confidence schema and methodology references. ThoughtProof's PoT/RV verdict reads as ThoughtProof's product output, with its own pipeline structure and dissent preservation. Neither verdict requires contextualisation against the other to be meaningful — but they compose cleanly when a consumer wants both.

4. **Failure-mode coverage is complementary across dimensions.** AHM detects behavioural patterns that pure reasoning verification cannot reach (sparse history, dimensional decomposition, infrastructure health signals). ThoughtProof detects reasoning-soundness failures that pure behavioural scoring cannot reach (arithmetic defensibility, category-error checking, epistemic overclaim detection). The two surfaces are disjoint in what they evaluate but complementary in what they cover.

The asymmetry between Job #5 and Job #4 (different subject types, different evaluator products applied) is what allows the four invariants to be exercised in both directions. A symmetric pair (e.g. both verdicts on the same agent) would have demonstrated the protocol mechanics but not the methodological independence each side preserves.

The composition is observable on-chain as a structured record:

- Job #4 deliverable hash: `0xf2bc97765af44471854c742d767d4f9c9a5b80a806f6a914e1c5512396214f9b`
- Job #4 submit TX (provider AHM): `0x392ef97c1d9bed0318cf512f9300d710752dfb2b041ef4c641cda872200a4331`
- Job #4 settlement TX (evaluator ThoughtProof): `0x4ab25466f2e790bd134ca68dd5c1a483b3e81171ed6066e9f45f3f50983a4c88`
- Job #4 PoT/RV epistemic block: `https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY`
- Job #5 deliverable hash: `0xf2fae3a46c7e82bfb1f0a7b867d542b338ae21ed588ccdb6a0de79bd59e6328d`
- Job #5 submit TX (provider ThoughtProof): `0xbf3e4641f5657838b6655efb9a210db7633d6a0777f92d3394c2e1f7648e0846`
- Job #5 settlement TX (evaluator AHM): `0x4efd2ecf83d26fc818d19234900ff7cf996b807cbed5f64e87a35ceac96421c8`
- Job #5 PoT/RV epistemic block: `https://arweave.net/MeKIu1SUBtjIYD7gCbVvjw1NLmRPIB2nBVedG8f5j_Q`

Both directions of the composition are externally auditable from on-chain data plus the published content URIs. No special trust in either evaluator is required to verify the substance.

## Section 4 — Why this is composition, not redundancy

The clean cycle-4 joint job is meant to show a different Envelope surface than the earlier cases. Case 1 showed action-time composition at an agent-to-MCP-server invocation. Case 2 showed wallet-bound aggregation. Case 3 showed pre-commit composition at transaction-composition time. This case exercises the ERC-8183 job lifecycle itself: two independent evaluators look at the same job from different surfaces and preserve different kinds of evidence.

At this surface, AHM and ThoughtProof are not duplicate judges. They answer different questions:

- **AHM evaluates the agent / behavioral / operational layer.** The relevant question is whether the participating agent looks healthy enough to trust in the job context: observed history, behavioral signals, infrastructure health, and confidence boundaries around sparse evidence.
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

## Section 5 — A note on cadence

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

## Section 6 — What this demonstrates about the Envelope as a primitive

This case exhibits the four invariants shared across all Envelope-in-Action cases: dimensions compose without coordination between issuers, composition is additive for trust not multiplicative for friction, each dimension's verdict remains interpretable in isolation, and failure-mode coverage is complementary across dimensions.

Relative to Cases 1, 2, and 3 — which exercised the format on action-time and wallet-bound aggregation surfaces — this case extends to a third topology: *settlement-time composition under a binary protocol*. Where action-time composition gates a future action and aggregation composition informs a present decision, settlement-time composition records two independent attestations against a completed action. The cadence is post-action; the artefact is durable on-chain settlement.

Three properties make this surface distinctive:

1. **The protocol provides no aggregation interface.** ERC-8183 settles binary per evaluator, per job. Multi-evaluator composition happens entirely above the protocol layer. The protocol surface is intentionally thin; the composition primitive lives in the evaluator-layer routing and the consumer-layer interpretation.

2. **The role pairing is symmetric across the dimension pair.** Both AHM and ThoughtProof can occupy either provider or evaluator roles. The case demonstrates both directions (Job #5: AHM evaluates ThoughtProof; Job #4: ThoughtProof evaluates AHM). The composition pattern works in either orientation, distinguishing this surface from earlier cases where the dimension pair is structurally asymmetric.

3. **The deliverable shapes are evaluator-product-native.** The provider's submission is shaped by what their methodology produces — a PoT/RV EpistemicBlock from ThoughtProof, a scoring attestation from AHM. The evaluator's verification is shaped by what their methodology evaluates. No shared deliverable schema is required; the protocol carries only the content hash and the binary verdict.

The four invariants compose cleanly with these three properties. Specifically: the absence of a protocol-layer aggregation interface (property 1) is what forces composition to remain additive rather than multiplicative (invariant 2). The symmetric role pairing (property 2) is what allows complementary failure-mode coverage (invariant 4) to be demonstrated in both directions. The native deliverable shapes (property 3) are what permit verdicts to remain interpretable in isolation (invariant 3) — each evaluator reads its own native artefact, not a shared lowest-common-denominator format.

## Section 7 — Cases adjacent to this one

The settlement-time surface at ERC-8183 admits natural extensions, each inviting contributions from the relevant issuers:

- **Three-dimension settlement composition**: behavioural diagnostics × reasoning verification × evaluator-stake reputation. The behavioural and reasoning dimensions in this case composed with a third dimension covering the evaluator's own stake-weighted track record on the protocol. The composition could surface as a richer consumer signal at settlement time without changing the protocol's binary settlement primitive.

- **Evidence-driven evaluator routing**: a composition where the consumer's routing logic selects an evaluator dynamically based on the deliverable's evidence surface — flat verdicts route to PoT/RV-style binary verification, rich traces route to PLV-style structured reproducibility, behavioural subjects route to AHM-style cross-registry scoring. The routing itself becomes a composable middleware surface.

- **Asynchronous re-evaluation**: a composition where one evaluator's verdict triggers a downstream re-evaluation by another evaluator on the same subject, with the second attestation landing on-chain alongside the settlement record. Relevant to settings where deliverable freshness matters (e.g. behavioural scoring with a >24h cache TTL composed with reasoning verification of the cached output).

Each is an invitation to whichever issuers cover the relevant dimensions.

## Section 8 — Inner evaluator narratives

_This section presents each evaluator's own account of their role in Cycle 4, written by the respective teams. §8.1 is authored by AHM; §8.2 is authored by ThoughtProof. §8.3 (composition rule application, on-chain settlement, and Arweave references) will be stitched jointly once both sides land._

### 8.1 AHM-evaluator narrative

AHM's role in Cycle 4 was to produce a structured behavioural attestation for the provider agent, consumable by ThoughtProof's downstream reasoning verification.

**Subject.** ACP agent #2624 ("Jeff CEO"), wallet `0x92ebf2f83e9981f2ee08187794e12dabfde953e9` — a Base mainnet agent active across the ACP proactive scan and AHM's rescan sources, with 49 days of observed activity and 146 transactions at the time of attestation.

**Scoring mode.** 2D mode (D1 + D2). D3 (Infrastructure Health) was excluded because no agent service endpoint was registered for synchronous probing, putting the scoring in 2D rather than 3D mode. This is a property of the subject (no probeable endpoint exposed), not of AHM's methodology — agents with registered service endpoints run in 3D mode by default.

**Attestation produced.**

| Field | Value |
| --- | --- |
| AHS composite | 57 |
| Grade band | D |
| Confidence | HIGH |
| Mode | 2D (D1 + D2) |
| Scan count | 10 cumulative |
| Observation window | 49 days |
| Transactions | 146 |

**Arithmetic.** Direct 2D weighting: D1 × 0.30 + D2 × 0.70 = 0.30 × 70 + 0.70 × 51 = 56.7, rounded to 57. EMA temporal smoothing (α=0.6) across 9 of 10 cumulative scans showing AHS 57 (one transient observation at 64/C from an Apr 15 D2 spike) produced the final composite.

**Confidence basis.** HIGH confidence derived from observation density (10 scans over 49 days), transaction count (146), and D-dimensional internal consistency. The "limited evidence ≠ adverse evidence" principle did not apply in this case — evidence sufficiency was met for high-conviction attestation, distinguishing this verdict from the INSUFFICIENT-confidence outputs AHM produces against sparse-history subjects.

**Surviving methodology question.** The attestation explicitly preserved one open concern: HIGH confidence at this observation density (10 scans) is aggressive against general epistemological standards. AHM's confidence schema was originally calibrated against the high-volume scoring regime (hundreds of observations) and the threshold mapping has not been recalibrated for the mid-density regime (9–50 observations) this case sits in. The attestation surfaced this concern rather than suppressing it — the reasoning chain is internally consistent under the methodology as published, but the calibration question is real and remains a live AHM workstream.

**Output structure surfaced to the consumer.** The attestation included the composite AHS, grade band, confidence enum, dimensional breakdown (D1=70, D2=51), the surviving methodology question, and the observation lineage (10 scans across 49 days, with the one transient deviation explicitly recorded). The dimensional decomposition is part of the deliverable, not collapsed into the headline score — consumers can act on dimensional signal directly rather than only on the composite.

**Deliverable artefact.** Job #4 deliverable JSON — keccak256 hash anchored on-chain via the `JobSubmitted` event. [Gist URL to be added when stitching §8.3.]

### 8.2 ThoughtProof-evaluator narrative

ThoughtProof's role in Cycle 4 was to verify the reasoning chain behind AHM's behavioural attestation — not to re-score the agent, but to answer the question the scoring methodology itself cannot ask: *is the reasoning behind this attestation internally defensible?*

**Subject.** The same ACP agent #2624 ("Jeff CEO") that AHM scored in §8.1, but consumed through a different evidence surface. ThoughtProof did not observe the agent's on-chain behaviour directly. The input was AHM's deliverable artefact: the scoring attestation with its dimensional breakdown, confidence enum, observation lineage, and methodology references. The question under verification was whether the reasoning chain from evidence to verdict is faithful — not whether the verdict is correct in some external sense.

**Verification product.** PoT/RV (Proof of Thought / Reasoning Verification). PoT/RV is a binary faithfulness check against a submitted claim, rationale, and evidence record. It does not require a structured execution trace or plan decomposition (that is PLV's surface). The routing decision — PoT/RV rather than PLV — was evidence-driven: AHM's deliverable provided a claim, a scoring methodology, and an observation record, but not a structured agent execution trace with tool calls and plan steps. PoT/RV was the appropriate product surface for this input shape.

**Pipeline.** Four generator perspectives — Technical/Engineering, Epistemology/Philosophy, Contrarian/Adversarial, and Data Science/Statistical — each produced an independent assessment of the deliverable's reasoning chain. These four perspectives were followed by adversarial red-teaming (generators critique each other's reasoning) and a synthesis stage that produces the final verdict.

**Initial generator distribution.** 3 BLOCK, 1 UNCERTAIN. Under a naive majority rule, this would have been a hard rejection. The synthesis stage exists precisely for this case: generator-level verdicts are inputs to adjudication, not votes.

**Red-team adjudication.** Two of the three BLOCKs were identified as resting on critical domain misunderstandings of the deliverable:

- The Technical generator flagged a real inconsistency between the deliverable's assumed weights and AHM's actual 2D-mode weights. In 2D mode, AHM applies D1 × 0.30 + D2 × 0.70 directly — no renormalization. The generator assumed wrong weights, found an arithmetic gap, and resolved it by inventing a renormalization pathway that does not exist in AHM's methodology. Red-team identified the weight assumption as the generator's error, not AHM's arithmetic.

- The Contrarian generator framed AHM as committing a "category error" by measuring agent quality through wallet-health signals. Red-team identified this as a misreading of the deliverable's explicit stated scope: the Jeff CEO attestation ran in 2D mode (D1 + D2), measuring wallet behavioural health specifically, not the agent's task-execution quality. The deliverable said so in plain text. AHM's broader product surface includes AHM Verify (a separate post-transaction output-verification service via a 6-role Claude adjudication pipeline), but that is a different product at a different lifecycle point — not a dimension that could be invoked for pre-transaction behavioural scoring.

- The Data Science generator's BLOCK was the most analytically rigorous of the three. Its critique — that HIGH confidence at 10 observations is aggressive against general epistemological standards — survived red-team adjudication as a legitimate calibration concern rather than a reasoning defect. It became the surviving dissent in the synthesis.

**Arithmetic verification.** The scoring chain was independently reproduced: D1 × 0.30 + D2 × 0.70 = 0.30 × 70 + 0.70 × 51 = 56.7, rounded to 57. EMA temporal smoothing (α=0.6) across 9 of 10 cumulative scans showing AHS 57, with one transient observation at 64/C from the Apr 15 D2 spike, confirms the final composite. The arithmetic is correct under the methodology as published.

**Verdict.** ALLOW at 0.72 confidence (medium-high). The confidence reflects one surviving dissenting concern carried explicitly into the record: HIGH confidence at this observation density (10 scans over 49 days) is aggressive against general epistemological standards. This concern was not suppressed, not overridden, and not converted into a BLOCK — it was preserved as a live methodology question because the reasoning chain is internally consistent under AHM's published methodology, even if the calibration threshold deserves separate work.

**Surviving methodology question.** Identical to the one AHM surfaced in §8.1 — both evaluators independently flagged the same concern from different directions. AHM flagged it as a property of their confidence schema calibration. ThoughtProof's Data Science generator flagged it as an epistemological observation about sample-size adequacy. The convergence is not coordinated; it is what independent evaluation of the same evidence surface produces when both sides are honest about uncertainty.

**Output structure surfaced to the protocol.** The settlement action was `complete()`, mined in block 41529925. The binary protocol saw only a successful settlement. Above the protocol surface, the full PoT/RV epistemic block was archived at `https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY` — containing the per-generator reasoning, the red-team adjudication, the synthesis with surviving dissent, the arithmetic verification, and the confidence derivation. The reason hash is anchored on-chain via the settlement transaction.

**Deliverable artefact.** Job #4 settlement TX: `0x4ab25466f2e790bd134ca68dd5c1a483b3e81171ed6066e9f45f3f50983a4c88`. PoT/RV epistemic block: [Arweave](https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY). Deliverable content: [gist](https://gist.githubusercontent.com/moonshot-cyber/7418ac58eb26eaf2fdca932462ea3c88/raw/57a25319c7360fd953fde3e8aa15daefe24dd51c/ahm-job4-deliverable.json), keccak256 hash anchored on-chain via `JobSubmitted`.

### 8.3 Composition rule application, on-chain settlement, and archival references

This section stitches the two evaluator narratives into a single composed-evaluation artefact. It does three things: states how the two attestations feed a settlement-time composition decision, consolidates the on-chain settlement record into one verifiable reference table, and closes on the uncoordinated convergence the pair produced.

**Composition rule application.** The two attestations cover disjoint evidentiary surfaces over the same subject (ACP agent #2624, "Jeff CEO"). AHM's attestation answers a behavioural question — *does the agent's observed on-chain history support trust at this confidence?* (AHS 57 / Grade D / HIGH, 2D mode). ThoughtProof's attestation answers a reasoning question — *is the chain from evidence to that verdict internally defensible?* (PoT/RV ALLOW @ 0.72). Neither verdict is an input to the other: AHM did not consult ThoughtProof's pipeline, and ThoughtProof did not re-score the agent. The composition rule is therefore not a blend and not an override — it is a domain-separated gate that the consumer assembles at settlement time:

- Each evaluator holds veto authority **only within its own domain**. A behavioural REJECT from AHM (e.g. INSUFFICIENT confidence against a sparse-history subject) gates the settlement on behavioural grounds; a reasoning BLOCK from ThoughtProof gates it on faithfulness grounds. This is a *mutual in-domain veto*, not a no-veto arrangement: each side can independently gate the composed decision within its own surface, and neither can overturn the other's in-domain finding — because the two are not measuring the same thing. The asymmetry that would be a problem (one side's verdict silently dominating the other's domain) does not arise, precisely because the domains are disjoint.
- The consumer chooses the policy: treat either attestation as a hard gate, require both to clear (the conservative composition — used here), or use one as an escalation trigger for the other. The protocol surface is unaffected by the choice; both jobs settled binary regardless.
- **The composition rule is defined for every combination of the two verdicts** (fully specified in the COMPOSED_EVALUATORS.md middleware writeup; summarised here). A reasoning ALLOW settles unless AHM reports *HIGH-confidence* adverse behaviour (Grade D–F at HIGH), in which case AHM's in-domain veto gates the settlement. A reasoning BLOCK always gates, regardless of behavioural standing. Crucially, a LOW- or INSUFFICIENT-confidence behavioural grade is *not* adverse evidence — under the limited-evidence-≠-adverse-evidence principle it settles (flagged for audit), it does not gate. The conflict cells are the point of composition: a blended score would let a healthy behavioural signal wash out a reasoning failure, or let a sparse history block a sound output. Domain-separated gating keeps each concern legible and actionable rather than averaged.
- **Degradation is fail-closed on absence, not on low confidence.** If an evaluator is unavailable, times out, or returns malformed data, the composed decision does not fall through to the surviving verdict — a missing reasoning verdict is not a faithfulness pass, and a missing behavioural attestation is not a behavioural pass. This is distinct from a present-but-low-confidence behavioural grade, which settles with an audit flag rather than gating. Absence fails closed; low confidence does not. This preserves the Section 5 invariant that insufficient evidence remains first-class — neither silently converted into a pass nor into a rejection.

A note on what the worked pair does and does not exercise. Job #4 and Job #5 demonstrate the composition *mechanics* — two independent evaluators settling a single job lifecycle with binary verdicts, no inter-issuer coordination, clean fee distribution — not an application of the composition *rule* above. In Job #4, ThoughtProof's role was to verify whether AHM's behavioural attestation was internally defensible (the deliverable under evaluation was AHM's reasoning), not to gate a settlement on Jeff CEO. AHM's Grade D / HIGH here is the *content* of the deliverable being verified, not a behavioural veto input feeding the composition rule. The PoT/RV ALLOW @ 0.72 therefore says "AHM's attestation reasoning is defensible," and the job settled `complete` on that basis. A live deployment applying the full composition rule (where AHM's grade is a veto input and ThoughtProof's verdict gates the same settlement) is the natural next case; this pair establishes that both evaluators can occupy either role and settle cleanly, which is the precondition for it.

**On-chain settlement summary.** Both jobs ran on the same AgentJobManager deployment lineage (Base Sepolia: cycle-4 contract `0xC07CE789206CBEEC3A41D5CedBdA93B1024aaDdd`, redeployed `0x27E64c0180b1c9D860561C423479492f25ff7bE3` on 2026-05-19). The composition is fully reconstructable from the following on-chain and archival references:

| Reference | Job #4 (ThoughtProof evaluates AHM) | Job #5 (AHM evaluates ThoughtProof) |
| --- | --- | --- |
| Provider | AHM | ThoughtProof |
| Evaluator | ThoughtProof | AHM |
| Subject | ACP agent #2624 ("Jeff CEO"), `0x92ebf2f83e9981f2ee08187794e12dabfde953e9` | Composition claim, PoT/RV 4-model consensus |
| Deliverable hash | `0xf2bc97765af44471854c742d767d4f9c9a5b80a806f6a914e1c5512396214f9b` | `0xf2fae3a46c7e82bfb1f0a7b867d542b338ae21ed588ccdb6a0de79bd59e6328d` |
| Submit TX | `0x392ef97c1d9bed0318cf512f9300d710752dfb2b041ef4c641cda872200a4331` | `0xbf3e4641f5657838b6655efb9a210db7633d6a0777f92d3394c2e1f7648e0846` |
| Settlement TX | `0x4ab25466f2e790bd134ca68dd5c1a483b3e81171ed6066e9f45f3f50983a4c88` | `0x4efd2ecf83d26fc818d19234900ff7cf996b807cbed5f64e87a35ceac96421c8` |
| Verdict | ALLOW @ 0.72 (block 41529925) | `complete` (4.975 USDC payout, 80/20 fee split) |
| PoT/RV epistemic block | [arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY](https://arweave.net/-c1iufNZVyZyTOOr4RVl0gnfSUQ52UmUYfzpIMoCFnY) | [arweave.net/MeKIu1SUBtjIYD7gCbVvjw1NLmRPIB2nBVedG8f5j_Q](https://arweave.net/MeKIu1SUBtjIYD7gCbVvjw1NLmRPIB2nBVedG8f5j_Q) |
| Deliverable content | [gist (ahm-job4-deliverable.json)](https://gist.githubusercontent.com/moonshot-cyber/7418ac58eb26eaf2fdca932462ea3c88/raw/57a25319c7360fd953fde3e8aa15daefe24dd51c/ahm-job4-deliverable.json) | _[AHM: Job #5 deliverable gist URL to confirm]_ |

Every cell above is verifiable from public data: the TX hashes resolve on Base Sepolia, the deliverable hashes are anchored on-chain via the `JobSubmitted` events, and the Arweave references are permanent. No trust in either evaluator is required to reconstruct what was attested and how it settled.

The two verdict cells are intentionally asymmetric. Job #4 carries a PoT/RV confidence (ALLOW @ 0.72) because ThoughtProof ran a substantive reasoning verification against AHM's behavioural deliverable. Job #5 records only the binary `complete` because AHM's role there was procedural attestation of ThoughtProof's PoT/RV artefact as provider-fulfilment — it exercised the attestation path (read, validate, attest) without invoking AHM's cross-registry behavioural scoring, since the deliverable was a verification artefact about a claim, not an agent or wallet subject. The asymmetry reflects the deliberate subject asymmetry of the pair (Section 3), not a missing measurement.

**Uncoordinated convergence.** The pair's most significant outcome is not that both jobs settled cleanly — that demonstrates the mechanics. It is that the two evaluators, operating independently and without coordination, surfaced *the same methodology question from two different directions*. AHM flagged it from the inside (§8.1): HIGH confidence at 10 observations is aggressive against a confidence schema originally calibrated for the high-volume regime, and the mid-density threshold mapping has not yet been recalibrated. ThoughtProof's Data Science generator flagged it from the outside (§8.2): HIGH confidence at this observation density is aggressive against general epistemological standards — a critique that survived red-team adjudication as a legitimate calibration concern and became the surviving dissent carried into the ALLOW verdict at 0.72.

Neither side knew the other would raise it. AHM reached it through self-audit of its own calibration lineage; ThoughtProof reached it through adversarial reasoning verification of the deliverable. That two independent evaluators, with disjoint methodologies and no shared model, converged on the same open question is exactly what composed evaluation is supposed to produce: not redundant agreement on the headline verdict, but complementary illumination of where the real uncertainty lives. A monolithic blended score would have averaged this signal away. The Envelope preserves it as a live, jointly-surfaced methodology question — and that question, raised twice from two angles, carries more weight than either evaluator raising it alone.