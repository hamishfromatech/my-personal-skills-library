---
name: home-ai-network-pair
description: NVIDIA PAIR — free open-source personal AI router pooling idle home PCs (RTX, DGX Spark, Apple M4+) into local inference clusters; mDNS discovery, mTLS pairing, live scheduler. (Established in cycle 18 — see original skill for architecture and routing mechanics.)
---

# Home AI Network: NVIDIA PAIR — Cycle-20 Refresh

*(Full framework established in cycle 18: proxy-layer interception, five live scheduling signals, no VRAM pooling/no sharding, the model-presence storage constraint (~20GB × N nodes), the 2× parallel-agentic speedup demo, Apache 2.0 source.)*

## Cycle-20 addendum (2026-09-06): IFA 2026 wave context — PAIR as the router in a full local stack

The IFA 2026 announcements (NVIDIA blog, Sept 3, 2026) position PAIR inside a complete local-AI push, each piece mapping to a library skill:

- **One-click local setup in the big three agents:** Hermes Agent (Nous Research), OpenClaw, and Perplexity Portable Computer all ship simplified local-model setup on Windows (auto-detect GPU → select model → run via llama.cpp with NVIDIA optimizations) — "that friction is disappearing."
- **Inference speedups:** llama.cpp up to **1.9×** on RTX 5090 (kernel optimizations, enhanced speculative decoding, faster prefill); vLLM 1.2× (RTX PRO 6000) and 1.4× (dual DGX Spark).
- **The local model wave:** Nemotron 3.5 Lightning (30B), GLM-5.3-Flash (multimodal MoE), Qwen3.8-Flash-Next + Qwen3.8-27B (local agentic/coding), Meta Muse Glimmer (30B coding/agentic), DeepSeek v4 Flash (284B MoE / 13B active — runs on 2× DGX Spark), plus open video models (LTX 2.5, MiniMax-H3 + FastH3 7× distilled).
- **RTX Spark PCs in October:** 1-petaflop Blackwell GPU, up to 128GB unified memory, 20-core Grace CPU — new Lenovo/Acer designs; the Windows Agent framework enables background agents under OS-level control.
- **PAIR's role:** "more than half of U.S. households have two or more PCs, and much of that computing power sits idle" — PAIR routes independent requests across them (mDNS discovery → approved pairing → mTLS → five-signal scheduler), with the demo household framing (~165 teraFLOPS idle across a family's machines).

**The strategic read for the library:** NVIDIA is building the *consumer local-datacenter* layer — silicon (Spark), software (PAIR), agent distribution (Hermes/OpenClaw/Perplexity one-click), and model supply (the open-weight wave) — as an alternative to cloud subscription economics ("the most powerful, secure, and cost-effective AI cluster is the one you already own"). For A-Tech content, the household framing is the accessible entry: PAIR is free, works with existing Ollama/LM Studio endpoints, and its constraint (model stored per node) is a budgeting question, not a blocker.

## Standing pairs

`sovereign-desk-cost-freedom-narrative`, `home-lab` adjacent skills, `privacy-preserving-local-ai`, `local-first-web-architecture-2026`, `hybrid-compute-privacy-gate` (the gating tier), `plugclaw-tee-consumer-agent-hardware` (the isolation tier), `open-source-ai-hosting-economics` (the cloud counterpart).

## A-Tech alignment (retained)

- **Open source:** Apache 2.0 routing layer over open inference engines — the local stack is fully openable.
- **Privacy:** prompts, files, and agent context stay on the home network; zero-internet operation after model download.
- **Financial freedom:** "free tokens" from idle hardware vs metered cloud — the owned-silicon asset story made concrete by a free router.
- **Practical:** setup sequence unchanged (install per machine → approve pairing → enable engine → download model per node → point agent at PAIR); add the per-node model-storage budget to any recommendation.