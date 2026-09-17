# Open-Source AI Give-Away/Keep Matrix — Evidence Base

## Primary Sources

1. Vikas Malpani (2026). "Open Source AI Business Models: How to Make Money Giving It Away." Comprehensive analysis of Mistral, HuggingFace, DeepSeek, and open-core veterans.

2. Hugging Face ARR data (2026): $100M+ ARR, 97% of users on free tier, 50K organizations, hundreds of petabytes served.

3. Z.ai/Zhipu (2026): Projected ~$1B annualized revenue, ~$112B valuation, open-sourcing GLM-5.2 models.

4. DeepSeek (2026): $400M-$500M annualized revenue run rate, ~$50B+ valuation, MIT-style license for R1.

5. Menlo Ventures (2025). "State of Generative AI in the Enterprise." Enterprise AI spend: $37B in 2025, 3.2x YoY increase.

## Key Revenue Data Points

### AI Company Revenue Benchmarks (2026)

| Company | Annualized Revenue | Valuation | Business Model |
|---|---|---|---|
| OpenAI | ~$25B | $852B | Consumer subscriptions + API + Enterprise |
| Anthropic | ~$47B | $965B | API + Enterprise (~80%) |
| Google Cloud | ~$70B run rate | Subsidiary | Cloud + AI infrastructure |
| Mistral | $400M | ~€1B target | Hosted inference + Enterprise + Le Chat |
| DeepSeek | $400-500M | $50B+ | Open weights + paid inference |
| HuggingFace | $100M+ | $4.5B | Open core + Enterprise Hub |
| Z.ai/Zhipu | ~$1B (projected) | $112B | MaaS + on-prem + API |
| MiniMax | $79M | Public (HK) | AI-native products + API |
| Cohere | $150M+ | Private | Enterprise API |
| xAI | ~$3B | $230B (pre-SpaceX merger) | API + Grok subscriptions |

### Open-Source Model Adoption (2025-2026)

- Apache 2.0 has won as the enterprise standard for AI models (Gemma 4, Qwen 3.5, Mistral Large 3, Yi)
- Over 60% of AI projects integrate open-source models
- 89% of AI-equipped organizations incorporate open-source somewhere in infrastructure
- Deploying open-source tools is 3.5x cheaper than proprietary software
- 68% of tech companies report open-source AI as core strategy
- Startups and mid-sized firms drive 45% of uptake

## The Give-Away/Keep Matrix Logic

The axes are deliberately NOT "open vs. closed." The real axes are:
- **What you open up to others to USE** (free to run/inference/hosted)
- **What you open up to others to MODIFY** (source code/weights)

A thing can be free to use but not free to modify. It can be free to modify but not free to host as a competing service. Each combination is a different business.

## Five-Layer Stack Metrics

| Layer | What Happens | Revenue | Key Metric |
|---|---|---|---|
| 1. Adoption Engine | Free weights, permissive license | $0 direct | Downloads, stars |
| 2. Self-Host Loss Leader | Docs, integrations, libraries | $0 direct | Activation rate (8-12% good) |
| 3. Managed Cloud | Hosted API, inference, cloud tier | First scaled revenue | Conversion (1-4% Y1, 5-8% mature) |
| 4. Enterprise Skin | SSO, audit, VPC, SLA, support | Highest margin | NDR (130%+ healthy) |
| 5. Network & Data Moat | Marketplace, data flywheel, brand | Pricing power | Network effects |

## License Trap Evidence

### Redis (March 2024)
- Dropped BSD → dual SSPL/RSALv2
- AWS forked Redis 7.2.4 → Valkey under Linux Foundation
- 83% of large enterprises testing/running Valkey by 2025
- May 2025: Redis added AGPLv3 back alongside source-available licenses — reversal

### Elastic (January 2021)
- Moved Elasticsearch from Apache 2.0 → dual SSPL/Elastic License v2
- AWS forked → OpenSearch under Apache 2.0
- August 2024: Elastic added AGPLv3 as third option — partial reversal

### HashiCorp (August 2023)
- Terraform moved from MPL 2.0 → BSL 1.1
- OpenTF Foundation forked → OpenTofu within weeks
- 10M+ downloads by 2025, still growing
- IBM acquired HashiCorp for $6.4B (February 2025) — has not reversed BSL

**Pattern**: License change → AWS forks → Linux Foundation governance → Enterprises migrate → Developer brand lost → Eventually relicense back.

## Mistral Trajectory

| Date | ARR | Key Event |
|---|---|---|
| End 2024 | $16M | Mostly small enterprise contracts |
| July 2025 | ~$120M (tripled in 100 days) | Revenue acceleration |
| December 2025 | $312M | Continued growth |
| January 2026 | $400M | CEO targeting €1B by end 2026 |
| February 2026 | — | Acquired Koyeb (French serverless cloud) to own GPU layer |

## DeepSeek Economics

- R1 training cost: ~$6M (base model) / ~$294K (RL component)
- V3/R1 inference: theoretical 545% profit margin
- 2025 net profit: ~$470M on 28-32% net margin
- Architecture: MoE activates 37B of 671B parameters per token (10x cost reduction)
- Model weights: MIT License (most permissive available)

## Solo Founder Data

44% of profitable SaaS businesses run by solo founders (Stripe 2024 Indie Founder Report).

Examples:
- Nomad List (Pieter Levels): $5.3M/yr
- Bannerbear: $991K/yr
- Carrd: $1.5M (2024)
- TweetHunter: $1M+ ARR
- Indie OSS (anonymous): $14.2K/mo

## Cross-References

- `open-source-ai-five-layer-stack` — The original five-layer stack this skill refines
- `open-source-ai-revenue-models` — Five revenue models framework
- `open-source-license-strategy-ai-era` — License decision framework
- `free-lunch-dilemma-open-source-ai-monetization` — The sustainability challenge
- `open-core-enterprise` — The open-core model
- `developer-led-gtm-open-source-monetization` — The GTM execution layer
- `open-source-ai-competitive-moats` — Moat strategy beyond licensing
- `open-source-licensing-landscape-2026` — Current licensing landscape