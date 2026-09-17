# ShareAI Metering Evidence Base

Source documentation for the ShareAI open source AI metering pattern. This file captures the underlying problem, architecture, pricing patterns, community framework, and implementation checklist that the SKILL.md applies.

Primary source: ShareAI (2026). "Open Source AI Monetization Without Closing the Project." shareai.now/blog/insights/open-source-ai-monetization/

---

## The Core Problem

AI features create variable per-prompt costs that sponsorships and donations cannot cover. Every RAG answer, every summary, every agent run, every code review triggers an inference call with a real cost. Unlike traditional open source — where the marginal cost of a new user is near zero — AI features scale costs linearly with usage.

The community expects open source. They expect to self-host, fork, modify, and run the project freely. But heavy users — teams running thousands of AI queries per day, enterprises deploying agents across workspaces, power users automating workflows — create real, recurring inference costs that no sponsorship model can absorb.

The result: maintainers either eat the cost (unsustainable), close the source (betrays the community), or remove the AI features (kills the differentiator). ShareAI offers a fourth path.

---

## The Pattern

The ShareAI pattern keeps the source, local workflow, and core accessible while metering only the AI traffic that depends on external inference:

1. **Keep source / local workflow / core accessible.** The project license does not change. Self-hosting remains free. The repository stays open. Users can fork, modify, and run the project without paying anyone.
2. **Meter the AI traffic that depends on external inference.** Only AI calls that route through inference providers are metered. Local models, user-supplied API keys, and non-AI features are untouched.
3. **Maintainer configures margin.** The maintainer sets a percentage or fixed surcharge above the inference cost. This margin is the maintainer's revenue per unit of AI usage routed through the metering layer.
4. **Users pay for AI usage directly.** Users who exceed the free allowance pay for their AI usage. Payment goes through the metering layer — the maintainer never handles billing.
5. **Maintainer earns monthly.** The metering infrastructure calculates the maintainer's margin on routed traffic and pays out monthly based on actual usage.

The critical design principle: **charge for heavy AI usage, not for normal participation.** The free allowance must be generous enough that a typical community user never encounters a paywall. Only sustained, heavy AI usage — the kind that creates real inference cost — triggers payment.

---

## Money Flow Architecture

The 5-step money flow:

```
1. Project routes AI inference
   The open-source project's AI features (RAG, summaries, agent runs, etc.)
   route selected inference calls through the metering layer instead of
   calling inference providers directly.

2. Metering layer
   The metering infrastructure (provided by ShareAI) tracks usage by
   user/workspace, enforces free allowances and caps, calculates the
   maintainer's configured margin, and handles billing.

3. User pays directly
   Users who exceed their free allowance pay for additional AI usage.
   Payment is handled by the metering layer — the maintainer never
   sees payment details, never sends invoices, never chases payments.

4. Marketplace inference
   The metering layer routes inference requests to compute providers
   in the marketplace. Providers fulfill the requests at their listed
   price; the metering layer adds the maintainer's margin on top.

5. Maintainer payout
   The metering layer calculates the maintainer's total margin earned
   across all routed traffic for the month and pays out. The maintainer
   sees usage analytics, revenue breakdown, and payout in the builder
   console.
```

---

## Builder vs Provider Distinction

ShareAI distinguishes two roles in the ecosystem:

- **Builder.** The open-source maintainer who integrates the metering layer into their project. The builder earns from the margin on AI traffic routed from their application. The builder's revenue scales with how much AI value their project delivers — more users, more AI usage, more margin revenue. The builder does not provide compute; they provide the application that generates AI demand.

- **Provider.** A compute contributor who offers inference capacity to the marketplace. The provider earns from fulfilling inference requests at their listed price. The provider's revenue scales with how much compute they contribute and how competitively they price it.

A single entity can be both: a maintainer who also runs inference capacity earns builder margin on their project's traffic plus provider revenue on compute they contribute. But the roles are distinct — most open-source maintainers start as builders only.

---

## Best First Features to Monetize

The features most suitable for metering share these traits: clear per-use inference cost, obvious user value, intuitive usage unit, and usage volume that scales with heavy users but not casual users.

### RAG Tools
Meter AI answers and long-context retrieval. Each RAG query triggers an embedding + generation call with clear cost. The usage unit — "answer" or "query" — is intuitive. Heavy users (teams querying large knowledge bases) generate significant cost; casual users (occasional lookups) stay within free allowance.

### Documentation Assistants
Hosted answers and onboarding copilots. Users pay for instant AI-generated answers instead of reading documentation. Per-answer or per-session pricing. Value is clear: time saved vs reading docs. Onboarding flows for new users/employees are a natural paid use case.

### Developer Tools
Code reviews, test generation, PR analysis. Developers see direct value in saved time and caught bugs. Per-PR or per-review unit is intuitive. Teams running automated reviews on every PR generate heavy usage; individual developers doing occasional reviews stay free.

### Note-Taking Apps
Summaries and semantic search. Per-document or per-summary pricing aligns with how users think about usage. Users with large note collections who summarize/search frequently generate heavy usage; occasional users stay within allowance.

### Chatbots / Agents
Conversations, resolved tasks, workflow runs. Per-run or per-conversation unit. Agent runs have obvious and often substantial compute cost (multi-step reasoning, tool calls, long contexts). This is the highest-cost-per-use category and often the most natural to meter.

### Browser Extensions
Page summaries and research actions. Per-action pricing. Lightweight per-use but high volume. Clear value per action (summarize this page, research this topic). Users who run bulk research workflows generate heavy usage; single-page users stay free.

---

## Pricing Patterns

### Included Credits + Paid Top-Ups
Every user or workspace receives a monthly free credit allowance. When credits run out, users buy top-ups. This is the simplest model and mirrors SaaS pricing that users already understand. Credits map to AI usage units (answers, runs, summaries). Unused credits may or may not roll over.

**Best for:** projects where usage varies month to month and users want predictable free usage.

### Free Core + Paid Hosted AI
The project and local/self-hosted AI remain completely free. The maintainer offers a hosted AI endpoint that is metered. Users who want zero-config AI (no API keys, no local model setup) pay for the hosted path. Users who want free run their own inference via local models or their own API keys.

**Best for:** projects with a technical user base that can self-host but also has non-technical users who want convenience.

### Workspace Caps
Free allowance is allocated per workspace rather than per user. Teams that exceed the workspace cap pay for additional AI usage. This aligns cost with the team generating it and prevents a single user's free allowance from being multiplied across many accounts.

**Best for:** team/enterprise deployments where usage is collective rather than individual.

### BYOK + Managed Usage Path
Power users bring their own API keys (free, no margin, no metering). The managed path routes through the metering layer for users who want convenience. This captures revenue from users who value simplicity while keeping the project fully free for users willing to manage their own keys.

**Best for:** developer tools where the audience includes both technical users (who will BYOK) and less technical users (who will pay for convenience).

---

## Community Communication Framework

The single biggest risk in metering an open-source project is community backlash. The community must understand the model before they encounter a paywall. The communication framework covers five points:

1. **What stays open.** Be explicit: the source, the license, self-hosting, local AI, BYOK — all remain free. List them.
2. **What creates AI cost.** Explain that AI features require inference compute that costs money per use. Be specific about which features generate cost and why.
3. **What's included.** State the free allowance clearly. "Every user gets X free AI answers per month." Make it concrete.
4. **What's paid.** State what triggers payment: exceeding the allowance, premium models, heavy team usage. Be specific about pricing.
5. **How to control spend.** Show users how to monitor their usage, set caps, switch to BYOK, or use local models. Users must feel in control of their spend, not trapped.

**Communication channels:** project README, changelog/release notes for the metering release, blog post explaining the reasoning, FAQ in docs, pinned issue for community discussion. Communicate early — before the metering goes live — not after.

---

## The 8-Step Open Source AI Monetization Checklist

1. **Select one AI feature to meter first.** Not all AI features — one. Pick the feature with the clearest usage unit, the most obvious value, and the most significant inference cost.

2. **Define the customer-facing usage unit.** Answer, run, document, ticket, review, conversation. The unit must be intuitive — users should understand what they are paying for without documentation.

3. **Set the free allowance.** Generous enough that casual community users never hit the paywall. Calculate based on the 80th percentile of current usage — only the top 20% of heavy users should ever pay.

4. **Integrate the metering layer.** Route the selected feature's AI inference through the metering infrastructure. Test that free allowance, caps, and billing work correctly before going live.

5. **Configure the builder margin.** Set the percentage or fixed surcharge above inference cost. Start conservative — enough to cover costs plus reasonable revenue, not enough to trigger sticker shock.

6. **Communicate to the community.** Publish the five-point communication (what stays open, what creates cost, what's included, what's paid, how to control spend) before the metering goes live.

7. **Go live and monitor.** Watch usage, revenue, and community reaction for the first month. Track: how many users hit the paywall, how many convert to paid, community sentiment, support tickets.

8. **Review and adjust monthly.** Based on real data: adjust the free allowance (too generous = no revenue, too tight = backlash), adjust margin (too thin = no profit, too high = user churn), refine messaging, consider metering the next feature.

---

## How to Avoid Community Backlash

- **Be specific.** Vague "we're introducing paid features" triggers fear. Specific "AI code review is metered at X per review, first 50 reviews/month free" triggers evaluation.
- **Keep the core open.** The license, the source, self-hosting, local AI — all unchanged. Say so repeatedly. Prove it by not changing the license.
- **Explain the AI compute cost.** Many community members do not understand that AI inference costs money per call. Explain it plainly: "Every AI answer requires a server to run a model, and that server costs money. Sponsorships cover about 5% of our current AI bill."
- **Include a free allowance.** No one should pay for normal participation. The free allowance must cover what a typical community user does in a month. If a casual user hits the paywall, the allowance is too low.
- **Charge for heavy usage, not normal participation.** The target is the team running 10,000 AI queries/day, not the individual doing 20. Frame the messaging around heavy/team usage, not around charging everyone.
- **Offer BYOK.** For technical users, the escape hatch of bringing your own API key removes the "I'm being forced to pay" objection entirely. They can always go free.
- **Communicate before, not after.** The community should never discover metering by hitting a paywall. Announce it, explain it, discuss it, then ship it.

---

## Related Sources

- **GitHub open-source funding analysis.** Data on sponsorship/donation coverage vs actual maintenance cost, establishing the gap that AI metering fills.
- **ShareAI Builder Console.** The interface where maintainers configure metering, set margin, view usage analytics, and receive payouts. The operational tool for the pattern.
- **ShareAI API documentation.** Technical integration guide for routing AI inference through the metering layer. Covers SDKs, authentication, usage tracking, and webhook events.

---

## Cross-References to A-Tech Monetization Skills

- `open-core-ai-feature-metering` — the broader open-core pattern; ShareAI metering is a specific implementation focused on keeping the entire project open while metering only AI traffic
- `tanso-ai-margin-ledger-metering` — margin ledger accounting; how metered AI revenue is tracked, reconciled, and reported for the maintainer's financial records
- `agent-marketplace-builder-economy` — marketplace dynamics; how Builder's Club and similar marketplaces use metered usage to compensate community tool builders
- `real-time-metering-ai-agent-revenue` — real-time usage tracking and billing; the technical infrastructure for metering agent workloads with variable, multi-step inference costs
- `open-source-ai-monetization-mastery-2026` — comprehensive guide; situates ShareAI metering among the full landscape of OSS AI monetization strategies (open core, hosted SaaS, dual licensing, support contracts, marketplace)