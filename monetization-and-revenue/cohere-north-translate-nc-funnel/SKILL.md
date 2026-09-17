---
name: cohere-north-translate-nc-funnel
description: Applies Cohere's North Small Translate release (Sept 10, 2026; 218B/25B MoE, CC BY-NC 4.0 non-commercial open weights, WMT26 83.6 leading DeepL/Google Translate and all open-weight rivals, commercial access through RWS Language Weaver) as the emerging NON-COMMERCIAL-WEIGHTS-AS-COMMERCIAL-FUNNEL pattern — free weights for research, paid route through a partner channel. Use when [evaluating NC-licensed model releases, designing vertical-model monetization where the open build is a lead-gen surface, comparing translation/vertical model options on cost-per-task, or briefing on sovereign-AI positioning]. NOT for [general license-strategy questions across source-available licenses (use license-axis-business-type) or per-vendor ARR tracking (use china-open-source-llm-arr-tracker)].
---

# Cohere North Small Translate: NC Weights as the Commercial Funnel

## Overview
Cohere shipped a 218B-total/25B-active MoE translation model as **free open weights under CC BY-NC 4.0** — and simultaneously routed every commercial use through RWS's Language Weaver product. The release is the cleanest demonstration yet of a vertical-model monetization pattern the library has not held: the open build is the credibility and evaluation surface; the commercial tier is a partnership channel with an enterprise localization vendor.

## When to Use
- Evaluating an NC-licensed model release for research/prototype work or for commercial deployment paths
- Designing monetization for a vertical (translation, medical, legal, domain-specific) model where the audience is enterprise buyers
- Teaching the "download the weights to evaluate, buy the product to deploy" pattern
- NOT for general open-weight license design (use license-axis-business-type)
- NOT for Chinese-lab ARR tracking (use china-open-source-llm-arr-tracker)

## Core Process / Workflow
1. **Read the licensing shape, not just the license.** CC BY-NC 4.0 = free to download, run, fine-tune, publish — for research and non-commercial use only. Cohere states the commercial path directly: "For enterprises that need more than open-weight research access, security, scalability, and a dedicated translation and localization platform, North Small Translate is available through RWS's Language Weaver product." The weights are the funnel; the product is the platform.
2. **Read the performance claim as a benchmark-led wedge.** WMT26 across all languages: **83.60** vs DeepL NextGen 81.37, Qwen 3.5 397B-A17B 81.56, GLM 5.2 FP8 76.50, Gemma 4 31B (on) 79.46, Google Translate 68.20 — using GPT-5.6-Sol as judge; the Agentic variant (find-and-fix errors in translation) scores 84.36. Best regional consistency across the full spread; beats DeepL NextGen in every non-European region by ~8–10 points in South Asia/MENA, ~4–5 in Southeast Asia, ~1–3 in East Asia. Long-context: 48.9 vs Google Translate 21.3 and Gemma 31B 19.4.
3. **Read the cost curve.** 80.1 score at **$0.000676/task** (661 tokens avg) vs Gemini 3.1 Pro Preview at $0.038928/task — the headline "5,762% more" is a vendor arithmetic frame; the comparable reads are Qwen 3.5 397B-A17B at $0.004525 and Cohere's own Command A+ at $0.005158 — roughly 7× and 7.6× respectively. Throughput: up to 1.4× higher output tokens/sec than Gemma 4 31B under identical concurrency/hardware.
4. **Position the sovereignty layer.** Cohere frames this as sovereign AI; the model is 50+ languages, runs on 1× B200 @ W4A4 or 2× H100s. RWS works with >80% of the world's top-100 brands — the commercial channel is a localization-services relationship, not a self-serve API.
5. **Audit the pattern's boundaries.** The judge is a closed frontier model (GPT-5.6-Sol); the WMT26 table is Cohere's own evaluation; the per-task cost math is vendor-reported. Treat as a strong pattern claim pending independent translation-quality benchmarking.

## Key Evidence
- First translation model in the North family; builds on the Tiny Aya / Command A Translate lineage
- Quantized near-lossless weights on Hugging Face with demo space and implementation guides
- Partnership co-development with RWS/Language Weaver research + language experts
- Watch: whether commercial licensing prices appear openly, whether other vertical models copy the NC-funnel shape, whether judge-selection (GPT-5.6-Sol) draws methodological pushback

## Pairs with
`license-axis-business-type` (the NC axis added to the taxonomy), `give-away-keep-matrix-oss-ai` (weights as lead-gen), `k2-horizon-open-science-fleet-2026` (the Apache-2.0 contrast — K2 gives away everything, Cohere gives away evaluation), `china-open-source-llm-arr-tracker` (DeepSeek/Zhipu hosting tolls as the opposite pole), `sovereign-ai-open-weight-cascade-2026`, `developer-led-gtm-open-source-monetization`.

## A-Tech Alignment
- **Open source**: NC weights keep research and the global open ecosystem moving while the commercial tier funds it — a legitimate middle tier worth tracking, distinct from source-available panic.
- **Privacy**: sovereign positioning (data stays with the customer) is the enterprise pitch; translation workloads carry sensitive business text.
- **Financial freedom**: cost-per-task framing ($0.000676 vs $0.039) is the practical budgeting lens for any team evaluating translation at volume.
- **Practical**: a two-question evaluation — "can I self-host for research today?" and "what does commercial access cost and through whom?" — applies to every NC release.

*Source: Cohere North Small Translate blog (Sept 10, 2026) — WMT26 evaluations, HF weights + quantizations, RWS Language Weaver commercial path; footnotes on benchmark methodology.*