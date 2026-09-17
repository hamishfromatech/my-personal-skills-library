---
name: give-away-keep-matrix-oss-ai
description: Applies the Give-Away/Keep Matrix and 5-Layer Monetization Stack for designing open-source AI product business models, distinguishing what you open for use vs. what you open for modification, and mapping the full adoption-to-monetization funnel. Use when designing or auditing the business model of an open-source AI product, deciding what to give away vs. charge for, or planning the monetization stack layers. NOT for purely proprietary products, closed-weight models, or projects without a monetization strategy.
---

# Give-Away/Keep Matrix for Open-Source AI

## Core Concept

The Give-Away/Keep Matrix (Malpani, 2026) is a 2×2 strategic framework for open-source AI business model design. The axes are deliberately not "open vs. closed" — they are **what you open for USE** (run/inference/hosted) and **what you open for MODIFY** (source code/weights). Each combination is a different business.

## The Matrix

| | **Free to MODIFY (source/weights)** | **Closed to MODIFY** |
|---|---|---|
| **Free to USE (run/inference)** | **Q2: True Open Source AI** — Weights on HuggingFace, self-host or buy managed. Revenue: hosted inference, fine-tuning. Examples: Mistral 7B/Mixtral, DeepSeek R1, Qwen 3.5, Gemma 4 | **Q1: Free Service** — Free to use, closed source. Free tier, hosted API. Pay for scale, premium models. Examples: ChatGPT free, Claude.ai free, DeepSeek web/app. Drives top-of-funnel. NOT OSS. |
| **Paid to USE** | **Q4: Open Core** — Source open, hosting/premium paid. Free self-host core. Pay for cloud, SSO, audit, SLA. Examples: HuggingFace, LangChain, LlamaIndex, Ollama Cloud, Databricks. $100B+ market cap proven. | **Q3: Proprietary SaaS** — Paid to use, closed source. No free tier. Pure enterprise sales. Examples: OpenAI o1/GPT-4 API, Anthropic Claude API. Highest margin, hardest CAC. |

## The 5-Layer Monetization Stack

Each layer feeds the layer above. Skip a layer and the stack collapses.

### Layer 1: Adoption Engine
- **What**: Free, permissively licensed thing (weights, model, library)
- **Metric**: Downloads, stars (not MRR)
- **Cost center, not profit center**: You spend money to make this great, get zero direct revenue
- **Example**: DeepSeek R1 — 5M downloads in 2 months = 5M qualified leads at zero marginal cost
- **Target**: If weights have 50 downloads, you don't have a business yet

### Layer 2: Self-Host Loss Leader
- **What**: Docs, quick-start guides, tutorials, HuggingFace Spaces, integrations
- **Metric**: Activation rate (of downloaders, what % ran in production within 7 days)
- **Good number**: 8-12%
- **Zero revenue, all activation**: Turns curious into committed

### Layer 3: Managed Cloud
- **What**: Hosted API, inference endpoints, cloud tier
- **Metric**: Conversion rate from self-hosters to paid (1-4% year one, 5-8% mature)
- **First dollar arrives here**: The free user who OOM'd on their laptop comes back and pays
- **Example**: Mistral's $400M ARR is mostly Layer 3

### Layer 4: Enterprise Skin
- **What**: SSO, RBAC, audit logs, VPC deployment, SLA, SOC 2, dedicated CSM
- **Metric**: Net dollar retention (130%+ means engine working)
- **Margin compounds here**: Customers pay $300K/yr not for models (free) but for compliance, support, accountability
- **Example**: HuggingFace Enterprise Hub majority of revenue

### Layer 5: Network & Data Moat
- **What**: Marketplace, data flywheel, brand, community
- **Highest margin, longest to build**: Cannot be cloned with $10M seed funding
- **Example**: HuggingFace's 1.5M models, 500K orgs, leaderboards — the Hub IS the moat

## Five Proven Revenue Models

Most successful OSS AI companies stack 2+ of these:

1. **Hosted inference**: Open weights, charge for running (Mistral: $0.40/$2 per M tokens)
2. **Managed cloud (open core)**: Free core, paid cloud (HuggingFace: Inference Endpoints $0.06/hr+)
3. **Enterprise skin**: Open engine, charge for chrome (SSO, audit, VPC, SLA)
4. **Custom training & consulting**: Open model, charge to make it good for one customer ($5K-$250K)
5. **Tools and pickaxes**: Don't sell model, sell things people need to build with it (LangChain, LlamaIndex, Pinecone)

## Case Studies

### Mistral: $16M → $400M ARR in 13 Months
- Apache 2.0 weights (Layer 1 working at full power)
- La Plateforme hosted API launched alongside weights (Layer 3)
- Le Chat Pro/Team/Enterprise (Layers 3-4)
- Acquired Koyeb (Feb 2026) to own GPU layer = own the margin

### HuggingFace: Enterprise Pickaxes
- Doesn't train frontier models; sells infrastructure everyone uses
- $70M ARR (2023) → 2,000+ paying enterprises (2025)
- Majority revenue at Layer 4 (Enterprise Hub), not Layer 3

### DeepSeek: 545% Margin Paradox
- Open weights + MoE architecture (37B active of 671B) = 10× cost reduction
- $470M net profit, 28-32% net margin (end 2025)
- Web/app free; API with nighttime discounts; enterprise custom
- Open weights lowered CAC and forced engineering efficiency

## The License Trap

Every restrictive license change (Redis, Elastic, HashiCorp) followed the same pattern:
1. Cloud provider starts offering managed version of your OSS
2. You change license to block them
3. Cloud provider forks last open version under Apache 2.0
4. Linux Foundation picks up fork
5. Enterprises migrate to fork
6. You lose the developer brand you spent a decade building
7. You eventually relicense back to something more permissive

**Cleaner play**: Keep Apache 2.0 on the model. Win on experience, not artifact. Compete with AWS on ergonomics, not license restrictions.

## The Three-Question Honesty Test

1. Can a customer self-host the production version end-to-end without paying you? If no → not open source, you have an open source teaser.
2. Could AWS/GCP/Azure spin up a competing managed service tomorrow from your code? If no → not open source, you have a source-available business.
3. If you raised prices 5× tomorrow, would the OSS community fork you within a month? If no → lock-in is real, open source is decorative.

## A-Tech Applications

- **A-Coder**: Apply Give-Away/Keep Matrix — open A-Coder core (Apache 2.0), charge for managed cloud + enterprise skin + custom training
- **Be Practical**: OSS AI business model curriculum (5-Layer Stack, Give-Away/Keep Matrix, license trap avoidance)
- **Builder's Club**: Community playbook for open-source AI monetization (stack selection, license choice, conversion optimization)

## Cross-References

- `open-core-ai-feature-metering` — The metering layer within the 5-Layer Stack
- `tanso-ai-margin-ledger-metering` — Infrastructure for Layer 3-4 margin tracking
- `open-source-ai-monetization-mastery-2026` — Strategic stack that this matrix operationalizes
- `agent-native-advertising-economics` — Layer 3 revenue model for AI agents
- `revenue-sharing-as-infrastructure-model` — Alternative Layer 3 model with zero entry barrier
- `free-lunch-dilemma-open-source-ai-monetization` — The free-lunch problem this matrix solves

## A-Tech Alignment

| Value | Alignment |
|---|---|
| Open-source AI | Framework explicitly designed for OSS AI; Apache 2.0 default; license trap avoidance |
| Data privacy | Open weights enable on-device/self-hosted = data sovereignty; enterprise VPC deployment |
| Financial freedom | Open weights lower CAC; managed cloud captures value; enterprise skin creates margin |
| Practical implementation | "What to do Monday morning" section; 3 decision points; stack selection guide |