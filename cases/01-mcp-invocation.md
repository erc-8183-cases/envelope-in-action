# The Envelope in Action: Action-Time Gating at Agent-to-MCP-Server Invocations

_Draft v1 — April 2026_
_First action-time case, contributed by: RNWY, ThoughtProof_

_Contributed under the case format proposed in "The Envelope in Action" (ThoughtProof, April 2026). Open to feedback from all Envelope issuers._

## Why this case exists

"The Envelope in Action" proposed a four-section format for supplementary case documentation and introduced the wallet-bound aggregation topology via SkyeProfile. This document contributes the first action-time case, using the agent-to-MCP-server surface where two dimensions on different native cadences — reasoning integrity and server-side quality and risk — meet at the point an agent is about to act.

Action-time gating asks a different question than wallet-bound aggregation: where wallet-aggregation cases ask "what does the Envelope say about this wallet?", action-time cases ask "do all relevant dimensions approve this particular action?" Both patterns use the same envelope format and the same issuer-signed attestations; they differ in when composition happens and what is being composed around.

## The surface

Model Context Protocol servers are becoming the de facto tool-integration layer for agents. An agent reads from an MCP server (prices, balances, user context), reasons over what it reads, then acts. Two things can go wrong — and they go wrong at different layers.

At this surface, two dimensions meet on different native cadences:

- **Reasoning integrity (ThoughtProof).** Produced per-call, once the model emits a candidate action.
- **Server-side quality and risk (RNWY).** Produced continuously. RNWY runs daily sweeps against stored server manifests; the action-time signal is the latest cached finding for the server in question, including endpoint quality, reputational signals, and threat-ruleset outcomes.

The composition is still action-time: both signals are consulted when the agent is about to act, and either can gate execution on its own. But they live on different timelines, and the case below uses each in its native mode.

## Failure mode 1: clean server, bad reasoning

The agent calls a legitimate price oracle. The oracle returns accurate data. The model hallucinates that ETH is priced at $50 and executes a terrible swap.

- **RNWY (server quality and risk):** clean. The latest cached scan shows the server passes the threat ruleset and scores well on quality and reputation.
- **ThoughtProof (reasoning integrity):** deny. The verdict chain shows the model's conclusion is not supported by the input it received.

The server-side signal says proceed. Reasoning integrity says stop. The agent stops.

## Failure mode 2: compromised server, sound reasoning

An injected MCP server returns plausibly-formatted but poisoned data. The model reasons correctly given that input — because the input itself is the attack.

- **RNWY (server quality and risk):** deny. The latest cached findings trip the scanner's threat ruleset, or surface quality or reputational indicators that meet RNWY's action-time threshold.
- **ThoughtProof (reasoning integrity):** allow. Given what the model was given, its reasoning is internally sound.

Reasoning integrity says proceed. The server-side signal says stop. The agent stops.

## Why this is composition, not redundancy

A monolithic "agent safety score" would have to reconcile these two signals into one number — and lose information in the reconciliation. Either the allow gets averaged with the deny into a soft-yellow "uncertain," or one signal dominates the other by weighting convention.

The Envelope composition is different: both signals remain independent, signed, and auditable, and either one can gate execution on its own. A downstream resolver or keeper consumes both as independent evidence. No interface coupling is required between issuers — neither needs to know the other exists, and neither needs to match the other's cadence. The composition happens at the consumption layer.

This is defense in depth via orthogonal verification. Server-side quality and risk cover a surface that reasoning integrity structurally cannot see (server compromise, degraded endpoints, reputational red flags), and vice versa (internal-consistency failure on clean input). That complementarity is what distinguishes composition from redundancy.

## A note on RNWY's broader surface

The two failure modes above use the per-server signal because that is what the surface requires. RNWY's production footprint at this intersection is larger than the single-server scan, and future cases in this format are expected to exercise more of it:

- **6,369 agent-to-MCP-server links**, with 40+ bidirectionally verified, enabling wallet → agent → server traversal rather than isolated server scoring.
- **An entity endpoint** that returns a wallet together with its associated agents and MCP servers in a single call.
- **A signed MCP attestation endpoint** live in production.

Dimension isolation — using only what a given case requires — is part of the format's discipline. The richer graph is relevant for adjacent surfaces such as agent-to-agent delegation and wallet-scoped agent inventories, not for the two failure modes here.

## A note on terminology

Short-hand labels used above ("reasoning integrity," "server-side quality and risk") are descriptive conveniences for this case, not canonical category names owned by either issuer. RNWY's own public axes are **Quality** and **Risk**. Where a label matters — for integration, for citation, for regulatory reference — readers should defer to each issuer's own documentation.

## Acknowledgments

Co-developed with RNWY during April 2026, after RNWY's MCP scanner endpoint (`rnwy-mcp-v1`) reached signed-endpoint status and ThoughtProof's verdict API reached production on Base. The two failure modes were articulated jointly in that exchange, and the framing of RNWY's cadence and surface scope reflects review feedback from the RNWY team.

The wallet-aggregation topology referenced above is implemented and documented in the `insumer-examples` repository, including working verification code for all participating issuers.
