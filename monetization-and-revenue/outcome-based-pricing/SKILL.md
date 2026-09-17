# Skill: Outcome-Based Pricing for AI Products

## Concept Summary
Outcome-based pricing charges customers for successful results rather than access, usage, or compute. This model is emerging as the dominant monetization strategy for AI-native products in 2025-2026, moving beyond SaaS seat-based models to align revenue with measurable value delivered.

## Key Principles
1. **Charge for work completed, not access granted** — Customers pay per resolved ticket, per drafted document, per generated asset.
2. **Accept cost variability in exchange for value alignment** — Difficult tasks cost more to serve but build stronger customer relationships.
3. **Hybrid models bridge uncertainty** — Base platform fee (2x delivery costs) + outcome credits for scalability.
4. **Find pricing sweet spot through friction** — Raise prices until customers pause; stop just before it blocks deals.
5. **Map products to ROI clarity** — Copilots = soft ROI; Agents = hard ROI; Service replacement = clear cost reduction.

## Alignment with A-Tech Values
- **Financial Freedom**: Transparent value exchange; customers only pay for realized outcomes. Predictable costs empower users.
- **Practical Implementation**: Forces product builders to deliver measurable results, not promises.
- **Open-Source AI**: Can be applied to open-source billing platforms (e.g., Zenskar-style flexible billing for AI agents).
- **Data Privacy**: Outcome metrics can be computed locally without sending sensitive data to pricing servers.

## Applications

### A-Coder (IDE)
- Charge per accepted code suggestion rather than per keystroke or API token.
- Hybrid: Base IDE license + per "accepted completion" or "bug resolved" credit.
- Metric: Developer acceptance rate (% of AI-generated code accepted) becomes the value north star.

### Be Practical (Book/Playbooks)
- Playbooks priced per implemented outcome (e.g., "AI adoption playbook: pay when team hits 80% usage rate").
- Outcome-based consulting tiers tied to measurable client results.

### Open Source AI Builder's Club
- Agent marketplace where builders earn per successful agent execution, not per download.
- Dual licensing: Open-source core + outcome-based premium execution tier.
- Revenue share model: Builders keep majority; platform takes fee only on successful outcomes.

## Implementation Checklist
- [ ] Define the unambiguous, measurable outcome for your product
- [ ] Model cost variance (best-case vs worst-case compute per outcome)
- [ ] Set platform fee at 2x calculated infrastructure costs
- [ ] Test pricing through incremental friction (raise until customers hesitate)
- [ ] Build outcome verification (can be privacy-preserving / on-device)
- [ ] Track unit economics from day one including human-in-the-loop costs

## Sources
- Bessemer Venture Partners AI Pricing Playbook (2026)
- HBR: How Behavioral Science Can Improve AI ROI (2025)

---

## CYCLE-22-RUN-2 UPDATE (2026-09-10): The Frontier Lab Validates Outcome Pricing

**Source:** Sarah Friar, OpenAI CFO, Goldman Sachs Communacopia + Technology Conference (Sept 8, 2026, via Reuters Sept 8–9): OpenAI is **"experimenting with pricing based on business outcomes rather than use"** while pushing into sector-specialized AI (chip design, life sciences, financial services) and explicitly undercutting open-source rivals on cost.

**What this changes for this skill:** The outcome-based model is no longer a 2025–2026 open-source/startup pattern — the largest closed frontier lab is publicly piloting it. Two structural implications for this skill's framework:

1. **The outcome tier is now a frontier-lab tier.** The three-phase pricing evolution (feature add-on → usage-based → outcome-based) is confirmed from the top of the market, not just the bottom. Any AI product roadmap should treat outcome quotes as a mainstream enterprise option, not an experiment.
2. **The margin logic inverts for open-weight builders.** Friar's "cost advantage over open-source" posture is a defense against open-weight margin compression — for an open-weight builder, the same outcome-pricing playbook run on hosted open inference (near-marginal-cost compute) can quote outcomes at margins frontier labs cannot match. The frontier lab is conceding that per-token pricing cannot prove enterprise ROI; outcome quotes plus open-weight cost structure is the winning combination.

**Companion skills created this run:** `gpt6-outcome-pricing-pivot` (the pivot's three signals), `mistral-24b-hardware-hedge` (the open-weight commercial validation the pivot responds to).
