# IBM Granite 4.2: Evidence Base

## Release Details

- **Date**: August 25, 2026
- **License**: Apache 2.0 (all sizes, all variants)
- **Sizes**: 3B, 8B, 30B (dense decoder-only transformers)
- **Architecture**: GQA, RoPE (θ=10M), SwiGLU MLP, RMSNorm, bfloat16
- **Context**: 131,072 base → 512K extended (phase 5)
- **Speculative decoding**: Yes (all sizes)
- **Availability**: Hugging Face collection + GitHub repository live

## Architecture Table

| Component | 3B Dense | 8B Dense | 30B Dense |
|-----------|----------|----------|----------|
| Layers | 40 | 40 | 64 |
| Attention heads | 40 | 32 | 32 |
| KV heads | 8 | 8 | 8 |
| MLP hidden size | 8,192 | 12,800 | 32,768 |
| Sequence length | 131,072 | 131,072 | 131,072 |
| Parameters | 3B | 8B | 30B |

## Pre-training

- ~15 trillion tokens across five phases
- Phase 1-2: Foundational pre-training (broad web-scale data)
- Phase 3-4: Mid-training (progressively higher-quality data annealing)
- Phase 5: Long-context training (128K → 512K extension)
- Learning rate schedule shifts at each phase

## SFT Details

- ~7.2M samples, ~100B tokens, ~65B trainable
- Data normalized to OpenAI Chat format
- Quality control: GPT-OSS-120B and Gemma 4 as LLM judges
- Deduplication: SHA-256 over (tools, messages) combination
- Removed: low-scoring samples, hallucinated info, invalid tool interactions, undefined function calls

### SFT Data Mixture

**Agentic (31.6%):**
| Category | % of agentic |
|----------|-------------|
| Software engineering | 69.0% |
| Tool calling | 12.1% |
| Terminal use | 8.0% |
| Math | 3.5% |
| Search | 0.8% |
| Action | 0.2% |

**Non-agentic (68.4%):**
| Category | % of non-agentic |
|----------|-----------------|
| Instruction following | 18.8% |
| Coding | 18.8% |
| Math | 14.6% |
| Multilingual | 7.0% |
| Science | 5.4% |
| Reasoning | 3.0% |
| Safety | 0.8% |

### Second SFT (30B only)
- Focus: agentic coding
- Upsamples SWE and coding data
- ~16% replay data from original corpus
- One additional epoch at lr=3.0e-6

## RL Pipeline

### Algorithm: Asynchronous GRPO
- Group Relative Policy Optimization (no value network)
- Leave-one-out baseline: each response judged against mean reward of other samples for same prompt
- Generation workers sample responses, drop into shared buffer
- Trainer pulls batch, takes optimizer step, streams updated parameters back
- KV cache reused across policy versions (single trajectory can span two adjacent policy versions)
- Truncated importance sampling clamps train-vs-generation log-prob ratio
- RLVR stage: 256 prompts × 16 responses = 4,096-example batch

### Pipeline Stages

**Foundational RL (all sizes):**
1. **RLVR** — math (boxed-answer, Lean proving), competitive coding (hidden-test sandbox), science MCQA, instruction following, single-step tool calling, reasoning puzzles with abstention
2. **Skill boosters** — separate GRPO runs: code, science, instruction following

**Agentic RL (8B and 30B only):**
3. **SWE** — real code repositories via OpenHands; reward = hidden test suite pass/fail
4. **Terminal** — active Linux shell; up to 64 rounds of interaction
5. **Search** — real-time web search; LLM judge for multi-hop questions

**Alignment (all sizes):**
6. **RLHF** — GenRM + safety reward; KL penalty (highest in pipeline); reasoning-length penalty

### Capability Split
- 3B: Foundational RL + RLHF (5 stages) — no agentic block
- 8B: Full pipeline (7 stages) — agentic RL included
- 30B: Full pipeline (7 stages) + second SFT for agentic coding

## Benchmark Results (Vendor-Published)

| Benchmark | 30B | 8B | 3B |
|-----------|-----|-----|-----|
| SWE-Bench Verified | 57.00% | 47.67% | — |
| Terminal-Bench 2.1 | 29.24% | 20.56% | — |
| AIME25 | 89.17% | 86.67% | 78.33% |
| GPQA | 66.41% | 64.14% | — |
| RULER (128K) | 81.38% | 71.41% | 55.30% |

Note: These are IBM's own published results. Independent verification pending.

## SFT Data Sources (Agentic)

Agent scaffolds/harnesses used for trajectory generation:
- OpenHands
- OpenCode
- Terminus-2
- SWE-agent
- OpenResearcher
- MiniSWE
- OpenSeeker
- EnvScaler
- Gemini CLI
- Hermes
- Codex
- Goose

## Deployment Recipes

### vLLM Serving
```bash
vllm serve ibm-granite/granite-4.2-8b \
  --tool-call-parser hermes \
  --max-model-len 262144
```

### SGLang Serving
```bash
python -m sglang.launch_server \
  --model-path ibm-granite/granite-4.2-8b \
  --tool-call-parser hermes
```

### Thinking Mode Control
```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:8000/v1", api_key="dummy")

# Non-thinking (fast, minimal latency)
response = client.chat.completions.create(
    model="granite-4.2-8b",
    messages=[{"role": "user", "content": "What is 2+2?"}],
    extra_body={"chat_template_kwargs": {"thinking": False}}
)

# Low-effort thinking (balanced)
response = client.chat.completions.create(
    model="granite-4.2-8b",
    messages=[{"role": "user", "content": "Explain this function"}],
    extra_body={"chat_template_kwargs": {"thinking": "low"}}
)

# Full thinking (max accuracy, latency)
response = client.chat.completions.create(
    model="granite-4.2-8b",
    messages=[{"role": "user", "content": "Debug this multi-file issue"}],
    extra_body={"chat_template_kwargs": {"thinking": True}}
)
```

### Local Deployment (3B)
```bash
# Ollama
ollama run granite-4.2-3b

# LM Studio
# Download Q4_K_M GGUF from Hugging Face
```

### Quantized Deployment (30B)
```bash
# FP8 on vLLM
vllm serve ibm-granite/granite-4.2-30b \
  --quantization fp8 \
  --tool-call-parser hermes

# NVFP4 on vLLM
vllm serve ibm-granite/granite-4.2-30b \
  --quantization nvfp4 \
  --tool-call-parser hermes
```

## Training Infrastructure

- CoreWeave-hosted NVIDIA GB200 NVL72 clusters
- Single NVLink domain: 72 GPUs
- Node interconnect: 400 Gb/s InfiniBand

## Concurrent Release

Granite Speech 5.0 Turbo CTC series (470M parameters):
- Transcribes 3 hours of audio in ~1 second on single H200 GPU
- ~2x speed of previous Hugging Face Open ASR leaderboard leader

## Open Questions

1. **Non-coding transfer**: Agentic SFT is 69% SWE. Transfer to data analysis, customer support, financial workflows unverified.
2. **Reasoning-length penalty**: May hurt tasks needing long deliberation (complex math, multi-hop research).
3. **3B capability split**: 3B skips agentic RL — reasoning model, not agent-trained.
4. **Independent verification**: Benchmark numbers are vendor-published.

## Sources

- IBM Granite 4.2 Hugging Face collection
- IBM Granite 4.2 GitHub repository
- IBM Research blog (August 25, 2026)
- Data Today coverage (Lars Cornelissen, August 26, 2026): "IBM Granite 4.2 ships reasoning and agent RL under Apache 2.0"
- Winzheng coverage (August 26, 2026): "IBM Granite 4.2 Open-Sources Three Reasoning Models"