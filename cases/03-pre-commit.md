# The Envelope in Action: Pre-Commit Composition at Transaction-Composition Time

_Draft v1 — April 2026_
_Third case (second action-time), contributed by: InsumerAPI, ThoughtProof_

_Contributed under the case format proposed in "The Envelope in Action" (ThoughtProof, April 2026). Open to feedback from all Envelope issuers._

## Why this case exists

"The Envelope in Action" proposes a four-section format for supplementary case documentation. Case 1 (agent-to-MCP-server invocations, RNWY x ThoughtProof) exercised the format on an action-time surface where the two dimensions cover orthogonal threat models -- server-side quality and reasoning integrity. Case 2 (wallet-bound aggregation, SkyeMeta) exercised it on a pre-action decision-class surface where all dimensions are consulted in one call and the relying party chooses thresholds at consumption time.

This document contributes the third case: an action-time pairing at **transaction-composition time**, where an agent has reasoned toward a specific transaction and is about to commit it on-chain. The two dimensions here are neither server-side (case 1) nor pre-action-aggregated (case 2) -- they are pre-commit and per-call, gating the same action in complementary ways.

Pre-commit is InsumerAPI's native dimension in the envelope. The pre-commit wallet-state attestation and the reasoning-integrity attestation are issued independently by their respective issuers; this case is co-authored to demonstrate how they compose at transaction-composition time.

## The surface or decision class

Transaction-composition time is the moment between an agent deciding what to do and the transaction being signed and broadcast. The agent has produced candidate call data: a target contract, a method, parameters, values, a nonce, possibly an allowance or approval flow. Two things have to be true for the composed transaction to be both **executable** and **correct**:

- The wallet must actually be able to execute it on-chain, per InsumerAPI's pre-commit evaluation over live chain state.
- The reasoning that produced it must be sound with respect to the agent's goal and the information the agent was given: the right asset, the right counterparty, the right amount, the right slippage, the right direction.

These are not the same check. On-chain executability is a capability predicate over chain state. Reasoning integrity is a semantic predicate over the agent's input-output relation. The attack surfaces are disjoint, and each check has a structural blind spot the other covers.

At this surface, two dimensions meet on different native cadences:

- **Pre-commit wallet-state (InsumerAPI).** Produced per-composition by POST /v1/attest. The candidate transaction's chain-state preconditions are expressed as conditions -- token balance thresholds, NFT ownership, EAS attestation validity, Farcaster identity, or any combination of these -- and evaluated against live chain state at the block anchoring the query. The attestation is an ECDSA P-256 signature over the Attestation payload, binding the attestation id, each condition's `conditionHash` and `chainId`, a chain-appropriate block anchor per condition, the `attestedAt` timestamp, and the per-condition and composite allow/deny verdicts (carried in the response as `met` per condition and `pass` for the composite). The block anchor is chain-family-specific -- `blockNumber` + `blockTimestamp` on EVM, `slot` on Solana, `ledgerIndex` + `ledgerHash` on XRPL, tip `blockHeight` + `blockHash` on Bitcoin (freshness anchor only on UTXO-model chains, which do not permit reproducible balance-at-block queries). The signature is verifiable offline against InsumerAPI's JWKS, and each `conditionHash` binds the signature to the exact predicate evaluated -- neither predicate nor verdict can be paraphrased downstream without breaking verification. When `format: "jwt"` is requested, the response additionally carries an ES256 JWT that binds the wallet as the `sub` claim, the attestation id as `jti`, along with standard issuer / issued-at / expiration claims. Freshness is anchored at the block (or its chain-appropriate equivalent); evaluation reads chain head at query time, and the attestation carries the anchor at which the evaluation ran. The relying party -- or the wallet's signing layer -- chooses the acceptable block-lag window for the decision class at hand. Reorg sensitivity is the consumer's choice, not the issuer's imposition.
- **Reasoning integrity (ThoughtProof).** Produced per-call, once the model has emitted the candidate transaction. The verdict is over the reasoning chain that produced the call data, not over the call data's executability.

The composition is action-time: both signals are consulted at transaction-composition time, and either can independently inform the relying party's decision -- in this surface, typically by gating signing and broadcast. In ERC-4337 and other account-abstraction flows this gating sits at UserOperation composition rather than EOA signing, but the composition semantics are identical.

## Failure modes single-dimension evaluation cannot detect

### Failure mode 1: sound reasoning, non-executable composition

The agent reasons correctly that it should swap 1 WETH for USDC on a particular DEX at the current price. The reasoning chain is sound: the intent matches the user's goal, the market reading is accurate, the counterparty is appropriate, the slippage bounds are reasonable. But the wallet holds 0.8 WETH, not 1.0 -- the balance condition evaluates against live chain state and does not meet the composition's requirement.

- **InsumerAPI (pre-commit wallet-state):** deny. The candidate transaction cannot execute against live chain state.
- **ThoughtProof (reasoning integrity):** allow. Given the agent's inputs, the reasoning produces a semantically correct call.

Reasoning integrity says proceed. Pre-commit wallet-state says stop. The agent stops -- and, crucially, stops **before** signing, not by reverting on-chain. A revert wastes gas, leaks intent, and in some cases triggers MEV sandwiching on the retry. Pre-commit denial avoids all three.

### Failure mode 2: executable composition, flawed reasoning

The agent composes a transaction that the wallet can execute -- the composition's conditions are satisfied against live chain state -- but the reasoning that produced it is flawed. The model hallucinated a price, confused two token addresses with similar symbols, inverted a swap direction, or chose a drained pool because it matched a pattern in its training.

- **InsumerAPI (pre-commit wallet-state):** allow. The composition is executable and the conditions evaluate cleanly against live chain state.
- **ThoughtProof (reasoning integrity):** deny. The verdict chain shows the composed call is not supported by the agent's stated goal or by the inputs the model received.

Pre-commit wallet-state says proceed. Reasoning integrity says stop. The agent stops -- again, before signing, so the flawed composition never becomes an on-chain artifact.

The generalization: at transaction-composition time, executability and semantic correctness are independent predicates. Any single-dimension gate has a structurally invisible failure surface the other dimension covers. Pre-commit wallet-state does not read the agent's intent; reasoning integrity does not read live chain state.

## Why this is composition, not redundancy

A monolithic "transaction safety score" at composition time would have to reconcile executability and semantic correctness into one number. It cannot -- they live on different object spaces. A transaction can be perfectly executable and completely wrong, or perfectly reasoned and completely non-executable. No weighted average preserves both signals.

The Envelope composition is different: both attestations remain independent, signed, and auditable, and either can independently inform the relying party's decision at composition time. No interface coupling is required between issuers -- InsumerAPI does not need to know how ThoughtProof verifies reasoning, and ThoughtProof does not need to know which chain-state predicates InsumerAPI checks. The composition happens at the consumption layer.

This is defense in depth via orthogonal verification on the same action. Pre-commit wallet-state covers a surface that reasoning integrity structurally cannot see (chain-state execution preconditions), and vice versa (semantic errors in the composed call). The disjointness is not contrived: the two checks live on different sides of the intent -> call data -> broadcast pipeline, and either side alone misses the other's failure class.

## A note on cadence

Both dimensions in this case are action-time, but they are not symmetric in cadence:

- Reasoning integrity is strictly per-call. The verdict exists only once the model has emitted a candidate transaction.
- Pre-commit wallet-state is per-composition but reads live chain state at evaluation time. The attestation's freshness is bounded by the chain head at evaluation, not by a scheduled sweep.

The Envelope does not require the two cadences to align. Each attestation carries its own freshness metadata and the relying party -- or the wallet's own signing layer -- chooses an acceptable window for the decision at hand.

## What this demonstrates about the Envelope as a primitive

This case exhibits the four invariants shared across all Envelope-in-Action cases: dimensions compose without coordination between issuers, composition is additive for trust not multiplicative for friction, each dimension's verdict remains interpretable in isolation, and failure-mode coverage is complementary across dimensions.

Relative to case 1 (RNWY x ThoughtProof at MCP invocation) and case 2 (wallet-bound aggregation at SkyeProfile), this case shows the format extending to a second action-time surface: transaction-composition, where both dimensions are pre-commit relative to the chain, and where the action being gated is the transaction itself rather than an MCP call or a wallet-admission decision. Three cases, three surfaces, two topologies (action-time and wallet-bound aggregation), same format, same invariants.

## Cases adjacent to this one

The pre-commit surface at transaction-composition time admits natural extensions, each inviting contributions from the relevant issuers:

- **Three-dimension pre-commit**: wallet-state x reasoning x behavioral-reputation, adding RNWY at composition time for agents whose historical behavior is itself a composition input.
- **Compliance-gated pre-commit**: wallet-state x reasoning x compliance, where a third dimension denies compositions that would violate jurisdictional or counterparty rules even when the first two dimensions admit.

Each is an invitation to whichever issuers cover the relevant dimensions.

## Acknowledgments

The pre-commit wallet-state dimension described here is issued by InsumerAPI; the reasoning-integrity dimension is issued by ThoughtProof. The case was developed jointly during April 2026, exercising the Envelope-in-Action format on a surface where both dimensions gate the same composition from different sides.

InsumerAPI's wallet-state dimension is also one of the ten dimensions aggregated in the SkyeProfile wallet-bound topology; this case exercises it at a different point in the agent lifecycle.
