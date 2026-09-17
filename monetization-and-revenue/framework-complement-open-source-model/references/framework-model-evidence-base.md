# Framework Model — Evidence Base

## Source
Solomon, G., Cahana, D. (2026). "How Open Source Stopped Competing With Itself." Notable Capital blog, March 3, 2026. https://notablecap.com/blog/how-open-source-stopped-competing-with-itself

## Three Generations of Open Source Business Models

### Generation 1: Services Model (Red Hat)
- Release open source software, charge for implementation services, support contracts, management
- Created real businesses but didn't improve with scale — ultimately services businesses, not software businesses
- Red Hat: $3.4B revenue at IBM acquisition (2019, $34B deal)

### Generation 2: Open Core + Cloud (HashiCorp, Confluent, Databricks, MongoDB)
- Open core: limited version as open source, enterprise features behind paywall
- Cloud: run open source at scale in optimized cloud environment
- Scaled faster and larger: Databricks reached $100B+ valuation in fraction of Red Hat's time
- Prerequisite: scaled production usage within enterprises before commercialization

### Generation 3: Serverless + Frameworks (Neon, Supabase, Vercel)
- Distribution is still the goal but much more broadly defined
- Usage among individual developers and early-stage startups often as valuable as enterprise adoption
- Monetize everyone from individual developers to large enterprises
- Many users never touch the open source product at all

## Serverless: Open Source as Marketing

### The Model
- Companies build cloud services dedicated to efficiently running open source software
- Economies of scale make the vendor's cloud service practically better on all criteria (including TCO) from day one
- Rather than waiting for open source to gain traction before monetizing, these companies go to market with a commercial product that's superior from the start
- Little reason to self-host the open source in production
- Open source becomes primarily a **marketing tool** — builds trust with developer community and provides an off-ramp if the company fails
- Most users start with the cloud version, even though they could run the open source themselves

### Previous Generation Convergence
Even previous cloud-first open source companies moved in this direction:
- Databricks: transitioned almost entirely to serverless (Databricks Serverless)
- MongoDB: transitioned to MongoDB Atlas (serverless)
- Their early ties to Spark/MongoDB were invaluable for market creation, but business model evolved beyond those roots

## Frameworks: Commoditizing Your Complements

### The Model
- Companies build open source tools that aren't prerequisites for adopting the commercial product, but create demand for it
- Borrows from Joel Spolsky's "commoditize your complements" playbook
- Making tools that create demand for your product free enables faster growth

### Vercel / Next.js
- Use Vercel without Next.js ✓
- Deploy Next.js without Vercel ✓
- But Next.js works better on Vercel → millions of Next.js users = customer acquisition funnel
- Next.js: 33.2M weekly downloads (Feb 2026), up from 8.6M YoY (≈4× growth)
- Vercel AI SDK: 7.7M weekly downloads (Feb 2026), up from 1.2M YoY (7× growth)
- Vercel valuation: $3.25B

> "The framework model aligns incentives in a way that open source never had before. That alignment creates a flywheel: the more developers Next.js serves, the more our business grows, and the more we can reinvest into open source development. The old problem was that the people building open source still needed other jobs to support themselves. With this model, the incentives finally point in the same direction. Great open source is great business, and that means we can fund it sustainably." — Malte Ubl, CTO of Vercel

### Browserbase / Stagehand
- Browserbase open sourced Stagehand (browser automation framework)
- Stagehand creates demand for Browserbase's hosted browser infrastructure
- Stagehand isn't a competitor to Browserbase — it's a standalone tool that works harmoniously with its commercial counterpart

## Key Distinction: Framework Model vs Open Core

| Aspect | Open Core | Framework Model |
|---|---|---|
| OSS relationship to commercial | Subset (concentric circles) | Distinct complement (puzzle pieces) |
| Competition dynamic | OSS inevitably competes with commercial | OSS creates demand, doesn't compete |
| Visualisation | Circle within circle | Two interlocking puzzle pieces |
| "Better together" story | Limited (free version IS a limited commercial) | Strong (two distinct products) |

## Why This Matters in the AI Era

### AI Prefers Open Source
- AI coding assistants demonstrate marked preference for open source tools
- Likely due to greater flexibility and more comprehensive documentation
- Even if OSS tool isn't run at scale in production, being open source increases likelihood of recommendation and use by LLMs
- Neon, Vercel, Browserbase already seeing this effect in growth metrics

### Developer Brand Amplification
- In era where AI increasingly intermediates between products and developers, brand awareness and trust matter more than ever
- Open source remains one of the most powerful tools for building both
- Creates mindshare that translates directly into commercial adoption
- Codegen accelerates this for trusted open-source brands
- Increased shipping speed of open-source contributions means best projects continue to get better in public

## New Commercialization Playbook

### Simultaneous Launch
- Rather than waiting years for open source adoption before commercializing
- Today's companies often release open source and commercial products simultaneously or with minimal gaps
- They're businesses from day one, not pseudo-charities turning into businesses

### Full-Spectrum Monetization
- Traditional: monetized only the high end (large enterprises with scaled production deployments)
- Modern: monetize all the way through — individual developers, startups, mid-market, enterprises
- Possible because commercial product has strong value prop for customers of all sizes

### Maintained Velocity
- Preserves key advantage that made open source powerful: fast feedback loops
- Companies iterate quickly based on user input
- Now getting commercial traction earlier from same individual users
- Accelerating both product development and revenue growth

## Practical Considerations

### Serverless model alignment with modern infrastructure
- Building reliable service that enterprises trust to run critical infrastructure takes time
- 4 nines uptime isn't achieved overnight
- Rather than selling enterprise-grade self-hosted software from day one, start by serving hobbyists and startups while building reliability
- By the time ready for enterprise scale, have both technical foundation and market presence

### Success criteria change
- No longer about enterprise production users as singular metric
- Today's winners measured by how many developers their open source touches, regardless of scale or production usage
- Open source has become the ultimate product-led growth tool
- Creating touchpoints across the entire spectrum of potential customers
- Maintaining the trust and velocity that made it powerful in the first place