# Dual-Identity Design Framework

## The Design Questionnaire

Before launching an MCP server for a SaaS product, answer these questions:

### 1. Identity Model

| Question | Option A (Merged) | Option B (Separate) |
|---|---|---|
| How are agent actions attributed? | All roll up to human user | Agent is first-class NHI |
| Can you distinguish human vs agent in audit log? | No | Yes |
| Can you bill per-agent? | No | Yes |
| Can you rate-limit per-agent? | No (only per-account) | Yes |
| Complexity | Low | Higher but necessary at scale |

**Recommendation:** Start with merged identity for MVP, but architect for separate NHI from day one. The migration cost is low early and prohibitive later.

### 2. Traffic Asymmetry Quantification

Measure or estimate the agent-to-human load ratio for your product:

- **Human actions per day:** 50-200 (session-paced, with breaks)
- **Agent tool calls per task:** 10-500+ (bursty, task-complexity-driven)
- **Peak multiplier:** A single agent session can generate 10x-100x the load of a human session

**Example (Salesforce via MCP):** A human makes 50-200 actions/day. An agent researching a lead might make 500 tool calls in 10 seconds before doing anything visible. Same infrastructure, same database queries, 10x-100x the load.

### 3. Rate-Limit Architecture

| Layer | Human Default | Agent Default | Escalation |
|---|---|---|---|
| Per-user | 200 actions/day | 10,000 calls/day | Configurable tier |
| Per-team | Aggregate of users | Aggregate of agents | Per-agent sub-limits |
| Per-tool | N/A | Per-tool call caps | Tool-specific thresholds |
| Burst | Short sessions | 500 calls/10s allowed | Throttle, don't block |

**The 3am scenario to prevent:** One agent pulls 90 days of messages across all channels. It blows through the team API rate limit. At 9am, four other employees can't use the product because their team is throttled. Design rate limits that are per-agent, not just per-team.

### 4. Cost Attribution Model

```
Cost Attribution Hierarchy:
├── Human User
│   ├── Session cost (subscription)
│   └── Direct API calls (usage)
├── Agent (NHI)
│   ├── Agent identity (who deployed it)
│   ├── Tool calls (per-tool metering)
│   ├── Workflow sessions (per-workflow cost)
│   └── Outcome events (per-result billing)
└── Team/Account
    ├── Aggregate human cost
    ├── Aggregate agent cost
    └── Blended unit economics
```

### 5. Audit Trail Requirements

Every API call must be tagged with:
- **Origin:** human or agent
- **Agent identity:** which agent (if applicable)
- **Acting on behalf of:** which human/team
- **Tool called:** which MCP tool
- **Cost incurred:** compute, egress, upstream API
- **Outcome:** success/failure (for outcome-based billing)

## The Three Pricing Models in Detail

### Model 1: Per-Call (Tool Invocations)
- **Examples:** Apify, xpay
- **xpay rates:** search $0.01, analyze $0.05, generate $0.10 per call
- **Nevermined:** sub-cent micropayments starting at $0.001/transaction
- **Trap:** Agents are inefficient. They retry, re-query, loop. A naive per-call model rewards the vendor for the agent's inefficiency. Uncomfortable fast.

### Model 2: Re-Wrapped Seats
- **Examples:** Microsoft Copilot ($30/user/mo), Salesforce Agentforce ($125-$150/user/mo)
- **What it is:** The seat is still anchored to a person. The agent is a privilege the person has.
- **Why it doesn't work:** If one human's agent does the work of three humans, the customer still has one seat. The vendor still gets one fee. Re-wrapping the seat doesn't fix the math; it postpones the conversation.
- **True per-agent pricing (agent as SKU):** Intercom Fin says "no seat charges for the AI agent itself." The fact they call out the alternative tells you it exists. Not mainstream yet.

### Model 3: Outcome-Based
- **Examples:** Intercom Fin ($0.99/resolution), Zendesk ($1.50/resolution committed, $2.00 pay-as-you-go)
- **The gap:** Defining the outcome is harder than charging for it. When Intercom says "we resolved the ticket" and the customer says "you didn't," who decides? No neutral arbiter exists yet. This gap will be filled by an entire infrastructure category that doesn't exist yet.

## The MCP Gateway Layer

### Platforms Building This
- **Kong:** Extended API Gateway to expose MCP servers as managed endpoints
- **Tyk:** API gateway with MCP support
- **MintMCP:** Dedicated MCP gateway with discovery + billing
- **Arcade:** Agent authentication and identity
- **TrueFoundry:** Agent infrastructure platform
- **Microsoft:** AI Gateway inside Foundry
- **Palo Alto Prisma AIRS:** Frames agents as "non-human identities" (NHIs)

### Gateway Functions
1. **Authentication:** Agent identity tokens vs human credentials
2. **Rate limiting:** Per-agent, per-tool, per-workflow (not just per-account)
3. **Cost attribution:** Per-call, per-outcome, per-agent session
4. **Audit logging:** Human vs agent action separation
5. **Agent discovery:** Dynamic onboarding, single-use agents

### The PM Measurement Opportunity
The gateway is where the data lives:
- Per-agent, per-tool, per-workflow cost
- Per-call latency
- Per-outcome success rates

The same data that lets a CISO sleep at night is the data that lets a PM answer: "What did our agents cost this month, by workflow, by outcome?" — the question their CFO has been asking for two years.

**The gap:** CISO tools exist. CFO tools don't. This is where the next big category for AI PMs lives.

## The Mobile-First Transition Analogy

| Phase | Mobile (2008-2015) | Agent Identity (2026-2028) |
|---|---|---|
| **Initial response** | Thin wrapper on desktop site | Per-seat pricing with agent add-on |
| **Early adopters** | Mobile-first redesign | Agent-first identity redesign |
| **Winners** | Those who redesigned around mobile | Those who redesign around agent identity |
| **Losers** | Those who treated mobile as a wrapper | Those who treat agents as a human seat privilege |
| **Timeline** | ~5 years to mainstream | ~18 months to table-stakes |

## The 18-Month Prediction

These are not edge-case questions. They will be table-stakes product requirements in the next 18 months for any SaaS product that opens an MCP server:

1. Does the product treat agent actions as user actions or as separate?
2. How is cost and usage attributed between human and agent?
3. Can the system tell, from a single API call, whether a human or agent triggered it?
4. What happens when one agent's actions affect other users' rate limits?

The PMs who notice this in 2026 get to shape what their products look like in 2027. The ones who don't will inherit the systems built by people who did.

## Source

- Shaili Guru (AI Product Management Guru), "The MCP Economy Is Already Here. Have you noticed it yet?" Substack, June 5, 2026. Covers the dual-identity problem, three pricing models, traffic asymmetry (10-100x), MCP gateway layer (Kong, Tyk, MintMCP, Arcade, TrueFoundry, Microsoft), and the 18-month PM roadmap. References Apify, Nevermined, x402, xpay, Intercom Fin, Zendesk, Deloitte TMT Predictions 2026, Palo Alto Prisma AIRS.