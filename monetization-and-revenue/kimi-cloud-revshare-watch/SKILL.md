---
name: kimi-cloud-revshare-watch
description: Monitors the reported Moonshot↔US-cloud revenue-share negotiations (Reuters Aug 26 2026 — Azure/AWS/Google Cloud hosting Kimi K3 for up to 30% of revenue) as the live test case for metered open-weight licensing reaching US hyperscalers, and the DeepSeek-MIT-vs-metered substitute risk that decides whether the model works. Use when tracking open-source AI licensing evolution, advising on model-hosting partnerships, evaluating metered-license exposure, or content on the open-weight monetization story.
---

# Kimi Cloud Revenue-Share Watch

## Overview

Reuters (Aug 26, 2026, three sources): **Moonshot AI is negotiating with Microsoft Azure, AWS, and Google Cloud to host Kimi K3 (2.8T-param open-weight MoE, 104B active, 1M context) for up to 30% of the revenue those platforms earn from K3 services** — the first revenue-sharing arrangement of its kind between a Chinese AI lab and major US clouds. No deal signed; open issues are the split, data access, and cross-cloud token-usage auditing. This extends the metered-license story already captured in `metered-open-license-revenue-share-2026` (Kimi K3's $20M-group-revenue/30% trigger; Qwen3.8-Max's ~$50M) into the **distribution layer**: the question shifts from "who pays to resell" to "do US hyperscalers put a Chinese lab's model on the invoice."

Context that makes this the live test: Moonshot's ARR crossed $100M (Mar) → $200M (May) → $300M (mid-Jun) pre-IPO (HK listing, reported ~$35–50B valuation talks); Chinasoft International signed a domestic revenue-share in July; Alibaba Cloud context — external cloud revenue +45% YoY, AI products ~$7.3B run-rate, MaaS bookings RMB16B ARR with RMB30B year-end target. Geopolitics is load-bearing: US Treasury Secretary said Moonshot might face the trade blacklist; unproven distillation allegations (Moonshot denies; attributes gains to architecture).

## When to Use

- Tracking whether metered open-weight licensing reaches hyperscaler distribution (the decisive adoption test)
- Advising a lab/vendor on hosting revenue-share structures (splits, audit mechanics, data-access terms)
- Advising a business exposed to metered licenses (reseller, cloud, startup near thresholds) — watch-list questions
- Content on "the toll at scale": free weights, taxed distribution
- NOT for: the license mechanics themselves (see `metered-open-license-revenue-share-2026`), or per-token pricing strategy

## Core Process / Workflow

### 1. Why the Meter Moved Up the Stack

Free open weights commoditized the token (DeepSeek V4 MIT set the floor; Qwen API ~$2/M in, $6/M out). Training frontier models cannot be repaid by metered usage alone, and closing the models would forfeit the distribution advantage that made Qwen/Kimi famous. The meter taxes **the layer where money now concentrates** — Model-as-a-Service resale and cloud hosting — while leaving the developer funnel free. Moonshot's cloud negotiation is that logic pressed one level higher: charge the *pipes*, not just the resellers.

### 2. The Substitution Risk (What Kills the Model)

**DeepSeek V4 ships under unmodified MIT — a genuinely free substitute one download away.** A scaled reseller asked for a 30% cut can switch. The metered model is an unproven exchange rate: it trades a little ecosystem for a little claim. Signals to watch (from the ainvest analysis, Aug 28 2026):

1. **Do the Moonshot↔hyperscaler talks close, and at what split?** The first market price for a frontier open model's meter.
2. **Does Alibaba hit its MaaS ARR target (RMB30B) with cloud margins climbing?** Whether the honest-money part compounds.
3. **Does DeepSeek hold the MIT line?** Whether "free open weights" was structural or a passing gesture.

### 3. Exposure Triage by Actor

| Actor | Exposure | Watch question |
|---|---|---|
| Cloud provider | Margin vs. geopolitics; auditability of usage across billing systems | Does hosting a blacklisted-risk model breach procurement/security policy? |
| Reseller/MaaS above thresholds | 30% toll vs. MIT substitute | What's the switching cost to DeepSeek-class MIT models? |
| Startup building on K3 | Threshold cliff (group revenue, TTM) | Where's your trigger point and what's the exit plan? |
| Lab/vendor | Ecosystem health vs. revenue capture | Does the meter chill the funnel that feeds your cloud business? |
| Enterprise buyer | Vendor concentration | Multi-model posture; portability of harness/prompt investments |

### 4. Cross-Chain Watchlist (Completing the Picture)

- Moonshot response to blacklist threat; HK IPO timing (Q3 2026 reported).
- Whether Qwen3.8-Max's Apache-2.0 abandonment (custom license, ~$50M trigger) gets replicated by other labs or reversed.
- First signed hosting deal terms disclosed (split %, minimums, audit mechanism) — compare against Chinasoft's undisclosed domestic split.
- Regulatory posture: EU/US treatment of Chinese open-weight models on US infrastructure.

## A-Tech Alignment

- **Open source**: the watch is about whether *open* survives monetization at the distribution layer — MIT-vs-metered is the live A/B test of openness as strategy.
- **Financial freedom**: threshold cliffs and revenue-share exposure are concrete risks for anyone building on open-weight models; the triage table is protective.
- **Practical implementation**: three watch signals + exposure triage are directly usable in briefings and content.
- **Honesty norm**: everything here is reported-not-confirmed (Reuters-sourced talks; no signed deal; vendor denials on distillation). Treat claims accordingly.

## References

- Reuters (Aug 26, 2026): Moonshot in talks with Microsoft/Amazon/Google for up to 30% of K3 hosting revenue; data-access and token-audit sticking points; all parties declined comment.
- **2026-09-01 verification (Reuters/The Globe and Mail wire recheck):** talks remain open, early-stage, and unsigned — the three unresolved issues are unchanged (split, data access, token-usage auditing) and all four parties still decline comment. Confirmed/expanded context since Aug 26: Moonshot raised **$2B at a ~$20B valuation in May** and reportedly closed **$3.5bn at ~$35bn in late August** (China's National AI Industry Investment Fund a lead), with reported interest at a **$50bn pre-money for a HK listing** — note the ainvest (Aug 28) figure of ~$300M ARR and the earlier $35bn round remain partially conflicting across outlets. Chinasoft International's July domestic revenue-share confirmed as precedent. Alibaba reported to be seeking similar rev-share agreements with major users of Qwen3.8-Max. Geopolitical overhang unchanged: US Treasury Secretary floated trade-blacklisting; unproven distillation allegations (Anthropic Fable) denied — Moonshot attributes K3's gains to original architecture changes. K3 stays third-party top-ranked (Arena.ai #1 web interface building; Artificial Analysis: comparable to GPT-5.5 / Claude Opus 4.8 on multi-step tasks), while 2.8T params keeps self-hosting impractical for the overwhelming majority of customers — which is precisely why the hyperscaler distribution layer is the contested prize. Next scheduled re-check: sign/close of any deal, split terms, and audit-mechanism details.
- ainvest (Aug 28, 2026) "Free Weights, Toll at Scale": who-pays/why-now/P&L analysis; DeepSeek-MIT substitution thesis; three watch signals; Alibaba cloud economics (+45% YoY, MaaS RMB16B→30B target, FCF −44.7B outflow, RMB67.7B capex).
- Startup Fortune (Aug 27/29, 2026): ARR trajectory, HK listing, Chinasoft precedent, blacklist remarks, K3 hardware/market context.
- Related existing skills: `metered-open-license-revenue-share-2026` (license mechanics — this skill tracks the distribution-layer test), `open-source-ai-hosting-economics`, `omla-open-weight-royalty-license`, `open-source-license-strategy-ai-era`, `ai-license-circumvention-defense`.
