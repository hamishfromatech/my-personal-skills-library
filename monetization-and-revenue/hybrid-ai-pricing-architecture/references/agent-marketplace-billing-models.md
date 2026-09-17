# Agent Marketplace Billing Models

## Sources
- Medium / Adnan Masood — "Inside the Fractured World of AI Agent Marketplaces" (2026)
- Gary Weiss / MCP-Server Medium — "The Rise of MCP: Protocol Adoption in 2026 and Emerging Monetization Models" (Feb 2026)
- Shopify Engineering — "Building the Universal Commerce Protocol (2026)"

## Marketplace Types
1. **MCP Registries** (community servers, low monetization)
2. **Consumer Stores** (end-user agent apps, direct purchase)
3. **SaaS Catalogs** (enterprise tool integrations, subscription)
4. **Hyperscaler Marketplaces** (AWS/Google/Azure, revenue share)

## Billing Models

### 1. Per Method Call
- Micro-transaction: $0.001–$0.01 per JSON-RPC call
- Best for: High-volume, low-complexity operations
- Risk: Death by a thousand cuts; customers fear runaway billing
- Mitigation: Monthly call caps + bulk pricing tiers

### 2. Per Data Volume
- $0.01–$0.10 per KB/MB processed
- Best for: Data transformation, file processing, analytics
- Risk: Large files create unexpected bills
- Mitigation: File size warnings + tiered volume discounts

### 3. Per Outcome
- $1–$100 per successful agent delivery
- Best for: Business-result workflows (leads, resolutions, deployments)
- Risk: Disputes over "success" definition
- Mitigation: Clear SLA + third-party verification or mutual acceptance

### 4. Per Session
- $0.50–$5.00 per agent engagement session
- Best for: Conversational agents, support bots
- Risk: Session boundaries are arbitrary
- Mitigation: Time-based definition (5-minute timeout) + idle detection

### 5. Tool-Specific Meters
- Specialized pricing per capability (e.g., $0.05 per code review, $0.20 per security scan)
- Best for: Differentiated capabilities with varying costs
- Risk: Complexity in pricing page
- Mitigation: Bundle into credit packs with transparent conversion

### 6. Hybrid
- Base gateway fee + any combination of the above
- Best for: Marketplaces with diverse sellers
- Risk: Overcomplicated billing
- Mitigation: Unified dashboard showing all components in one view

## Protocol Integration
- A2A (Google): Agent-to-agent authentication
- AP2 (Google): Agentic payment protocol
- UCP (Shopify/Google): Universal Commerce Protocol for checkout
- MCP (Anthropic): Model Context Protocol for tool access
- x402: Cross-chain payment settlement

## A-Tech Opportunity
A-Tech should position as a curated, ethical gateway:
- Verified server directory (trust moat)
- Transparent billing (no hidden fees)
- Outcome-first pricing (alignment with builder success)
- Open-source audit of all billing calculations (transparency moat)

## Date of Extraction
2026-05-31
