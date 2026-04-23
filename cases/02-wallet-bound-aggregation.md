# The Envelope in Action: Wallet-Bound Aggregation at Pre-Transaction Trust

_Draft v1 — April 2026_
_First wallet-bound case, contributed by: SkyeMeta (SkyeProfile)_

_Contributed under the case format proposed in "The Envelope in Action" (ThoughtProof, April 2026). Open to feedback from all Envelope issuers._

## Why this case exists

"The Envelope in Action" proposed a four-section format for supplementary case documentation and contributed the first action-time case (agent-to-MCP-server invocations). This document contributes the first wallet-bound case, using the topology currently orchestrating nine issuers and ten signed dimensions at SkyeProfile.

The wallet-aggregation topology precedes action-time gating in the agent lifecycle. Where MCP-invocation cases ask "should this action proceed?", wallet-aggregation cases ask "what does the Envelope say about this wallet, before any action has been proposed?"

## The surface

Pre-transaction trust is the decision layer a relying party reaches before an action is composed. Examples:

- A lending protocol evaluating a wallet before issuing a line of credit
- A compliance gateway screening a wallet before onboarding
- A DAO determining voting eligibility before accepting a proposal
- An agent platform pre-qualifying a wallet before granting session scope

At this surface, one lookup returns the full envelope for a given wallet: all issuer-signed attestations across all covered dimensions, each verifiable offline against its issuer's JWKS. The relying party picks which dimensions matter for its specific decision class and applies its own thresholds.

Two properties of this topology shape the failure-mode analysis below:

- **Single-call fan-out.** One call resolves all ten dimensions in parallel and returns each issuer's attestation independently signed, independently verifiable, in a unified envelope. The relying party does not need bilateral integration with each issuer.
- **Consumer-side threshold-setting.** No dimension has been evaluated against an action yet — each attestation is a standing assessment of the wallet along its dimension. The relying party applies thresholds at consumption time, using whichever subset of dimensions its decision class requires.

## Failure modes single-dimension screening cannot detect

The pairings below are illustrative. The actual orchestration spans ten signed dimensions; any two-dimension pair is one projection of the surface. The point is that every single-dimension pre-check has a structurally invisible failure surface that at least one other dimension in the envelope covers.

### Failure mode 1: clean behavior, insolvent wallet

A lending protocol pre-screens a wallet using only a behavioral-trust signal. The wallet has no sybil indicators, a healthy interaction history, and no fraud flags. The behavioral dimension admits.

- **Behavioral trust:** clean.
- **Wallet state:** deny. The wallet's on-chain balances and holdings do not support the credit line being considered.

Single-dimension behavioral screening admits a wallet that wallet-state evaluation would deny. The failure is not in the behavioral signal — it is structurally outside what the behavioral dimension can see.

### Failure mode 2: solvent wallet, governance exclusion

A DAO evaluates a proposer's wallet using only token-holding state. The wallet holds the governance token above the eligibility threshold. Wallet-state admits.

- **Wallet state:** clean. Holdings above threshold, no anomalies.
- **Governance:** deny. The wallet's governance standing is disqualified under the DAO's charter.

Single-dimension wallet-state screening admits a wallet that governance evaluation would deny. The failure is again structural — wallet-state does not speak to governance history.

The generalization: any single dimension, consulted alone, has a structurally invisible failure surface. Wallet-aggregation exposes the full set of dimensions in one signed call so the relying party can construct the screen appropriate to its decision class, rather than inheriting the blind spots of whichever single dimension it happened to pick.

## Why this is composition, not redundancy

Three properties distinguish wallet-aggregation from a single-score aggregator:

1. **No issuer speaks for another.** Each attestation is independently signed by its own issuer, against its own threat model. A DAO reading the governance dimension does not need to trust the behavioral issuer; a lender reading wallet-state does not need to trust the governance issuer. Trust boundaries remain issuer-specific.

2. **No dimension is collapsed into a score.** A monolithic "wallet trust score" would have to decide in advance how much weight solvency carries relative to governance relative to identity. No such constant exists across decision classes — lending thresholds are not DAO thresholds are not compliance thresholds. Wallet-aggregation defers weighting to the consumer, where the decision actually lives.

3. **Same envelope, different relying parties, different correct decisions.** The same envelope that a lender uses to admit a wallet can have a DAO deny the same wallet, and both can be correct. Making that possible in one signed artifact — without pre-committing to a weighting — is the topology's job.

This is defense in depth via dimensional orthogonality. Each dimension covers a surface the others structurally cannot see, and the relying party assembles the screen its decision class requires. No single-dimension blind spot becomes the de facto policy.

## A note on cadence

Wallet-aggregation composes across dimensions with materially different native cadences:

- Wallet-state reflects the chain head; it is effectively per-query, recomputed at the moment of the call.
- Behavioral and settlement dimensions reflect historical windows and are updated on the issuer's own refresh cadence.
- Governance and compliance dimensions may reflect policy-receipt or sanctions-list states that update on their own schedules.

Each attestation carries its own freshness metadata. The aggregation does not normalize cadence — it surfaces each dimension's own freshness to the relying party, which decides what window is acceptable for the decision class at hand.

## What this demonstrates about the Envelope as a primitive

This case exhibits the four invariants shared across all Envelope-in-Action cases: dimensions compose without coordination between issuers, composition is additive for trust not multiplicative for friction, each dimension's verdict remains interpretable in isolation, and failure-mode coverage is complementary across dimensions.

The wallet-aggregation topology demonstrates them at a different surface than action-time gating — before any action has been proposed, at the decision layer where a relying party is still deciding whether to admit the wallet to a class of transactions.

## Cases adjacent to this one

Wallet-aggregation is not a single decision class; it is a family of decision classes sharing the same envelope and differing in which dimensional subsets they consult and at what thresholds. Future wallet-bound cases expected:

- **Compliance-first pre-screens**, using compliance and governance dimensions with hard thresholds.
- **Lending pre-qualification**, using wallet-state and behavioral dimensions with tiered thresholds.
- **Agent-platform session scoping**, using identity, security-posture, and settlement dimensions to pre-qualify an agent's wallet before an action is composed.

Each of those is a case waiting to be written by whichever relying party runs that decision class in production against a multi-issuer envelope.

## Acknowledgments

The wallet-aggregation topology described here is implemented at SkyeProfile, orchestrating nine issuers and ten signed dimensions. The wallet-state dimension referenced in the failure modes is issued by InsumerAPI.

The format exercised in this case was proposed by ThoughtProof in "The Envelope in Action" (April 2026). A short format-fit note follows.

## Format-fit notes (for the case format itself)

Exercising the four-section format on a wallet-bound case surfaced three places where v4's action-time framing did not quite fit:

1. **"The surface"** reads as "where in the agent lifecycle does composition happen" — apt for action-time, less apt for pre-action decision classes. Softening to "the surface or decision class" would extend cleanly to wallet-bound cases.
2. **"Failure modes neither dimension catches alone"** assumes both dimensions are evaluated at the same decision point. For wallet-aggregation, the failure mode reads as "failure modes single-dimension evaluation cannot detect" — the same argument from the other direction. A neutral phrasing covers both topologies.
3. **"Each dimension can gate execution on its own"** is action-time language. For wallet-bound cases, each dimension can independently inform or block the relying party's decision, which is not always the same as gating execution. Neutralizing to "independently inform the relying party's decision" keeps the action-time reading intact while extending to wallet-bound.

Each is a one-word reframe. Offered as suggested edits rather than required changes — if v5 prefers to keep action-time framing crisp, a parallel "wallet-bound cases" paragraph in the format section would also work.
