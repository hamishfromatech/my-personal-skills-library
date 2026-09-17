# Hg Analysis: Monetizing the Agentic Layer in B2B Software

**Source**: Hg (private equity), with Shiv Pabari
**Date**: May 2026
**Scope**: How B2B software companies should monetize the agentic layer — both their own agents and third-party agents accessing their platform.

---

## 1. The Agentic Layer Challenge

An agentic layer is forming across B2B software. Agents now execute significant parts of the work previously done by humans inside SaaS products — data entry, compliance checks, invoice processing, issue resolution, certification workflows, and more.

This creates two monetization questions that arrive simultaneously:

1. **How to monetize agents you build**: The agent is your product extension; you own the outcome it delivers.
2. **How to manage and monetize third-party agents connecting to your platform**: External agents (general-purpose like Claude/ChatGPT, or specialized tools) are already connecting to B2B platforms via APIs and exposed functionality.

### Why This Is Time-Sensitive

- Customer workflows settle around whichever agent arrives first.
- Once a workflow habit forms around an agent, it sticks — switching costs are high.
- Third-party access is already a reality, not a future scenario:
  - Salesforce exposed its entire platform via Headless 360.
  - Enterprise customers are connecting general-purpose agents (Claude, ChatGPT) to their B2B software stacks.
- Blocking third-party agent access is rarely a viable long-term strategy. The question is not whether to allow it, but how to monetize and govern it.

---

## 2. Pricing Your Own Agents

When you build and own the agent, you control the outcome it delivers. This enables outcome-aligned pricing — the highest-value-capture model.

### 2.1 Outcome-Aligned Pricing

Price aligned to the outcome the agent delivers:

- Per certification confirmed
- Per issue resolved
- Per invoice processed without human review
- Per compliance check completed

This model captures more revenue per customer than traditional per-seat licensing because revenue scales with the value the customer actually receives, not the number of humans sitting in front of the software.

### 2.2 FabricAI Case Study (Visma / Hg Portfolio)

FabricAI provides agentic invoice processing. Their model charges **per invoice processed without human involvement**. This captures more revenue per customer than the previous (per-seat or per-license) model because:

- The agent removes humans from the loop, increasing throughput.
- Each processed invoice is a measurable, billable outcome.
- Customers pay for results, not access.

### 2.3 Pricing Progression (Phased Approach)

Moving directly to outcome-based pricing is hard at launch because:
- Metering infrastructure may not be mature.
- Credit weighting (how much each task type is "worth") requires calibration.
- Customers need time to trust agent reliability before paying per outcome.

**Recommended progression:**

1. **Launch phase — Fixed uplift + generous allowances**:
   - Add a fixed uplift to the existing license fee.
   - Include generous usage allowances so customers can experiment.
   - Low risk for both sides; focuses on adoption over optimization.

2. **Growth phase — Credits with task-weighted consumption**:
   - Introduce a credit system where higher-value tasks consume more credits.
   - Begin correlating price to value delivered.
   - Several Hg portfolio companies adopting credit-based models.

3. **Mature phase — Outcome-based pricing**:
   - Charge per completed agent outcome.
   - Full value alignment: revenue scales with outcomes delivered.
   - Requires robust metering and customer trust in agent reliability.

### 2.4 Credits as a Unifying Metric

Credits serve as an intermediate pricing unit between flat fees and pure outcome pricing:

- Higher-value tasks consume more credits (e.g., a complex compliance action costs more credits than a routine data lookup).
- Customers buy credit bundles; consumption maps to actual agent activity.
- Enables tiered value capture without requiring perfect outcome metering from day one.

**Key design challenge**: Getting credit weighting right. A routine data lookup and a complex compliance action are very different in value. If weighting is too flat, you undercharge for high-value work. If too granular, pricing becomes opaque and hard to sell.

Several Hg portfolio companies are adopting credit-based models as the bridge between license fees and outcome pricing.

---

## 3. Pricing Third-Party Agent Access

When a third-party agent connects to your platform, you are NOT delivering the outcome — the third-party agent is. This eliminates outcome-based pricing as an option. You didn't build the agent, you don't control its reliability, and the value it creates is partly attributable to the third party.

### 3.1 Tier-Gating (Most Common Approach)

Govern external agent access through plan levels:

- Agents inherit the permissions of the customer whose account they act on behalf of.
- A customer on a lower tier has restricted agent access; higher tiers unlock more agent-accessible functionality.
- This preserves existing pricing architecture while accommodating agent-driven usage.

### 3.2 Layer Consumption-Based Pricing on Top

Tier-gating alone doesn't capture the increased usage that agents drive (agents can take far more actions per hour than a human user). Layer consumption-based pricing on top of tier-gating:

- Revenue scales as agent usage grows.
- Agents taking thousands of API actions generate more consumption revenue than a human taking dozens.
- Aligns revenue with actual platform load and value extraction.

### 3.3 The Salesforce / ServiceNow Convergence

Salesforce and ServiceNow independently converged on the same approach:

- **Charge per agent action through Flex Credits.**
- The same consumption unit applies regardless of what triggers the action:
  - Salesforce's own Agentforce agent
  - A customer-built agent
  - A third-party tool connecting via API
- This is the **"indifferent to interface"** commercial model: the platform doesn't care (commercially) whether the action came from its own agent, the customer's agent, or a third party. All actions are billed through the same unit.

This is a significant strategic design because it:
- Removes the need to distinguish (for billing purposes) between agent sources.
- Prevents revenue leakage when customers route work through third-party agents.
- Ensures the platform monetizes the agentic layer regardless of who builds the agent.

---

## 4. Infrastructure Requirements

Consumption-based and agent-action-based pricing is impossible without the right infrastructure. Four components are required:

### 4.1 Agent Connection Layer

A clean, consistent way for agents to connect to the platform and take actions:

- Standardized API surfaces designed for agent consumption (not just human UI).
- Clear action enumeration: what can an agent do, and how does it request to do it?
- Must support identification of which agent is acting and on whose behalf.

### 4.2 Consumption Metering

Track and bill for all agent activity:

- Every agent action is logged, attributed, and counted.
- Without this, consumption-based pricing is impossible — you cannot bill for what you cannot measure.
- Must handle high-volume, high-frequency agent actions (agents act faster and more often than humans).

### 4.3 Access Controls — Technically Enforced

Access controls must be technically enforced, not just contractually defined:

- If a third-party agent should only access certain data or actions based on the customer's tier, this must be enforced at the API level.
- Contractual restrictions alone are insufficient — agents will attempt actions programmatically, and the platform must gate them in code.
- Agents inherit customer permissions; enforcement must be per-request, not per-session.

### 4.4 Identity and Attribution

Identify which agent is acting and on whose behalf:

- Distinguish between Salesforce's Agentforce, a customer-built agent, a third-party tool, and a human user.
- Attribution enables billing (which agent consumed which credits), auditing (who did what), and governance (did the agent act within permitted boundaries).

---

## 5. Structural Advantages for Incumbents

Established B2B software vendors have structural advantages in the agentic layer that general-purpose AI providers (OpenAI, Anthropic) cannot easily replicate:

### 5.1 Regulatory Accountability
- Incumbents in regulated industries (finance, healthcare, compliance) are already accountable to regulators.
- Customers in these industries need a party who is accountable — general-purpose AI providers often disclaim liability.
- This makes the incumbent's own agent more trustworthy than a general-purpose alternative.

### 5.2 Proprietary Domain Logic
- Years of building domain-specific workflows, business rules, and edge-case handling.
- General-purpose agents lack this domain depth; they can call APIs but don't inherently understand the business logic.
- An incumbent's agent, built on proprietary domain logic, is more reliable for complex, industry-specific tasks.

### 5.3 Deep Customer Data
- Incumbents hold rich historical customer data within their platforms.
- This data provides context that makes agent actions more accurate and relevant.
- General-purpose agents connecting via API start with less context.

### 5.4 Trusted Relationships
- Customers in complex, regulated industries turn to vendors they already trust.
- The agentic layer is an extension of an existing relationship, not a new procurement decision.
- This trust advantage compounds: customers prefer the incumbent's agent because the incumbent is already integrated into their risk and compliance frameworks.

### Strategic Implication

These advantages mean that for complex, regulated B2B use cases, the incumbent's own agent is likely to be more reliable and more trusted than a general-purpose AI agent. Incumbents should lean into this — build their own agents, price them on outcomes, and use their structural advantages to win the agentic layer in their domain.

However, third-party agents will still connect (customers want choice, and general-purpose agents are improving rapidly). The "indifferent to interface" billing model ensures incumbents monetize regardless of which agent wins the workflow.

---

## 6. Summary: The Commercial Model

| Scenario | Pricing Model | Rationale |
|---|---|---|
| You build the agent (launch) | Fixed uplift + generous allowances | Focus on adoption; metering immature |
| You build the agent (growth) | Credits, task-weighted | Bridge to outcome pricing; value-correlated |
| You build the agent (mature) | Outcome-based (per result) | Full value alignment; revenue scales with outcomes |
| Third-party agent accesses platform | Tier-gating + consumption-based | You don't deliver the outcome; monetize the access and usage |
| Any agent action (Salesforce/ServiceNow pattern) | Unified consumption unit (Flex Credits) | "Indifferent to interface" — bill the action, not the source |

---

## 7. Key Takeaways

1. **Two monetization questions arrive together**: own-agent pricing and third-party-access pricing. Address both now.
2. **Outcome-based pricing is the goal for your own agents** — but phase it in (uplift → credits → outcomes).
3. **Third-party access can't be priced on outcomes** — use tier-gating + consumption metering instead.
4. **The "indifferent to interface" model is emerging as the standard**: bill per agent action regardless of source (Salesforce and ServiceNow both converged here independently).
5. **Infrastructure is a prerequisite**: agent connection APIs, consumption metering, technically enforced access controls, and identity attribution. Without these, consumption-based pricing is impossible.
6. **Incumbents have structural advantages**: regulatory accountability, domain logic, customer data, and trust. These make their own agents more reliable for complex/regulated use cases.
7. **Speed matters**: workflows settle around the first agent that arrives. Habits stick. Third-party access is already happening. Being slow risks ceding the agentic layer to competitors or general-purpose AI providers.