# Agentic Payments Protocol Ecosystem: Reference Data

## AP2 (Agent Payments Protocol)

**Launched:** September 16, 2025
**Developer:** Google, with 60+ partner organizations
**Status:** Specification published; A2A x402 extension production-ready for crypto; card-based implementations maturing
**License:** Open specification
**GitHub:** github.com/google-agentic-commerce/AP2

**Core Concepts:**
- **Mandate:** Tamper-proof, cryptographically signed (ECDSA) JSON-LD object proving user authorization
- **Intent Mandate:** User specifies conditions under which agent may purchase (delegated authority)
- **Cart Mandate:** Explicit user authorization for specific items and prices (real-time purchase)
- **Payment Mandate:** Shared with payment networks to signal agent involvement

**Supported Payment Rails:**
- Credit/debit cards
- Bank transfers / real-time payments
- Stablecoins (via A2A x402 extension)
- Cryptocurrencies (Ethereum, Solana, Sui via extensions)

**Key Partners:** Adyen, American Express, Coinbase, Etsy, Forter, Intuit, JCB, Mastercard, MetaMask, PayPal, Revolut, Salesforce, ServiceNow, Shopify, Stripe, UnionPay, Visa, Worldpay

## A2A (Agent-to-Agent Protocol)

**Launched:** April 2025
**Developer:** Google
**Status:** Production-ready; 50+ launch partners
**Relationship to AP2:** AP2 extends A2A into payments. A2A handles agent discovery and communication; AP2 handles authorization and settlement.

## x402 Protocol

**Developer:** Coinbase
**V2 Launched:** December 2025
**Status:** Production-ready; Stripe integrated February 2026; Cloudflare supports x402 transactions
**License:** Open-source
**Core Mechanism:** HTTP 402 status code + payment instructions in response; client signs payment and attaches to header; receives resource. No accounts, no sessions.

**Supported Chains:** Base, Ethereum, Polygon, Solana, Avalanche, Sui
**Fees:** Zero protocol fees; only on-chain gas (fractions of a cent on L2s)
**Ideal Use Cases:** Machine-to-machine payments, API monetization, compute resources, agent-to-agent services

## MPP (Machine Payments Protocol)

**Developer:** Stripe + Tempo
**Mainnet Launched:** March 18, 2026
**Status:** New; 100+ integrated services at launch
**Core Mechanism:** "Sessions" model — agent pre-authorizes spending limit; streams granular micropayments continuously within session without per-transaction on-chain overhead

**Supported Rails:** Stablecoins (Tempo chain), fiat (Stripe Shared Payment Tokens), cards (Visa extension), Bitcoin Lightning (Lightspark extension)
**Fees:** No native gas token on Tempo; fees paid in any major stablecoin via integrated AMM; Stripe processing fees for fiat

## ACP (Agentic Commerce Protocol)

**Developer:** OpenAI + Stripe
**Launched:** September 2025
**License:** Apache 2.0
**Status:** Deployed in ChatGPT Instant Checkout (Feb 2026); OpenAI pivoted to app-based model (March 2026); protocol remains open standard with Shopify, Salesforce, PayPal support
**Core Mechanism:** Four RESTful endpoints (Create/Update/Complete/Cancel Checkout) with SharedPaymentTokens

## Market Projections

**McKinsey (2025):** Global agentic-commerce orchestrated revenue could reach $3–5 trillion by 2030, primarily in goods.
**Morgan Stanley:** Agentic shoppers could represent $190–385 billion in U.S. e-commerce spending by 2030 (10–20% of market).
**MarketsandMarkets:** AI agents market will reach $52.62 billion by 2030.
**Mordor Intelligence:** Agentic AI developer ecosystem and SDK market to grow from $2.40 billion (2025) to $16 billion (2030).
**NVIDIA (2025):** 86% of respondents expected AI budget increases in 2026; 88% reported revenue impact.

## The Pricing Problem for AI Agents

Traditional payment processors impose fee structures that destroy agent economics:
- Standard card processing: 2.9% + $0.30 per transaction
- On a $0.50 AI agent API call: fee = $0.345 (69% of transaction value)
- On a $0.05 microtask: fee = $0.345 (690% of transaction value)

Purpose-built agent payment infrastructure (x402 with zero protocol fees, MPP with streaming micropayments) solves this by:
- Eliminating per-transaction fixed fees
- Enabling sub-dollar transactions at scale
- Supporting 24/7 machine-speed settlement

## Nevermined Integration Data

**Case Study:** Valory cut deployment time of payments/billing infrastructure for the Olas AI agent marketplace from 6 weeks to 6 hours using Nevermined.
**Platform Capabilities:**
- Ledger-grade metering with cryptographically signed usage records
- Dynamic pricing (usage, outcome, value-based)
- Credits-based settlement
- Native support for x402, A2A, MCP, and AP2
- Instant settlement in fiat and cryptocurrency

## Crossmint Comparison

Crossmint provides unified agent payment infrastructure through a single API:
- Agent wallets (fiat + stablecoin, non-custodial with programmable guardrails)
- Virtual cards (Visa/Mastercard, scoped permissions)
- Stablecoin onramps (150+ countries)
- Agentic checkout (Amazon, Shopify, major merchants)
- Agentic credentials (W3C verifiable credentials)
- Multi-protocol architecture supporting x402, AP2, ACP, MPP as they mature
