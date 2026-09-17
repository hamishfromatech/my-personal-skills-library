# Agentic AI Pricing: COMPASS Framework & Impossible Triangle Reference

> Source: Zuora Subscribed Institute / Mansard (2026). This reference documents the COMPASS Framework for agentic AI pricing model selection and Tien Tzuo's Impossible Triangle of pricing forces.

---

## Table of Contents

1. [The Pricing Transition Problem](#1-the-pricing-transition-problem)
2. [The Impossible Triangle (Tien Tzuo)](#2-the-impossible-triangle-tien-tzuo)
3. [The COMPASS Framework (Mansard)](#3-the-compass-framework-mansard)
4. [COMPASS Matrix Mapping](#4-compass-matrix-mapping)
5. [Four Canonical Pricing Models](#5-four-canonical-pricing-models)
6. [Hybrid Models (Most Practical)](#6-hybrid-models-most-practical)
7. [Pilot & Evolution Path](#7-pilot--evolution-path)
8. [Decision Walkthroughs](#8-decision-walkthroughs)
9. [Anti-Patterns & Failure Modes](#9-anti-patterns--failure-modes)
10. [Glossary](#10-glossary)

---

## 1. The Pricing Transition Problem

Per-seat (per-agent license) pricing makes up approximately **41% of agentic AI pricing today**, but this model cannibalizes itself as agents become more autonomous. The core problem: a per-seat license grants unlimited usage against a variable inference cost floor. As agents execute more calls, tool uses, and context-heavy operations per seat, margin collapses.

The market is in transition from:

| Era | Dominant Model | Margin Dynamic |
|---|---|---|
| **Copilot/Assistive** | Per-seat (+ usage add-on) | Stable — human in the loop bounds usage |
| **Agentic (current)** | Mixed: per-seat, per-activity, per-outcome | Volatile — inference cost scales with autonomy |
| **Autonomous (future)** | Per-outcome / hybrid | Aligned — price tracks value, cost variance managed |

The COMPASS Framework exists to systematize the choice of meter for this transition rather than defaulting to per-seat out of habit.

---

## 2. The Impossible Triangle (Tien Tzuo)

Three forces pull on every agentic AI pricing decision. You can optimize for two; the third trades off.

### 2.1 Cost-to-Serve

The real, variable cost of running the agent. Components:

- **Per-call model compute**: Token consumption per inference
- **Context window cost**: Larger/longer contexts = more tokens
- **Tool-use overhead**: Each tool call triggers additional model round-trips
- **Multi-step accumulation**: Agents that chain 10+ steps multiply inference cost

Pricing that optimizes for Cost-to-Serve protects margin by tying revenue to actual usage. Per-activity and per-output models do this; per-seat does not.

**Risk of over-optimizing**: Bills become unpredictable, raising Customer Adoption friction. Token-level metering exposes raw cost to buyers who don't want to think about tokens.

### 2.2 Customer Adoption

How easy it is for a customer to start and continue using the agent. Levers:

- **Generous free tiers**: Let buyers test value before committing
- **Predictable bills**: Flat-rate or capped pricing reduces budget anxiety
- **Bundled access**: One price for a suite of agent capabilities

Pricing that optimizes for Customer Adoption removes friction but can starve margin under variable AI cost. Per-seat and base-subscription models serve this corner.

**Risk of over-optimizing**: If usage scales faster than the flat price covers inference, the seller loses money on the heaviest users. The classic SaaS "good customer / bad customer" problem, amplified by variable compute.

### 2.3 Value Delivered

How directly pricing tracks the business value the agent creates. Measurement approaches:

- **Per resolved ticket** (support)
- **Per closed deal** (sales)
- **Per document drafted and accepted** (legal)
- **Per anomaly detected and confirmed** (security)

Pricing that optimizes for Value Delivered captures maximum willingness-to-pay and aligns seller incentives with buyer outcomes. Per-outcome models serve this corner.

**Risk of over-optimizing**: Requires robust measurement infrastructure. If attribution is fuzzy (the agent contributed but didn't own the outcome), per-outcome pricing becomes contentious and hard to enforce. Also transfers cost variance entirely to the seller — a high-volume, low-resolution-rate month is a margin disaster.

### 2.4 The Trade-Off Explicitly

| Optimize | Trade Away | Example |
|---|---|---|
| Cost-to-Serve + Value Delivered | Customer Adoption | Per-outcome with cost pass-through — precise but complex to buy |
| Cost-to-Serve + Customer Adoption | Value Delivered | Per-activity at flat rate — easy to buy but may underprice high-value work |
| Customer Adoption + Value Delivered | Cost-to-Serve | Per-outcome with no cost cap — buyer loves it, seller bleeds on heavy users |

**The discipline**: Name explicitly which two corners you optimize and which one you trade. If you can't name the trade-off, your pricing isn't designed — it's defaulted.

---

## 3. The COMPASS Framework (Mansard)

A two-axis decision matrix that maps an agent's characteristics to the appropriate pricing model family.

### 3.1 Axis 1: Scope of Agent's Work

| Scope | Definition | Example |
|---|---|---|
| **Task** | One discrete action | Answer a question, classify a record, summarize a document |
| **Process** | Multi-step workflow | Triage a support ticket, qualify a lead, route an approval |
| **Goal** | Owns an outcome | Resolve a support issue end-to-end, close a deal, complete a financial close |

The progression Task → Process → Goal reflects increasing autonomy and ownership. Higher scope generally warrants higher-attribution pricing (per-output or per-outcome) because the agent's contribution to value is clearer.

### 3.2 Axis 2: Level of Attribution

| Level | Definition | Measurement Difficulty |
|---|---|---|
| **Diffuse** | Contributes to outcome, but many other factors also influence it | High — hard to isolate agent's impact |
| **Medium** | Measurably moves the outcome but doesn't fully own it | Medium — directional attribution possible |
| **Direct** | Owns the outcome; unambiguously measurable | Low — outcome is the agent's output |

Attribution is the hinge: if you can't measure the agent's contribution to value, per-outcome pricing is aspirational, not operational. Diffuse attribution forces you toward per-agent or per-activity models (which charge for access/usage, not results).

---

## 4. COMPASS Matrix Mapping

The full 3×3 matrix mapping scope × attribution to pricing model:

| Scope \ Attribution | Diffuse | Medium | Direct |
|---|---|---|---|
| **Task** | Per Agent / Per Activity | Per Activity / Per Output | Per Output / Per Outcome |
| **Process** | Per Agent / Per Activity | Hybrid | Per Outcome / Hybrid |
| **Goal** | Per Agent / Per Activity | Hybrid | Per Outcome |

### Reading the matrix

- **Task + Diffuse**: The agent does one small thing and its impact is hard to isolate. Per-agent (simple license) or per-activity (per call) are the only viable options. Don't attempt per-outcome — you can't attribute.
- **Task + Direct**: One discrete action with a clear, measurable result (e.g., a generated image, a classified record). Per-output or per-outcome works well.
- **Process + Medium**: Multi-step workflow that measurably helps but doesn't own the result. **Hybrid** is the dominant recommendation — base subscription for floor revenue + activity/output metering for usage alignment.
- **Goal + Direct**: The agent owns the outcome and it's measurable. **Per Outcome** is the clear answer — this is where outcome-based pricing is both fair and operationally feasible.
- **Goal + Diffuse**: The agent owns an outcome but many factors influence it (e.g., a sales agent that contributes to closing but so does the human rep, the product, the market). Fall back to Per Agent / Per Activity because you can't cleanly attribute.
- **Goal + Medium**: Hybrid — base + outcome with guardrails.

### Matrix logic summary

- **Left column (Diffuse)**: Always Per Agent or Per Activity — you can't charge for outcomes you can't measure.
- **Center column (Medium)**: Hybrid — blend predictability (base) with alignment (usage/outcome metering).
- **Right column (Direct)**: Per Output or Per Outcome — charge for the result.
- **Bottom row (Goal)**: Higher propensity for outcome-based because the agent owns the result.
- **Top row (Task)**: Lower propensity for outcome-based because the unit of work is too small to attribute value cleanly.

---

## 5. Four Canonical Pricing Models

### 5.1 Per Agent (Per AI License)

**Mechanism**: Fixed fee per AI agent license, analogous to per-seat SaaS pricing.

**Best fit**: Bounded scope agents where usage is predictable and capped. Task + Diffuse quadrant.

**How it works**:
- Buyer pays $X/month per agent license
- Unlimited usage within fair-use limits (or hard caps)
- Seller absorbs inference cost variance

**Breaks when**: The agent processes thousands of variable-cost activities per billing cycle. Inference cost exceeds the license fee. Margin goes negative for power users. This is the cannibalization risk — 41% of the market sits here today.

**Example**: Early-stage AI assistant with bounded scope (one agent, one task, capped daily volume).

### 5.2 Per Activity (Per Call / Per Query / Per Workflow Run)

**Mechanism**: Charge per discrete unit of agent action — one conversation, one query, one workflow run.

**Best fit**: When activity is the natural unit and aligns revenue with cost without exposing raw token economics to the buyer.

**How it works**:
- Buyer pays $Y per conversation / per workflow execution
- Seller absorbs token-level variance but controls it through activity-level pricing
- Margin protected as long as average cost-per-activity < price-per-activity

**Strength**: Hides complexity. The buyer doesn't need to understand token pricing, context windows, or tool-call overhead — they buy conversations, not compute.

**Example**: Salesforce Agentforce at ~$2 per conversation. The buyer pays per conversation; Salesforce manages the inference economics internally.

### 5.3 Per Output (Per Generated Artifact)

**Mechanism**: Charge per discrete artifact the agent produces — a document, an image, a demand letter, a report.

**Best fit**: When the artifact is the unit of value and is directly attributable. Task + Direct or Process + Direct quadrants.

**How it works**:
- Buyer pays $Z per generated output (per image, per letter, per report)
- Seller prices based on value of the artifact, not cost to produce
- Margin = price-per-artifact minus average inference cost per artifact

**Strength**: Aligns price with a tangible deliverable the buyer values and can count.

**Examples**:
- Adobe Firefly: credits per generated image
- EvenUp: per demand letter generated for personal injury cases

**Risk**: If artifacts are easy to generate in bulk (low marginal cost, low value), the model degrades. Best when each artifact carries meaningful, attributable value.

### 5.4 Per Outcome (Per Defined Business Result)

**Mechanism**: Charge per defined business outcome — per resolved ticket, per closed deal, per qualified appointment.

**Best fit**: When the agent owns the outcome and it's unambiguously measurable. Goal + Direct quadrant.

**How it works**:
- Buyer pays $W per resolved ticket / per closed deal / per qualified lead
- Seller bears full cost variance — if resolution rate drops, cost-per-outcome rises while revenue-per-outcome stays flat
- Maximum alignment of seller incentive with buyer value

**Strength**: Buyer pays only for results. No risk of paying for activity that produced nothing. Highest willingness-to-pay capture.

**Risk**: Transfers cost variance entirely to the seller. A high-volume, low-resolution month is a margin disaster. Requires measurement infrastructure (how do you define and verify "resolved"?). Disputes over what counts as an outcome.

**Examples**:
- Intercom Fin: ~$0.99 per resolved ticket (resolution verified by conversation analysis)
- Zendesk: per-resolution pricing for AI agents

**Mitigation**: Pair per-outcome with a cost cap or floor (see Hybrid models below).

---

## 6. Hybrid Models (Most Practical)

Most real-world agentic AI pricing is hybrid because pure models break at the edges. Hybrids blend predictability with alignment.

### 6.1 Base Subscription + Activity Overage

**Structure**:
- Monthly/annual base subscription (floor revenue, predictable)
- Included activity allowance (e.g., 1,000 agent conversations/month)
- Overage rate per activity beyond allowance (e.g., $0.50/conversation)

**Why it works**:
- Seller gets guaranteed floor revenue to cover fixed costs
- Overage captures value from heavy users without punishing light users
- Buyer has predictable base cost; only power users pay more

**COMPASS fit**: Process + Medium, Process + Diffuse, Goal + Diffuse

**Example**: $500/month base includes 2,000 agent actions; $0.25 per action over 2,000.

### 6.2 Prepaid Credits + Drawdown

**Structure**:
- Buyer prepurchases credits (e.g., 10,000 credits for $2,000)
- Different activities draw down credits at different rates (e.g., 1 credit per query, 5 credits per workflow run, 20 credits per generated report)
- Credits expire after a period or roll forward

**Why it works**:
- Upfront capital for the seller
- Buyer sees granular value-per-action without per-token complexity
- Naturally segments activities by cost/value through credit weighting

**COMPASS fit**: Task + Medium, Process + Medium, any scope with Medium attribution

**Example**: 10,000 credits for $2,000. Simple query = 1 credit. Workflow run = 5 credits. Generated report = 20 credits.

### 6.3 Outcome-Based with Cost Cap

**Structure**:
- Primary meter: per outcome ($0.99 per resolved ticket)
- Guardrail: monthly cost cap (buyer never pays more than $X/month total)
- Or: seller-side cap (seller won't bear more than $Y in inference cost per outcome — above that, converts to per-activity pricing)

**Why it works**:
- Captures value alignment of per-outcome (buyer pays for results)
- Caps downside for both parties — buyer has budget certainty, seller has margin floor
- Prevents the "margin disaster" scenario of pure per-outcome

**COMPASS fit**: Goal + Direct, Process + Direct

**Example**: $0.99/resolved ticket, capped at $5,000/month. Below cap, pure outcome pricing. At cap, converts to flat rate for remaining resolutions (buyer protected, seller floor secured).

### 6.4 Choosing a Hybrid

| If your agent is... | Recommended Hybrid |
|---|---|
| Process + Medium attribution | Base subscription + activity overage |
| Task + Medium attribution | Prepaid credits + drawdown |
| Goal + Direct attribution | Outcome-based with cost cap |
| Goal + Diffuse attribution | Base subscription + activity overage (don't attempt outcome) |

---

## 7. Pilot & Evolution Path

### 7.1 Pilot Before Scaling

Before rolling out a new pricing model across all customers:

1. **One product line**: Test on a single agent/use case, not the entire portfolio
2. **One customer cohort**: A representative segment (e.g., mid-market support teams)
3. **Two billing cycles**: Long enough to see renewal signals and seasonal variance

**Measure**:
- **Margin-per-activity**: (Revenue per activity) − (Inference cost per activity). Is it positive and stable?
- **Customer renewal signal**: Are customers on the new pricing model renewing at the same or better rate than the control cohort?
- **Adoption friction**: Did the new pricing slow down sales cycles or increase support tickets?

### 7.2 Pricing Must Evolve

Agentic AI pricing is not set-and-forget. As the agent matures, pricing should shift:

| Agent Maturity | Value Clarity | Pricing Direction |
|---|---|---|
| **Augmentation** (copilot, human-in-loop) | Fuzzy — hard to attribute | Per-agent or per-activity (access-based) |
| **Replacement** (autonomous, owns outcome) | Hard ROI — measurable impact | Per-output or per-outcome (value-based) |

**The trajectory**: augmentation → replacement, fuzzy value → hard ROI. Your pricing should follow this arc. If your agent is evolving toward autonomy but your pricing is still per-seat, you're leaving margin on the table for light users and losing money on heavy users.

---

## 8. Decision Walkthroughs

### Walkthrough 1: Support Ticket Resolution Agent

**Agent**: AI agent that resolves support tickets autonomously (answers questions, processes refunds, escalates when needed).

**Step 1 — Scope**: **Goal**. The agent owns the outcome — resolve the support issue.

**Step 2 — Attribution**: **Direct**. A "resolved ticket" is unambiguously measurable (conversation closed, customer confirmed resolution, no reopen within X days).

**Step 3 — Matrix**: Goal + Direct → **Per Outcome**.

**Step 4 — Impossible Triangle**: Optimize Value Delivered (price tracks resolution) + Customer Adoption (buyer pays only for results, no risk). Trade away Cost-to-Serve (seller bears cost variance).

**Step 5 — Risk**: Pure per-outcome exposes seller to margin disaster on high-volume, low-resolution months.

**Step 6 — Hybrid**: Outcome-based with cost cap. $0.99/resolved ticket, capped at $5,000/month. Below cap: pure per-outcome. At cap: flat rate for remaining resolutions.

**Result**: Intercom Fin model. Buyer pays for results with budget certainty; seller captures value with margin floor.

---

### Walkthrough 2: Lead Qualification Agent

**Agent**: AI agent that qualifies inbound leads — scores them, enriches data, routes to sales reps.

**Step 1 — Scope**: **Process**. Multi-step workflow (enrich → score → route).

**Step 2 — Attribution**: **Medium**. The agent measurably improves lead quality and conversion, but the human rep, product, and market also influence the closed deal.

**Step 3 — Matrix**: Process + Medium → **Hybrid**.

**Step 4 — Impossible Triangle**: Optimize Customer Adoption (predictable base cost) + Cost-to-Serve (activity overage protects margin). Trade away Value Delivered (can't cleanly charge per closed deal because attribution is shared).

**Step 5 — Hybrid**: Base subscription + activity overage. $300/month base includes 500 qualified leads; $0.40 per qualified lead over 500.

**Result**: Predictable for buyer, margin-protected for seller, defers per-outcome pricing until attribution improves.

---

### Walkthrough 3: Document Classification Agent

**Agent**: AI agent that classifies incoming documents (invoice, contract, receipt) and routes to the right system.

**Step 1 — Scope**: **Task**. One discrete action per document.

**Step 2 — Attribution**: **Diffuse**. Classification contributes to downstream efficiency but many factors influence the overall outcome.

**Step 3 — Matrix**: Task + Diffuse → **Per Agent / Per Activity**.

**Step 4 — Impossible Triangle**: Optimize Cost-to-Serve (per-activity aligns revenue with usage) + Customer Adoption (per-activity is simple to understand). Trade away Value Delivered (can't attribute business value to classification alone).

**Step 5 — Model**: Per Activity. $0.02 per document classified.

**Result**: Simple, margin-aligned, doesn't over-claim value attribution. Don't attempt per-outcome — the agent's contribution to business value is too diffuse.

---

### Walkthrough 4: Legal Document Drafting Agent

**Agent**: AI agent that drafts demand letters, contracts, and legal summaries from case files.

**Step 1 — Scope**: **Task** (drafting) to **Process** (research + draft + cite).

**Step 2 — Attribution**: **Direct** (the artifact — the drafted document — is the measurable output).

**Step 3 — Matrix**: Task + Direct → **Per Output**. Or Process + Direct → **Per Outcome / Hybrid**.

**Step 4 — Impossible Triangle**: Optimize Value Delivered (per document drafted) + Cost-to-Serve (output pricing covers inference). Trade away Customer Adoption (buyer pays per artifact, which is less predictable than a flat fee).

**Step 5 — Model**: Per Output. $X per demand letter drafted.

**Example**: EvenUp — per demand letter generated for personal injury cases. The artifact is the unit of value and is directly attributable.

---

## 9. Anti-Patterns & Failure Modes

### 9.1 Per-Seat for Autonomous Agents
**The trap**: Defaulting to per-seat pricing because it's familiar and easy to sell.
**Why it fails**: As agent autonomy increases, usage per seat scales unboundedly while the price stays flat. Inference cost exceeds license revenue for power users. Margin goes negative.
**The fix**: Move to per-activity or per-output before the cannibalization hits. Use COMPASS to identify the right meter.

### 9.2 Per-Outcome Without Attribution Infrastructure
**The trap**: Charging per resolved ticket / per closed deal when you can't robustly measure the outcome.
**Why it fails**: Disputes over what counts as "resolved" or "closed." Buyers challenge charges. Seller can't verify outcomes at scale.
**The fix**: Don't attempt per-outcome in the Diffuse attribution column. Build measurement infrastructure first (resolution verification, outcome confirmation, dispute workflows), then migrate.

### 9.3 Pure Per-Outcome Without Cost Cap
**The trap**: Full per-outcome pricing with no guardrails.
**Why it fails**: A high-volume, low-resolution month (e.g., a product bug wave generates thousands of hard-to-resolve tickets) destroys seller margin. Cost-per-outcome spikes while revenue-per-outcome stays flat.
**The fix**: Always pair per-outcome with a cost cap (buyer-side budget certainty + seller-side margin floor).

### 9.4 Exposing Token Economics to Buyers
**The trap**: Metering at the token level — charging per 1,000 tokens like an API provider.
**Why it fails**: Buyers don't understand or want to manage token budgets. It makes bills unpredictable and creates constant negotiation over context window sizes and model selection.
**The fix**: Use per-activity or per-output metering that hides token complexity. The seller manages inference economics internally.

### 9.5 Pricing for the Agent You Have, Not the Agent You're Building
**The trap**: Pricing for current capability (copilot, human-in-loop) and not evolving as the agent becomes autonomous.
**Why it fails**: When the agent evolves from augmentation to replacement, per-seat or per-activity pricing under-captures the value of autonomous outcomes.
**The fix**: Plan the pricing evolution trajectory: augmentation (access-based) → replacement (value-based). Revisit pricing every 2-3 billing cycles as the agent matures.

---

## 10. Glossary

| Term | Definition |
|---|---|
| **Agentic AI** | AI systems that take autonomous actions to accomplish goals, not just respond to prompts |
| **Per Agent** | Fixed per-license pricing (analogous to per-seat SaaS) |
| **Per Activity** | Per discrete agent action (call, query, workflow run) |
| **Per Output** | Per generated artifact (document, image, report) |
| **Per Outcome** | Per defined business result (resolved ticket, closed deal) |
| **Hybrid** | Blended model combining two or more canonical meters (e.g., base + overage) |
| **Cost-to-Serve** | Variable inference cost of running the agent (tokens × context × tool calls) |
| **Customer Adoption** | Ease of starting and continuing use (free tiers, predictable bills, bundles) |
| **Value Delivered** | How directly price tracks business value created by the agent |
| **Scope of Work** | COMPASS axis: Task (one action) → Process (workflow) → Goal (owns outcome) |
| **Attribution** | COMPASS axis: Diffuse → Medium → Direct (how cleanly agent's impact is measurable) |
| **Impossible Triangle** | Tien Tzuo's framework: optimize two of Cost-to-Serve / Adoption / Value, trade the third |
| **COMPASS Matrix** | 3×3 mapping of Scope × Attribution to pricing model recommendations |
| **Margin-per-activity** | Revenue per activity minus inference cost per activity — the core unit economics metric |
| **Cost variance transfer** | In per-outcome, the seller absorbs variability in cost-per-outcome rather than passing it to the buyer |
| **Cannibalization risk** | Per-seat pricing loses margin as agent autonomy scales usage per seat |

---

*Reference compiled from Zuora Subscribed Institute / Mansard (2026) research on agentic AI pricing frameworks. The COMPASS Framework and Impossible Triangle provide complementary lenses: COMPASS selects the model, the Impossible Triangle validates the trade-offs.*