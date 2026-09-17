---
name: agent-runtime-model-pricing-disaggregation
description: Captures 2026's repricing of open-weight hosted inference — DeepSeek V4-Pro moving from flat to peak/off-peak dual pricing (off-peak ~2× the prior flat rate; peak 2× that; Reuters: 50%–1,000%+ increases by model/token type), alongside frontier token price stabilization and cache-discount stratification — evidence that open-weight inference prices track compute demand curves, not the "open = cheap" narrative. Use when forecasting AI inference costs, evaluating open-weight hosting economics, or advising on workload scheduling for token budgets.
---

# Agent Runtime & Model Pricing Disaggregation

## Overview
The open-weight price narrative inverted in one month: DeepSeek moved V4-Pro (the model co-designed with its open Harness runtime) from a flat rate to **peak/off-peak dual pricing** — off-peak $0.66/M input + $1.98/M output (cache hit $0.022), peak double that — where even off-peak is roughly **double the previous flat $0.435/$0.87**. Reuters calculated increases ranging from 50% to more than 1,000% depending on model and token type. Open weights no longer imply flat-or-falling prices; hosted open-model pricing now behaves like electricity, not like software.

## When to Use
- Forecasting inference costs for agent workloads or product unit economics
- Deciding between hosted open-weight inference, frontier APIs, and owned silicon
- Analyzing a vendor's pricing structure for demand-shaping intent
- Content: "Open weights just got a rush-hour premium — what that tells you about compute scarcity"

**NOT for**: China-wide ARR tracking (use `china-open-source-llm-arr-tracker`), subscription/plan-limit psychology (use `flat-rate-to-metered-repricing-2026`), owned-silicon economics (use `sovereign-desk-cost-freedom-narrative`).

## Core Process

### 1. The DeepSeek V4-Pro repricing table (per million tokens)
| Line | Off-peak | Peak | Prior flat |
|---|---|---|---|
| Input | $0.66 | $1.32 | $0.435 |
| Output | $1.98 | $3.96 | $0.87 |
| Cache hit (input) | $0.022 | $0.044 | — |

Read the structure, not just the level: dual pricing means the vendor is **shaping demand across the day** (agentic batch workloads should shift off-peak), cache-hit pricing at 1/30th of input rewards long-context agent loops, and the ~2× off-peak increase over the flat rate reveals that the historical price was below the sustainable marginal cost curve. Still an order of magnitude below Western frontier rates (Claude Opus 5 ≈ $5/$25), but the gap that made DeepSeek "unbeatable" narrowed considerably.

### 2. What this means for the open-weight thesis
The library's structural claims stand but need a time dimension: open-weight hosted inference remains far cheaper than closed, and revenue still accrues at the hosting layer (DeepSeek ~$70M Jan–Jul → ~$1.2B ARR per the ARR tracker). But **price stability is gone**. Any cost model built on "open models cost roughly nothing and keep falling" should add: peak/off-peak spreads, cache-rate stratification (which rewards architecture choices), and repricing risk comparable to any utility.

### 3. The strategic pairing worth teaching
DeepSeek shipped the open runtime and the repricing in the same fortnight. That is the disaggregation in miniature: **the runtime is free (community lock-in), the tokens carry the margin (pricing power at the bottleneck)**. Compare with the metered open-license skills (`metered-open-license-revenue-share-2026`, `license-axis-business-type`): metering revenue shares taxes success; dual-pricing tokens taxes timing. Both are the same insight — when weights are open, the pricing surface moves to whatever is scarce: hosting capacity, distribution, or time-of-day compute.

### 4. Practical implications for workloads
1. **Schedule agents off-peak** — batch, eval sweeps, and fan-out subagent loops are peak-shiftable; interactive chat is not.
2. **Engineer cache hits** — at 1/30th of input price, prompt-prefix stability and context reuse are now direct line-item savings.
3. **Track token-type deltas** — the 50%–1,000%+ range means per-workload math, not headline comparisons.
4. **Treat inference pricing like cloud egress** — expect time-of-day tiers, commitment discounts, and burst premiums to spread across open-weight hosts.

### 5. Honest caveats
Vendor-published pricing with no independent cost accounting; the prior flat rate may itself have been loss-leading or capacity-clearing during a price-war phase (the ARR tracker documented that the sector raised prices while usage still grew — the binding constraint was GPU capacity, not willingness to pay). This repricing is consistent with that: the price war is over; scarcity pricing has begun.

## References
- Pairs with `flat-rate-to-metered-repricing-2026` (the tool-subscription version of the same repricing — together they bracket the AI cost curve), `china-open-source-llm-arr-tracker` (sector context: price war "empirically dead"), `open-source-ai-hosting-economics`, `deepseek-harness-open-runtime` (the runtime-giveaway half of the same strategy), `sovereign-desk-cost-freedom-narrative` (owned silicon as the hedge against token repricing).
- A-Tech alignment: financial freedom (off-peak scheduling + cache engineering are immediate cost moves; "the meter has time zones now"), open source (open weights ≠ price immunity — a needed correction to the channel's own narrative), practical (dual-pricing table + four workload rules), privacy (self-hosting regains an advantage: no peak/off-peak exposure, no vendor repricing risk).