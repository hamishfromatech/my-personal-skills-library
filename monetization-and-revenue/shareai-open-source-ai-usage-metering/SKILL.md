---
name: shareai-open-source-ai-usage-metering
description: Applies the ShareAI pattern for monetizing open-source AI projects by keeping the core project open while metering optional AI-heavy features through a routing layer. Use when [designing monetization for OSS AI projects, separating free core from paid AI usage, implementing usage-based billing for open-source tools].
---

# ShareAI: Open-Source AI Usage Metering

## Overview

The ShareAI framework (shareai.now, 2026) provides a routing, billing, and payout layer for open-source projects that have AI features with variable inference costs. The core insight: AI features break the economics of traditional open source because serving costs are no longer flat — every model call, RAG lookup, agent run, or generation triggers a real, variable, per-user cost. Heavy users and light users both star the same repo, but one creates cents of cost and the other creates hundreds of dollars of inference bills per month.

ShareAI solves this by keeping the source, local workflow, and core product fully open while routing selected AI inference requests through a metering layer. The maintainer configures a margin/surcharge on routed traffic. Users who want the managed AI path pay ShareAI directly for their usage. ShareAI pays the maintainer monthly based on the margin earned. The project never closes, the license never changes, and self-hosting remains free.

This skill provides the complete pattern: the problem framing, the 7-step monetization plan, pricing patterns, community communication templates, the RAG-specific billing-unit insight, and the distinction between Builder payouts and Provider rewards.

## When to Use

- **Designing monetization for an open-source AI project** where the core must stay open but AI features create real per-user inference costs.
- **Separating the free core from paid AI usage** — deciding what stays free, what gets metered, and how to communicate the boundary.
- **Implementing usage-based billing for an open-source tool** without building billing, routing, and payout infrastructure from scratch.
- **Transitioning from sponsorships/donations to usage-based AI revenue** when community goodwill no longer covers the inference bill.
- **Monetizing a RAG application** where downloads are the wrong billing event and queries are the right one.

## NOT For

- **Projects without AI features.** No AI traffic means nothing to meter — use traditional OSS monetization (open core, hosting, support, dual licensing).
- **Communities that reject any paid path.** If the community will not accept metered AI usage even with a generous free allowance, this pattern causes backlash before revenue.
- **Trivial AI costs.** If inference costs are negligible, sponsorship suffices and metering adds friction without meaningful return.
- **Projects intended to go closed-source.** ShareAI is built for maintainers committed to keeping the project open.

## The Core Problem: Why AI Breaks Open-Source Economics

Traditional open source has near-zero marginal cost per user. A new user downloads the code, runs it locally, and costs the maintainer nothing. AI features destroy this model:

- Every RAG answer, summary, agent run, code review, or generation triggers an inference call with a real cost.
- Heavy users and light users both star the same repo but create vastly different operating costs.
- A single power user automating workflows can generate more inference cost than a thousand casual users.
- Sponsorships and donations scale with goodwill, not with the inference bill — they cannot absorb variable, usage-driven costs.

The result: maintainers either eat the cost (unsustainable), close the source (betrays the community), or remove the AI features (kills the differentiator). ShareAI offers the fourth path.

## The ShareAI Pattern

```
1. Keep source / local workflow / core product accessible
   The project license does not change. Self-hosting remains free.
   The repository stays open. Users can fork, modify, and run freely.

2. Route selected AI inference through ShareAI
   Only AI calls that depend on external inference are routed.
   Local models, user-supplied API keys, and non-AI features are untouched.

3. Configure margin/surcharge
   The maintainer sets a percentage or fixed margin above inference cost.
   This margin is the maintainer's revenue per unit of routed AI usage.

4. Users pay ShareAI directly
   Users who exceed the free allowance pay for AI usage through ShareAI.
   The maintainer never handles billing, invoices, or payment details.

5. ShareAI pays the maintainer monthly
   ShareAI calculates total margin earned across routed traffic and pays out.
```

The critical design principle: **charge for heavy AI usage, not for normal participation.** The free allowance must be generous enough that a typical community user never encounters a paywall.

## The 7-Step Monetization Plan

### 1. Define what stays free

Be explicit. The source, the self-hosted workflow, the core product, local model support, BYOK — all stay free. Write it down. This is the foundation the community trusts.

### 2. Name the successful outcome

What does the user achieve with the metered AI feature? Not "AI answers" but "every developer gets instant, accurate documentation answers without reading 50 pages." The outcome is what users pay for, not the inference call.

### 3. Measure the full cost path

Trace the actual inference cost per unit of the successful outcome. A RAG answer may involve: embedding query → vector search → context retrieval → LLM generation → optional reranking. Each stage has a cost. Know the total, not just the model call.

### 4. Set an allowance and paid path

Define the free allowance (generous enough for casual use) and the paid path (top-ups, per-unit pricing, workspace caps). Use customer-understandable units — answers, runs, documents — not tokens.

### 5. Route selected inference through ShareAI

Integrate the metering layer. Route only the AI calls you want to monetize. Keep local models and BYOK paths free. The integration is a routing change, not a rewrite.

### 6. Add limits and failure rules

Define what happens when users hit caps: graceful degradation, fallback to local mode, clear messaging. Never silently break — always explain why the limit exists and how to continue.

### 7. Explain the model in plain language

The community must understand the model before they encounter a paywall. Explain what stays open, what creates AI compute cost, what's included vs. paid, and why the paid path exists. Transparency is non-negotiable.

## Best First Features to Monetize

Start with ONE feature — not the entire AI surface. The first metered feature should be one users already love with obvious inference cost and clear per-unit value.

| Feature | Why It Works | Natural Billing Unit |
|---|---|---|
| **RAG tools** | Clear per-query cost, clear value, easy to unit-price | Answer / query |
| **Documentation assistants** | Users pay for instant answers instead of reading docs | Answer / session |
| **Developer tools** (code reviews, test generation, agent runs) | Developers see clear value in saved time | Review / run / PR |
| **Note-taking apps** (summaries, semantic search) | Per-document pricing aligns with how users think | Summary / document |
| **Chatbots / agents** (conversations, resolved tasks) | Agent runs have obvious compute cost | Run / conversation |
| **Browser extensions** (page summaries, research actions) | Lightweight, high-volume, clear value per use | Action / summary |

## Pricing Patterns

- **Included credits + paid top-ups.** Every user/workspace gets a monthly free credit allowance; top-ups are paid. Simplest model, mirrors SaaS pricing users already understand.
- **Free core + paid hosted AI.** The project and local/self-hosted AI remain free; the maintainer's hosted AI endpoint is metered. Users who want zero-config AI pay; users who want free run their own inference.
- **Workspace caps for heavy teams.** Free allowance per workspace; teams that exceed the cap pay for additional AI usage. Aligns cost with the team generating it.
- **BYOK + managed usage path.** Power users bring their own API keys (free, no margin); the managed path routes through metering for users who want convenience. Captures revenue from users who value simplicity over cost optimization.

## Community Communication Patterns

Open-source communities accept metered AI usage when the boundary is honest. They revolt when a project markets AI as free, hides the cost, then suddenly removes access.

**Do:**
- Be specific about what stays open: source, self-hosting, local models, BYOK.
- Explain AI compute costs in plain language — users need to understand why AI usage is priced differently from the rest of the project.
- Use customer-understandable units: answers, runs, documents — not tokens or API calls.
- Give light users a fair starting allowance that covers normal participation.
- Make the paid path opt-in where possible — users should choose the managed path over BYOK because of convenience, not because they were forced.

**Don't:**
- Call the surcharge a tax — tie it to the AI work the user values.
- Hide the cost structure — if a RAG answer costs $0.02 in inference and you charge $0.03, say so.
- Remove AI features that were previously free without warning or migration.
- Gate the core product behind the AI paywall — the core stays useful without paid AI.

## Builder Payouts vs. Provider Rewards

ShareAI operates two distinct reward mechanisms. Confusing them undermines community trust.

| Mechanism | Who Earns | What They Earn For | How It Works |
|---|---|---|---|
| **Builder payouts** | OSS maintainers / app developers | Margin on AI traffic their project routes through ShareAI | Maintainer configures a surcharge; users pay for routed AI usage; ShareAI pays the maintainer the margin monthly |
| **Provider rewards** | Compute providers | Contributing inference compute to the ShareAI marketplace | Providers list inference capacity; ShareAI routes requests; providers earn based on compute fulfilled |

A maintainer can be both a Builder (earning from app traffic margin) and a Provider (earning from compute contribution), but the two revenue streams are distinct: one rewards building AI-powered applications, the other rewards supplying the compute that runs them.

## RAG-Specific Pattern: Price Queries, Not Downloads

> **"Open Source RAG App Monetization: Price Queries, Not Downloads"**

Downloads are the wrong billing event for RAG applications. A single download can generate thousands of queries over the lifetime of a deployment. If you charge per download, you capture none of the variable inference cost that queries create. If you charge per query, you align revenue with the actual cost driver.

**The billable unit should be a successfully completed RAG answer — not a download, not a token, not a session.**

### Why downloads fail as a billing unit for RAG

- One download → potentially thousands of queries over months or years.
- The heaviest users (production deployments) generate the most queries but pay the same one-time download fee as a user who tries it once.
- Download-based pricing captures adoption, not usage — and inference cost is driven by usage.

### Why tokens fail as a billing unit for RAG

- Tokens are invisible to end users — they don't know what a token is.
- Token counts vary wildly with context length, model choice, and prompt structure — making costs unpredictable for users.
- Tokens conflate retrieval cost (embedding, vector search) with generation cost (LLM output) even though they have different cost profiles.

### Why a completed RAG answer is the right unit

- Users understand "an answer" — it maps to the value they receive.
- It bundles the full cost path (embedding → retrieval → generation → optional reranking) into one transparent price.
- It aligns revenue with the successful outcome, not the input volume.

### RAG pipeline cost profiles

Each pipeline stage has a different cost structure that the maintainer should understand when setting per-answer pricing:

| Pipeline Stage | Cost Profile | Notes |
|---|---|---|
| **Indexing** | Upfront, per-document | One-time cost when documents are added; scales with corpus size |
| **Retrieval** | Low, per-query | Vector search is cheap but not free; scales with query volume |
| **Generation** | High, per-query | LLM call is the dominant cost; scales with context length and output length |
| **Workflow steps** (reranking, summarization, multi-hop) | Variable, per-query | Optional stages that add cost and value; good candidates for tiered pricing |

The maintainer should measure the blended cost per completed answer across all stages, then set the per-answer price with margin above that blended cost.

## A-Tech Applications

| Product | Free Core | Metered AI |
|---|---|---|
| **A-Coder** | IDE, editor, local tooling, local model support, BYOK | AI code review, PR analysis, test generation, agent runs — routed through ShareAI with per-review or per-run billing |
| **Be Practical** | Curriculum, exercises, community access, local feedback | AI-powered personalized feedback, assessment grading, learning path optimization — per-assessment or per-session billing |
| **Builder's Club** | Marketplace, community, publishing tools | Community AI tool usage — each builder's AI traffic routes through ShareAI with their configured margin; marketplace handles discovery and payouts |

## A-Tech Alignment

- **Open source.** The core project stays open. Only AI usage is metered. The project license, source repository, and self-hosted workflow remain unchanged. Users can always run the project and their own inference for free.
- **Data privacy.** Self-hosted core keeps all non-AI data local. Routed inference only applies to opted-in AI features. Usage data collected for billing is the minimum necessary — what was used, by whom, how much — not content, not behavior, not surveillance.
- **Financial freedom.** Sustainable AI feature funding without closing the project. The maintainer earns from the margin on AI traffic their project generates, creating a revenue path that scales with actual AI value delivered rather than goodwill.
- **Practical implementation.** The 7-step monetization plan provides a concrete sequence. ShareAI provides the routing, billing, and payout infrastructure. The maintainer integrates the metering layer, configures their margin, and communicates to their community — ShareAI handles the rest.

## Cross-References

- `open-core-ai-feature-metering` — the broader three-layer separation model (core access / commercial value / AI consumption) that this pattern implements for the AI consumption layer
- `shareai-open-source-ai-metering-pattern` — the foundational ShareAI metering pattern; this skill extends it with the 7-step plan, Builder vs. Provider distinction, and RAG-specific billing-unit guidance
- `tanso-ai-margin-ledger-metering` — dual-sided margin ledger for teams that need per-customer, per-feature, per-model margin visibility on metered AI revenue; complementary infrastructure for self-hosted metering
- `open-source-risk-removal-monetization-2026` — the broader 2026 shift from selling code access to selling risk removal; ShareAI metering is one implementation of usage-based controls within that framework
- `open-source-ai-monetization-mastery-2026` — comprehensive OSS AI monetization playbook (Give-Away/Keep Matrix, 5-Layer Stack, license-trap analysis); ShareAI metering fits within the Self-Host Loss Leader and Managed Cloud layers

## Source

Based on ShareAI (2026). "Open Source AI Monetization Without Closing the Project." shareai.now/blog/insights/open-source-ai-monetization/

RAG-specific pattern: ShareAI (2026). "Open Source RAG App Monetization: Price Queries, Not Downloads." shareai.now/blog/insights/rag-monetization/

See [references/evidence-base.md](references/evidence-base.md) for the full evidence base: the cost-path economics, the 7-step plan detail, pricing pattern comparisons, community communication templates, Builder vs. Provider reward mechanics, and the RAG pipeline cost analysis.