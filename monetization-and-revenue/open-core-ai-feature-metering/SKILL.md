---
name: open-core-ai-feature-metering
description: Apply the "free core, paid AI features" open-core pricing model that separates core product access from metered premium AI consumption. Solves the margin-erosion problem when AI inference creates variable per-customer cost that flat subscriptions hide. Use when pricing an open-core product with AI features, when heavy AI users are quietly defining economics for everyone, when designing metered AI tiers, or when avoiding community backlash from AI paywalls.
---

# Open-Core AI-Feature Metering

## Overview

Traditional open-core pricing controls product access. AI adds a new layer: every answer, summary, code review, or agent run creates variable inference cost. A flat subscription hides that cost until power users turn a useful feature into a margin problem. This skill provides the separation model — keep the core useful and free, meter the AI consumption that creates real and uneven cost — plus the credits/caps/top-ups structure and the anti-backlash communication pattern.

## When to Use

- Pricing an open-core product that has (or is adding) AI features
- Heavy AI users are quietly defining the economics for everyone on a flat plan
- Designing metered AI tiers, included allowances, or top-up pricing
- Deciding what stays in the free core vs. what is metered as paid AI usage
- Avoiding community backlash when introducing AI pricing
- NOT for pure proprietary SaaS with no open core — this is the open-core-specific model

## Core Process / Workflow

### 1. Understand why AI changes open-core pricing

Traditional open-core separates community adoption from commercial value via product access (free core → paid enterprise features). AI does not fit the access-only model because:

- Model providers price API usage by units (input tokens, cached input, output tokens, image tokens, audio, tool calls).
- Two customers on the same plan can create very different costs: one workspace runs a few AI searches/month; another generates thousands of answers/summaries/agent tasks daily.
- If both pay the same flat price for unlimited AI, the heaviest user defines the economics for everyone.
- AI gross margins are 50–60% vs. 80–90% for classic SaaS — COGS matter again.

### 2. Separate the three things that should not be blended

| Layer | What it is | Pricing |
|---|---|---|
| **Core product access** | The product, project, or community edition users adopt without paying for every AI-heavy workflow | Free / open source |
| **Commercial product value** | Paid features: admin controls, enterprise support, SSO, audit logs, collaboration, permissions, commercial licensing | Subscription / enterprise contract |
| **AI consumption** | Metered inference traffic from premium AI features, high-volume teams, heavy workflows | Usage-metered, separately billed |

This protects the community edition without pretending AI usage is free, and gives commercial customers a fairer model: pay for product access through the plan, pay for premium AI when they actually use it.

### 3. Draw the free-core vs. metered-AI line

The line must be easy for users to understand. Do not strip the free core just because AI exists.

| Keep in the free core | Meter as paid AI usage |
|---|---|
| Core workflows, local usage, project setup, basic search, documentation, non-AI features | AI answers, summarisation, rewriting, extraction, generation, enrichment, premium model calls |
| Community-friendly collaboration, templates, basic connectors, developer experience | RAG queries, agent runs, code review jobs, report generation, document batches, high-volume workspaces |
| Transparent limits that make the product useful before purchase | Top-ups, overages, workspace budgets, advanced AI allowances, team-level controls |

Worked examples:
- **Documentation platform:** keep publishing, indexing, basic search free; meter AI answers, summaries, premium retrieval.
- **Developer tool:** keep local scans free; meter AI code review, remediation suggestions, generated tests.
- **Support product:** keep ticket workflows open; meter AI triage, reply drafting, resolution summaries.
- **Analytics product:** keep dashboards and standard reports; meter AI-generated insights, NL analysis, scheduled summaries, large report batches.

### 4. Use credits, caps, and top-ups — not unlimited AI by default

Avoid launching premium AI features as unlimited. Unlimited feels simple in the pricing table but pushes every unknown cost back onto the product team.

**Starting structure:**
1. **Included allowance:** enough AI usage for customers to try the feature and see value.
2. **Visible limits:** workspace/project/org caps that prevent surprise spend.
3. **Paid top-ups:** a way for heavy users to keep going without forcing every customer onto a higher plan.
4. **Routed premium usage:** the AI traffic that is customer-paid, with the product's margin attached.

This does not mean every prompt needs a scary price tag. It means the product gives a clear allowance, explains what counts, and reserves customer-paid usage for the AI work that creates ongoing cost and value.

### 5. Start with ONE premium AI workflow

Do not reprice the entire open-core product in one move.
1. Pick one premium AI workflow where usage is clearly valuable and uneven.
2. Define the customer-facing unit (answers, reports, documents, reviews, tickets, tasks, runs).
3. Decide what is included.
4. Route the paid usage; watch how customers behave.
5. Expand only after learning from that workflow.

### 6. Avoid community backlash with honest communication

Open-core users accept paid AI usage when the boundary is honest. They revolt when a project markets AI as free, hides the cost, then suddenly removes useful access.

**Use plain language:**
- Explain what stays available in the free core.
- Explain which AI features create variable usage cost.
- Show the included allowance before paid usage starts.
- Let teams set caps or budgets.
- Avoid calling the surcharge a tax — tie it to the AI work the customer values.

The right message is not "pay because AI is expensive." The better message is "the core stays useful, and premium AI usage is priced separately so heavy usage does not make the whole product worse for everyone."

### 7. Avoid the monetisation anti-patterns

- **Feature gating** behind a paid wall: sophisticated users fork the open version, remove the gates, publish it — creating two competing products that confuse the market and alienate the best developers.
- **Training-data upsells** ("your data will train better models if you upgrade"): ethically suspect in the AI era; users are increasingly savvy. Don't.
- **Platform lock-in through proprietary protocols:** the vendor lock-in open-source users specifically chose the product to avoid. Works short-term, damages trust long-term.
- **Support as the primary revenue:** works for enterprise-first open source (Red Hat) but not for developer/founder-focused tools where users expect self-service and community support.

### 8. Route AI inference through a metering layer (implementation pattern)

For A-Tech products, the AI-consumption layer can be routed so the product team does not rebuild routing, metering, billing, and payout infrastructure from scratch:
1. Route AI inference traffic from the existing product through a metering/marketplace layer.
2. Configure a surcharge or margin for that routed usage.
3. The customer pays the metering layer directly for routed usage.
4. The metering layer routes inference through the marketplace.
5. The product is paid monthly based on generated earnings from app traffic.

This is useful when AI usage varies by customer, workspace, team, feature, model, document volume, or workflow complexity. The product keeps its roadmap, repository, hosting, licenses, plans, customer experience, and community relationship; the AI-usage path is metered separately.

### 9. A-Tech application matrix

| Product | Application |
|---|---|
| **A-Coder** | Core (local scans, project setup, basic rules) free; meter AI code review, generated fixes, test generation, premium model runs; included allowance per workspace; top-ups for heavy teams; enterprise tier keeps SSO/audit/SLA as the commercial-value layer |
| **Be Practical** | Core curriculum access free; meter AI tutoring sessions, personalised feedback, premium-model calls; included allowance per learner; top-ups for intensive tracks |
| **Builder's Club** | Core community/marketplace free; meter AI agent runs, enrichment skills, premium integrations; marketplace revenue share for community-built premium skills; transparent allowance and caps |
| **A-Tech monetisation policy** | The three-layer separation as the default open-core-AI pricing architecture; the "start with one workflow" rule as the discipline that prevents pricing-complexity sprawl |

## References

- See [references/open-core-ai-metering-evidence.md](references/open-core-ai-metering-evidence.md) for the source model, the AI-COGS economics, the metering-layer routing pattern, the anti-pattern evidence, and cross-references to adjacent monetisation skills.