---
name: framework-complement-open-source-model
description: Apply the framework model of open source monetization where the open source tool is NOT a subset of the commercial product (open core) but a DISTINCT complementary product that creates demand for the commercial offering — "commoditize your complements." Use when designing open source strategy for AI/infrastructure companies, distinguishing framework model from open core, deciding what to open source when the OSS isn't the product itself, or building developer funnels via complementary open source tools. NOT for open core models (where OSS is a subset of commercial), pure SaaS, or when the open source IS the product you sell.
---

# Framework Model: Open Source as Complementary Demand Engine

## The Core Distinction

| Model | Open Source Relationship | Visualisation |
|---|---|---|
| **Open Core** | OSS is a **subset** of the commercial product | Concentric circles — OSS is inside commercial, inevitably competing |
| **Framework Model** | OSS is a **distinct complementary product** | Puzzle pieces — two separate products with a "better together" story |

In the framework model, the open source tool is NOT a prerequisite for adopting the commercial product, but it creates demand for it. You can use the commercial product without the OSS. You can use the OSS without the commercial product. But they work better together.

## The Strategic Logic: Commoditize Your Complements

Borrowed from Joel Spolsky's playbook. By making the tools that create demand for your product free, you enable faster growth. The open source is **marketing infrastructure** — it builds trust with the developer community and provides an off-ramp if the company fails.

### Vercel / Next.js (Canonical Example)
- You can use Vercel without Next.js
- You can deploy Next.js apps without Vercel
- But Next.js works **better** on Vercel → millions of Next.js users become a customer acquisition funnel
- Next.js: 33.2M weekly downloads (Feb 2026), up from 8.6M YoY
- Vercel AI SDK: 7.7M weekly downloads, up from 1.2M YoY (7× growth)

> "The framework model aligns incentives in a way that open source never had before. That alignment creates a flywheel: the more developers Next.js serves, the more our business grows, and the more we can reinvest into open source development." — Malte Ubl, CTO of Vercel

### Browserbase / Stagehand
- Browserbase open sourced Stagehand (browser automation framework)
- Stagehand creates demand for Browserbase's hosted browser infrastructure
- The framework is not a subset of Browserbase — it's a separate tool that works better with Browserbase

## Why This Model Works in 2026

### 1. AI Prefers Open Source
AI coding assistants demonstrate a marked preference for open source tools over commercial alternatives — likely due to greater flexibility and more comprehensive documentation. Even if an OSS tool isn't run at scale in production, being open source increases the likelihood it will be recommended and used by LLMs.

### 2. Developer Brand Amplification
In an era where AI increasingly intermediates between products and developers, brand awareness and trust matter more than ever. Open source remains one of the most powerful tools for building both — creating mindshare that translates directly into commercial adoption.

### 3. Frameworks Enable Simultaneous Launch
Unlike traditional open source (wait years for adoption before commercializing), framework model companies often release open source and commercial products simultaneously. They're businesses from day one, not pseudo-charities turning into businesses.

### 4. Full-Spectrum Monetization
Traditional open source monetized only the high end (large enterprises). Framework model monetizes all the way through: individual developers, startups, mid-market, enterprises — because the commercial product has a strong value prop for customers of all sizes.

## When to Use the Framework Model

### Good Fit
- You have a commercial product (hosting, infrastructure, platform) that is **distinct from** but **complemented by** a developer tool
- The developer tool creates demand for the commercial product but doesn't compete with it
- AI coding assistants are likely to recommend your tool because it's open source
- You want to build developer trust and brand without giving away the commercial value

### Poor Fit
- The open source IS the product you sell (use open core or hosted SaaS instead)
- The open source and commercial product compete for the same use case (use open core)
- You have no commercial product that benefits from the OSS distribution (pure OSS without a commercial complement = charity, not business)
- Your customers don't care about open source values (B2C, non-technical buyers)

## Design Decisions

### What to open source
- The tool that **creates demand** for your commercial product
- NOT the commercial product itself
- NOT a crippled version of the commercial product
- A genuinely useful standalone tool that developers would adopt even without your commercial product

### What to keep commercial
- The hosting, infrastructure, platform, or service that the framework creates demand for
- Enterprise features (SSO, audit, compliance) on the commercial product
- The operational layer that makes the framework production-ready

### License choice
- Permissive (MIT, Apache 2.0) for maximum framework adoption
- The framework doesn't need AGPL protection because it's not the commercial product
- The commercial product's moat is the operational layer, not the license

## The Flywheel

```
Free framework adoption → Developer trust + brand
        ↓
AI assistants recommend the framework → More adoption
        ↓
Framework users hit scale/production needs → Commercial product becomes attractive
        ↓
Commercial revenue → Reinvest in framework development
        ↓
Better framework → More adoption → Stronger flywheel
```

## A-Tech Value Alignment

| Value | Alignment |
|---|---|
| Open-source AI | Framework is genuinely open source (permissive license); commercial product is separate but benefits from OSS trust |
| Data privacy | Framework can be self-hosted; commercial product can offer data sovereignty; no lock-in to the framework itself |
| Financial freedom | Framework model monetizes full spectrum (individual to enterprise); not dependent on enterprise-only sales; faster revenue ramp than traditional OSS |
| Practical implementation | Proven at scale (Vercel/Next.js: $3.25B valuation, 33.2M weekly downloads); framework model companies are businesses from day one |

## What This Skill Is NOT

- NOT open core (OSS is a subset of commercial — different model, different dynamics)
- NOT pure open source charity (the commercial product is the business; the framework is the funnel)
- NOT a recommendation to always open source (if your differentiation is the implementation, OSS exposes it to competitors)
- NOT applicable when the OSS and commercial product compete (that's open core with a marketing problem)

## References

See `references/framework-model-evidence-base.md` for the full model comparison, case studies (Vercel/Next.js, Browserbase/Stagehand, Neon, Supabase), and the serverless open source variant.