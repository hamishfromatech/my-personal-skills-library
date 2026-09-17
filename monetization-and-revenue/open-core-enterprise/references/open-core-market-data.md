# Open-Core Market Data and Sources

## Hugging Face Case Study

### Scale
- 5M+ users on the model hub
- 500K+ models and datasets hosted
- $235M valuation (Series C, 2023)
- Profitable enterprise unit by 2024

### Model
- **Open Core:** Model hub, inference API (rate-limited), community features, open-source libraries (Transformers, Datasets, Diffusers)
- **Enterprise Hub:** SSO, audit logs, private model hub, dedicated support, advanced security
- **Inference API:** Pay-per-use for production inference beyond rate limits
- **Enterprise Inference:** Custom deployment, SLA, dedicated compute

### Key Insight
Hugging Face proved that open-source AI infrastructure can be monetized without ads, paywalls on core access, or aggressive growth hacking. The enterprise tier adds governance and operational safety nets — not model access restrictions.

## GitLab Pricing Evolution

### Current Model (2026)
| Tier | Price | Positioning |
|------|-------|-------------|
| Free | $0 | Individual developers, small teams |
| Premium | $19/user/mo | Code review, CI/CD, priority support |
| Ultimate | $99/user/mo | Security, compliance, portfolio management |

### Historical Context
- Started with open-core CE (Community Edition) and proprietary EE (Enterprise Edition)
- Moved to unified codebase with feature flags to reduce maintenance burden
- Continuously adjusted tier boundaries based on community feedback

## MongoDB and Elastic: License Shifts

### MongoDB (SSPL, 2018)
- Shifted from AGPL to Server Side Public License (SSPL)
- Response to cloud providers offering MongoDB as a service without contributing back
- Controversial but effective at protecting open-core sustainability

### Elastic (SSPL, 2021)
- Similar shift from Apache-2.0 to SSPL + proprietary dual license
- Response to AWS OpenSearch fork
- Community backlash but enterprise revenue protection

### Lessons for A-Tech
- License choice at inception matters more than license change later
- Community trust is easier to preserve than to rebuild
- If a license change is needed, involve the community council early and transparently

## 2026 Market Context

### Open-Source AI Adoption Gap
- **Technologychecker.io data:** 5.6M open-source AI projects counted by Stanford
- **Real deployment heavily favors closed-source:** Botpress 1,558 open-source deployments vs. 52,682 closed-source deployments
- **Implication:** The open-source AI ecosystem has a massive "last mile" problem — projects exist but are not deployed at scale

### Enterprise Demand for Vendor-Neutral AI
- **Forbes, April 2026:** "Open Source AI Is Moving From Sideshow To Strategy"
- Enterprises want vendor-neutral AI to avoid lock-in and ensure auditability
- Open-source models (Llama, Mistral, DeepSeek) gaining enterprise traction
- But deployment complexity, security concerns, and lack of support slow adoption

### Risk Transfer as Product
- **Zitadel, 2026:** Open-source infrastructure monetizes through legal clarity, not feature scarcity
- Enterprise buyers pay for: indemnification, warranty, compliance guarantees, custom terms
- The code is free; the confidence to use it professionally is the product

## Dual Licensing Success Metrics

| Company | Open License | Commercial Model | Revenue (est.) | Community Health |
|---------|-----------|-------------------|---------------|------------------|
| GitLab | MIT (CE) | SaaS tiers + proprietary EE | $500M+ ARR | High |
| MongoDB | SSPL | Atlas cloud + enterprise | $1B+ ARR | Moderate (post-license-change) |
| Elastic | SSPL + proprietary | Cloud + enterprise | $1B+ ARR | Moderate (post-license-change) |
| Hugging Face | Apache-2.0 | Enterprise Hub + Inference | Profitable unit | Very High |
| Sentry | BSL → Apache-2.0 | SaaS + on-prem | $100M+ ARR | High |
| Zitadel | Apache-2.0 | Cloud + enterprise license | Growing | High |

### A-Tech Target Position
- License: AGPL-3.0 (core) + proprietary commercial
- Model: Hugging Face-inspired with GitLab-style tiering
- Community health: Very High (primary differentiator)
- Revenue target: Sustainable self-funding within 24 months
