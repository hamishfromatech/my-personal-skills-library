# Commodity-Indexed AI Agent Pricing Framework: Evidence Base

## Primary Source

**Dalmia, Abha (June 12, 2026)** — "A Commodity Indexed Pricing Framework for Autonomous AI Agents." Munich Personal RePEc Archive (MPRA Paper No. 129348). Deposited 12 June 2026.

### Abstract

"The success of autonomous AI agents is destroying the revenue models of the companies deploying them. Per seat licensing collapses when one agent replaces many users; cost plus pricing induces meter watching that limits adoption; pure outcome pricing transfers catastrophic inference cost risk to providers. We propose a three term linear pricing framework adapted from long term Liquefied Natural Gas (LNG) contracts: P = Σ(αᵢ·Cᵢ) + β·V + γ, decomposing price into a multi provider infrastructure floor, a value linked multiplier, and a platform constant. We introduce the Inference Capture Ratio (ICR) as a monetization health metric and use Salesforce's 2024-2026 Agentforce pricing evolution, alongside secondary evidence from Microsoft, Intercom, and Zendesk, to show how the market is converging on this structure."

### Keywords

AI Pricing and Monetization

### Subject Classification

- J31: Wage Level and Structure; Wage Differentials
- J33: Compensation Packages; Payment Methods
- M11: Production Management
- M21: Business Economics

---

## The Three Pricing Failures (Detailed)

### 1. Per-Seat Licensing Collapse

Per-seat pricing assumes each human user is a billing unit. When one AI agent consolidates the work of multiple users, the number of billing units drops. Salesforce's Agentforce evolution (2024-2026) documents the failure:
- 2024: Per-seat Copilot pricing ($50/user/month) — assumes each human is a seat
- 2025: Consumption-based AI credits — acknowledges agents, not humans, are the unit
- 2026: Hybrid structure converging on the three-term framework

The failure mechanism: agents don't need seats. They need inference. Per-seat pricing captures neither the cost (inference) nor the value (outcomes) of agent operation.

### 2. Cost-Plus Meter-Watching

Cost-plus pricing (inference cost + margin) makes every API call a visible expense. The transparency that seems customer-friendly actually:
- Triggers loss aversion (every call feels like spending money)
- Causes usage rationing (customers limit adoption to control costs)
- Creates bill anxiety (unpredictable monthly costs)
- Suppresses the experimentation that drives value discovery

Microsoft's Copilot Studio consumption pricing (November 2024) encountered this: organizations that saw per-token billing reduced usage, limiting the value they could discover. The meter-watching effect is a behavioral economics finding: visible costs suppress consumption even when the value exceeds the cost.

### 3. Pure Outcome Pricing Catastrophic Risk

Pure outcome pricing (Intercom Fin at $0.99/resolution, Zendesk at $1.50-$2.00/resolution) has a hidden fat-tail risk:
- Most resolutions are cheap (1-3 API calls, standard model)
- A few resolutions are catastrophically expensive (50+ calls, frontier model, multi-step reasoning)
- Without an infrastructure floor, the provider absorbs the full cost variance
- The provider loses money on expensive resolutions and makes it back on cheap ones — but the variance is unbounded

The LNG contract analogy: long-term LNG contracts don't use pure spot pricing (catastrophic risk) or pure fixed pricing (misalignment). They use indexed pricing (cost floor + value premium + constant). The same structure resolves AI agent pricing.

---

## The Three-Term Framework (Mathematical Detail)

### P = Σ(αᵢ·Cᵢ) + β·V + γ

#### Term 1: Multi-Provider Infrastructure Floor — Σ(αᵢ·Cᵢ)

- **Cᵢ**: Unit inference cost from provider i (e.g., $/1M tokens for OpenAI, Anthropic, self-hosted)
- **αᵢ**: Weight (fraction of total inference routed to provider i); Σαᵢ = 1
- **Σ(αᵢ·Cᵢ)**: Blended inference cost floor

The floor is **commodity-indexed**: it automatically adjusts as inference prices change. When OpenAI drops prices 50%, the floor drops. When a new efficient SLM handles 70% of tasks at 10% of cost, the floor drops. This is the structural advantage over fixed pricing.

**Why multi-provider**: A single-provider floor is:
- Brittle (vendor outage = total floor failure)
- Price-vulnerable (vendor price increase = margin destruction)
- Lock-in-creating (hard to switch when indexed to one provider)

A multi-provider weighted portfolio (per the Plan-and-Execute pattern in `ai-agent-finfops-cost-optimization`) creates a blended floor that is lower, more stable, and more adaptable than any single-provider floor.

#### Term 2: Value-Linked Multiplier — β·V

- **V**: The value metric — what the customer values (outcomes, usage, revenue influenced)
- **β**: The value-capture rate — the fraction of created value the provider captures

The multiplier is where margin lives. It aligns pricing with value: more value delivered → more revenue captured. The key design decision is selecting V (the value metric) and calibrating β (the capture rate).

**Value metric selection criteria**:
1. **Resistant to gaming**: The metric can't be trivially achieved (e.g., "tickets closed" where the agent closes without resolving)
2. **Correlated with customer value**: The metric must reflect actual value delivered, not just activity
3. **Measurable and verifiable**: The metric must be auditable
4. **Customer-aligned**: The customer must agree the metric represents value

#### Term 3: Platform Constant — γ

The fixed fee covers platform-level costs that don't scale with usage:
- Model update integration (new models, prompt optimization)
- Routing logic maintenance
- Monitoring and observability
- Compliance and governance (SOC 2, audit trails, security reviews)
- Platform features (dashboards, analytics, integrations)
- Customer success infrastructure
- Minimum viable revenue per customer

The constant prevents the freeloader problem: low-usage customers still contribute to platform sustainability.

---

## The Inference Capture Ratio (ICR)

### Definition

**ICR = (Σ(αᵢ·Cᵢ) + β·V) / Total Inference Cost**

Where Total Inference Cost = the actual compute cost incurred by the provider.

### Interpretation

- **ICR = 1.0**: Pricing exactly covers inference cost. Zero margin for platform or profit.
- **ICR = 1.5**: Pricing captures 1.5x inference cost. The 0.5x excess covers platform overhead and profit.
- **ICR = 2.0**: Pricing captures 2x inference cost. Healthy margin.
- **ICR > 4.0**: Potentially extractive. Competitive vulnerability.

### Why ICR, Not Gross Margin

Traditional gross margin (revenue - COGS / revenue) is misleading for AI agents because:
- COGS is dominated by inference cost, which is volatile and dropping
- A high gross margin can mask an ICR < 1.0 (if the value multiplier is high but the floor is too low)
- ICR directly measures whether the pricing structure covers the underlying commodity cost

ICR is the **commodity-specific health metric** that gross margin obscures.

---

## Market Evidence: Convergence on the Three-Term Structure

### Salesforce Agentforce (2024-2026)

**Reconstructed from investor communications and public product announcements:**

| Period | Pricing Model | Term(s) Present | Gap |
|---|---|---|---|
| 2024 | Per-seat ($50/user/month) | γ only (misassigned to seats, not platform) | Missing Σ and β·V |
| 2025 | Consumption-based AI credits | Σ (consumption ≈ cost pass-through) | Missing β·V and proper γ |
| 2026 | Hybrid (converging) | Σ + β·V + γ (emerging) | Converging on three-term |

The trajectory documents the market discovering the three-term structure through iteration: each pure model failed, and the hybrid that emerged contains all three terms.

### Microsoft 365 Copilot (2024-2025)

- Per-seat ($30/user/month) for base Copilot → γ term (platform constant)
- Copilot Studio consumption pricing (November 2024) → Σ term (infrastructure floor)
- Trajectory: adding β·V (value multiplier) for outcome-based agent pricing

### Intercom Fin ($0.99/resolution)

- Pure outcome pricing → β·V term only
- Missing Σ (infrastructure floor) → catastrophic cost risk on expensive resolutions
- Missing γ (platform constant) → low-usage customers are unprofitable
- Market pressure pushing toward adding Σ and γ

### Zendesk ($1.50-$2.00/resolution)

- Same pure outcome structure as Intercom
- Same structural vulnerability
- Same convergence pressure toward three-term

### Bain & Company (2025)

"Pricing AI: How SaaS Companies Are Reinventing Monetization in the Agentic Era" — documents the industry-wide shift from per-seat to hybrid pricing. The three-term framework formalizes what Bain observes empirically.

---

## The LNG Contract Analogy

### Why LNG?

Long-term LNG contracts solve the same structural problem as AI agent pricing:
- **Input cost is volatile** (natural gas prices fluctuate; inference prices drop ~20-40% annually)
- **Value is linked to usage** (LNG value linked to energy delivered; AI value linked to outcomes delivered)
- **Infrastructure is fixed** (LNG terminals and ships; AI platform and compliance)

### How LNG Contracts Work

LNG contracts decompose price into:
1. **Commodity-indexed floor**: Price indexed to natural gas spot price (input cost pass-through)
2. **Value premium**: Premium above the floor reflecting delivery, reliability, and strategic value
3. **Infrastructure constant**: Fixed fees for terminal access, shipping, processing

This is structurally identical to P = Σ(αᵢ·Cᵢ) + β·V + γ. The LNG industry solved this pricing problem decades ago; AI agent pricing is rediscovering it.

### References (from the paper)

- International Gas Union. World LNG Report 2023 (IGU Publications, 2023).
- Yergin, D. The Prize: The Epic Quest for Oil, Money, and Power (Simon & Schuster, 1991).
- Hinterhuber, A. "Is Innovation in Pricing Your Next Source of Competitive Advantage?" Business Horizons 57(3), 2014.
- Nagle, T.T. and Müller, G. The Strategy and Tactics of Pricing, 6th ed. (Routledge, 2017).
- Lambrecht, A. and Skiera, B. "Paying Too Much and Being Happy About It." Journal of Marketing Research 43(2), 2006.

---

## The Privacy-First Structural Advantage (A-Tech-Specific Analysis)

### The Local-First Floor Advantage

For cloud-first competitors, the infrastructure floor (Σ) is always positive — they pay for every token of inference. For A-Tech's local-first architecture:

| Scenario | Cloud-First Floor | A-Tech Local-First Floor |
|---|---|---|
| On-device inference (SLM) | N/A (can't do it) | $0 (user's hardware) |
| Local mid-tier model | N/A | $0 (user's hardware) |
| Cloud frontier (escalation only) | $X per token | $X per token (same, but only for escalated queries) |

**The blended floor for A-Tech**: Near-zero for the 70-90% of inference that runs on-device; standard cloud cost for the 10-30% that requires frontier escalation. The blended floor is dramatically lower than any cloud-first competitor.

### The Pricing Implication

A-Tech can:
1. **Charge the same total price** → dramatically higher margin (same β·V + γ, near-zero Σ)
2. **Charge a lower price** → same margin, competitive advantage (undercut cloud-first competitors)
3. **Offer a free tier** → Σ = $0 makes a free tier sustainable (only β·V + γ need to be covered, and those can be zero for a free tier)

The privacy-first architecture is not just an ethical choice — it is a **pricing moat** that the three-term framework makes visible.

---

## Complete Bibliography (from the MPRA paper)

1. Hinterhuber, A. "Is Innovation in Pricing Your Next Source of Competitive Advantage?" Business Horizons 57(3), 2014: 413-423.
2. Nagle, T.T. and Müller, G. The Strategy and Tactics of Pricing, 6th ed. New York: Routledge, 2017.
3. Lambrecht, A. and Skiera, B. "Paying Too Much and Being Happy About It: Existence, Causes, and Consequences of Tariff Choice Biases." Journal of Marketing Research 43(2), 2006: 212-223.
4. Bala, R. and Carr, S. "Usage Based Pricing of Software Services Under Competition." Journal of Revenue and Pricing Management 9, 2010: 204-216.
5. Choudhary, V. "Software as a Service: Implications for Investment in Software Development." Proceedings of the 40th Annual Hawaii International Conference on System Sciences, 2007.
6. Ghose, A. and Han, S.P. "Estimating Demand for Mobile Applications in the New Economy." Management Science 60(6), 2014: 1470-1488.
7. International Gas Union. World LNG Report 2023. IGU Publications, 2023.
8. Yergin, D. The Prize: The Epic Quest for Oil, Money, and Power. New York: Simon & Schuster, 1991.
9. Salesforce Inc. "Agentforce: The Evolution of Flexible AI Credits." Salesforce Help Documentation, 2026. Pricing trajectory reconstructed from investor communications and public product announcements 2024-2026.
10. Microsoft Corporation. "Microsoft 365 Copilot Pricing and Licensing." Microsoft Learn, 2024-2025. Copilot Studio consumption pricing introduced November 2024.
11. Intercom. "Fin AI Agent Pricing." Intercom Product Documentation, 2024-2025.
12. Poyar, K. "A New Framework for AI Agent Pricing." Growth Unhinged, 2026.
13. Zendesk Inc. "AI and Automation Pricing." Zendesk Product Documentation, 2024-2025.
14. Gartner. "Worldwide IT Spending Forecast." Gartner Research, 2025.
15. McKinsey Global Institute. The Economic Potential of Generative AI: The Next Productivity Frontier. McKinsey & Company, 2025.
16. Bain & Company. "Pricing AI: How SaaS Companies Are Reinventing Monetization in the Agentic Era." Bain Technology Practice, 2025.
17. Hagiu, A. and Wright, J. "Multi-Sided Platforms." International Journal of Industrial Organization 43, 2015: 162-174.
18. Bergemann, D. and Välimäki, J. "Dynamic Pricing of New Experience Goods." Journal of Political Economy 114(4), 2006: 713-743.
19. Hart, O. Firms, Contracts, and Financial Structure. Oxford University Press, 1995.