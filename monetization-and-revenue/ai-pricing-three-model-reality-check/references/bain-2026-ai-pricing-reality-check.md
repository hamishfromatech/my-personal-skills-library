# Bain 2026 AI Pricing Reality Check — Detailed Reference

## Source
Bain & Company analysis of approximately 200 B2B SaaS companies' AI pricing models (June 2026). The analysis clarifies critical distinctions between effort, output, and outcome pricing, and reveals that most "usage-based" pricing is actually capacity pricing.

---

## 1. The Three Pricing Models

### Effort-Based Pricing (~35% of market)
Charges for resources the AI consumes.

**Meter examples**: tokens processed, compute hours, agent-hours, inference calls.

**Where it works**: Broad AI portfolios and infrastructure, where the meter acts as a common currency across many use cases. Infrastructure providers (LLM APIs, model hosting, agentic platforms) typically default here.

**Quality risk**: Buyer carries the risk. If the AI consumes resources but produces low-value output, the buyer still pays.

**Strengths**:
- Simple to meter and audit (usage logs are concrete)
- Scales naturally across heterogeneous workloads
- Aligns with vendor cost structure (compute is a real input cost)

**Weaknesses**:
- Disconnects price from value delivered
- Buyers bear risk of inefficient prompts/models
- Hard to justify to non-technical buyers who don't understand token economics

### Output-Based Pricing (~55% of market)
Charges for what the AI produces.

**Meter examples**: summaries generated, records updated, leads recommended, documents classified, tickets routed.

**Where it works**: Focused use cases where the AI produces visible, countable business artifacts. Higher-level applications (sales intelligence, content generation, data enrichment) typically land here.

**Quality risk**: Buyer carries the risk. Vendor gets paid regardless of whether the output creates downstream business value. A recommended lead that is never contacted, or a summary no one reads, still generates revenue for the vendor.

**Strengths**:
- Connects price to a tangible, countable deliverable
- Easier to benchmark against manual alternatives
- Buyers can forecast based on expected output volume

**Weaknesses**:
- Volume of output ≠ value of output
- Incentivizes vendor to maximize quantity, not quality
- Requires clear definition of what counts as a billable "output"

### Outcome-Based Pricing (~10% of market)
Charges for business results.

**Meter examples**: issues resolved, fraud recovered, revenue collected, deals closed, SLAs met.

**Where it works**: Workflows where the outcome is observable, uniquely attributable to the AI, and contractible between buyer and seller. Customer support is the clearest fit (resolved conversation is a clean, countable, attributable unit).

**Quality risk**: Vendor carries the risk. Payment depends on the business result actually materializing.

**Strengths**:
- Maximally aligned with buyer value
- Strongest value-capture story for vendor (share of upside)
- Differentiating in sales cycles (skin in the game)

**Weaknesses**:
- Requires the three conditions (observable, attributable, contractible) — most workflows fail at least one
- Vendor bears revenue volatility and quality risk
- Hard to scale beyond narrow, well-bounded workflows
- Attribution disputes (did the AI close the deal, or the sales rep?)

---

## 2. The Output vs. Outcome Distinction (Critical)

This is the most commonly confused distinction in AI pricing. "Usage-based" headlines mask which side of the line a product actually sits on.

### The Quality Risk Test
The single sharpest discriminator: **Who carries the quality risk?**

| | Output | Outcome |
|---|---|---|
| Vendor gets paid if... | AI produces the artifact | Business result materializes |
| Quality risk | Buyer | Vendor |
| Example (sales) | Recommended lead | Qualified lead |
| Example (data) | Updated record | Completed business process |
| Example (support) | Suggested response | Resolved conversation |

### Worked Examples
- A tool that **recommends 50 leads** → output. Whether any convert is the buyer's problem.
- A tool that **delivers 50 qualified leads** (verified, intent-scored, sales-ready) → outcome. Payment depends on qualification holding up.
- A tool that **updates 1,000 CRM records** → output. Whether those updates drive revenue is irrelevant to the invoice.
- A tool that **completes the lead-to-cash process end-to-end** → outcome. Payment depends on the process completing.

### Why the Confusion Persists
Marketing teams prefer "outcome-based" because it sounds value-aligned. Sales teams prefer it because it reduces procurement friction. But most products labeled "outcome-based" are actually output-based with aspirational framing. The test is structural, not promotional: follow the money — does the invoice depend on the business result, or just the artifact?

---

## 3. Capacity Pricing Dominates (~80% of Introductions)

### What Capacity Pricing Is
Buyers commit to a fixed amount of AI capacity (credits, tokens, interactions). Unused capacity is not refunded. Optional overages apply when capacity is exceeded.

This is **not** pure usage-based pricing — it is a commitment model with usage metering on top. Economically it behaves like seat-based pricing (predictable revenue, unused capacity retained by vendor) while offering the appearance of usage alignment.

### Why Vendors Choose It
- Preserves vendor economics similar to seat-based pricing (predictable revenue, no refund obligation)
- Provides buyer budget predictability (fixed line item, capped exposure)
- Avoids the "bill shock" problem of pure metered usage
- Familiar procurement pattern (commit → consume → renew)

### Why Buyers Accept It
- Budget predictability (finance teams favor fixed commitments)
- No surprise invoices from usage spikes
- Often bundled with seat licenses (lower marginal friction)

### Examples
- **Atlassian Rovo credits**: Fixed allocation of AI actions per user/plan; overages gated or upsold.
- **ServiceNow Now Assist**: Capacity-based AI features tied to subscription tier.
- **Adobe Firefly credits**: Generative credits allocated monthly; additional credits purchasable.

### The Strategic Implication
"Usage-based" as a market category is misleading. The dominant pattern is **capacity commitment with usage-denominated metering**. When evaluating or designing "usage-based" pricing, always ask: is this direct usage (pay for what you consume) or capacity (commit, then consume against the commitment)?

---

## 4. Market Split on Effort vs. Output

The market does not uniformly prefer one model. The split tracks product position:

| Product Position | Dominant Model | Rationale |
|---|---|---|
| Infrastructure (models, hosting, agent platforms) | Effort-based | Tokens/compute is a common currency across all downstream use cases; buyer is technical and understands the meter |
| Focused application (sales intelligence, content, enrichment) | Output-based | AI produces a visible, countable business artifact; charge connects to visible work |
| Customer support | Outcome-based | Resolved conversation is the rare observable + attributable + contractible outcome |
| Broad portfolio / platform | Effort-based or hybrid | A single output meter doesn't generalize across many use cases; effort provides common currency |

### Why Infrastructure Defaults to Effort
Infrastructure providers serve many downstream use cases they don't control. An output meter that makes sense for summarization (summaries generated) is meaningless for code completion or embeddings. Effort (tokens/compute) is the only common currency. Buyers are technical enough to accept resource-based metering.

### Why Applications Default to Output
Application vendors own the use case. The AI produces a specific, visible artifact (lead, summary, classification). Charging for that artifact connects price to something the buyer's business users recognize, not just infrastructure consumption they may not understand.

---

## 5. Outcome-Based Pricing — Limited but Real Role

### Where It Has a Real Home: Customer Support
Companies like Sierra, Fin (Intercom), and Decagon charge per resolved conversation. This works because a resolved conversation is:

1. **Observable**: The system can record that a conversation reached resolution.
2. **Attributable**: The AI handled the conversation; attribution to the AI is unambiguous (vs. a human agent).
3. **Contractible**: "Resolved" can be defined (conversation closed without escalation, or with customer confirmation) and agreed between buyer and seller.

### Where It Struggles Beyond Support
- **Marketing**: Attribution is disputed (was it the AI recommendation, the campaign, the market, the rep?). Multiple touchpoints, long cycles, competing claims.
- **HR**: Hiring decisions span many systems and humans (sourcing, screening, interviewing, deciding). AI is one input among many.
- **Engineering productivity**: "Lines of code" is meaningless; "features shipped" involves designers, PMs, QA. Outcome is emergent and multi-causal.
- **Sales**: "Deals closed" involves reps, pricing, market conditions. AI contribution is partial and contestable.

### The Pattern
Outcome-based works when the AI is the **primary, bounded agent** for a workflow with a clean termination signal. It fails when the AI is **one input** into a multi-actor, multi-system workflow with fuzzy boundaries.

---

## 6. The Three Conditions for Outcome-Based Pricing

A workflow supports outcome-based pricing only if all three conditions hold:

### Condition 1: Observable
Can the outcome be measured, automatically and reliably?
- ✅ Conversation resolved (system records closure state)
- ✅ Fraud transaction flagged and confirmed
- ❌ "Customer satisfaction improved" (not directly measurable)
- ❌ "Brand awareness increased" (no clean signal)

### Condition 2: Attributable
Can the outcome be uniquely attributed to the AI, not to other factors?
- ✅ AI agent handles support ticket end-to-end (no human in loop)
- ❌ AI recommends a lead, but sales rep qualifies and closes (shared attribution)
- ❌ AI suggests code, but engineer writes and reviews (shared contribution)

### Condition 3: Contractible
Can buyer and seller agree on a precise definition that holds up over time?
- ✅ "Resolved conversation = ticket closed without human escalation within 24h"
- ❌ "Qualified lead" (what counts as qualified? who decides?)
- ❌ "Successful hire" (did the AI source them, or just screen them?)

### Decision Rule
- All three conditions met → outcome-based is viable
- Any one fails → fall back to output-based (charge for the artifact)
- Multiple fail → fall back to effort-based (charge for resources)

---

## 7. Pricing Terminology Confusion

The analysis identifies a core problem: "usage-based" and "outcome-based" are headlines that mask the real strategic distinctions. The **specific meter** is the strategy, not the category label.

### Common Confusions
- **"Usage-based"** is used for effort-based (tokens), output-based (summaries), and capacity (committed credits). These are economically different.
- **"Outcome-based"** is applied to output-based products (recommended leads) that don't actually carry quality risk.
- **"Value-based"** is used interchangeably with outcome-based, but value-based is a pricing philosophy (price to willingness to pay), not a meter.

### The Clarifying Questions
1. **What is the meter?** (tokens, artifacts, results)
2. **Who carries quality risk?** (buyer or vendor)
3. **Is it direct usage or capacity?** (pay-per-consume or commit-then-consume)
4. **Does payment depend on the business result, or just the artifact?** (outcome vs. output)

These four questions cut through labeling to reveal the actual economic structure of a pricing model.

---

## 8. Practical Decision Framework

### For Vendors Designing AI Pricing

**Step 1: Classify your product position**
- Infrastructure / platform → lean effort-based
- Focused application → lean output-based
- Customer support / bounded agentic workflow → consider outcome-based

**Step 2: Decide capacity vs. direct usage**
- Predictable buyer demand + need for budget certainty → capacity
- Variable, technical buyer, infrastructure DNA → direct usage
- Default market pattern is capacity (~80%); choose direct usage deliberately

**Step 3: If considering outcome-based, test the three conditions**
- Observable? Attributable? Contractible?
- All pass → proceed, but design the contract carefully (definition, edge cases, fraud/gaming prevention)
- Any fail → use output-based and be honest about it

**Step 4: Be precise in marketing**
- Don't label output-based as outcome-based. Buyers and analysts will eventually notice.
- The specific meter is your strategy. Name it accurately.

### For Buyers Evaluating AI Pricing

**Step 1: Ask what the meter actually is**
- Tokens? Artifacts? Results? Credits?

**Step 2: Ask who carries quality risk**
- If you pay regardless of value → you carry it (output or effort model)
- If vendor only gets paid on result → they carry it (outcome model)

**Step 3: Ask if it's capacity or direct usage**
- Am I committing to a pool I lose if unused? (capacity)
- Am I paying only for consumption? (direct usage)

**Step 4: Benchmark the meter against your value driver**
- If you're buying support automation, outcome-based (per resolution) aligns incentives
- If you're buying infrastructure, effort-based (tokens) is appropriate
- If you're buying a focused app, output-based is expected — verify the output definition is tight

---

## 9. Key Data Summary

| Metric | Value |
|---|---|
| Companies analyzed | ~200 B2B SaaS |
| Effort-based market share | ~35% |
| Output-based market share | ~55% |
| Outcome-based market share | ~10% |
| Capacity pricing among AI pricing introductions | ~80% |
| Direct usage share | ~20% |

### Named Vendor Examples
- **Effort-based (capacity)**: Atlassian Rovo, ServiceNow Now Assist, Adobe Firefly
- **Outcome-based (customer support)**: Sierra, Fin (Intercom), Decagon

---

## 10. Anti-Patterns to Avoid

1. **Labeling output-based as outcome-based** — the most common misrepresentation. If the vendor gets paid when the artifact is produced, regardless of downstream value, it's output-based.

2. **Assuming "usage-based" means pay-per-consume** — ~80% of the time it means capacity commitment. Check whether unused capacity is refunded.

3. **Forcing outcome-based where attribution is messy** — marketing, HR, engineering productivity. The three conditions test exists for a reason.

4. **Using effort-based meters for business-user applications** — tokens are meaningless to a sales leader. Use output meters that map to recognizable work products.

5. **Using a single output meter across a broad portfolio** — no single output definition generalizes across summarization, code completion, and embeddings. Fall back to effort-based.

6. **Ignoring the quality risk allocation** — the decision of who bears risk is the core strategic choice, more than the meter itself.

---

*Reference based on Bain & Company analysis of ~200 B2B SaaS companies' AI pricing (June 2026).*