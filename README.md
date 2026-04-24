# The Envelope in Action

A library of lived composition cases for the [ERC-8183](https://github.com/ethereum/ERCs) Multi-Attestation Envelope — how independent issuer attestations compose across surfaces in the agent lifecycle.

## What this is

ERC-8183 defines a multi-attestation envelope for agent trust: multiple issuers signing independent attestations across orthogonal dimensions, each verifiable offline against its own JWKS. The **spec** tells you how the envelope is structured. This **case library** shows how envelope attestations actually compose — at real surfaces, against real failure modes, by real issuers.

Each case follows a four-section format defined in [FORMAT.md](./FORMAT.md):

1. The surface or decision class
2. Failure modes single-dimension evaluation cannot detect
3. Why this is composition, not redundancy
4. What this demonstrates about the Envelope as a primitive

## Published cases

- **Case 1** — [Agent-to-MCP-server invocation](./cases/01-mcp-invocation.md) (RNWY × ThoughtProof)
- **Case 2** — [Wallet-bound aggregation at pre-transaction trust](./cases/02-wallet-bound-aggregation.md) (SkyeMeta)
- **Case 3** — [Pre-commit composition at transaction-composition time](./cases/03-pre-commit.md) (InsumerAPI × ThoughtProof)

## What this repository is, and is not

**Is:** a library of contributed composition cases and format-fit notes for ERC-8183's Multi-Attestation Envelope.

**Is not:** governance of the ERC-8183 specification itself. The spec lives in its own authoring process; this repository is downstream reference material for practitioners, compliance readers, and policy audiences.

## Maintainers

- Raul Jäger (ThoughtProof)
- Douglas Borthwick (InsumerAPI & SkyeMeta)
- Pablo A. Lopez (RNWY)

Case-level attribution (who authored each case) stays split per case and is recorded in each document's header and in the format document's series index.

## Contributing

Any issuer of an ERC-8183 envelope dimension can contribute a case, a format-fit reframe, or a correction. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the workflow.

## License

All documents in this repository are licensed under [CC BY 4.0](./LICENSE). Reuse, translation, and citation are welcomed without prior permission; please preserve attribution to contributors and link back to this repository.

## Context

This case library is designed to complement organization-level frontier-AI auditing frameworks (e.g. [Brundage et al. 2026, AVERI](https://arxiv.org/abs/2601.11699)) by operating at the deployment and per-action layer those frameworks explicitly scope out. The Envelope is the composition primitive; these cases are the lived evidence that composition works.
