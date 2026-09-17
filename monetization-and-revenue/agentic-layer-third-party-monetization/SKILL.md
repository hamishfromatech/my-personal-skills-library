---
name: agentic-layer-third-party-monetization
description: Framework for monetizing the agentic layer in B2B software, covering both own agents and third-party agent access. Use when designing agent monetization strategy, pricing third-party API/agent access, or building agent-compatible billing infrastructure. NOT for consumer AI products or non-platform software.
---

# Agentic Layer Third-Party Monetization

## Overview
As an agentic layer forms across B2B software, companies must monetize both agents they build (outcome-aligned pricing) and third-party agents accessing their platform (tier-gating + consumption metering). The commercial model should be "indifferent to interface."

## When to Use
- Designing agent monetization strategy for B2B software
- Pricing third-party AI agent access to your platform
- Building agent-compatible billing and metering infrastructure
- Transitioning from license-based to agent-action-based revenue
- NOT for consumer AI products
- NOT for non-platform software without API/agent access

## Core Process / Workflow

### 1. Determine Agent Ownership

**You build and own the agent**:
- Aim for outcome-aligned pricing (per certification, per resolution, per processed invoice)
- Highest value capture: revenue scales with value customer receives
- Start with fixed uplift + generous allowances → move to outcome-based as metering matures
- Consider credits as unifying metric (higher-value tasks = more credits)

**Third-party agent accesses your platform**:
- Outcome-based NOT available (you're not delivering the outcome)
- Use tier-gating: agents inherit customer's plan permissions
- Layer consumption-based pricing: revenue scales with agent usage
- Make commercial model "indifferent to interface" (same unit regardless of trigger source)

### 2. Design Pricing Progression
1. **Launch phase**: Fixed uplift on existing license with generous usage allowances
2. **Growth phase**: Credits with task-weighted consumption (higher-value = more credits)
3. **Mature phase**: Outcome-based for own agents + consumption-based for third-party

### 3. Implement Agent-Action Billing (Salesforce/ServiceNow Pattern)
- Charge per agent action through unified consumption unit (Flex Credits)
- Same unit regardless of: own agent, customer-built agent, or third-party tool
- Example: Salesforce charges per agent action whether from Agentforce, customer-built, or third-party

### 4. Build Required Infrastructure
1. **Agent connection layer**: Clean, consistent API for agents to connect and act
2. **Identity tracking**: Identify which agent is acting and on whose behalf
3. **Consumption metering**: Track and bill for all agent activity (without this, consumption pricing impossible)
4. **Access controls**: Technically enforced, not just contractual

### 5. Leverage Structural Advantages (for incumbents)
- Regulatory accountability → trusted in regulated industries
- Proprietary domain logic → more reliable than general-purpose agents
- Deep customer data → better context for agent actions
- Existing trusted relationships → customers turn to vendors they know

### 6. Act Quickly
- Customer workflows settle around whichever agent arrives first
- Once formed, habits stick
- Third-party access is already happening — blocking is rarely viable
- Being slow risks ceding the agentic layer to others

## References
- See [references/hg-2026-agentic-layer-monetization.md](references/hg-2026-agentic-layer-monetization.md) for full analysis.