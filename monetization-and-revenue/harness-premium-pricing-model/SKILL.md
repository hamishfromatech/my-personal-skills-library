---
name: harness-premium-pricing-model
description: Applies PricingSaaS "The Harness Premium" (Rob Litterst, Sept 4, 2026) synthesizing Box and GitLab earnings calls, Anthropic's Fable 5.1 cache repricing, Okta and Figma earnings, and the Clay two-meter precedent — as models commoditize, SaaS and AI companies sell the harness (the model-agnostic orchestration/governance layer) at high margin while the pricing conversation shifts to price-per-task, and buyers still pay for seats because predictability wins deals. Use when [pricing an AI or SaaS product in a commoditizing-model market, positioning a platform against per-token AI-native pricing, designing hybrid seat-plus-consumption packaging, or briefing on where AI margin migrates]. NOT for [open-weight model pricing specifically — use the open-source monetization playbook — or usage-metering infrastructure mechanics].
---

# The Harness Premium: Selling Orchestration When Models Commoditize

## The convergence both CEOs landed on

Within the same fortnight, Box (Aaron Levie) and GitLab (Bill Staples) reported earnings and explained the same thing: what they actually sell as models commoditize. Both used the same word — **harness**.

**GitLab's Staples:** "We have several structural advantages versus typical AI native tools whose monetization is based on tokens… Duo Agent Platform is cloud-agnostic, model-agnostic — we support all of the models, including open source, open weight models. Customers can deploy it in any cloud, including air-gapped data centers… for many of our customers, the token or inference cost is actually not embedded in the GitLab agreement. **They pay us for the access to the platform, and the work done in the platform — the context, the harness, the governance and auditability — not the inference. Those are all very high-margin products.**"

**Box's Levie:** "Customers will increasingly want [token-intensive agent] workflows to happen inside of platforms that are model neutral, because the more tokens your use case requires, the more price sensitive you're going to be… By having a model neutral layer — our agentic harness — we can make sure we're directing the workload to whatever is effectively the cheapest model at the accuracy level the customer wants."

The through-line: value is not offering tokens; it's **helping customers deploy tokens productively**. The harness is the orchestration layer that manages tokens and inference — which is another way of saying: it helps customers complete tasks and jobs.

## The model layer shifts to price-per-task

At the model layer, the pricing conversation is shifting to jobs. Anthropic released Claude Fable 5.1 at the *same sticker price* as Fable 5 but cut cached-token costs 75% (from $1.00 to $0.25 per million). Cache is disproportionately used by repeatable agentic workflows — tool definitions and schemas loaded into context, identical across users — and cache is close to free to serve. As Ben Thompson (Stratechery) put it: a better cache layer charged less lets Anthropic lower the **price-per-task** even while the price-per-generated-token stays flat.

Post token-maxxing, the conversation is jobs and workloads at both the model and app level. That would seem to point straight at outcome-based pricing — but so far, it hasn't happened.

## Why buyers still buy seats (the predictability premium)

**Okta's Todd McKinnon:** "The first thing everyone says is, 'Seats are going away, you've got to charge per agent.' That all may be true, but the way customers are using agents now and the way they want to buy is per user… **For now, they like the predictability. It's helping these deals move faster.** I think that's the winning formula for now." (Okta is building the "scaffolding" for consumption pricing as agent usage gets heavy — later, not never.)

**Figma's Praveer Melwani:** AI credits embedded in each seat made seats more valuable, supporting upgrades, conversion, and retention — roughly two-thirds of paid customers above $10K ARR added full seats at renewal, and customers exceeding built-in credits buy add-on subscriptions or enable pay-as-you-go.

Short-term conclusion: **agent pricing looks like legacy SaaS hybrid — a platform license for the harness plus a variable rate for additional tokens.**

## The Clay precedent: two meters, two margins

A year before the harness language, Clay restructured by splitting one credit model into two meters:
1. **Data Credits** — for 3rd-party data or AI from Clay's marketplace (thin margin, near pass-through).
2. **Actions** — for orchestration, work execution, workflow steps (high margin).

Splitting them decoupled platform operations from data purchasing, made the data margin look small (competitive) and the orchestration margin capture explicit. Clay never called it the harness premium; it is the same move.

## The playbook

1. **Inventory what you actually sell when the model is a commodity:** context, governance, auditability, deployment flexibility (cloud-agnostic, air-gapped, open-weight support) — those are the high-margin lines; name them in the price book.
2. **Unbundle inference from the platform fee where enterprise buyers are price-sensitive** — let customers bring cheaper models (including open weights) and pay you for orchestration, not tokens.
3. **Move your own cost curve down via caching/session engineering before cutting list price** — price-per-task is a cache story more than a rate-card story.
4. **Package predictability for the buyer:** seats (or seat-like bundles) with embedded credits for the base workload; variable consumption for the tail. Use Okta's sequencing — seats now, scaffolding for consumption later.
5. **Split your meters like Clay** if you resell data or third-party AI: thin visible margin on pass-through inputs, explicit premium on your orchestration.
6. **Expect outcome-based pricing to keep slipping** — the convergence point is hybrid harness-plus-usage, not pure per-outcome.

## Watch items

- Whether Okta/Figma-style seat economics survive as agent usage scales (Okta's own scaffolding timeline is the tell).
- Whether model labs follow Fable 5.1's cache-cut pattern (price-per-task competition without sticker-price war).
- Whether harness incumbents (GitLab, Box) extend model-neutrality claims into open-weight routing as a default — the positioning collision with AI-native token-monetizing tools is the market's live experiment.

## Honest caveats

- Source is a pricing-industry newsletter synthesizing earnings calls — framing ("harness") is the CEOs' own positioning, and margin claims ("very high-margin") are unaudited management statements.
- Fable 5.1 cost figures are Anthropic-reported; cache-price effects on real workloads vary by context-reuse pattern.
- The seat-vs-consumption equilibrium is explicitly "for now" — treat as a transitional pricing regime, not a steady state.
- Box/GitLab/Okta/Figma/Clay are enterprise-scale cases; solo-founder translation requires adapting the margin logic, not the price points.

## Pairs with

`harness-maturity-matrix` and `harness-engineering-ai-agents-2026` (the engineering side of the harness this monetizes), `open-source-ai-monetization-playbook-2026` (the 5-layer stack — the harness premium is the pricing corollary of layer 4), `ai-pricing-model-taxonomy-2026`, `outcome-based-pricing` (the convergence candidate this skill argues is further away than assumed), `plan-limit-cognitive-thirst-trap` (seat-embedded-credits is the sanctioned version of metering psychology), `saas-agent-bypass-repricing`.

## A-Tech alignment

- **Open source:** GitLab's pitch is explicit — model-agnostic including open weights and open-source models is the *premium feature*; open-weight support converts from ideology to pricing power.
- **Privacy:** air-gapped deployment as a paid harness feature ties the privacy posture directly to margin (the governed-deployment premium).
- **Financial freedom:** the solo-builder translation — sell the harness (orchestration, governance, verification) on top of open models rather than reselling tokens at commodity margin; seats-with-credits is the packaging that makes small AI services financeable.
- **Practical:** the two-meter Clay split and the six-step playbook are directly reusable in pricing reviews; "sell the harness, not the tokens" is a one-line positioning test for any AI product pitch.