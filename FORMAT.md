# The Envelope in Action: A Format for Lived Composition Cases

_Draft v5.3 -- April 2026_
_Format proposed by: ThoughtProof. Reframed in v5 following feedback from RNWY (case 1) and SkyeMeta (case 2)._

_This is a proposed format for supplementary case documentation, not a spec amendment. Open to feedback from all Envelope issuers._

## Why this document exists

Nine issuers are signing attestations into the same ERC-8183 Multi-Attestation Envelope, covering ten dimensions in total (one issuer covers two dimensions). The spec is sound, and the first composition topology -- wallet-bound aggregation via SkyeProfile -- is already in production: a single wallet lookup returns independently signed attestations from multiple issuers, each verifiable offline against its own JWKS.

What's missing is a shared way of showing **where else** in the agent lifecycle these attestations compose, and in terms that a compliance officer, a regulator, or an enterprise architect can reason about without reading the spec first.

This format complements organization-level frontier-AI auditing frameworks such as the one proposed by Brundage et al. (2026) at AVERI (AI Verification and Evaluation Research Institute, arXiv:2601.11699), by operating at the deployment and per-action layer those frameworks explicitly exclude from scope. Envelope attestations function as a deployment-level, per-action verification primitive: continuous automated monitoring signals that can be composed across independent issuers without central coordination, at surfaces (agent invocations, wallet screens, transaction compositions) that sit below organization-level audit scope.

This document proposes a format for those cases, defines the shared invariants every case must show, and indexes the cases contributed so far.

## The Envelope, briefly

The Envelope treats agent trust as a composition of orthogonal dimensions rather than a single aggregated score. Each attestation is issued independently, signed by its own issuer, and evaluated at its own verification point. No issuer speaks for another; no dimension substitutes for another.

### Topologies and surfaces

Two distinctions structure this format:

**Composition topologies** (how attestations compose in time):

- **Wallet-bound aggregation**: dimensions are fetched and bundled per-wallet, independent of any specific action. A relying party asks "what does the Envelope say about this wallet?" and picks which dimensions matter for its decision threshold.
- **Action-time gating**: dimensions are consulted at a specific decision point, with each attestation able to independently inform the relying party's decision. A resolver or keeper asks "do all relevant dimensions approve this particular action?" and composes the verdicts at consumption time.

**Surfaces** (where within a topology the composition is exercised):

A topology can have multiple surfaces. Action-time gating, for example, has been exercised so far at agent-to-MCP-server invocations (case 1) and at transaction-composition time (case 3). Wallet-bound aggregation has been exercised as a pre-transaction decision class at SkyeProfile (case 2). Each surface is a distinct decision point within its topology; the topology determines when composition happens, the surface determines what is being composed around.

### A note on cadence

Issuers produce their attestations on different native cadences. Some signals are inherently per-call (a reasoning verdict exists only once the model has produced its output). Others are continuous (a scanner's view of a server is the latest cached finding from a recurring sweep). Others are per-query against live chain state (a pre-commit evaluation reads the chain head at evaluation time). The Envelope does not require issuers to align their cadences -- each attestation carries its own freshness metadata, and the consumer decides what window is acceptable for the decision at hand.

## The format

Each "Envelope in Action" case follows four sections:

1. **The surface or decision class.** Where in the agent lifecycle does the composition happen, or which decision class is the relying party evaluating?
2. **Failure modes single-dimension evaluation cannot detect.** Two or more concrete scenarios where any single dimension, consulted alone, would reach the wrong conclusion.
3. **Why this is composition, not redundancy.** Demonstrate complementary failure-mode coverage -- each dimension must cover a surface the others structurally cannot see.
4. **What this demonstrates about the Envelope as a primitive.** Shared across all cases; reinforces the common invariants.

The goal is not canonization. Any issuer can contribute a case involving their own dimension. Cases compose: two issuers writing about the same surface eventually converge; an issuer writing alone about their own dimension is equally valuable for readers who need to understand that dimension in isolation.

## What every case demonstrates about the Envelope as a primitive

Regardless of which dimensions, which topology, or which surface a case involves, a well-written Envelope-in-Action case shows four invariants:

1. **Dimensions compose without coordination between issuers.** Attestations are signed independently, against each issuer's own threat model, on each issuer's own schedule. The Envelope is the coordination medium; no bilateral integration is required.

2. **Composition is strictly additive for trust, not multiplicative for friction.** Adding a second dimension does not multiply the cognitive or engineering load on the consumer -- it adds one more independent signal to inform the decision.

3. **Each dimension's verdict remains interpretable in isolation.** A regulator, auditor, or operator can read any attestation without needing to understand the others. No verdict requires translation through another issuer's framework.

4. **Failure-mode coverage is complementary, not redundant.** The failure modes each case describes are not contrived edge cases -- they are canonical classes of failure at a particular surface, and each is structurally invisible to the other dimension(s).

## The series so far

| # | Surface                          | Topology                    | Dimensions paired                        | Contributors                          |
|---|----------------------------------|-----------------------------|------------------------------------------|---------------------------------------|
| 1 | Agent-to-MCP-server invocation   | Action-time gating          | Server quality & risk x Reasoning        | RNWY, ThoughtProof                    |
| 2 | Pre-transaction trust            | Wallet-bound aggregation    | All ten signed dimensions                | SkyeMeta                              |
| 3 | Transaction-composition time     | Action-time gating          | Pre-commit wallet-state x Reasoning      | _(published; InsumerAPI, ThoughtProof)_ |

Three cases published, covering two topologies and two action-time surfaces, with the same format and the same invariants throughout.

## Cases in waiting

Other surfaces and topologies will produce cases as they mature. Each is an invitation to whichever issuers cover the relevant dimensions:

- **Ownership x regulatory state** at issuance and transfer checkpoints (action-time gating, new surface)
- **Identity x behavioral reputation** at agent-to-agent delegation (action-time gating, new surface; would draw on RNWY's wallet-agent-server graph among others)
- **Any dimension x permanent attestation storage** for forensic auditability (cross-cutting; applies to any case)
- **Compliance-first wallet screens** (wallet-bound aggregation, new decision class)
- **Lending pre-qualification** (wallet-bound aggregation, new decision class)
- **Agent-platform session scoping** (wallet-bound aggregation, new decision class)

Cases involving three or more dimensions are welcomed and expected -- the published cases are pairs only because two dimensions meet at those surfaces. Additional extensions of the pre-commit case (three-dimension, compliance-gated) are likely natural follow-ups.

## Why this format, now

The Envelope is in a familiar place for young standards: the spec is sound, the first topology is live, but audiences outside the builder community -- regulators, compliance teams, enterprise architects, non-crypto developers -- still find the broader compose model abstract. A shared case format gives those audiences concrete artifacts to reason about without requiring them to read the spec first.

As issuers contribute additional cases, the Envelope acquires a library of lived compositions that does the adoption work the spec itself cannot do.

## A note on terminology

Short-hand labels used across cases ("reasoning integrity," "server-side quality and risk," "pre-commit wallet-state," and similar) are descriptive conveniences, not canonical category names owned by any issuer. Where a label matters -- for integration, for citation, for regulatory reference -- readers should defer to each issuer's own documentation. RNWY's public axes, for example, are **Quality** and **Risk**; other issuers use their own vocabularies, and each case acknowledges its issuers' canonical naming where it differs from the prose shorthand.

## Contributing a case

Any issuer of an ERC-8183 Envelope dimension can contribute a case. Cases can be:

- **Pair cases**: two dimensions meeting at one surface (case 1, case 3 published).
- **Topology cases**: a single implementer writing up how a topology works in production at their surface (case 2 published).
- **Multi-dimension cases**: three or more dimensions at one surface.
- **Format-fit notes**: if exercising the format on a new topology or surface surfaces reframes that extend the format cleanly, those are welcomed as format-fit notes within the case, and folded into the format document on the next publish.

The format is maintained as a living document. Reframes contributed through case writing are credited in the changelog and in the case's own acknowledgments.

## Changelog

- **v5.3 (April 2026)** -- Case 3 (pre-commit transaction-composition, InsumerAPI x ThoughtProof) published. Three-case library complete: case 1 (action-time at MCP invocation), case 2 (wallet-bound aggregation at SkyeProfile), case 3 (action-time at transaction-composition).
- **v5.2 (April 2026)** -- Case 3 (pre-commit transaction-composition, InsumerAPI x ThoughtProof) marked as forthcoming rather than published. The series launches with the first topology established by case 1 (action-time, MCP) and the second topology established by case 2 (wallet-bound aggregation, SkyeProfile); case 3 joins the library on publish as a library expansion. Sequencing note contributed by Douglas Borthwick.
- **v5.1 (April 2026)** -- AVERI citation corrected to match arXiv:2601.11699 institutional naming; license (CC BY 4.0), suggested citation, and versioning scheme added; minor terminology consistency pass.
- **v5 (April 2026)** -- Three topology-neutral reframes contributed by Douglas Borthwick (SkyeMeta) through the wallet-bound case (case 2):
  - "The surface" -> "The surface or decision class," extending the format across action-time and wallet-bound cases without breaking either reading.
  - "Failure modes neither dimension catches alone" -> "Failure modes single-dimension evaluation cannot detect," neutralizing the phrasing across both topologies.
  - "Each dimension can gate execution on its own" -> "Each dimension can independently inform the relying party's decision," extending beyond action-time gating to wallet-bound decision classes.
  - Explicit topology-vs-surface distinction added to "The Envelope, briefly."
  - Series index table added; "Cases in waiting" reorganized by topology.
  - AVERI reference added to "Why this document exists."
- **v4 (April 2026)** -- Cadence framing (TP per-call vs. RNWY daily-cached), scanner-language neutrality ("trips the ruleset" / "meets threshold" rather than definitive diagnosis), RNWY's canonical Quality/Risk naming protected via terminology note, new subsection on RNWY's broader surface (6,369 agent-to-MCP-server links, entity endpoint, signed MCP attestation endpoint). Review feedback from Pablo A. Lopez (RNWY).
- **v1-v3** -- Internal drafting.

## License, citation, and contact

This document, together with published cases, is released under CC BY 4.0 -- contributors retain attribution; reuse, translation, and citation are welcomed without prior permission. Code referenced in the companion case documents is covered by the license of its respective implementation repository.

Suggested citation:

> ThoughtProof, RNWY, and SkyeMeta (2026). _The Envelope in Action: A Format for Lived Composition Cases, v5.3_. Available at https://github.com/erc-8183-cases/envelope-in-action.

Issuers interested in contributing a case, proposing a format-fit reframe, or flagging an inaccuracy can open a pull request or issue at https://github.com/erc-8183-cases/envelope-in-action, or reach the maintainers directly via the contact listed there.

Versioning: the format document uses semantic point releases (v5.1 -> v5.2 -> v6). Minor changes to wording, metadata, or changelog bump the point release; structural changes to the four-section format or invariants bump the major version and trigger review by prior contributors.

## Acknowledgments

The format proposed here was drafted by ThoughtProof, with substantive co-authoring from RNWY (case 1; review feedback that shaped v4), SkyeMeta (case 2; three format-fit reframes folded into v5), and InsumerAPI (case 3; pre-commit predicate and attestation specification).

The wallet-aggregation topology referenced throughout is implemented and documented in the `insumer-examples` repository, which includes working verification code against the participating issuers' JWKS endpoints. Published cases are maintained as separate documents alongside this format proposal.
