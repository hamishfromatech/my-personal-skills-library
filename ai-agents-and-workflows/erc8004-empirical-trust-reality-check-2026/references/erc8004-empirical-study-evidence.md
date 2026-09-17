# ERC-8004 Empirical Study — Full Evidence Base

## Citation

Xiong, X. (Imperial College London), Li, Z. (Ohio State), Wei, W. (Bristol), Wang, Q. (CSIRO), Knottenbelt, W. (Imperial College), Wang, Z. (Manchester). "Can Trustless Agents Be Trusted? An Empirical Study of the ERC-8004 Decentralized AI Agent Ecosystem." arXiv:2606.26028 (v2). First multi-chain empirical study of ERC-8004. Dataset available on request for reproducibility.

## Protocol Background

- ERC-8004 "Trustless Agents" — authors: Marco De Rossi (MetaMask), Davide Crapis (Ethereum Foundation), Jordan Ellis (Google), Erik Reppel (Coinbase). Created 2025-08-13; still **Draft** status.
- Reference contracts deployed to Ethereum mainnet January 29, 2026; BSC and Base ~Feb 3, 2026.
- Position in agentic-internet stack: Application → Trust (ERC-8004) → Payment (x402) → Communication (A2A/MCP) → Settlement.
- Three on-chain singleton registries per chain:
  - **Identity Registry** (ERC-721 + URIStorage): agentId, owner, agentURI → off-chain registration file (services array: A2A, MCP, OASF, ENS, DID, web; x402Support flag; supportedTrust array).
  - **Reputation Registry**: giveFeedback(agentId, value int128, valueDecimals ≤18, tag1, tag2, endpoint, feedbackURI, feedbackHash); revokeFeedback; appendResponse; getSummary(agentId, clientAddresses, tag1, tag2) — plain mean over selected non-revoked records.
  - **Validation Registry**: validationRequest/validationResponse (0–100 verdict). **No mainnet deployment observed during the study window** — the high-assurance tier (stake re-execution, zkML, TEE) is aspirational.
- Only integrity constraint enforced on-chain: agent's owner/operator cannot rate own agent.

## Dataset

- Chains: Ethereum (32,343 agents), BSC (90,145), Base (50,985) — 173,441 total; crawl window through May 13, 2026.
- Reputation market: ETH 3,058 feedback / 1,559 rated agents / 618 reviewers; BSC 29,444 / 4,310 / 76; Base 122,798 / 28,592 / 3,073. Total >150k feedback records, >170k agents.
- Collected: on-chain events, off-chain registration/feedback files, gas costs (USD via Binance hourly prices), x402 settlements (operational definition: paired AuthorizationUsed + Transfer on USDC, i.e., EIP-3009 upper bound).

## Identity Findings

- Registration: ETH 32k / BSC 90k / Base 51k. Only 29.4% (ETH), 83.4% (BSC), 26.9% (Base) have valid registration files.
- Batch registration: on ETH, 2.6% of txs created 48.3% of agents (446 batch txs → 15,607 agents, max 182, median 10).
- Ownership concentration (Gini): ETH 0.733, Base 0.708 (top 1% own 58.5%/54.9%); BSC 0.134.
- Identity–activity gap: 53% (ETH) / 9% (BSC) / 37% (Base) never activated a URI. Activation, when it happens, is immediate (92–93% within one day) — agents not activated early likely never will be.
- Agent quality (five categories): fully functional (valid file + live service) = 3% ETH, 4% BSC, 15% Base.
- Quality typology: empty 49.5%, templated-inert 47.0%, thin-active 2.6%, **genuine 0.9%**. Base hosts nearly all genuine agents.
- Service types: BSC web-centric (~70%); Base strongest agent-native mix (MCP 29.1%, web 26.0%, A2A 14.6%).

## Reputation Market Findings

- Tag semantics collapse: untyped value field (−10³⁸ to 10³⁸, ≤18 decimals). Real examples in data: 0–100 personality ratings (BSC, median 70), 0–5 health-checks (ETH), booleans (Base trade all 0), open revenue metrics (Base, mean 1,639), signed identity (−10). Same tag `review` on ETH: median 4, IQR 4–86 (0–5 and 0–100 scales mixed).
- Cross-chain portability fails: 629 multi-chain agents (0.4%); BSC–Base score correlation Spearman ρ=0.05 (p=0.56); ETH–Base ρ=0.14 (p=0.48). Each chain is an isolated reputation silo.

## Security Findings (C1–C4 detail)

### C2 Robustness — the math
- Mean aggregator breakdown point = 0. Single crafted value v* = (n+1)τ − nm moves score from m to target τ regardless of n.
- Admissibility: |v*| ≈ n|τ−m| stays ≪ 10³⁸ until n > 10³⁶–10³⁸ (gap 1–100). Most-rated agent: n=1,552 — thirty orders of magnitude short. Universe-age feedback volume (~10¹⁷) still 20 orders short.
- Clamped values: k = n(τ−m)/(v_max−τ) ceiling-valued ratings suffice; at τ=90 median agent flipped by k=1; 68–88% by ≤5; still >60% at τ=95.
- Median fix would give 50% breakdown point — recommendation #3.

### C3 Groundedness
- Feedback classified by strongest declared evidence: no payment proof AND no task linkage = 98.7% (ETH), 100% (BSC), 99.3% (Base).
- Reviewer-level test (conservative): reviewer has ANY x402/EIP-3009 USDC transfer ever. Base: 6.2% of reviewers qualify, submitting 5.1% of feedback. 93.8% of reviewers submitted 94.9% of feedback with no payment history. Conservative lower bound — stricter pair-level test would only increase unsupported share.
- Table: ETH payment-proof feedback 0.07% (100% Sybil-flagged), task linkage 1.2% (100% flagged), no evidence 98.7% (40.6% flagged). Base: payment proof 0.6% (92.6% flagged — same as no-evidence 92.6%).

### C4 Cost
- Median per-feedback gas cost: $0.055 ETH / $0.0042 BSC / $0.0027 Base.
- Median attack cost = median per-feedback cost (k=1).
- Value at stake (attributable x402 volume per Base agent): mean $16.74, median $0.70. Attack cost 259× lower than median value at stake.

### Sybil analysis
- Detection: shared-first-funder directed graph (A→B means A first funded B); cross-chain merging; contract funders resolved to operator EOA (155/157).
- Sybil-flagged reviewers: 73.5% ETH, 59.2% BSC, 90.6% Base.
- Behavior patterns: repeated targeting (low fan-out, high repeated-feedback share — e.g., BSC group: 20 wallets, 21,863 records over 312 agents, Fanout 0.014, RS 0.873, score 70 = 73.2% of records, 6,295 records in one 24h window) and queue sweeps (Base: 80 contract-funded wallets × 10 feedback txs each × score 100, clone-like nonce templates).
- Entropy-based template concentration: D_score = 2^(−Σp log₂ p); low D = template reuse.

### Market damage
| Chain | Sybil feedback | Affected agents | No baseline | Median Δ (mixed cases) |
|---|---|---|---|---|
| ETH | 41.4% | 26.4% | 15.8% | +11.0 |
| BSC | 96.3% | 81.4% | 77.9% | −9.1 |
| Base | 92.6% | 96.2% | 86.8% | −0.2 |

- ETH breakdown by quality: Valid-with-service agents 81% affected, median Δ +15.9 (targeted inflation of hireable agents). Base: manipulation indiscriminate (94–98% affected across all quality tiers).

## Protocol Recommendations (full detail)

1. **Liveness predicate**: single canonical test (URI resolves + compliant file) so all consumers validate identically; hide inactive identities from discovery.
2. **Typed value field**: tag registry with unit/range/direction per tag; contract-enforced ranges; canonical overall-rating tag; aggregate within-tag only. Precedents: QuantuLabs SDK encodes $150.00 as value=15000, decimals=2; Nuwa fork clamps 0–100.
3. **Robust aggregation**: median/trimmed mean; clamping; per-reviewer contribution caps (observed max 1,181 records/pair); weight by distinct evidence-backed reviewers. Isolated precedent: Helixa computes its own 0–100 Cred Score ignoring the raw mean (but only Helixa users benefit).
4. **Verifiable interaction requirement**: require settled x402 payment or attested Validation Registry task; at minimum record backed/unbacked counts in default summary.
5. **Influence cost scales with stakes**: slashed stake or weight tied to settled payment volume; cost of changing a score should grow with the value it controls.
6. **Default Sybil defense**: reference filter + weighting scheme; tie influence to stake/payment proof/attested identity; per-funder and per-cluster caps.
7. **Sequence portability**: verifiable cross-chain identity binding + per-chain integrity BEFORE reputation sharing; else weakest chain contaminates all.

## Worked Example (Appendix E)

Agent with n=1,552 honest records at mean 99.9. Adversary submits one record: v* = (1553×0) − 1552×99.9 ≈ −1.55×10⁵. |v*| ≈ 10⁵ ≪ 10³⁸. Score collapses to 0. Same construction inflates an unknown agent to the top at equally negligible cost. Honest density never dilutes the attack within any realistic n.

## A-Tech Applications

- **Due-diligence checklist** for any agent marketplace, credit bureau, or reputation-gated hiring system: run the C1–C4 questions before wiring value.
- **Design guide** for A-Tech-aligned builders (the credit-infrastructure projects in this library): implement recommendations 2–6 by default; treat recommendation 7 as a sequencing constraint.
- **Content angle**: "The agent economy's trust layer failed its first audit" — empirical verification vs ecosystem hype; connects to A-Tech's open-source ethos because the study itself is open, on-chain, and reproducible.

## Cross-References

- agentic-credit-infrastructure-convergence-2026: the 7+ project convergence assumed a working ERC-8004 reputation substrate; this study supplies the evidence that behavioral scoring layers (Handsel Proving Ground, Souq Evaluator, Apex staked jury) exist precisely because the raw registry fails C1–C4.
- Cycle 12 trust-by-verification principle: quantified here (93.8%/94.9% unpaid reviewers and feedback).
- Choice-architecture and nudge skills: platform design as behavior-shaping — here platform design determines whether trust is earnable at all.
