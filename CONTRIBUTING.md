# Contributing to the Envelope in Action case library

This repository collects composition cases and format-fit notes for the ERC-8183 Multi-Attestation Envelope. Contributions are welcome from any issuer whose attestation dimension is part of the Envelope, and from practitioners who integrate Envelope attestations at a specific surface.

## What you can contribute

### 1. A new case

Write a case if you are an issuer (or a pair / group of issuers) and you want to show how your dimension(s) compose at a specific surface or decision class.

Use [FORMAT.md](./FORMAT.md) as your structure. The four sections (surface, failure modes, composition-not-redundancy, primitive invariants) are mandatory. Short-hand labels are fine; canonical dimension names should match each issuer's own public documentation.

A good case:

- Pairs (or groups) dimensions where each covers a surface the others structurally cannot see.
- Presents concrete, non-contrived failure modes.
- Avoids single-score / reconciliation framings.
- Carries explicit issuer attribution in the document header.

### 2. A format-fit note

If exercising the format on a new topology or surface surfaces a reframe that extends the format cleanly without breaking prior readings, contribute it as a format-fit note inside your case. Substantive reframes are then folded into the format document on the next point release, with changelog credit.

See case 2 for a worked example.

### 3. A correction or clarification

If a case claim is inaccurate, ambiguous, or has been superseded by an implementation change at an issuer, open an issue or PR. Corrections are merged on maintainer consensus.

## Workflow

1. **Open an issue first** describing what you plan to contribute (new case / format-fit note / correction). This gives the maintainer group a chance to flag overlap with work-in-progress or sequencing considerations.
2. **Fork and branch** (branch name: `case-<number>-<short-slug>` or `format-fit-<short-slug>`).
3. **Open a PR** when your draft is at v1. Use the PR template; one of the maintainers will review within ~48 hours.
4. **Iterate.** Expect light prose review plus technical fact-checking against your own published APIs / docs.
5. **Merge** on maintainer approval. Your case lives at a stable URL from merge onward.

## What does not belong here

- ERC-8183 specification changes (those belong in the spec's authoring process).
- Governance proposals for the broader Envelope ecosystem.
- Marketing copy or token-economy narrative.
- Work product that is not signed off by its issuer of record.

## Code of conduct

Peer-professional. Case documents name the issuers involved; contributors are expected to represent those issuers accurately and in good faith.

## Questions

Open an issue tagged `question` or reach the maintainers via the contacts listed on their respective organizations' websites.
