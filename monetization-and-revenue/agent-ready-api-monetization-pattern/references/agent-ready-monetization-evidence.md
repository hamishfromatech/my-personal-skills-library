# Agent-Ready Monetization Evidence Base

## Source
Nevermined Team. "Monetization Strategies for AI Agents." August 13, 2026.
https://nevermined.ai/blog/monetization-strategies-ai-agents

## Key Framework: Agent Readiness

Four requirements: Discovery, Access, Commerce, Delivery. The full interaction from discovering a capability to receiving the purchased result must work programmatically.

## Protocol Stack

- **MCP**: Connects AI apps to external tools. An MCP server can expose a paid capability, but MCP itself doesn't determine price or settle payment.
- **A2A**: Discovery and communication between independent agents. Agent Card describes identity, endpoint, skills, auth requirements.
- **AP2**: Delegated authorization. Official AP2 mandate framework gives agents cryptographic evidence of what a user approved. Checkout and payment mandates connect authority to specific purchases.
- **x402**: HTTP-native payment. Service returns 402 Payment Required with structured payment details; client retries with proof of authorization.
- **MPP (Machine Payment Protocol)**: Co-authored by Stripe and Tempo, another standard for machine payments.

## Payment Rails

Cards, bank transfers, stablecoins, prepaid credits, subscription entitlements, smart accounts, batched settlement. No single rail fits every workload. Choose based on transaction size, geographic availability, settlement timing, accounting requirements, processing cost.

## "upto" Scheme in x402

At GA, Amazon Bedrock AgentCore payments introduced the "upto" scheme within x402, enabling agents to set spending ceilings rather than committing to fixed prices. This unlocks true pay-per-inference and dynamic pricing — merchants serving LLM tokens, compute, or usage-metered APIs can charge for exactly what was consumed at the end of a call.

## Amazon Bedrock AgentCore Payments (GA)

- Wallet support: Coinbase and Stripe Privy wallets (stablecoin microtransactions)
- Protocol-agnostic: x402 + MPP support
- Payment limits: Per-session spend cap + expiry time (deterministic, infrastructure-layer check)
- Observability: CloudWatch logs + AgentCore Observability dashboards
- Framework integrations: Strands Agents plugin, LangGraph middleware, OpenClaw plugin
- Use cases: Pay for paid web content (Cloudflare, CloudFront), pay-per-inference (BlockRun), consumer/deep research (Travala, Elsa AI, Heurist AI)

## LemonCake (Open-Source Alternative)

Open-core x402 payment rail for monetizing MCP servers and HTTP APIs. Card-funded, spend-capped, no crypto required.
- Buyer-side MCP: `agent-payment-mcp` (MIT)
- Seller SDK: `@lemon-cake/mcp-sdk` (MIT) — `lc.charge()` wraps handlers: preflight → run → settle
- Gateway + dashboard: hosted (closed)
- Takes 3% via Stripe Connect Direct Charge (seller keeps 97%)
- Per-agent identity: bind Pay Tokens to agents, per-agent spend rollup, kill switch (pause/resume/revoke)
- Compliance: JP FSA (not required), US FinCEN (non-custodial), EU MiCA (non-CASP), UK FCA (Tech Service Provider)

## BNB Agent Studio v2

Agents can now charge for work and get paid on-chain (x402 + ERC-8183). TypeScript support added. Paymaster covers testnet gas. Two wallet options: TWAK (autonomous signing) and Altana (self-custodial, scoped session keys, onchain permissions).

## Locus (API Billing Layer)

"One balance for 600 APIs." Wholesale pool model — platform funds credit pool, allocates to end users, applies markups. 600+ pay-per-use services from 48 providers. Enterprise pricing: 8-week pilot $15K, then $5K/month minimum, 5-3% platform fee.

## Revenue Benchmarks (kone Network)

From kone's 46,000+ advertiser network:
- Agent CTR: 3-6% (vs 0.1-0.3% display)
- Dev tool agents: 7.2% CTR; Research: 5.6%; Productivity: 4.8%; Shopping: 4.4%; General chatbots: 2.4%
- Revenue per 1k sessions: Fintech $95, B2B SaaS $78, Dev tools $62, Education $44, Consumer $31
- Median ARPU: $1.25/month; Top decile: $4.00+
- CPA rates: $3-$50+; CPC: $0.50-$4.00; CPM: $3-$15
- 70% publisher share

## Common Pitfalls

1. Monetizing before retention (need 3+ sessions/month baseline)
2. Firing recommendation tool on every query (kills CTR)
3. Burying the CTA
4. Ignoring the dashboard
5. Not disclosing sponsorship (honest disclosure doesn't kill conversions)