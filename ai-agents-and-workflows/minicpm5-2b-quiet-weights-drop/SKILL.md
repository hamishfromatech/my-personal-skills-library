---
name: MiniCPM5-2B-quiet-weights-drop
description: Applies the September 2026 quiet Apache-2.0 open-weights release of MiniCPM5-2B — the top-ranked sub-4B on-device agent model (AA Intelligence Index v4.2 = 15, first among 47 open models under 4B) with RL+OPD post-training, 16 RL-expert on-policy distillation, and an unusually complete quiet-drop artifact set — to evaluate and deploy small on-device agents. Use when selecting small open-weight models for phones/edge, evaluating quiet-release model cards, or designing RL-teacher + on-policy-distillation small-model pipelines. NOT for frontier-scale deployment or for models whose benchmarks have been independently row-verified.
---

# MiniCPM5-2B: The Quiet Open-Weights Drop

## Overview
ModelBest/OpenBMB released MiniCPM5-2B weights on Hugging Face **Sept 6–7, 2026 — no press release, no launch event** — seven weeks after unveiling at WAIC 2026 (July 19). Apache-2.0 license on both model AND released training data (Ultra-FineWeb, UltraX, UltraData-Code, UltraData-Math, UltraData-SFT-Agent-2609, UltraData-RL-2609). The unusual artifact set makes it the most complete quiet-drop of 2026.

## When to Use
- Selecting the best open-weight model under 4B for on-device agents (phones, smart cockpits, laptops)
- Evaluating quiet-drop model releases (what the repo reveals vs what marketing claimed)
- Designing small-model RL pipelines (RL teachers + on-policy distillation into one dense network)
- NOT for production dependency until row-level verification (see honest caveats)
- NOT as evidence that sub-4B models reach frontier — HLE of 8.9 says otherwise

## Core Process / Workflow
1. **Pull the artifact set**: BF16 final RL+OPD post-trained checkpoint; SFT/Midtrain/Base companion checkpoints; GGUF, MLX, GPTQ builds; a DSpark draft model for speculative decoding; and the open training datasets.
2. **Run with standard engines**: plain LlamaForCausalLM — "no custom kernels, no model-code fork" — vLLM 0.21+, SGLang 0.5.16+, Transformers 5.6+; GGUF/MLX for llama.cpp/Ollama/LM Studio/Apple Silicon.
3. **Choose SGLang for tool-calling**: the model emits XML-style tool calls; SGLang's built-in minicpm5 parser converts to OpenAI-compatible tool_calls natively — standard tool-calling clients work without a custom shim.
4. **Enable speculative decoding** with the DSpark draft model via SGLang for cheaper reasoning passes (accelerates decoding, outputs unchanged).
5. **Verify before trusting the card's internal rows** — see honest caveats.
6. **Track the hosted-API signal**: no hosted API listed MiniCPM5-2B yet; self-host experiment, not a dependency.

## Key Evidence
- **Spec sheet**: 2,516,756,480 total params (1,981,982,720 non-embedding); dense Llama-family, 42 layers, hidden 2048, GQA 16 query heads / 2 KV heads, vocab 130,560, BF16.
- **Context**: released config = 131,072 tokens (128K) — the WAIC-touted 512K figure does NOT appear in the checkpoint; design against 131,072 until an extension recipe ships.
- **Independent composite**: Artificial Analysis Intelligence Index v4.2 (Sept 4) scores MiniCPM5-2B at **15 — first among 47 open-weights models under 4B** and first of that size class on a harder methodology (40% weight on private held-out tests; adds AA-Briefcase agentic eval + GDP.pdf long-context reasoning). AA's verbosity tracking: MiniCPM5-2B emitted **57M output tokens across index tasks, the most concise in its class** against a 72M median.
- **Three-stage pipeline**: base → midtrain → post-training SFT on 400B tokens of deep-thinking data + specialized RL teachers + On-Policy Distillation merging sixteen RL expert models (five agentic) back into one small dense network; RL+OPD credited with +10.96 average gain on reasoning/general tasks and +6.96 on agentic tasks (internal deltas).
- **Chip story**: nine chips with day-zero support via FlagOS (Huawei Ascend, NVIDIA, domestic accelerators).

## Honest Caveats (the discipline)
- **Vendor-selected comparison set**: the card's 53.9 internal average normalizes each axis so the best comparison model scores 100 — a relative score, not an absolute one.
- **Most striking rows are OpenBMB's own reproductions** — SWE-bench Verified at 46.4 and the 53.9 internal average remain unverified by anyone outside the lab; the AA v4.2 composite confirms placement, not row-level figures.
- **Context-window discrepancy**: 512K claimed at WAIC vs 131,072 in the shipped config.
- Treat as "a very promising hypothesis you can run locally tonight" until rows re-run.

## Pairs with
`k2-horizon-open-science-fleet-2026`, `open-weight-agentic-model-wave-august-2026`, `blind-launch-stealth-model-playbook`, `open-source-ai-hosting-economics`, `sovereign-ai-open-weight-cascade-2026`

## A-Tech Alignment
- **Open source**: Apache-2.0 model AND open training datasets — the complete quiet-drop pattern: no announcement, no launch event, repo appears, changelog line follows.
- **Data privacy**: fully self-hostable on-device; no cloud dependency for edge agents.
- **Financial freedom**: sub-4B SOTA-class at $0 license on consumer hardware/phone NPU; the "quiet-drop" pattern (no marketing overhead) is itself a cost signal.
- **Practical**: the two-hours-to-verify discipline (re-run SWE-bench yourself before trusting the card) is the transferable rule.

*Sources: OpenBMB Hugging Face repo (Sept 6–7, 2026); Artificial Analysis Intelligence Index v4.2 (Sept 4, 2026); orcarouter.ai unpacked analysis, Sept 7, 2026.*