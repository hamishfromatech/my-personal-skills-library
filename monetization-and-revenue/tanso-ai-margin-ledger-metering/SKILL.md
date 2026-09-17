---
name: tanso-ai-margin-ledger-metering
description: Applies the Tanso Core open-source monetization engine pattern for B2B AI products, combining usage metering, prepaid credits, entitlements, and Stripe billing into a single ledger that tracks both what you billed and what it cost you, enabling per-customer margin visibility. Use when building B2B AI products that sell credits or usage and need to see margin per customer, per feature, per model in real time. NOT for consumer products, pure subscription SaaS without variable inference costs, or teams that don't need cost-side tracking.
---

# Tanso: AI Margin Ledger & Metering Engine

## Core Concept

Tanso Core (tansohq/tanso-oss, AGPL-3.0, July 2026) is a self-hosted monetization engine for B2B AI products that solves a specific gap: billing platforms meter usage but don't know your inference costs, while LLM observability tools know costs but don't bill. Tanso keeps both sides in one ledger, so margin per customer is a query, not a spreadsheet project.

## The Gap It Fills

| Existing Tool Category | What It Does | What It Misses |
|---|---|---|
| Billing platforms (Stripe, Orb, Metronome) | Meter usage, generate invoices | No idea what inference costs you; can't answer margin questions |
| LLM observability (Langfuse, Helicone) | Track costs to the token | Don't bill anyone; can't connect cost to revenue |

**Result without Tanso**: Margin per customer lives in neither system. Most teams reconstruct it in a spreadsheet, quarterly, if at all.

## Core Features

### 1. Dual-Sided Ledger
Every metered event carries:
- **Revenue side**: input/output tokens, model, provider, what you billed for it
- **Cost side**: what that usage cost you (per token, per model, per provider)
- Margin per customer, per feature, per model is a real-time query

### 2. Real-Time Enforcement
- Entitlement checks, usage caps, credit limits applied at event ingestion
- If a customer is out of credits, the check fails now — not on a reconciliation job three weeks later
- For AI products where a runaway integration can burn real money in an afternoon, this is the difference between a limit and a suggestion

### 3. Credits as First-Class Primitive
- Prepaid credit pools per customer
- Grants, deductions, expirations, full transaction history
- Most AI products sell prepaid usage; bolting this onto subscription billing is painful

### 4. Credit Weights (Tariff)
Server-side table mapping usage units to credits per feature and model:
- "A `deep-research` call costs 5 credits, and 8 on `gpt-4.1`"
- Weights resolve most-specific first: `(feature, model)` → `(feature, any model)` → `1.0`
- Scheduled effective times; console editor; pre-flight credit quote
- Reprice server-side without redeploying client

### 5. Stripe as Payment Adapter
- Billing state lives in Tanso; Stripe handles payments and invoicing
- Your catalog, subscriptions, proration, and cycle rollover are in your database — inspectable and portable

### 6. Agent-Native (MCP)
- Optional MCP server: agents can check credit balances, inspect entitlements, manage subscriptions
- Same authenticated, account-scoped access as any other client
- Tools that spend money require explicit `confirmAction: true` before execution
- Disabled by default; one YAML snippet to enable

## Architecture

| Layer | Technology |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 3.5 |
| Database | PostgreSQL |
| Migrations | Liquibase |
| Payments | Stripe |
| Email | Resend |
| AI/MCP | Spring AI (optional) |
| Build | Maven |

## Quick Start

```bash
git clone https://github.com/tansohq/tanso-oss.git
cd tanso-oss/deploy
cp .env.example .env     # set JWT_SECRET
docker compose up -d --build
./setup.sh               # seeds test account, prints credentials
```

API on `localhost:8080`, docs at `/swagger-ui.html`.

## Credit Weight Workflow

```bash
# Pre-flight: what would this burn?
curl -X POST http://localhost:8080/api/v1/client/entitlements \
  -H "X-API-Key: $TANSO_API_KEY" \
  -d '{"customerReferenceId":"demo-user","featureKey":"ai.chat",
       "usage":{"usageUnits":1,"model":"gpt-4.1"}}'
# → "creditQuote": {"weight":8, "estimatedCredits":8}

# Record the usage
curl -X POST http://localhost:8080/api/v1/client/events \
  -H "X-API-Key: $TANSO_API_KEY" \
  -d '{"customerReferenceId":"demo-user","featureKey":"ai.chat",
       "eventName":"chat completion","eventIdempotencyKey":"evt-123",
       "usageUnits":1,"costInput":{"model":"gpt-4.1"}}'
# → "creditsDeducted":8, "remainingBalance":42
```

## Migration Notes

If your client sends pre-multiplied units (`usageUnits: 5` for a "5-credit action"):
1. Deploy client to send raw units **first**
2. Then publish the tariff
3. Never the reverse (server multiplies your multiplied numbers)

Repricing also affects: `maxUsage` caps, Stripe meter prices, grant sizing.

## A-Tech Applications

- **A-Coder**: Integrated margin-per-customer billing for AI code assistant (credit tariff by model, real-time enforcement)
- **Be Practical**: B2B AI monetization curriculum (margin ledger pattern, credit weights, MCP agent billing)
- **Builder's Club**: Open-source Tanso fork for A-Tech billing infrastructure (AGPL-3.0, self-hosted)

## Cross-References

- `open-core-ai-feature-metering` — Tanso implements the metering layer that open-core-ai-feature-metering describes
- `agent-native-advertising-economics` — Tanso's MCP server enables agent-native billing with consent gates
- `agentic-payments-protocol-ap2` — Tanso's credit system complements AP2 payment protocols
- `revenue-sharing-as-infrastructure-model` — Tanso's ledger can track rev-share splits
- `tanso-ai-margin-ledger-metering` — this skill

## A-Tech Alignment

| Value | Alignment |
|---|---|
| Open-source AI | AGPL-3.0; self-hosted; inspectable billing code; no vendor lock-in |
| Data privacy | Self-hosted; billing state in your database; no third-party data sharing |
| Financial freedom | Margin visibility prevents money-losing features; real-time enforcement prevents runaway costs |
| Practical implementation | Docker quick start; Spring Boot standard; Next.js example; Stripe integration |