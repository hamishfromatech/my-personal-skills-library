---
name: metered-open-license-revenue-share-2026
description: Applies the August 2026 "metered open license" revenue-share turn in open-weight AI monetization (Moonshot Kimi K3 / Alibaba Qwen3.8-Max). Use when designing open-source AI licensing, choosing between Apache/MIT/BSL/revenue-share licenses, advising on model monetization strategy, or analyzing open-weight business models.
---

# Metered Open License Revenue Share 2026

## Overview

August 2026 marked the emergence of a third path in open-weight AI monetization — the **metered open license**: weights stay downloadable and free, but the license installs a revenue threshold above which scaled commercial users owe the lab a negotiated revenue share. Moonshot AI set the template with Kimi K3 (2.8T parameters, largest open-weights release at the time): free below $20M in total group revenue (trailing twelve months), forbidding commercial use above that line until a paid agreement is signed — reportedly up to **30% of revenue**. Alibaba followed with Qwen3.8-Max (2.4T parameters, abandoning its earlier Apache 2.0 licensing), reportedly triggering at ~$50M group revenue.

The market framed this as "open-source AI is no longer free." The more precise reading: **the license is a meter, not a paywall**. It charges the buyers who demonstrably convert free weights into revenue — resellers, cloud providers, scaled deployments — and leaves the ecosystem-recruiting funnel (developers, hobbyists, researchers) untouched.

> **CYCLE-22-RUN-3 UPDATE (2026-09-11):** Z.ai's GLM-5.3 License (Aug 28, 2026) extended the meter to a **security-review requirement** for commercial hosting above $10B aggregate revenue — the meter now gates on WHO deploys (security clearance at scale), not just on revenue (Moonshot: $20M attribution trigger; Alibaba: ~$50M category trigger; Z.ai: $10B hosting security review). The three-lab philosophies: DeepSeek = MIT everywhere (unconditional); Moonshot = attribution at scale; Z.ai = security clearance at scale. See `glm53-revenue-gated-security-review` for the full clause analysis and the hosting-vs-routing distinction.

## The Three-Path Landscape

| Path | Example | Trade |
|---|---|---|
| Close the model | US labs (closed APIs) | Lose the distribution advantage that made open weights famous |
| Keep it free (unmodified permissive) | DeepSeek V4 (MIT, no gates, no meters) | Free distribution as the moat; monetize elsewhere |
| **Meter the open license** | Moonshot K3, Alibaba Qwen3.8-Max | Trade a little ecosystem for a claim on downstream revenue |

## Who Pays, and Why

The meter is installed precisely at the layer where money now concentrates:

- Running the model on your own hardware: **free**
- Building an app on top of an API: ordinary token prices
- Offering the model's output to paying customers at scale ("Model-as-a-Service" / "AI Work Assistant" classification): cross $20M (Moonshot) or ~$50M (Alibaba) **group revenue** — not model revenue — and you enter commercial negotiation

Note the trigger is the **licensee's whole group revenue**, not revenue attributable to the model. This widens the toll base substantially.

## Why Now

- Free open weights commoditized the token. Alibaba's managed Qwen API runs ~$2/M input and ~$6/M output tokens — at those prices, usage cannot pay for frontier training and serving.
- A lab could close (losing distribution) or stay free (getting nothing from resale). The meter is the third path.
- Moonshot's negotiations with Microsoft Azure, AWS, and Google Cloud (reported Aug 26, 2026) to host Kimi K3 for up to **30% of hosting revenue** would be the first revenue-sharing pact between a Chinese AI lab and a major US cloud — the first real market price for a frontier open model's meter.
- Context: Moonshot's ARR passed $100M (March) → $200M (May) → $300M (mid-June); valuation ~$31.5B→$50B pre-IPO; Kimi K3 launch triggered a global market selloff (Nasdaq −1.4%, Nvidia briefly losing top market cap).

## What It Does and Doesn't Do

**The claim that could fail is competitive, not legal**: DeepSeek remains on clean MIT. A scaled reseller asked to hand over 30% has a genuinely free substitute one download away. If enough take it, the meter chills the very ecosystem that feeds the lab's cloud business. Free distribution was the moat that made Qwen famous — the new license trades a little ecosystem for a little claim, and the exchange rate is unknown.

**For Alibaba specifically**, direct meter revenue is a rounding error against its cloud MaaS (RMB 16B ARR, RMB 30B year-end target; AI products ~$7.3B annual run rate, 35% of cloud external revenue, triple-digit growth for 12 quarters). The meter's value is strategic: it protects cloud MaaS pricing and converts deployments that previously paid nothing (Qwen in customer data centers or third-party clouds) into a claim that can be pressed later.

**Signals to watch**:
1. Whether Moonshot–US cloud talks close, and at what split (first market price for the meter)
2. Whether Alibaba's MaaS ARR hits RMB 30B while cloud margins climb
3. Whether DeepSeek holds the MIT line or follows into metering (structural weapon vs passing gesture)

## Comparison With Existing License Models

| Model | Trigger | Revenue to lab | Ecosystem cost |
|---|---|---|---|
| Apache 2.0 / MIT (permissive) | None | $0 (unless cloud/API is the business) | None |
| BSL / FCL (source-available) | Restricted fields | License fees | Fork risk, community friction |
| Open-core | Feature boundary | Support/managed revenue | Feature-split maintenance |
| **Metered open license** | **Licensee revenue threshold (group-wide)** | **Negotiated % of downstream revenue** | **Reseller substitution risk; threshold uncertainty** |

The metered license is closer to BSL in spirit (permission with a commercial boundary) but the boundary is the *user's revenue*, not the *use case* — a genuinely new axis.

## Implications for Open-Source Builders

- **For model labs**: the meter converts distribution into an option on downstream revenue. Price the threshold above the ecosystem-building zone (individual devs, startups under the line) and below where commercial conversion is demonstrable.
- **For builders using open weights**: model choice now has a license dimension that changes with your revenue. Track your group revenue against thresholds of every model in your stack; the Kimi K3/Qwen3.8-Max class of licenses means a successful product can retroactively create a liability.
- **For downstream distributors** (hosting providers, resellers): expect labs to negotiate hosting revenue shares — precedents exist (Moonshot–Chinasoft signed July 2026; talks with US clouds pending).
- **Geopolitical overlay**: US officials have weighed blacklisting Moonshot; the meter negotiations are entangled with export-control and data-access disputes. License terms can become diplomatic instruments.

## Decision Framework: Choosing a License in the Metered Era

| Your situation | Recommended posture |
|---|---|
| Lab wanting distribution + monetization | Metered license only if you have a real cloud/API business to protect; otherwise permissive (DeepSeek logic) |
| Startup building on open weights < $20M revenue | Permissive-licensed models safest; metered models fine (below threshold) — but model future revenue against the threshold |
| Scaled reseller / hosting provider | Negotiate revenue shares before building on metered models; or build on MIT-licensed alternatives for pricing predictability |
| Enterprise adopting open weights | Ask: does our group revenue trip any threshold? Who in our stack is the licensee? |

## A-Tech Values Alignment

- **Open-source AI**: The meter is a stress test of the open-weight thesis — A-Tech's position is that open weights remain the strategic default (DeepSeek's MIT line is the reference point), and this skill documents exactly where and why the free-license consensus is fracturing.
- **Data privacy**: Not central here, but the cloud-hosting negotiations hinge on data access and usage tracking — metering enforcement requires telemetry, which has privacy implications for how token usage gets audited across clouds.
- **Financial freedom**: For solo builders and small startups, the threshold structure is a gift — free until demonstrably commercial. The skill documents the cliff edges so builders don't trip them.
- **Practical implementation**: Concrete threshold numbers ($20M/$50M/30%), a license-comparison table, and a decision framework for builders.

## Related Skills

- `ai-agents-and-workflows/open-source-fm-openness-economics/` — economics of open foundation models
- `monetization-and-revenue/open-core-business-model-strategic-framework/` — the classic open-core boundary design this extends
- `monetization-and-revenue/omla-open-weight-royalty-license/` — the closest prior license innovation (royalty-based)
- `monetization-and-revenue/open-source-ai-monetization-stack/` and `shareai-open-source-ai-usage-metering/` — metering patterns for downstream builders
- `monetization-and-revenue/give-away-keep-matrix-oss-ai/` — what stays free vs paid
