---
name: agent-ready-api-monetization-pattern
description: Framework for making APIs and digital services "agent-ready" — machine-readable capabilities, explicit commercial terms, scoped payment authority, and multi-model monetization. Use when designing monetization for AI agents, building payment-protected MCP servers, evaluating agentic payment protocols (x402, AP2, MPP), or deciding between usage/workflow/outcome/credit/hybrid pricing for agent products.
---

# Agent-Ready API Monetization Pattern

## Overview
A comprehensive pattern for turning any API or MCP server into a revenue-generating product that autonomous AI agents can discover, authorize, and pay for without human intervention. Based on Nevermined's agent-readiness framework (August 2026) covering discovery, access, commerce, and delivery requirements.

## When to Use
- Building or monetizing MCP servers, APIs, or tools for AI agents
- Designing payment flows for autonomous agent consumption
- Evaluating x402, AP2, MPP, or other agent payment protocols
- Choosing between usage-based, workflow, outcome, credit, or hybrid pricing
- Building agent billing, metering, and settlement infrastructure
- Deciding how to expose commercial terms to autonomous buyers

## NOT For
- Human-facing SaaS subscription pricing (different assumptions)
- Pure protocol design without revenue capture
- On-chain crypto-only payment (fiat and hybrid rails are valid)

## Core Process / Workflow

### 1. Agent Readiness Assessment — Four Requirements

An API is "agent-ready" when it satisfies four connected requirements:

| Requirement | Question | Implementation |
|---|---|---|
| **Discovery** | What does the service provide? | OpenAPI spec, MCP server tools/resources/prompts, A2A Agent Card |
| **Access** | Which credentials and permissions? | Scoped credentials, OAuth 2.1, tool allowlists |
| **Commerce** | What does the action cost? | Explicit pricing, payment authorization, refund/retry rules |
| **Delivery** | Did the paid request succeed? | Completion status, settlement record, reconciliation trail |

A payment button alone does **not** make a service agent-ready. The full interaction — from discovering a capability to receiving the purchased result — must work programmatically.

### 2. Expose Commercial Terms in Machine-Readable Form

Agents must not interpret sales pages. Define explicitly:
- The billable unit (API request, token, tool execution, workflow, outcome)
- The price or pricing formula
- Accepted payment and credit types
- Minimum balances, spending/usage limits
- Expiration conditions
- Refund and retry rules
- Whether failed calls generate charges

### 3. Separate Identity, Access, and Payment

These functions answer different questions and should remain separate:
1. Which agent made the request? (identity)
2. Who authorized that agent? (delegation)
3. Which resources may it access? (access control)
4. What may it spend? (payment authority)
5. Which payment covers this action? (settlement)

Keeping them separate makes policy changes easier — rotate credentials, revoke spending, adjust access without rebuilding identity.

### 4. Choose a Monetization Model

| Model | Revenue Trigger | Best For | Risk |
|---|---|---|---|
| **Usage-based** | Measurable consumption (request, token, compute) | Measurable units closely tied to cost | Predictability for customers |
| **Task/Workflow** | Defined unit of completed work | Deliverable-based services | Provider absorbs cost variability |
| **Outcome-based** | Verified business result | High-value, attributable outcomes | Attribution difficulty |
| **Credit-based** | Prepaid unit convertible across resources | Budget control, multiple resource types | Must explain credit mapping |
| **Hybrid** | Combines recurring + variable | Mature agent products | Complexity in billing logic |

### 5. Design Payments for Autonomous Consumption

Human checkout breaks autonomous workflows. Use delegated authority:
- Maximum spend per transaction
- Daily/weekly/workflow budgets
- Approved merchants or service categories
- Time-limited authority
- Transaction-count limits
- Immediate revocation

### 6. Protocol Stack Selection

| Protocol | Scope | Role |
|---|---|---|
| MCP | Tool/resource connection | Exposes capabilities, does not set price |
| A2A | Agent-to-agent discovery | Agent Card describes identity/skills |
| AP2 | Delegated authorization | Cryptographic evidence of user approval |
| x402 | HTTP-native payment | 402 response → payment → retry with proof |
| MPP | Machine payment protocol | Stripe/Tempo standard for machine payments |

These compose: discover via A2A → invoke via MCP → prove authority via AP2 → pay via x402/MPP.

### 7. Meter Before Scaling

Define the billable event **before** accepting payment. Record per event:
- Customer and agent identifier
- Resource/service used
- Timestamp, pricing plan, units consumed
- Price applied, payment/credit reference
- Completion status, settlement result

Track fulfillment cost (inference, search, compute, human review, sub-agents) at the same level as pricing to measure margin.

## References
- See [references/agent-ready-monetization-evidence.md](references/agent-ready-monetization-evidence.md) for protocol details, platform comparisons, and implementation patterns.