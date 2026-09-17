# Agentic Credit Infrastructure Convergence — Evidence Base

## Full Source Material

### 1. The Credit-Not-Payment Thesis (AEP)

Source: economicagents.org (AEP — Agent Economic Protocol), August 2026.

Core framing:
> "Autonomous economic agents can now pay for APIs, compute, and data using stablecoins. But the ability to spend is not the same as the ability to spend wisely. The missing runtime layer for economic agents means no budgets, no way to compare prices, no credit history, and no accountability. They are economic actors with no economic intelligence."

> "Payment lets AI agents transact. **Credit lets AI agents scale.**"

AEP-provided problem/solution mapping:
| Problem | AEP Solution |
|---|---|
| Autonomous agents spend with no limits | Budget caps enforced by the smart contract itself |
| No way to compare providers or prices | Intent resolution finds the best option automatically |
| Every payment is a one-off transaction | Credit lines, escrow, and revenue sharing between agents |
| No track record or accountability | Onchain credit scores, analytics, and audit trails |

### 2. The Governance Gap (Datta / Truth Ventures)

Source: Will Jones, TNW, Aug 27 2026. Contributed article by Varun Datta (Web3 VC, CEO Truth Ventures).

Key argument: Wallets are solved; Cloudflare's August 4 2026 launch shipped not just wallets but "delegated authority system that provides the interface required to enable machine agents to make independent decisions on behalf of their owners or funders."

> "Most objections to the machine economy have to do with governance. Companies and individuals are worried about how artificial intelligence will use their money and whether their agents will be able to make the correct decisions on their behalf. What if agents purchase incorrect data sets thousands of times per hour, or fail to stop because the limit is only checked in a monthly reconciliation rather than at the moment of payment? These are serious issues, and the answer has to be enforced on the rails."

Critical insight on card networks: "Card networks are unable to economically price payments at a fifth of a cent because the fixed fee and the dispute infrastructure consume it." Stablecoin micropayments change the arithmetic.

x402 settled: 160M+ transactions, $41.2M, across 7 chains, ~$0.26 avg each (per Agent Economy figures cited).

Where value accrues: "It will be the settlement, metering and identity layers that count most. The biggest technology winners will be the systems working hard behind the scenes."

### 3. The x402 Foundation Operational Launch

Source: Linux Foundation press release, July 14 2026. Coinbase contributed the x402 protocol to the Foundation.

**40 founding members.** Premier: Adyen, AWS, American Express, Circle, Cloudflare, Coinbase, Fiserv, Google, Mastercard, Monad Foundation, MoonPay, Ripple, Shopify, Solana Foundation, Stellar Development Foundation, Stripe, Visa.

General: Aleo, Fireblocks, Galaxia Moneytree, Hecto Financial, Injective, KakaoPay, Kite AI, LayerZero Labs, Merit Systems, NEAR Foundation, Orthogonal, Polygon Labs, Quant Network, SKALE, t54 labs, utexo, World Liberty Financial, zerohash.

Associate: BSV Association, Cardano Foundation, Casper and Japan Contents Blockchain Initiative, OMA3.

Jim Zemlin (Linux Foundation CEO): "AI agents and automated systems are becoming active participants in the global economy, yet they have lacked a native, secure way to transact."

Supporting quotes from members establish a consistent market thesis: agents need open, interoperable payment rails that work across networks (Visa, Mastercard), support both cards and stablecoins, and prevent vendor lock-in. Members include both the traditional payments incumbent tier AND the crypto-stablecoin tier — this is convergence, not competition.

### 4. Handsel — Full Architecture

Source: github.com/charlieseay/handsel (fork of Kairose-master/handsel), Apache 2.0. Live on Base mainnet with real USDC since 2026-07-30. Live on Base Sepolia (sandbox, faucet money) at handsel-nu.vercel.app.

**Pitch:** "The whole product in two clicks. Put a `bounty:$5` label on a GitHub issue → a bot escrows $5 and posts the job. An AI worker claims it, writes the fix, submits a diff; the platform opens the PR; your own CI grades it. Click merge → escrow pays the worker. Everything between your two clicks is agent-to-agent."

**Credit scoring engine** (lib/credit-engine/scoring.ts):
- Behavioral events → weighted score (300–990) → rating (AAA–D) → programmable credit limit → risk level
- Weights: Performance 40% · Reliability 30% · Reputation 20% · Risk 10%
- Self-reported success weighted DIFFERENTLY from ground-truth-verified success (an agent can't inflate its own score)
- Score→rating/risk thresholds are NOT hardcoded: DMN-style decision table editable by admin live at /admin/credit-rules

**On-chain layer:**
- ERC-4337 smart accounts (Kernel v3.1); gas sponsored on testnet, self-paid from ETH float on mainnet
- Scoring engine mirrors every recalculated limit to on-chain registry + EAS attestation
- Optional: with env vars unset, everything runs off-chain identically

**Labor Market** (the core two-sided market):
1. Requester escrows USDC bounty on-chain + writes specific acceptance criteria + may attach source material (PDF/CSV/text/MD, Vercel Blob-backed)
2. Worker whose score clears job threshold accepts
3. Accepting dispatches the worker's real runtime (platform Claude runtime, owner webhook, cloud API key, or external MCP agent) — genuine work, not button-pretending
4. Real output submitted on-chain automatically when run finishes
5. **Auto-graded code jobs**: requester attaches Python acceptance tests; platform runtime (never worker's own) runs tests in sandbox; pass/fail verdict → graded fact (JOB_TESTS_PASSED/FAILED); failed → auto-refund + same spec reposted (cap 2 auto-reposts); passed → auto-release escrow IF requester opted in (cap AUTO_APPROVE_MAX_BOUNTY_USD default $50)
6. No acceptance tests → requester reviews real output: Approve (escrow releases) or Dispute
7. Disputed → locks until independent party (not requester, not worker) reviews actual requirements vs actual output + test verdict + force-settles — requester can't withhold payment forever by refusing to click Approve

**Proving Ground / Verified Tasks** (the trust answer to "an AI grading its own work isn't a credible reputation signal"):
- Server procedurally generates problem + hidden answer (grader ≠ solver — solving agent never sees answer)
- Escrows bounty on-chain
- On callback: grades real output against hidden ground truth server-side
- Correct answer settles escrow via commit-reveal (front-running resistant)
- Credit events from this path marked as verified facts, not self-evaluated opinions; scoring engine weighs accordingly
- Cross-user: doesn't dispatch immediately (would run stranger's agent and bill their key without consent); escrows bounty + sends proposal via agent-to-agent negotiation channel; accepting kicks off solve under solver owner's own session

**Agent Template Marketplace:** publish agent's "recipe" (custom instructions) for others to spawn copies, priced or free. Listings show genuine portfolio from exemplar agent's real history (current score, verified-task pass count, real sample outputs) — never marketing claims. **Credit history never transfers**: cloned agent starts at real cold start (score 0, unrated).

**Agent-to-agent negotiation** (/messages → Agent Negotiations): structured machine-readable channel for division of labor (subcontract proposals, counters, accept/reject, questions). Open by design (any registered agent → any other), per-sender rate limit + block list. NEVER moves money or creates binding obligation by itself — accepting proposal is just information; posting actual escrowed job with agreed terms is always separate explicit step.

**Bring any agent (MCP-worker adapter):** any MCP-speaking agent (LangGraph, CrewAI, custom Python loop, another platform's agent, zero-dep reference server) hired as first-class worker. Paste Streamable-HTTP URL + tool name + optional auth header → platform probes tool, mints per-agent webhook secret, calls that MCP server whenever dispatched a job. Auto-mine sweeps 'mcp' workers opportunistically.

**BYO Agent paths:** Cloud API worker (no terminal, AES-256-GCM encrypted key, server-side calls), Local worker (one command `node handsel-worker.mjs --token …`, polls outbound like CI runner, Ollama/LM Studio/OpenAI-compatible), Webhook (POST to your https endpoint).

**BYOK:** user stores own Anthropic API key (AES-256-GCM encrypted, never logged/returned) so their agent runs bill their own account — what makes public deployment cost-sustainable.

**Public spectacle** (/live, /directory, /world): no-login shareable views of the real economy. `/live` self-updating "mission control" with animated counters, on-the-floor-now panel, streaming activity feed, top-earners board — every number a live `getGuestOverview` query, nothing invented. Plus a Minecraft rendering (read-only Paper plugin) floating open jobs as in-world holograms.

**Access control:** real matrix `admin_grants` (user × permission), not single hardcoded admin flag. Different accounts hold different capabilities (disputes, credit_rules, ...). One `ADMIN_EMAIL` is superadmin bootstrap implicitly holding every permission (matrix can never lock operator out). `/admin/credit-rules` lets `credit_rules` permission holder edit score→rating and score→risk-level decision tables directly — actual lending policy, changeable with no code deploy.

**Balance sheet:** every agent gets real financial statement — Assets (USDC balance, undrawn credit line, receivables [bounties escrowed for delivered not-yet-approved work]) − Liabilities (outstanding drawn credit) = Net Worth. Every figure live read, nothing inferred.

**Known limitations (honestly stated):**
- Risk Analytics (/risk) real; Insurance (/insurance) honest placeholder — no fake capital pool
- Approve/Dispute user-triggered for jobs without acceptance tests; auto-graded jobs settle both directions
- Job attachments only support text-extractable formats (HTML/text/CSV/JSON/MD/PDF via pypdf); binary formats upload fine but worker runtime honestly reports it can't read them rather than fabricating content
- No formal security audit of Solidity contracts; live on Base mainnet holding real funds since 2026-07-30; "start with amounts you would shrug at"

**MCP connector:** 28 tools across hiring, earning, proofs, governance, DeFi sandbox (testnet). Same endpoint lets Claude/ChatGPT hire a swarm AND (via `connect_mcp_worker` + `set_auto_mine`) lets any external MCP-speaking agent get hired as graded auto-mining worker.

**Self-sybil-attack disclosure** (docs/self-sybil-attack.md), **failure modes disclosure** (docs/failure-modes.md — every production defect that froze or lost money + root cause + fix), **security audit disclosure** (docs/security-audit.md — same defects organized by adversary + severity). Radical operational transparency.

Install the skill for agents (one command, no account/wallet/OAuth):
```bash
curl -fsSL https://handsel-main.vercel.app/install-skill.sh | sh
```

### 5. AEOS / Phanes — Full Module Inventory

Source: github.com/tofaelttk/phanes, Apache 2.0, PyPI `pip install phanes`.

19 modules, 84 tests (incl. Fiat-Shamir tamper detection, VSS tamper detection, tamper rejection):

| Layer | What It Solves | Status |
|---|---|---|
| Identity | DID-based agent identity, selective disclosure, delegation chains | Complete |
| Contracts | Binding agreements, escrow, milestone release, penalty enforcement | Complete |
| Disputes | Auto-resolution, VRF arbitrator selection, confidence-weighted voting | Complete |
| Risk | Behavioral profiling, circuit breakers, counterparty scoring, insurance pools | Complete |
| ML Engine | Isolation Forest anomaly detection, Markov models, entropy drift detection | Complete |
| Graph Intel | PageRank trust, collusion detection, cascade simulation, Sybil detection | Complete |
| Threshold Crypto | Shamir secret sharing, t-of-n signatures, time-lock puzzles | Complete |
| Tokenization | Programmable tokens with decay, staking, accrual, governance | Complete |
| State Channels | Off-chain micro-transactions, cooperative/force close | Complete |
| BFT Consensus | PBFT distributed ledger, view changes, quorum certificates | Complete |
| Stripe Settlement | PaymentIntent escrow, authorize-then-capture, refund on dispute | Complete |
| USDC Settlement | On-chain ERC-20 escrow on Ethereum, Base, Arbitrum, Polygon | Complete |
| Persistence | SQLite WAL-mode, ACID transactions, schema migrations, crash recovery | Complete |
| MCP Server | 11 tools for Claude/GPT native integration via Model Context Protocol | Complete |
| REST API | FastAPI server, 17 endpoints | Complete |
| TypeScript SDK | Full client library with crypto, identity, contracts, typed HTTP client | Complete |
| Bulletproofs | Rust Ristretto255 zero-knowledge range proofs with Python FFI | Complete |
| Formal Verification | TLA+ specs for contract escrow and PBFT consensus safety proofs | Complete |
| Ledger | Append-only hash chain, Merkle proofs, full audit trail | Complete |

Comparison table AEOS provides vs Stripe MPP / Skyfire KYA / Google AP2: AEOS includes binding contracts, escrow+milestones, dispute resolution, risk engine, anomaly detection, threshold crypto, state channels, graph intelligence, BFT consensus, zero-knowledge proofs, on-chain settlement, database persistence, formal verification, MCP integration, TypeScript SDK, immutable audit trail — none of which Stripe/Skyfire/Google include.

CSV/Stripe example:
```python
from aeos.settlement import StripeSettlementEngine
engine = StripeSettlementEngine("sk_test_...")
result = engine.create_escrow("contract-001", 25000, "usd", "did:alice", "did:bob")
engine.capture_escrow("contract-001")    # On fulfillment
engine.refund_escrow("contract-001")     # On dispute
```

### 6. Souq — Three-Party + Encryption Detail

Source: github.com/s0nderlabs/souq, Apache 2.0. First ERC-8183 implementation.

Three participants: Client (posts, funds escrow), Provider (delivers encrypted work), Evaluator (reviews, approves/rejects, triggers payment 90% provider / 5% platform fee / 5% evaluator fee). Two markets: Direct Assignment (client picks upfront) + Open Market (agents bid, client picks).

Hybrid ECIES + AES-256-GCM encryption for deliverables:
```
Provider submits work:
  plaintext → AES-256-GCM(random key) → ECIES wrap key for evaluator's pubkey → IPFS

Evaluator approves:
  ECIES unwrap with evaluator's privkey → ECIES re-wrap same AES key for client's pubkey → IPFS

Client reads:
  ECIES unwrap with client's privkey → AES-256-GCM decrypt → plaintext
```
Agents derive keypairs from BIP-39 seed via BIP-44 path `m/44'/60'/0'/0/0`; humans derive keypairs from deterministic wallet signature. Encrypted blob never re-encrypted — only AES key wrapper changes during re-encryption.

22 MCP tools: setup_wallet + get_wallet_info (wallet); create_job / set_provider / set_budget / fund_job / submit_work / complete_job / reject_job / claim_refund / apply_for_job (job lifecycle); get_job / list_jobs / get_notifications / read_deliverable (read+notify); register_identity / give_feedback (identity+reputation); create_policy / trigger_assessment / check_compliance (Sigil compliance).

x402 micropayments to relay: each API call 0.001 USDT, signed + settled via EIP-3009 `TransferWithAuthorization`. 50 free calls after faucet; safe deployment + read-only RPC always free.

Deployed on Sepolia: AgenticJobEscrow 0x2AE8... / SigilGateHook 0xEB5d... / USDT0Mock 0xABfd... / IdentityRegistry (ERC-8004) 0x8004... / ReputationRegistry 0x8004... / Sigil 0x2A1F...

### 7. TaskMarket — Adoption Snapshot

Source: cryptobriefing.com via Daydreams Systems, Aug 24 2026. taskmarket.dev.

~980 distinct addresses interacted. 373 tasks posted → 16,640+ submissions (44+ submissions/task avg → competitive environment, not tumbleweeds). 399 submissions reached settlement. 177 worker agents received USDC payouts. Daydreams seeded early activity with 20 builder bounties worth $1,000 each.

Five ways to structure work: Bounty, Claim, Pitch, Benchmark, Auction. Each suited to different task + verification approach.

Daydreams actively drafting + publishing ERC-8004 agent identity standard via EIP process. If adopted broadly → TaskMarket's protocols become infrastructure rather than one company's product.

USDC settlement chosen deliberately: insulates workers from volatility during task-acceptance-to-payment window; makes platform financials auditable (native token rewards aren't).

### 8. Apex Fusion Vector — Switzerland-as-Service

Source: zycrypto.com (sponsored), Aug 18 2026. apexfusion.ai.

"Vector is a purpose-built implementation of Cardano's protocol stack." eUTXO accounting model deliberate fit: "an agent committing capital needs to know the exact cost and outcome before it commits." eUTXO = deterministic transactions, low/known-in-advance fees, failed transactions cost nothing on-chain, parallelizes for throughput.

11 months live on mainnet. Pilot with OriginTrail (Decentralized Knowledge Graph = decentralized infrastructure for multi-agent AI memory; agents publish + query shared knowledge as cryptographically verifiable assets). Vector bonds job + holds escrow; agents do work; results published to DKG as verifiable knowledge assets; job settles against a result that can be independently checked rather than merely asserted. "Escrow and proof stop being separate systems."

Ancestry project pilot: agents rebuilt 385,000-record WWI archive into knowledge graph across 20,000+ work packages, running full marketplace lifecycle themselves. Every extracted fact traces to model that produced it + terms contracted under + settlement that closed the job. Public at genealogy.vector.apexfusion.org.

Christopher Greenwood (CEO Apex Fusion Foundation): "Inside your own walls [internal agent governance] is achievable... The question we have been living with for a year is what happens when your agents leave the building."

"The agent economy needs a Switzerland, so we built one. Neutral, verifiable, stewarded by a Swiss foundation, and open by design. The intelligence layer arrived faster than anyone predicted. The trust layer is the part we chose to build."

MCP-native: agent built on Claude/GPT/Cursor/custom stack integrates through a single connection. "Point it at the open-source repositories, hand it the bootstrap prompt, and it can register, post or take jobs, deliver work, and settle."

### 9. Aixyz — Next.js-for-Agents Pattern

Source: github.com/AgentlyHQ/aixyz, 82 stars. aixyz.sh.

`bunx create-aixyz-app my-agent` → agent exposes three endpoints automatically:
- `/.well-known/agent-card.json` (A2A discovery)
- `/agent` (A2A JSON-RPC + x402 payment gate)
- `/mcp` (MCP tool sharing)

Config declares identity + payment address:
```ts
const config: AixyzConfig = {
  name: "Weather Agent",
  x402: { payTo: "0x...", network: "eip155:8453" }, // Base mainnet
};
```

Agent + price:
```ts
export const accepts: Accepts = { scheme: "exact", price: "$0.005" };
```

Per-tool MCP payment gating:
```ts
export const accepts: Accepts = { scheme: "exact", price: "$0.0001" };
```

CLI: `aixyz erc-8004 register` / `aixyz erc-8004 update`. Auto-generated AgentCard with Open Graph tags.

11 examples: boilerplate, chainlink, flight-search with Stripe, local-llm via Docker, custom-facilitator, custom-server, express, sub-agents, with-tests, fake-llm (deterministic testing), with-vercel-blob.

### 10. Lucid Agents — Protocol-Agnostic SDK

Source: github.com/mammothina/lucid-agents (fork of daydreamsai/lucid-agents), MIT. docs.daydreams.systems.

9 packages: @lucid-agents/types, core, http, wallet, payments, analytics, identity, a2a, ap2, plus adapters (hono, tanstack, express, next) + cli.

Architecture: Layer 1 Core (protocol-agnostic runtime with extension system, no protocol-specific code). Layer 2 Extensions (http(), payments(), wallets(), identity(), a2a(), ap2() — optional capabilities added via composition). Layer 3 Adapters (framework integrations using the HTTP extension).

Payment policies (bi-directional):
```ts
payments({
  config: {
    policyGroups: [{
      name: 'Daily Limits',
      outgoingLimits: { global: { maxTotalUsd: 100.0, windowMs: 86400000 } },
      incomingLimits: { global: { maxTotalUsd: 5000.0, windowMs: 86400000 } },
      blockedSenders: { domains: ['https://untrusted.example.com'] },
    }],
  },
  storage: { type: 'sqlite' },
})
```

Accept payments on EVM (Base, Ethereum, Sepolia) or Solana (mainnet, devnet). Auto-detects EVM vs Solana from address format. Payment analytics with summary stats + transaction history + CSV/JSON export for accounting systems.

Entrypoint example (paid + streaming):
```ts
addEntrypoint({
  key: 'chat',
  streaming: true,
  async stream(ctx, emit) {
    const stream = await ai.chat.stream({ messages });
    for await (const chunk of stream) {
      await emit({ kind: 'delta', delta: chunk.delta, mime: 'text/plain' });
    }
    return { output: { completed: true }, usage: { total_tokens: stream.usage.total_tokens } };
  },
});
```

## 8 Convergence Patterns

1. **ERC-8004 identity standard** — every project reads/writes it; Daydreams drafting the EIP; Apex uses staked reputation behind claimed capabilities
2. **x402 payment rail** — HTTP-native, sub-cent micropayments; Linux Foundation July 2026 governance; AEP intercepts/governs x402 before execution; Souq uses EIP-3009 TransferWithAuthorization at 0.001 USDT/call
3. **ERC-4337 smart accounts** — programmable wallets with spending policies; Handsel Kernel v3.1; Souq WDK; AEOS supports both ERC-4337 kernel and EOA modes
4. **Escrow-with-verification** — Universal: funds locked until work accepted/graded. Handsel auto-grading via acceptance tests run by platform runtime (never worker's own). Souq 3-party (Client/Provider/Evaluator). Apex bonded escrow + staked-jury dispute. TaskMarket 5 modes including Benchmark.
5. **Behavioral credit scoring** — reputation from verified work history not self-reported claims. Handsel: 300–990 score, Performance 40% + Reliability 30% + Reputation 20% + Risk 10%, Proving Ground grader≠solver. AEP: payment/revenue/reliability history → credit scores. AEOS: behavioral profiling + Isolation Forest anomaly detection + Markov + entropy drift + graph intelligence (PageRank trust, Sybil detection). Apex: staked reputation behind claimed capabilities.
6. **MCP-native** — Handsel 28 MCP tools via remote MCP server (OAuth in browser). AEP 15+ MCP tools via `npx @economicagents/mcp`. AEOS 11 MCP tools (JSON-RPC/stdio). Souq 22 MCP tools (`npx -y @s0nderlabs/souq-mcp@latest`). Apex MCP-native single-connection integration. Aixyz `/mcp` endpoint built-in. Lucid Agents `a2a()` extension.
7. **On-chain compliance/kyc hooks** — Souq Sigil Scribe AI policy generation + on-chain SigilGateHook enforcement (provider + evaluator must pass all listed policies or contract reverts). AEOS risk engine + counterparty scoring. Apex staked reputation + bonded escrow.
8. **Dispute resolution first-class** — AEOS 3-tier auto-resolution with VRF arbitrator selection + confidence-weighted voting. Apex staked-jury dispute resolution. Handsel independent arbiter (not requester, not worker) force-settles. Souq Evaluator (3rd party) approves/rejects.

## Key Distinctions Across Projects

| Project | Settlement Layer | Identity | Credit/Reputation | Scope |
|---|---|---|---|---|
| Handsel | Base mainnet USDC | ERC-8004 + EAS attestation | Full behavioral score 300-990 | Marketplace + credit + escrow + work-proofs + agent-to-agent negotiation + BYO-agent (MCP/webhook/cloud/local) |
| AEP | ERC-4337 + x402 | ERC-8004 | Payment history + revenue consistency + reliability → credit | Runtime layer for any agent framework; intent resolution; fleet management |
| AEOS/Phanes | Stripe + USDC multi-chain | DID | ML anomaly + graph intel + risk profiling + insurance pools | 19-module "economic OS" including threshold crypto + BFT + formal verification |
| Souq | Base Sepolia (ERC-8183) | ERC-8004 | ReputationRegistry (give_feedback score 0-100) | 3-party marketplace with ECIES encryption + Sigil compliance |
| TaskMarket | Base Mainnet USDC | ERC-8004 (drafting EIP) | implicit (submission/settlement history) | 5-mode task marketplace + Lucid Agents SDK + Dreams Router |
| Apex Vector | Cardano eUTXO (AP3X) | staked reputation | staked reputation (full chain of custody) | Neutral settlement layer + OriginTrail DKG provenance; 20K+ work packages |
| Aixyz | x402 + ERC-8004 + ERC-4337 | ERC-8004 | (delegated to wallet policies) | Next.js-like framework; one-command scaffold; auto-expose A2A+MCP+x402 |
| Lucid Agents | x402 + AP2 + ERC-8004 | ERC-8004 | (policy-based, persistent storage) | Protocol-agnostic multi-runtime SDK |

## Synthesis: Why This Is the Maturation Point

The simultaneity matters. Seven or more open-source projects launch production-grade agent credit infrastructure between June-August 2026, all converging on the same 4-layer stack, all Apache 2.0/MIT. The x402 Foundation operational launch (July 14, 2026) provides the governance layer with 40 members spanning Visa/Mastercard/Stripe/AWS/Google AND Solana/Stellar/Ripple/Cardano — meaning the traditional payments incumbent tier and the crypto/stablecoin tier are NOW collaborative rather than competitive. The credit-not-payment thesis (AEP, Datta) reframes the value-accretion question: not "will agents pay" (solved) but "will agents scale economically" (open problem solved by credit).

The universal trust principle: **self-reported success ≠ independently-verified success**. Handsel's Proving Ground (grader ≠ solver), Souq's 3-party Evaluator, Apex's staked-jury, AEOS's VRF arbitrator, AEP's verified-task path — all encode this. No project allows an agent to inflate its own credit score by grading its own homework.

For the open-source agent economy, 2026 H2 is the moment credit became the primitive that lets autonomous agents scale beyond a single wallet balance, with 7+ reference implementations to learn from.
