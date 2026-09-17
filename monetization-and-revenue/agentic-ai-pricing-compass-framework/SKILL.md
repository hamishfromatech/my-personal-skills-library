---
name: agentic-ai-pricing-compass-framework
description: Systematic framework for selecting agentic AI pricing models using the COMPASS matrix and Impossible Triangle. Use when pricing AI agents, determining meter selection for autonomous AI systems, or designing hybrid pricing architectures. NOT for traditional SaaS or non-agentic AI products.
---

# Agentic AI Pricing COMPASS Framework

## Overview
The COMPASS Framework maps agentic AI pricing decisions across two axes — Scope of Agent's Work (Task/Process/Goal) and Level of Attribution (Diffuse/Medium/Direct) — to select the optimal pricing model from four canonical options. Combined with Tien Tzuo's Impossible Triangle (Cost-to-Serve / Customer Adoption / Value Delivered), it provides a systematic approach to designing pricing that survives the transition from per-seat to usage/outcome-based monetization as agents become autonomous.

## When to Use
- Pricing AI agents or autonomous AI systems
- Selecting the right meter (per-agent, per-activity, per-output, per-outcome)
- Designing hybrid pricing architectures for agentic products
- Transitioning from seat-based to usage/outcome-based pricing
- NOT for traditional SaaS without autonomous AI
- NOT for copilot/assistive AI (use effort/output models instead)

## Core Process / Workflow

### 1. Map the Agent on COMPASS Axes

**Scope of Agent's Work**:
- **Task**: One discrete action (answer question, classify record, summarize)
- **Process**: Multi-step workflow (triage ticket, qualify lead, route approval)
- **Goal**: Owns an outcome (resolve support issue, close deal, complete financial close)

**Level of Attribution**:
- **Diffuse**: Contributes to outcome influenced by many factors
- **Medium**: Measurably moves outcome but doesn't fully own it
- **Direct**: Owns the outcome, unambiguously measurable

### 2. Apply the COMPASS Matrix

| | Diffuse | Medium | Direct |
|---|---------|--------|--------|
| **Task** | Per Agent / Per Activity | Per Activity / Per Output | Per Output / Per Outcome |
| **Process** | Per Agent / Per Activity | Hybrid | Per Outcome / Hybrid |
| **Goal** | Per Agent / Per Activity | Hybrid | Per Outcome |

### 3. Stress Test Against the Impossible Triangle
Name explicitly which two corners you optimize and which you trade:
- **Cost-to-Serve**: Inference cost (tokens × context × tool calls)
- **Customer Adoption**: Ease of starting/continuing (free tiers, predictable bills)
- **Value Delivered**: Price tracks business value (per resolution, per deal)

### 4. Select from Four Canonical Models
1. **Per Agent**: Fixed per AI license — bounded scope only
2. **Per Activity**: Per call/query/workflow — aligns with cost, hides token economics
3. **Per Output**: Per artifact — when output is the unit of value
4. **Per Outcome**: Per business result — transfers cost variance to seller

### 5. Design Hybrid Architecture (Most Practical)
- **Base subscription + activity overage**: Floor revenue + margin protection
- **Prepaid credits + drawdown**: Upfront capital + usage flexibility
- **Outcome-based with cost cap**: Value alignment + seller downside protection

### 6. Pilot Before Scaling
- Run one product line, one customer cohort, two billing cycles
- Measure margin-per-activity and customer renewal signal
- Pricing must evolve: augmentation → replacement, fuzzy value → hard ROI

## References
- See [references/zuora-2026-compass-framework.md](references/zuora-2026-compass-framework.md) for full framework details.