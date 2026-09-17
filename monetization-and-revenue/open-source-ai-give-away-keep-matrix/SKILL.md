---
name: open-source-ai-give-away-keep-matrix
description: Strategic framework for deciding which layers of an AI product to open-source (give away) vs. keep proprietary, using the Give-Away/Keep Matrix and five-layer monetization stack. Use when designing an open-source AI business model, when deciding what to open-source vs. keep closed, when building a hosted-inference or open-core AI company, or when planning solo-founder open-source monetization. NOT for purely proprietary AI products with no open-source component, for traditional SaaS without AI, or for companies that have already decided to keep everything closed.
---

# Open-Source AI Give-Away/Keep Matrix

## Overview

The question for AI founders in 2026 is no longer "should I open source my code" — that war is over. Open source is now the default. The question is sharper: **What specific thing am I giving away to lower my customer acquisition cost, and what specific thing am I keeping to extract margin?**

The Give-Away/Keep Matrix provides the strategic framework for this decision. It maps what you open up to others to USE against what you open up to others to MODIFY. Each combination is a different business.

## The Give-Away/Keep Matrix

| | **Closed to MODIFY** | **Open to MODIFY** |
|---|---|---|
| **Free to USE** | **Q1: Free Service** — Free to use, closed source. Free tier / hosted API. Pay for scale, premium models. Examples: ChatGPT free tier, Claude.ai free, DeepSeek web/app. Drives top-of-funnel. Not true OSS. | **Q2: True Open Source AI** — Free to use AND modify. Weights on HuggingFace. Self-host or buy managed. Examples: Mistral 7B/Mixtral, DeepSeek R1/V3 weights, Qwen 3.5, Gemma 4. Revenue: hosted inference, fine-tuning. |
| **Paid to USE** | **Q3: Proprietary SaaS** — Paid to use, closed source. No free tier. No source access. Pure enterprise sales. Examples: OpenAI o1/GPT-4 API, Anthropic Claude API. Highest margin. Hardest CAC. | **Q4: Open Core** — Source open, hosting/premium paid. Free to self-host core. Pay for cloud, SSO, audit, SLA. Examples: HuggingFace, LangChain, LlamaIndex, Ollama Cloud, Databricks. $100B+ market cap proven model. |

The matrix is not a beauty contest. It is a strategic choice. Most AI companies are confused about which quadrant they live in. They tell themselves they are in Q2 (true open source) because they posted weights to Hugging Face, but the only thing open sourced is a 7B model nobody runs in production while the 70B model customers actually want is behind an API. That is Q1 with a marketing campaign attached, not Q2.

## The Five Proven Open-Source AI Revenue Models

### Model 1: Hosted Inference
You open the model weights. You charge for running it.
- **Mistral example**: Apache 2.0 model free to download. API costs $0.40/M input tokens, $2/M output on Mistral Medium 3.1. Vast majority of revenue is paid API, not self-hosted.
- **Why pay if free?**: GPUs cost money, latency matters, scale is brutal, 99.9% uptime is a full-time job. The model is free. The convenience is not.

### Model 2: Managed Cloud (Open Core)
The classic OSS playbook moved into AI.
- **HuggingFace example**: Free Transformers library, Hub, model viewer, leaderboards. Pay for Inference Endpoints ($0.06/hour+), Pro account ($9/user/month), Enterprise Hub, consulting. $70M ARR by 2023, 367% YoY growth, 2,000+ paying enterprises by 2025.

### Model 3: Enterprise Skin
You open the engine. You charge for the chrome: SSO, RBAC, audit logs, compliance certifications, dedicated support, VPC deployment.
- **What you're selling**: "I will not get fired by my procurement team for using this."
- **Ollama example**: Local CLI free forever. Pro ($20/mo) and Max ($100/mo) tiers for enterprise features.

### Model 4: Custom Training & Consulting
You give away the model. You charge to make the model good for one specific customer.
- **Pricing**: $5K to $250K per engagement depending on scale, plus ongoing hosting.
- **Margin**: In the engineering hours, not the software.

### Model 5: Tools & Pickaxes
You do not sell the model. You sell the things people need to build with the model.
- **Examples**: LangChain, LlamaIndex, Pinecone, Weaviate, Chroma, vLLM
- **Pattern**: Open source library is free. Cloud version is $X.
- **Lesson**: When you are in a gold rush, sell the pickaxes.

## The Five-Layer Monetization Stack

Each layer feeds the layer above. Skip a layer and the layer above stalls.

### Layer 1: Adoption Engine
The free, permissively licensed thing. This is the cost center, not the profit center. You spend money to make this great and get zero direct revenue. What you get is **reach**.
- **Metric**: Downloads and stars, not MRR
- **Mistral example**: R1 had 5 million downloads in 2 months — 5 million qualified leads at zero marginal cost

### Layer 2: Self-Host Loss Leader
Docs. Quick-start guides. Tutorials. Hugging Face Spaces. LangChain integrations. The Ollama desktop installer. None of these makes money. All turn Layer 1 people from curious into committed.
- **Metric**: Activation rate — of people who downloaded, what % ran it in production within 7 days. Good: 8-12%

### Layer 3: Managed Cloud
Where the first dollar arrives. The free user who tried to self-host, watched it OOM, comes back and pays for the same thing run on your GPUs.
- **Mistral**: $400M ARR is mostly Layer 3
- **Metric**: Conversion rate from self-hosters to paid: 1-4% year one, 5-8% in mature businesses

### Layer 4: Enterprise Skin
Where margin compounds. Customers pay $300K/year not for the models but for the VPC deployment, SSO into Azure AD, audit logs, SOC 2 Type II, dedicated CSM.
- **Metric**: Net dollar retention: 130%+ means the engine is working

### Layer 5: Network & Data Moat
Where companies become uncopyable. HuggingFace's moat is not Transformers (forkable in an afternoon). The moat is the Hub: 1.5M models, leaderboards, community, brand.
- **HuggingFace**: $4.5B valuation with a fraction of Mistral's annualized revenue — because of the network effect

## The License Trap

Three companies tried restrictive licensing to block cloud providers. All three got burned:

1. **Redis** (March 2024): Dropped BSD → dual SSPL/RSALv2. AWS forked Redis 7.2.4 → Valkey under Linux Foundation. 83% of large enterprises testing/running Valkey by 2025.

2. **Elastic** (January 2021): Moved Elasticsearch from Apache 2.0 → dual SSPL/Elastic License v2. AWS forked → OpenSearch under Apache 2.0. Elastic lost developer mindshare.

3. **HashiCorp** (August 2023): Terraform moved from MPL 2.0 → BSL 1.1. OpenTF Foundation forked → OpenTofu. 10M+ downloads by 2025.

**The pattern is identical**: License change → AWS forks last open version → Linux Foundation picks up governance → Enterprises migrate to fork → You lose developer brand → Eventually relicense back.

**The takeaway**: If your business needs a license restriction to survive, you do not have an open source business. You have a proprietary business with open source marketing. Keep Apache 2.0 on the model. Win on the experience around the model.

## Solo Founder Open-Source AI Plays

44% of profitable SaaS businesses are now run by solo founders (Stripe 2024). Three viable plays:

### Play A: Open Wrapper, Closed Product
Use open-source AI models as a component. Self-host on a $400/month GPU box. Build a niche-specific product on top. Customer pays for the product, not the model.

### Play B: Single Tool With Cloud Tier
Build one small open-source tool that solves a specific problem. Apache 2.0. Offer hosted cloud at $15-50/month. With 50,000 downloads: 500-1,500 paying customers at $25/month = $12K-$37K MRR.

### Play C: Open Source Plumbing, Paid Services
Ship a useful open-source library (evaluation framework, prompt versioning tool, fine-tuning utility). Charge for hosted dashboards, custom integration consulting ($200-300/hour), or a paid SaaS that uses the library.

## Case Study: Mistral ($16M → $400M ARR in 13 Months)

Four things done right:
1. **Open sourced the models that matter** — Full weights, Apache 2.0, permissive license. Legal team takes 30 minutes, not 30 days.
2. **Built serious paid API simultaneously** — La Plateforme launched alongside open weights. Same model, hosted. 60% of revenue from Europe.
3. **Moved up the stack fast** — Le Chat Pro ($14.99/month), Team ($24.99/seat), Enterprise custom. Layering Models 1, 2, 3, 4 simultaneously.
4. **Acquired infrastructure** — Bought Koyeb (French serverless cloud) to own the GPU layer. Same play AWS ran against Linux.

## Case Study: DeepSeek (545% Margin Paradox)

Open weights produced a profitable business through:
1. **Architecture**: MoE activates only 37B of 671B parameters per token. Cost per inference dropped 10x.
2. **Trust loop**: Open weights gave instant credibility. US enterprises that would never sign a contract with DeepSeek run weights on their own GPUs because weights cannot phone home. Then come back for fine-tuning support.
3. **Price ladder**: Web/app free → API with nighttime discounts → V3 cheaper than R1 → Enterprise custom.

## A-Tech Alignment

- **Open-source AI**: The entire framework is built on open-source principles — Apache 2.0 licensing, open weights, community adoption
- **Data privacy**: On-device preprocessing, federated learning architectures for model training
- **Financial freedom**: Solo founder plays enable $200K-$400K ARR businesses without VC funding; open-source reduces customer acquisition cost
- **Practical implementation**: The five-layer stack and Give-Away/Keep Matrix provide concrete decision frameworks, not abstract theory

## When to Use This Skill

- Designing an open-source AI business model from scratch
- Deciding what to open-source vs. keep closed
- Building a hosted-inference or open-core AI company
- Planning solo-founder open-source monetization
- Evaluating whether to change your AI product's license
- Designing a multi-layer monetization stack for an AI product

## When NOT to Use This Skill

- Purely proprietary AI products with no open-source component
- Traditional SaaS without AI
- Companies that have decided to keep everything closed
- Non-software businesses
- AI research without commercial intent