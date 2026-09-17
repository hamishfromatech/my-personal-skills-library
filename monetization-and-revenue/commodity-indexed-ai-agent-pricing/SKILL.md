---
name: commodity-indexed-ai-agent-pricing
description: Apply the three-term commodity-indexed pricing framework (P = Σ(αᵢ·Cᵢ) + β·V + γ) adapted from long-term LNG contracts to price autonomous AI agents sustainably. Use when designing AI agent pricing architecture, when per-seat or pure outcome pricing is failing, when needing the Inference Capture Ratio (ICR) monetization health metric, or when modeling the multi-provider infrastructure floor + value-linked multiplier + platform constant pricing structure.
---

# Commodity-Indexed AI Agent Pricing Framework

## Core Thesis

The success of autonomous AI agents is destroying the revenue models of the companies deploying them. Three pricing models break:

1. **Per-seat licensing collapses** when one agent replaces many users
2. **Cost-plus pricing induces meter-watching** that limits adoption
3. **Pure outcome pricing transfers catastrophic inference cost risk** to providers

The solution is a **three-term linear pricing framework** adapted from long-term Liquefied Natural Gas (LNG) commodity contracts:

**P = Σ(αᵢ·Cᵢ) + β·V + γ**

Decomposing price into:
- A **multi-provider infrastructure floor** (Σ(αᵢ·Cᵢ)) — commodity-indexed inference cost
- A **value-linked multiplier** (β·V) — outcome or usage value capture
- A **platform constant** (γ) — fixed platform/maintenance fee

This is the structural pricing architecture that the existing pricing skills (outcome-based, hybrid, three-body problem, pricing taxonomy) point toward but do not fully formalize. It provides the mathematical foundation for sustainable AI agent pricing.

---

## The Three Failure Modes (Why Current Pricing Breaks)

### Failure 1: Per-Seat Collapse

When one AI agent replaces 10 users, per-seat revenue drops 90%. Salesforce's Agentforce pricing evolution (2024-2026) documents this: the company shifted from per-seat Copilot pricing to consumption-based AI credits as agents began replacing multi-user workflows. Per-seat works when each human is a billing unit; it collapses when agents consolidate workflows.

### Failure 2: Cost-Plus Meter-Watching

Cost-plus pricing (charging inference cost + margin) makes every API call a visible expense. Customers monitor usage, ration calls, and limit adoption to avoid bill shock. The transparency that seems customer-friendly actually suppresses the adoption that drives value. Microsoft's Copilot Studio consumption pricing (November 2024) encountered this: usage dropped when every token was metered.

### Failure 3: Outcome-Only Catastrophic Risk

Pure outcome pricing (Intercom Fin at $0.99/resolution, Zendesk at $1.50-$2.00/resolution) transfers all inference cost risk to the provider. When a resolution requires 50 API calls across expensive frontier models, the provider loses money on that interaction. Most interactions are cheap; a few are catastrophically expensive. Without an infrastructure floor, the provider bears unlimited downside.

---

## The Three-Term Framework

### Term 1: Multi-Provider Infrastructure Floor — Σ(αᵢ·Cᵢ)

The infrastructure floor is a **commodity-indexed cost pass-through** adapted from LNG contracts, where price is indexed to the cost of inputs (natural gas, transport, processing) across multiple suppliers.

For AI agents:
- **Cᵢ** = the unit cost of inference from provider i (e.g., OpenAI, Anthropic, open-source self-hosted)
- **αᵢ** = the weight (fraction of inference routed to provider i)
- **Σ(αᵢ·Cᵢ)** = the blended inference cost floor

This term ensures the provider never loses money on compute. It is indexed to actual commodity (inference) costs across a multi-provider portfolio, so it automatically adjusts as model prices drop (which they do — ~80% in 2025-2026).

**Why multi-provider**: Single-provider floors are brittle (vendor lock-in, price changes, outages). A multi-provider portfolio (frontier for orchestration, mid-tier for standard, SLM for high-frequency) creates a blended cost floor that is lower and more stable than any single provider. This connects directly to the `ai-agent-finfops-cost-optimization` Plan-and-Execute pattern (90% cost reduction via heterogeneous routing).

**Why indexed**: Inference costs are commoditizing. An indexed floor automatically passes cost reductions to customers (building trust) and protects provider margin (the floor rises if costs rise, falls if costs fall). This is structurally different from fixed pricing, which becomes misaligned as costs change.

### Term 2: Value-Linked Multiplier — β·V

The value-linked multiplier captures the value the agent creates beyond raw compute:

- **V** = the value metric (outcomes delivered, usage hours, tasks completed, revenue influenced)
- **β** = the value-capture rate (the fraction of created value the provider captures)

This term is where the provider's margin lives. It aligns pricing with customer value: when the agent delivers more value, the provider earns more. When it delivers less, the provider earns less.

**Value metric selection** (by use case):
| Use Case | Value Metric (V) | Capture Rate (β) Range |
|---|---|---|
| Customer support | Per-resolution (outcome) | 0.1-0.3 of cost-saved-per-resolution |
| Code generation | Per-merged-PR (outcome) | 0.05-0.15 of engineer-hour-saved value |
| Data analysis | Per-insight-delivered (outcome) | 0.1-0.25 of analyst-hour-saved value |
| Workflow automation | Per-task-completed (usage) | 0.02-0.10 of manual-task-cost |
| Research | Per-report-delivered (outcome) | 0.05-0.20 of consultant-hour-saved value |

### Term 3: Platform Constant — γ

The platform constant is a fixed fee covering platform overhead that doesn't scale with usage:
- Infrastructure maintenance (model updates, routing logic, monitoring)
- Compliance and governance (audit trails, security reviews, SOC 2)
- Platform features (dashboards, analytics, integrations)
- Minimum viable revenue per customer (prevents freeloader problem)

This term ensures the provider can sustain the platform even when usage is low. It is typically $500-$5,000/month for enterprise, $50-$500/month for SMB.

---

## The Inference Capture Ratio (ICR)

### Definition

**ICR = (Σ(αᵢ·Cᵢ) + β·V) / Total Inference Cost**

The ICR measures how much of the total inference cost is captured by the pricing structure. A healthy ICR is ≥ 1.5 (the pricing captures 1.5x the raw compute cost, providing margin for platform overhead and profit).

### ICR Health Bands

| ICR | Health | Interpretation |
|---|---|---|
| < 1.0 | Critical | Pricing does not cover inference cost — losing money on compute |
| 1.0-1.2 | Danger | Barely covering compute; no margin for platform or profit |
| 1.2-1.5 | Marginal | Modest margin; vulnerable to cost spikes or usage drops |
| 1.5-2.5 | Healthy | Sustainable margin; covers platform overhead and profit |
| 2.5-4.0 | Strong | Premium margin; room for R&D reinvestment |
| > 4.0 | Extractive | Possibly overcharging; vulnerable to competition |

### Using ICR for Pricing Diagnosis

1. **ICR < 1.0**: Raise the infrastructure floor (Σ) or increase the value capture rate (β). You are subsidizing customer compute.
2. **ICR 1.0-1.5**: Increase β or add a platform constant (γ). You have no margin buffer.
3. **ICR > 4.0**: Lower β or reduce γ. You are creating pricing pressure for competitors to undercut.
4. **ICR declining over time**: Inference costs are dropping faster than you're passing savings to customers. Re-index the floor.

---

## Market Evidence: Convergence on This Structure

### Salesforce Agentforce (2024-2026)

Pricing trajectory reconstructed from investor communications:
- **2024**: Per-seat Copilot pricing ($50/user/month)
- **2025**: Consumption-based AI credits (per-action pricing)
- **2026**: Hybrid structure converging on infrastructure floor + per-outcome multiplier + platform fee

The evolution documents the market discovering this structure through iteration: pure per-seat failed (agent replaces users), pure consumption caused meter-watching, pure outcome created cost risk.

### Microsoft 365 Copilot (2024-2025)

- Per-seat pricing ($30/user/month) for base Copilot
- Copilot Studio consumption pricing (November 2024) for custom agents
- Trajectory: converging on infrastructure floor (Azure compute pass-through) + value multiplier (per-workflow) + platform constant (per-seat for management features)

### Intercom Fin ($0.99/resolution)

Pure outcome pricing — the β·V term without the infrastructure floor or platform constant. Works because Intercom controls the model stack and can absorb cost variance. Vulnerable to catastrophic-cost resolutions. The market is converging toward adding the floor (Σ) and constant (γ) terms.

### Zendesk ($1.50-$2.00/resolution)

Similar pure outcome model. Same structural vulnerability. Market pressure is pushing toward the three-term structure.

---

## A-Tech Application Matrix

### A-Coder (AI Development Environment)

**Pricing architecture**:
- **Infrastructure floor (Σ)**: Multi-provider inference cost pass-through (frontier for orchestration, mid-tier for standard coding, SLM for completions). Local-first default: zero floor for on-device inference; cloud pass-through only for escalated queries.
- **Value multiplier (β·V)**: Per-merged-PR pricing. V = PRs merged with AI assistance. β = 5-15% of engineer-hour-saved value.
- **Platform constant (γ)**: $50-$500/month per team for management features (dashboards, analytics, governance, MCP marketplace access).

**ICR target**: 2.0-3.0 (strong margin, room for R&D reinvestment into open-source contributions)

**Privacy-first advantage**: Local-first inference means the infrastructure floor is often zero (on-device compute). This creates a structural cost advantage over cloud-bound competitors whose floor is always positive. The value multiplier and platform constant become the primary revenue — not compute pass-through.

### Be Practical (Practical AI Curriculum)

**Pricing architecture**:
- **Infrastructure floor (Σ)**: Multi-provider inference cost for AI-assisted learning features (adaptive content, comprehension checks, personalized exercises)
- **Value multiplier (β·V)**: Per-skill-acquired pricing. V = skills demonstrated via assessment. β = 10-25% of course-equivalent-cost-saved.
- **Platform constant (γ)**: $20-$100/month per learner for platform access (progress tracking, certification, community)

**ICR target**: 1.5-2.5 (healthy margin, accessible pricing)

### Builder's Club (Community Platform)

**Pricing architecture**:
- **Infrastructure floor (Σ)**: Multi-provider inference cost for community AI features (agent marketplace, code review, contribution matching)
- **Value multiplier (β·V)**: Per-contribution-facilitated pricing for marketplace; per-bounty-resolved for bounty system
- **Platform constant (γ)**: $100-$1,000/month per organization for governance, analytics, marketplace listing

**ICR target**: 1.5-2.5 for platform; individual marketplace transactions priced per the agent-to-agent-economy model

---

## The Privacy-First Structural Advantage

The three-term framework reveals a structural advantage for privacy-first, local-first architecture:

| Pricing Term | Cloud-First Competitor | A-Tech (Local-First) |
|---|---|---|
| Infrastructure floor (Σ) | Always positive (cloud compute cost) | Often zero (on-device compute) |
| Value multiplier (β·V) | Same value, same capture | Same value, same capture |
| Platform constant (γ) | Higher (cloud infrastructure maintenance) | Lower (user provides hardware) |

**The implication**: A-Tech can charge the same value-linked multiplier and platform constant while having a near-zero infrastructure floor. This creates either higher margin (same price, lower cost) or competitive pricing advantage (lower price, same margin). The privacy-first architecture is not just an ethical choice — it is a pricing moat.

---

## Implementation: The Pricing Committee

Per the cross-functional pricing discipline established in `revenue-design-discipline` and `ai-agent-pricing-three-body-problem`:

### Roles

1. **Product**: Defines value metric (V) and validates outcome definitions
2. **Engineering**: Measures inference costs (Cᵢ) and routing weights (αᵢ) across providers
3. **Finance**: Sets platform constant (γ), monitors ICR, manages revenue recognition
4. **Sales**: Communicates pricing structure to customers; collects WTP data for β calibration
5. **Customer Success**: Monitors adoption vs. meter-watching; calibrates floor vs. friction

### Pricing Cycle (Quarterly)

1. **Re-index the floor**: Update Cᵢ across all providers (inference costs drop ~20-40% annually)
2. **Calibrate the multiplier**: Review V (value metric) and β (capture rate) against customer value delivered
3. **Adjust the constant**: Review γ against platform cost evolution
4. **Monitor ICR**: Diagnose pricing health; intervene if ICR < 1.5 or > 4.0
5. **Pass savings**: If floor dropped, pass to customers (trust) or capture as margin (profit) — alternate quarterly

---

## Anti-Patterns

1. **Single-provider floor**: Indexing to one provider creates vendor lock-in and price vulnerability. Always use multi-provider weighted portfolio.
2. **Fixed floor (not indexed)**: A fixed floor becomes misaligned as inference costs drop. The floor must be indexed to actual costs.
3. **Value metric gaming**: If V can be gamed (e.g., per-resolution where "resolution" is easily achieved), the multiplier fails. V must be defined to resist gaming (verified outcomes, quality-checked completions).
4. **Zero platform constant**: Removing γ creates the freeloader problem — low-usage customers cost more to serve than they generate. Always include a minimum platform fee.
5. **ICR > 5.0**: Overcharging creates competitive vulnerability. If ICR is consistently above 4.0, competitors will undercut.
6. **ICR < 1.0**: Subsidizing compute destroys the business. The floor must cover inference cost.
7. **Opaque pricing**: The three-term structure should be transparent to customers (they can see the floor, understand the multiplier, predict the constant). Opaque pricing destroys trust and prevents customer budgeting.

---

## Integration with Existing A-Tech Skills

- **`ai-agent-pricing-three-body-problem`**: The three-body problem (product, consumption, costs) is resolved by the three-term framework: costs → floor (Σ), consumption → multiplier (β·V), product → constant (γ)
- **`agentic-commerce-pricing-consolidation-2026`**: The outcome-based pricing consolidation is the β·V term; this framework adds the missing Σ and γ terms
- **`revenue-design-discipline`**: The pricing committee and quarterly cycle operationalize revenue design for the three-term framework
- **`ai-agent-finfops-cost-optimization`**: The Plan-and-Execute model routing (90% cost reduction) directly reduces Cᵢ in the floor term
- **`profitable-ai-unit-economics`**: The ICR is the monetization health metric that the unit-economics framework needs
- **`untrainable-corner-pricing-moat`**: The value multiplier (β·V) is where the untrainable-corner moat is encoded — higher value delivery justifies higher β
- **`bessemer-ai-pricing-playbook-2026`**: The hybrid pricing convergence is formalized by the three-term structure
- **`hybrid-ai-pricing-architecture`**: The three-layer hybrid architecture (base + usage + outcome) maps directly to the three terms (γ + Σ + β·V)

---

## Summary

The commodity-indexed three-term pricing framework (P = Σ(αᵢ·Cᵢ) + β·V + γ) resolves the three failure modes of AI agent pricing: per-seat collapse, cost-plus meter-watching, and outcome-only catastrophic risk. The multi-provider infrastructure floor (Σ) covers compute cost and is indexed to actual commodity prices. The value-linked multiplier (β·V) captures value delivered. The platform constant (γ) sustains the platform. The Inference Capture Ratio (ICR) diagnoses pricing health. For A-Tech, the local-first architecture creates a structural advantage: near-zero infrastructure floor means the value multiplier and platform constant become the primary revenue — the privacy-first architecture is a pricing moat, not just an ethical choice.