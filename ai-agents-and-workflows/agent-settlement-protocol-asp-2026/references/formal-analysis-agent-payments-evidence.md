# Formal Analysis of Agent Payment Protocols — Evidence Base

**Source:** Jiang, Yu, Chang, Jangid, Niu, Wang & Zhang, "A Formal Analysis of Agent Payment Protocols," arXiv:2609.00060 (submitted Aug 30, 2026). Tamarin formal verification of four representative agent payment protocols.

## What Was Done
- **Protocols formalized in Tamarin:** x402, MPP (Machine Payments Protocol — Visa/Stripe/Tempo), ACP (Agentic Commerce Protocol — OpenAI/Stripe), AP2 (Agent Payments Protocol — Google).
- **Method:** a common abstraction of the agent-payment lifecycle; source-grounded models capturing each protocol's roles, state, trust assumptions, and lifecycle transitions. Rather than assuming a complete property taxonomy, the authors used **source-backed verification questions and counterexample traces** to expose missing bindings, state constraints, and cross-stage correspondences.
- **Output:** 18 shared security principles distilled across the four protocols.

## Scale of Findings
- 86 verification cases: 46 reproductions of known/calibration issues + **40 previously undocumented formal-consistency findings**.
- For each retained violation: the missing protocol relation is isolated, a minimally strengthened reference model is constructed, and the intended property is re-verified.
- New x402 findings evaluated across three implementations; ten representative findings validated through implementation PoCs, SDK/schema-level witnesses, and source-aligned executable traces spanning five security principles.

## The Central Claim
> Delegated authorization must remain consistent with its resulting economic and service effects **across actors, states, and protocol stages**.

In other words: the guarantees that matter (e.g., "the user authorized *this* purchase at *this* price and got *this* fulfilment") are distributed across messages, participants, and lifecycle stages such that no single message or participant enforces them — and the specifications leave the cross-stage correspondences implicit.

## Why This Matters for the Library
1. **Complements, not duplicates, the empirical trust track.** The library holds the *empirical* reality check (erc8004-empirical-trust-reality-check-2026: Sybil dominance, commensurability/robustness/groundedness/economics failures) and the *design* skill (agent-settlement-protocol-asp-2026). This paper adds the *formal-methods* track: even well-designed protocols carry binding gaps that counterexample-driven Tamarin analysis surfaces.
2. **The 40 undocumented findings are a due-diligence checklist.** Any team deploying agent payments (x402/MPP/ACP/AP2) should treat "formal-consistency findings" as a category of review: binding (authorization ↔ economic effect), state constraints (stage transitions), and cross-stage correspondences.
3. **Methodological pattern worth copying:** source-backed verification questions instead of assumed property taxonomies — closer to how real specs drift, because it anchors to what the implementation actually does.
4. **Positions the payment layer as safety-critical infrastructure.** A formal analysis published within a week of the ASP specification (arXiv:2609.02208) and against production scale (x402 ~75M transactions/30 days per the library's x402-production-checklist) marks the moment agent payments attract formal-methods scrutiny — the same maturation curve payments and consensus protocols went through.

## Contextual Timeline (Aug 30–Sept 2 2026, from the searches)
- **Aug 30:** Formal analysis of agent payment protocols posted (arXiv:2609.00060)
- **Aug 31 / Sept 2:** ASP specification draft v0.6 posted (arXiv:2609.02208) — fulfilment-coupled settlement layer over x402/CPP
- Same period: Forbes council piece "Why Agentic Payments Involve More Than Pay-Per-Call" (Sept 4) arguing authorization/metering/settlement must stay separate — converging with ASP's separation logic; t54's 20M-transaction trust-layer case study (AWS blog, Sept 1) showing production trust-gating at scale
- Same period: independent PoC "Agent Settlement Protocol" repo on Solana devnet (Aleks-NFT/agent-settlement-protocol) — a *different* ASP solving multi-step atomic execution with trust-weighted economics; note the name collision, and note the convergence: settlement integrity is the recognized unsolved problem, by at least three unrelated teams

## Usage Rules
- Cite as: formal-methods evidence that shipped agent-payment specs have binding gaps; pair with erc8004 empirical findings for the reputation layer and ASP for the settlement layer.
- Do not over-claim: the paper's own framing is counterexample-driven consistency analysis, not a completeness proof of all security properties.
- Watch item: whether the 18 shared security principles get adopted into spec revisions (x402 Foundation / MPP / ACP / AP2 processes).

## Related Skills
- `agent-settlement-protocol-asp-2026` (same cycle — the specification this analysis would stress-test)
- `x402-production-checklist` — operational layer where such findings land
- `erc8004-empirical-trust-reality-check-2026` — empirical trust-layer counterpart
- `agentic-trust-security-protocols-2026` — trust/security protocol family