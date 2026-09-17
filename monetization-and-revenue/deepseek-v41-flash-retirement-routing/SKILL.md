---
name: deepseek-v41-flash-retirement-routing
description: Applies the DeepSeek-V4.1-Flash release (Sept 10, 2026; MIT weights, 552B MoE backbone, 1M context, Causal Encoder-Decoder architecture, KV cache compressed to 890 bytes/token) and its SCHEDULED FORCED MODEL SWAP — from 04:00 UTC Sept 14, 2026, every API request addressed to `deepseek-v4-pro` is served by V4.1-Flash and billed at Flash prices (no opt-out, no code change, a changelog line only) — as the first open-lab RETIREMENT ROUTING pattern. Use when [designing API dependency-review checklists for model-swap risk, advising teams on multi-model failover architecture, pricing cache-engineering decisions for agent workloads, or teaching the vendor-initiated-swap playbook]. NOT for [benchmark-quality rankings of frontier models (use china-open-source-llm-arr-tracker) or closed-lab severance cases (see openai-cursor-severance-case-2026 — that pattern severs; this one substitutes)].
---

# DeepSeek V4.1-Flash: Retirement Routing as a Forced Model Swap

## Overview
A Chinese open-weights lab used its flagship API tier as a migration lever: on Sept 14, 2026, every `deepseek-v4-pro` call began serving V4.1-Flash — a smaller (552B vs 1.6T), cheaper, architecturally NEW model — at Flash pricing, whether the caller asked or not. This is the first open-weight lab to convert a version retirement into a forced model swap on live traffic, and it packages the complete playbook: pre-announcement beta, calendar-public cutover, price-driven opt-in pressure, and a self-disclosed benchmark table honest about where the new model LOSES.

## When to Use
- Reviewing API dependencies that hard-code model names in gateways, agent frameworks, or SDK clients
- Designing failover architecture where the provider can change the model behind an unchanged endpoint name
- Advising teams on cache-engineering decisions (the cached-input line is where the swap saves real money)
- Writing content on "what happens when your AI vendor retires the model you built on"
- NOT for closed-lab dependency severance where supply is cut entirely (use openai-cursor-severance-case-2026)
- NOT for choosing among frontier models by benchmark alone (use redmonk-open-weight-decision-lenses)

## Core Process / Workflow
1. **Understand what shipped.** V4.1-Flash: 552B backbone MoE (8B active reading / 16B writing — asymmetric activation aimed at input-heavy agent sessions), 1M context, native vision, MIT license, trained from scratch on 45T tokens (7:1 text:multimodal). Causal Encoder-Decoder: the 20-layer decoder's global KV cache is projected from encoder final states, not built per-layer — prefill gets the cheap half, decode the expensive half. KV cache at **890 bytes/token** (≈¼ of V4-Flash; ≈1/437th of V1 per the vendor's own chart). DSpark speculative drafting; Engram 196B memory tables.
2. **Read the pricing table as the agent-workload pitch.** Peak rates per 1M tokens: input cache-hit $0.006 (vs $0.044 on V4 Pro — **−86.4%**), cache-miss input $0.30 (vs $1.32 — −77.3%), output $1.20 (vs $3.96 — −69.7%). Off-peak halves everything. Worked session (2M input tokens 90% cached + 100K output, peak): V4 Pro $0.739 → V4.1 Flash **$0.191 (−74.2%)**. "Cache-hit charges often account for a large share of agent costs" — the compression work and the pricing are the same strategy.
3. **Calendar the cutover.** The two-day beta (`deepseek-v4.1-flash-expires-on-0910`) expired on schedule; the release landed Sept 10 across web/app/API; the Pro reroute fires **Sept 14, 04:00 UTC (noon Beijing)** and stays "until V4.1 Pro ships" (no date). Legacy names already route. No opt-out path is documented.
4. **Run the pre-cutover checklist** (transfers from every forced-swap scenario):
   - Inventory every call naming `deepseek-v4-pro` / `deepseek-v4-flash` / `deepseek-v4-flash-vision-exp` — including inside agent frameworks and gateway configs.
   - Re-run your own evaluation against `deepseek-flash` BEFORE the cutover, weighted toward factual recall, multilingual QA, and long-document work — the base-model table shows V4 Pro winning 12 of 16 rows (SimpleQA-Verified −12.9pp, LongBench-V2 −6.3pp, MultiLoKo −5.4).
   - Pin reasoning effort as an INTEGER (1–100), not a name — the string aliases disagree between DeepSeek's reference encoder (low=50, high=75, max=100) and vLLM's recipe (low=25, high=50, max=100); a "high" request runs at 75 in one path and 50 in the other, and the effort dial moves Terminal-Bench 2.1 by 8.2 points between 25 and 100.
   - Re-forecast spend (concurrency rises 500→2,500) and decide the regression plan: another provider, self-hosting the MIT weights, or accepting the change.
5. **Read the honest parts.** DeepSeek publishes its own harness-sensitivity spread: DeepSWE 74.2 (mini-SWE) / 72.6 (own harness) / 69.8 (Claude Code) / 65.6 (Codex) — an 8.7-point scaffold spread LARGER than its lead over Kimi K3. It names Terminal-Bench 4.0 as a 20.6-point gap to Claude Opus 5 (31.2 vs 51.8). It omits the GPT-6 Astra column (Astra 57.9 on TB 4.0). All benchmark columns for rivals are collected, not re-run; Artificial Analysis had not measured V4.1-Flash at writing.

## Key Evidence
- MIT weights on Hugging Face day one, 51-page technical report, vLLM day-one recipe (≈511 GB on disk; ≥614 GB accelerator memory)
- 734 HF likes in 24 hours; the "new architecture family" framing (a `.1` that founds a family, per teortaxesTex)
- Effort dial 1–100: 25→100 lifts 8-benchmark average 67.1→76.3, DeepSWE 66.0→74.2 — at ~2.5× output tokens
- Workload fit: leading on 12/12 agentic rows vs V4 Pro; trailing on 12/16 base-model (knowledge) rows
- Watch items: does V4.1 Pro ship as promised; does the routing precedent spread (GLM/Qwen/Moonshot); the first independent Artificial Analysis measurement

## Pairs with
`openai-cursor-severance-case-2026` (the severing-vs-swapping boundary case), `china-open-source-llm-arr-tracker` (DeepSeek's hosting economics; refresh with this entry), `flat-rate-open-source-inference-featherless` (cache-price economics), `gpt6-astra-frontier-pricing` (the frontier counterpart pricing in the same week), `open-source-license-economics-2026` (MIT as the exit hatch), `agent-runtime-model-pricing-disaggregation`.

## A-Tech Alignment
- **Open source**: MIT weights are the counterweight to the forced swap — the customer who dislikes the swap can self-host or re-route; openness is the consumer protection.
- **Privacy**: the swap changes no data-handling contract, but the pre-cutover re-test is the natural moment to re-check the hosted-endpoint data-residency question (Reuters: STAR-Market listing; the distillation advisory).
- **Financial freedom**: cache engineering is the controllable lever — the worked example shows a 74% cost cut on identical workload; "price per completed task, not per token" is the budgeting rule.
- **Practical**: a five-item checklist that turns an unrequested vendor change from an incident into a scheduled re-test.

*Source: DeepSeek-V4.1-Flash release + pricing page (Sept 10, 2026); Datanorth analysis (Sept 10); OrcaRouter "new base model, not a point release" analysis (Sept 10); ProgressiveRobot breakdown; teortaxesTex naming observation.*