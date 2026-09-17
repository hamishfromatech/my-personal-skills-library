# The Agentic Economy on Base: Evidence Base

## Source

- **Base (Coinbase L2),** "The Agentic Economy Is Here" (blog.base.org, May 29, 2026)
- Base is an Ethereum L2 blockchain built by Coinbase

## The Three Phases of the Agent Economy

### Phase 1: Social Actions (Early 2025)
- First onchain wave was primarily social: posting, replying, creating media, trading attention, building communities, launching projects, testing financial rails
- Between October 2024 and February 2025: ~16K agents launched on Base through Virtuals (AI agent platform for spinning up agents with their own tokens)
- Source: Coinbase

### Phase 2: Paying Customers (Mid 2025 – Early 2026)
- Models improved at using tools, planning, maintaining context
- Agent financial infrastructure improved in parallel:
  - Wallets got easier to embed
  - Stablecoin adoption surged, transaction fees dropped to sub-cent (enabling microtransactions)
  - x402 launched (May 2025): enabling payments to become part of a normal internet request
  - Builders shipped agent-accessible services worth paying for

- **On Base, last 30 days (as of May 29, 2026):**
  - 3.1M x402 transactions
  - $1.2M in value transferred
  - Number of sellers grew 23%
  - Number of buyers grew 37%

### Phase 3: Earning Agents (2026 – Emerging)
- A wallet lets an agent receive funds as easily as it spends them
- An earning agent can: sell research, run a paid service, accept payments, hire other agents, pay operating costs
- Some agents interact with market and lending protocols through defined permissions
- Agents become the specialization that other agents pay for

**Early earning-agent case studies:**
- **Felix:** agent running its own businesses; reported $261,395+ in revenue from agent-run products
- **Kelly Claude:** agent running its own businesses; product revenue across paid app-building service, books, and app sales
- **Factory Floor:** tracks agents with live products, in-review apps, and revenue sources across Stripe, Gumroad, and App Store

## The Services Agents Pay For (Detailed)

### Intelligence (Inference and Model Access)

| Service | Description | x402 Integration |
|---------|-------------|-------------------|
| Venice | Lets wallets authenticate and pay for inference on Base across chat, image, audio, video, embeddings, other routes | Wallet authentication + USDC on Base |
| BlockRun (Base Batches) | Routes agents across 50+ AI models | Pay-per-call USDC settlement on Base via x402 |
| Dolphin AI | Model development and distributed inference | Emerging |
| Bankr's x402 Cloud | Helps builders turn endpoints into paid services | USDC settlement on Base; payments sent to builder's wallet |

### Execution (Browser and Compute)

| Service | Description |
|---------|-------------|
| Browserbase | Agents pay with USDC on Base for cloud browser sessions; sessions give agents a browser to navigate websites, gather context, run workflows, return results |

### Research (Data and Knowledge)

| Service | Description |
|---------|-------------|
| Exa | Web search and content data |
| Wolfram Alpha | Computation |
| agentic.market | Bundled talent market workflow: combines job search, Exa neural search, Parallel search, content extraction, and stock quote data to study hiring trends, salary ranges, hot skills, and company context for a few cents per run |

### Travel (Real-World Data)

| Service | Description |
|---------|-------------|
| Tripadvisor | Reviews, photos, and localized search for agents helping travelers |
| FlightAware | Real-time flight information |
| Amadeus | Booking for hotels and flights |

### Agent Enablement (Developer Platforms)

| Platform | x402 Support |
|----------|-------------|
| Cloudflare | x402 Foundation member; infrastructure support |
| Amazon Bedrock AgentCore Payments | Brings x402 and Coinbase wallet infrastructure into enterprise agent workflows; micropayment flows for web content, APIs, MCP servers, and other agents |

## Base's Accessibility Work for Agents

Base identifies what the agent economy needs (low friction):
- Low-cost transactions ✅ (sub-cent stablecoin fees)
- Wallets for sending and receiving funds ⬆ (improving)
- Stablecoins for quick settlement ✅ (USDC on Base)
- Services with machine-readable prices ✅ (x402)
- x402-enabled APIs, spend limits, receipts, permissions, audit trails, identity, reputation ⚠ (emerging/needs standardization)
- Protocols agents interact with directly ✅ (x402)

Base's contributions:
- Released Base MCP (lets agents engage with the Base ecosystem: trade, swap, lend onchain)
- Working with providers to embed wallets natively in agent infrastructure
- Working with ecosystem teams to make apps, APIs, and protocols easier for agents to find, evaluate, pay for, and use

## Key Quotes

From the Base blog:
- "Agents are no longer simply using the internet, they are becoming paying customers."
- "If agents can spend to complete work, they can also earn by selling work. The same wallets and payment rails that make agents internet customers can make them internet businesses."
- "Base is giving builders the sandbox to make this happen fast."

## Infrastructure Context

- x402: open payment protocol enabling payments as part of normal internet requests (launched May 2025)
- x402 Foundation founding members: Visa, Google, AWS, Stripe, Coinbase
- USDC on Base: stablecoin with sub-cent transaction fees enabling microtransactions
- Base MCP: lets agents engage with the Base ecosystem programmatically
- Virtuals: AI agent platform that lets anyone spin up an agent and its own token (~16K agents launched Oct 2024 – Feb 2025)

## Relationship to Existing A-Tech Skills

| Existing Skill | Relationship |
|----------------|-------------|
| agent-to-agent-economy-operating-guide | The operating guide covers 12 live platforms where agents earn; this skill explains the earning evolution and the spending-to-earning transition |
| agent-pay-card-network-integration | Covers card-network tokenization (Mastercard Agent Pay, Visa); this skill covers crypto-native x402 settlement |
| agentic-payments-protocol-ap2 | Covers AP2 specification; this skill uses x402 as the earning-agent payment rail |
| agent-marketplace-builder-economy | Covers marketplace platform economics; this skill covers what agents DO as businesses on those marketplaces |
| untrainable-corner-pricing-moat | The untrainable corner is what earning agents sell (expensively-verifiable work commands pricing power) |
| ai-agent-finfops-cost-optimization | Earning agents must manage operating costs to be profitable; this skill is the revenue side |
| mcp-dual-identity-problem | The agent-as-product-persona; earning agents expose services via MCP |
| agent-protocol-stack-2026 | The six-protocol stack; earning agents operate across the full stack |

## Methodology Notes

- Base is a participant in the agent economy (not a neutral observer) — the blog is a platform ecosystem update, not independent research
- Revenue figures for Felix ($261,395+) are self-reported by the agent/operator, not independently audited
- Transaction and value figures (3.1M, $1.2M) are on-chain data from Base/x402 (verifiable)
- The "earning agent" phase is described as emerging — most agent activity is still spending, not earning
- The 23%/37% seller/buyer growth is month-over-month (last 30 days), not annualized
- The service stack is specific to the Base ecosystem; other chains and platforms may have different service ecosystems

---

*Evidence base compiled from Base blog "The Agentic Economy Is Here" (May 29, 2026) and cross-referenced with existing A-Tech agent economy skills.*