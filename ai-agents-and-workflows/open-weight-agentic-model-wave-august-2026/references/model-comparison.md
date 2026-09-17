# Open-Weight Agentic Model Wave — Model Comparison

## Benchmark Comparison (August 2026)

### Coding Benchmarks

| Benchmark | GLM-5.3 | GLM-5.2 | Kimi K3 | DeepSeek-V4-Pro | Qwen3.8-Max | Opus 4.8 | Fable 5 | GPT-5.6 Sol | Muse Glimmer | Qwen3.8-27B |
|-----------|---------|---------|---------|-----------------|-------------|----------|---------|-------------|--------------|-------------|
| Terminal Bench 2.1 | 88.2 | 81.0 | 88.3 | 87.9 | 86.6 | 85.0 | 88.0 | 88.8 | 51.7 | 73.0 |
| Terminal Bench 3.0 | 28.3 | 4.6 | 17.4 | — | — | 21.1 | 33.7 | 34.6 | — | — |
| DeepSWE v1.1 | 66.9 | 46.2 | 67.5 | 62.7 | 56.6 | 58.0 | 69.7 | 72.7 | — | 42.2 |
| SWE-bench Pro | — | — | — | — | — | 53.4 | — | — | 51.2 | 61.7 |
| SWE-Marathon v1.1 | 42.5 | 19.4 | 48.1 | — | — | 48.8 | 33.1 | 42.5 | — | — |

### Cyber Benchmarks

| Benchmark | GLM-5.3 | GLM-5.2 | Kimi K3 | Qwen3.8-Max | Opus 4.8 | Fable 5 | GPT-5.6 Sol |
|-----------|---------|---------|---------|-------------|----------|---------|-------------|
| CyberGym | 84.5 | 77.2 | 80.0 | 78.5 | 78.1 | 83.8 | 83.6 |
| ExploitGym 2h | 105 | 29 | 36 | 14 | 80 | 181 | 216 |
| ExploitGym 6h | 130 | 39 | 70 | 26 | 120 | 247 | 293 |
| ExploitBench | 54.4 | 24.4 | 32.2 | 28.8 | 40.0 | 78.0 | 76.5 |

### Agentic Benchmarks

| Benchmark | GLM-5.3 | GLM-5.2 | Kimi K3 | DeepSeek-V4-Pro | Qwen3.8-Max | Opus 4.8 | Fable 5 | GPT-5.6 Sol |
|-----------|---------|---------|---------|-----------------|-------------|----------|---------|-------------|
| Toolathlon Verified | 73.0 | 59.9 | 76.5 | 74.1 | 72.5 | 76.2 | 74.7 | 74.9 |
| AutomationBench v1.0.6 | 48.2 | 26.2 | 46.7 | 31.8 | 39.8 | 41.0 | 46.2 | 45.8 |
| Agents' Last Exam (CLI) | 28.5 | 23.8 | 27.6 | 25.7 | 27.0 | 25.7 | 23.8 | 28.6 |
| HLE w/ Tools | 62.5 | 54.7 | 59.8 | 60.0 | 56.2 | 57.9 | 63.9 | 64.5 |

## Architecture Details

### Muse Glimmer (Meta)
- **Architecture:** 30B dense, Apache 2.0
- **Training:** Logit distillation from Muse Spark → mid-training on agent-heavy data → post-training (SFT + on-policy distillation + RL)
- **Quantization:** ~4-bit, shrinks to under 20GB (fits 24-32GB GPU envelope with KV cache + perception encoder + drafter)
- **Speculative Decoding:** DFlash drafter proposes token blocks; main model verifies in parallel; identical output quality
- **Deployment:** llama.cpp, MLX, ExecuTorch, Ollama, LM Studio, Unsloth, vLLM, SGLang
- **Reasoning:** Built-in chain-of-thought (assistant to=self turn), reasoning_strength parameter (xhigh/high/medium/low)
- **Context:** 128K native, default
- **Multimodal:** Interleaved text + images via perception encoder

### Qwen3.8-27B (Alibaba)
- **Architecture:** 27B dense multimodal, vision encoder included (language_only=false)
- **Total params:** 27,781,427,952 (BF16 with vision encoder)
- **Layers:** 64, hidden dim 5,120
- **Pattern:** 3 Gated DeltaNet blocks to 1 Gated Attention block
- **Training:** Multi-token prediction
- **Context:** 262,144 native, YaRN extension to 1,000,000
- **License:** Apache 2.0 (confirmed in license file, commercial use permitted)
- **FP8 variant:** Qwen3.8-27B-FP8 (fine-grained FP8 quantization, block size 128)
- **Hosted pricing (OpenRouter):** $0.45/M input tokens, $3.20/M output tokens

### GLM-5.3 (Z.ai)
- **Base model:** Same as GLM-5.2 (all gains from post-training)
- **Training framework:** slime (open-source, Megatron + SGLang)
- **Key innovation:** Synthesized environments for long-horizon RL training
- **Thinking effort:** low/high/max (disabled no longer supported)
- **API pricing:** Points-based quota, 50% off-peak discount (14:00-18:00 UTC+8 weekdays)
- **ZCode integration:** 98%+ cache hit rate, ~30% more effective tokens, Goal mode, Remote Control via WeChat/Feishu
- **Security Disclosure Ledger:** Public record of 2,436 findings across 269 OSS projects

### Nemotron 3.5 Lightning (NVIDIA)
- **Architecture:** 30B MoE, 3B active parameters
- **Training:** Speculative decoding (MTP baked in), harness-optimized training
- **Drafter models:** DSpark (for DGX Spark), DFlash (alternative)
- **Quantization:** NVFP4 checkpoint alongside BF16
- **Routing:** NeMo Switchyard routes tasks to optimal model
- **Deployment:** LM Studio, llama.cpp, Ollama, Unsloth, vLLM, SGLang
- **License:** OpenMDW-1.1 (weights, data, recipes released)
- **Benchmark:** Artificial Analysis Intelligence Index — Pareto frontier for accuracy-speed

### DeepSeek-V4-Pro-0813
- **Architecture:** MoE, 1.6T total, 49B active (reported, not confirmed at release)
- **Harness:** DeepSeek Harness v0.1 (MIT licensed, open alternative to Claude Code/Codex)
- **Context:** 1M tokens
- **Reasoning effort:** Selectable (low/high/max), replaces fixed thinking budget
- **API:** OpenAI Responses API with one-click Codex setup
- **Pricing (from Aug 16, 2026):**
  - Off-peak: $0.66/M input, $1.98/M output
  - Peak: $1.32/M input, $3.96/M output
  - Cache: $0.022/M off-peak, $0.044/M peak

## Security Disclosure Ledger (GLM-5.3)

| Metric | Value |
|--------|-------|
| Total findings tracked | 2,436 |
| Publicly disclosed | 53 |
| Under embargo | 2,383 |
| Critical & High severity | 1,097 |
| OSS projects affected | 269 |
| Years of impact | 45 (oldest from 1981) |
| Average vulnerability lifespan | 26.6 years |
| Severity: Critical | 107 |
| Severity: High | 990 |
| Severity: Medium | 1,286 |
| Severity: Low | 53 |

## Open-Closed Gap Assessment

The open-closed gap as of August 2026:
- **Coding:** GLM-5.3 competitive with closed models on Terminal Bench 2.1 (88.2 vs 88.8 GPT-5.6 Sol), but behind on Terminal Bench 3.0 (28.3 vs 34.6)
- **Cyber:** GLM-5.3 behind closed frontier (ExploitGym 6h: 130 vs 293 GPT-5.6 Sol, 247 Fable 5)
- **Agentic:** All models within 7 points on Toolathlon; GLM-5.3 at 73.0 vs 76.5 Kimi K3
- **Local deployment:** Muse Glimmer and Qwen3.8-27B enable on-device agents that were previously cloud-only

## Key Insight

> "As agent capability improves, much of the difficulty in scaling post-training moves from the model to the environment." — Z.ai (GLM-5.3 announcement)

The bottleneck for agentic model improvement has shifted from model architecture to environment engineering: creating executable, verifiable training environments that resemble real professional work.