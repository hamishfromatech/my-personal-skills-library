# Daily Research Report — 2026-08-29

**Date:** August 29, 2026 (Auckland time)
**Research Focus:** Neuro-marketing, behavioral psychology, AI revenue, privacy-first AI, developer experience, open-source business model trends

---

## Executive Summary

Today's research cycle identified **three high-impact novel developments** resulting in **three new skills** created across two categories (ai-agents-and-workflows and developer-experience-and-flow). The most significant findings cluster around a convergent theme: **the open-weight agentic AI ecosystem is maturing from model releases to training methodology**.

The three new skills:
1. **IBM Granite 4.2** (Apache 2.0, August 25, 2026) — First major open-weight reasoning model with real-sandbox agentic RL, not synthetic trajectories
2. **Lego-RL** (Apache 2.0, arXiv:2608.17393) — First harness-native RL framework proving that coding agent gains are harness-specific
3. **SPADE** (MIT, arXiv:2608.19197) — Self-play framework where one model generates adaptive training environments and learns from them

Additional research across neuromarketing, behavioral psychology, open-source AI monetization, privacy-first AI, and developer experience reinforced existing skills without meeting the novelty threshold for new skills.

---

## Skills Created (3 new skills)

### 1. IBM Granite 4.2: Real-Sandbox Agentic RL Under Apache 2.0
**Category:** ai-agents-and-workflows
**Source:** IBM Research (August 25, 2026), Data Today, Winzheng

**Key Findings:**
- Family of dense reasoning LLMs (3B, 8B, 30B) with native chain-of-thought and switchable thinking modes
- 8B and 30B trained with multi-stage agentic RL on **real, not simulated** environments (OpenHands for SWE, real Linux shell for terminal, real web search for multi-hop)
- Asynchronous GRPO with leave-one-out baseline (no value network)
- Three-way thinking switch: full, low-effort, non-thinking on same weights
- OpenAI-compatible tool calling out of the box via vLLM/SGLang
- Apache 2.0 on everything — weights, instruct checkpoints, quantized variants
- 30B achieves 57% SWE-bench Verified (vendor-published, pending independent verification)
- First Granite release built from the ground up for agents

**Key Insight:** IBM chose the costlier path of real-sandbox RL over synthetic trajectory post-training. This establishes a concrete technical reference for open-source agentic training: use real environments with outcome-based rewards, not model-generated "fake execution" data. The publication of training design details (OpenHands harness, hidden test rewards, 64-round shell interaction) offers a reproducible path for the community.

**A-Tech Alignment:** Open-source (Apache 2.0), data privacy (self-hostable, air-gapped), financial freedom (3B on laptops, 8B on single GPU), practical implementation (vLLM/SGLang recipes, thinking mode control)

### 2. Lego-RL: Harness-Native Reinforcement Learning for Coding Agents
**Category:** developer-experience-and-flow
**Source:** Du et al., arXiv:2608.17393, August 2026, Apache 2.0

**Key Findings:**
- First framework for training coding agents with online RL inside their native harnesses (Claude Code, OpenHands, OpenCode)
- Headline: RL gains are **harness-specific** — training in the harness the agent will run in produces gains that don't fully transfer
- Qwen3.5-35B-A3B trained for 3 epochs (126 steps): +6.4 OpenHands, +5.8 Claude Code, +9.4 OpenCode on SWE-bench Verified
- Beats both next base generation (Qwen3.6-35B-A3B) and KAT-Coder-V2.5-Dev (post-trained from it) in all three harnesses
- KAT-Coder evidence: +3.4 in its authors' harness (Claude Code), but +0.6 OpenCode and -0.4 OpenHands — harness specificity confirmed
- Architecture: native agents via thin adapters → Harbor sandbox → verifier reward → verl policy update
- PPO, GRPO, GSPO on FSDP/VeOmni/Megatron; synchronous or asynchronous
- Live dashboard with per-trial trajectory inspection

**Key Insight:** The harness is part of the policy. A model's benchmark score in one harness does not predict its score in another. This extends a pattern visible across the DevEx skill cluster: tool architecture affects behavior (Xu et al.), multi-agent coordination is task-shaped (Destefanis & Aste), prior AI exposure moderates adoption (Agarwal et al.), documentation behavior is harness-dependent (Gao & Chen). Lego-RL adds: training context = deployment context.

**A-Tech Alignment:** Open-source (Apache 2.0), data privacy (self-hosted training), financial freedom (train on own GPUs), practical implementation (working code, Claude Code integration, live dashboard)

### 3. SPADE: Self-Play in Adaptive Synthetic Executable Environments
**Category:** ai-agents-and-workflows
**Source:** Liu et al., arXiv:2608.19197, August 2026, MIT license

**Key Findings:**
- Self-play framework where one model learns both roles: environment designer (writes executable Python with reset()/step()) and reasoning agent (solves them)
- Designer trained with **hint-based regret** — gap between agent's return with and without privileged hint
- High regret = environment at capability frontier; zero regret = too easy or too hard
- Creates adaptive curriculum that moves with the learner, avoiding fixed-environment saturation
- Applied to Qwen3 at 4B, 8B, 30B-A3B in cognitive games and multi-turn tool use
- Outperforms fixed-environment baselines (GPT-5.5 corpus, RLVE) on held-out math, science, code, procedural reasoning past saturation point
- Model-agnostic: Qwen3, Qwen3.5, GPT-OSS, Nemotron, GLM supported
- Slime/SGLang + Megatron-LM + Ray integration; Tinker alternative backend

**Key Insight:** Environment generation, not model scaling, is the bottleneck for RL post-training. SPADE makes environment design a learnable component, generating an adaptive curriculum that never saturates. This complements IBM Granite 4.2's real-sandbox approach: SPADE for scalable curriculum generation, Granite 4.2 for real-world task distribution. Both address the same core problem from different angles.

**A-Tech Alignment:** Open-source (MIT), data privacy (self-hosted, no external API), financial freedom (generate environments without human task creation), practical implementation (working code, Slime/SGLang integration)

---

## Cross-Domain Synthesis

### Theme 1: From Model Releases to Training Methodology

The August 2026 open-weight wave (covered by existing `open-weight-agentic-model-wave-august-2026`) focused on model releases: GLM-5.3, Muse Glimmer, Qwen3.8, DeepSeek-V4, Nemotron. The current cycle reveals a shift to **training methodology**:

- **Granite 4.2**: Real-sandbox agentic RL training recipe (documented, reproducible)
- **Lego-RL**: Harness-native RL (the deployment harness IS the training harness)
- **SPADE**: Self-play environment generation (the model creates its own curriculum)
- **Ornith** (existing skill): Self-proposed task RL (model generates its own tasks)

This represents a maturation: the community has enough open-weight models; the next frontier is how to train them better for agentic workloads.

### Theme 2: The Harness Is the Policy

Lego-RL's harness-specificity finding connects to a broader pattern across the DevEx skill cluster:

- Tool architecture affects agent behavior (Xu et al., `tool-architecture-coding-agent-behavior`)
- Multi-agent coordination is task-shaped (`multi-agent-coding-coordination-network`)
- Prior AI exposure moderates adoption (`agentic-coding-prior-ai-exposure-moderation`)
- Documentation behavior is harness-dependent (`agent-friendly-documentation-behavior`)
- Agent experience requires harness-aware design (`agent-experience-ax-devex-evolution`)

Lego-RL adds the training dimension: if the harness shapes behavior at deployment, it also shapes learning during training. Train in the harness you will deploy in.

### Theme 3: Real Environments vs Synthetic Environments

A spectrum is emerging:

| Approach | Environment | Adaptivity | Real-World Transfer |
|----------|-----------|------------|-------------------|
| Synthetic trajectories (old) | Model-generated fake execution | None | Low |
| Fixed real sandboxes (Granite 4.2) | Real repos, shells, web | None (fixed pool) | High |
| Fixed synthetic pools (RLVR) | Hand-built | None | Medium |
| Self-proposed tasks (Ornith) | Model generates | Medium | Medium |
| **Adaptive synthetic (SPADE)** | Model generates + validates | **High** | Medium |
| **Harness-native (Lego-RL)** | Real repos in target harness | None (fixed pool) | **Highest** |

No single approach dominates. SPADE solves the saturation problem. Granite 4.2 and Lego-RL solve the transfer problem. The future likely combines both: adaptive curriculum for broad capability + real-sandbox training for deployment-specific performance.

### Theme 4: Open-Source AI Monetization Continues (Incremental)

Today's research reinforced existing skills:
- **Malpani Give-Away/Keep Matrix** — covered by `give-away-keep-matrix-oss-ai`
- **Alibaba Qwen3.8-Max revenue-share** (Reuters, August 7, 2026) — covered by `open-source-ai-revenue-share-trend`
- **Moonshot K3 revenue-share negotiations** with Microsoft, Amazon, Google — incremental update to existing skills
- **Mozilla State of Open Source AI** (33% usage, 4% revenue; 50x inference cost drop) — covered by `open-source-ai-harness-frontier-2026`
- **Mistral $16M→$400M ARR** — covered by `open-source-ai-hosting-economics`

### Theme 5: Privacy-Preserving FL Research (Incremental)

Multiple FL/DP papers identified, all reinforcing existing skills:
- **HEAD-FL** (Seyedi et al., ePrint 2026/1376) — reinforces `head-fl-adaptive-dp-homomorphic-aggregation`
- **AdaDP-FedSec** (Zhou & Yuan, Nature Scientific Reports, August 2026) — already covered by existing `adadp-fedsec-adaptive-dp-secure-aggregation`
- **FLiPD** (Chandran et al., ePrint 2026/324) — already covered by `flipd-majority-collusion-resistant-secure-aggregation`
- **DP-FedAdamW** (Liu et al., CVPR 2026) — already covered by `dp-fedadamw-dpfl-large-model-optimizer`
- **DDP-SA** (Wei et al., arXiv:2604.07125) — reinforces `ddp-sa-distributed-dp-secure-aggregation`
- **FedGSA** (Zheng et al., arXiv:2608.03267) — already covered by `fedgsa-grassmann-manifold-dp-federated-lora`
- **DP-LAC** (arXiv:2605.10272) — already covered by `dp-lac-lightweight-adaptive-clipping`
- **DPTrainer** (JetBrains, August 27, 2026) — already covered by `dptrainer-drop-in-differential-privacy`

### Theme 6: Neuromarketing Research (Incremental)

- **AI-Neuromarketing CXM Framework** (Topcugil & Hiziroglu, Future Business Journal, August 14, 2026) — already covered by `ai-neuromarketing-cxm-integration-framework`
- **Promotional-Preventive Framing ERP** (Wang et al., Scientific Reports, July 2026) — already covered by `promotional-preventive-framing-erp-neuromarketing`
- **Concept2Brain** (Santos-Mayo et al., Nature Communications, July 2026) — already covered by `concept2brain-predictive-neural-response-model`
- **Systematic Literature Review** (Delen & Hiziroğlu, 2026) — incremental; field mapping already covered by `neuromarketing-lda-five-pillar-taxonomy`
- Various Zenodo neuromarketing papers (Sathiya & Jeyanthi; Ravi & Seetharaman; Koduru & Chandra) — incremental; multimodal EEG+CV already covered by `multimodal-eeg-cv-purchase-intent-prediction`
- **Predictive Neuromarketing Bayesian Framework** (Mavroudis et al., 2026) — already covered by `predictive-neuromarketing-bayesian-framework`

### Theme 7: Behavioral Psychology (No Novel Frameworks)

No novel behavioral psychology frameworks identified in today's research cycle that met the novelty threshold for new skills.

---

## Incremental Updates (Not New Skills)

### AI Agents and Workflows
- Kimi K2.6 open-source release (Moonshot AI) — incremental; covered by existing agentic model skills
- TielCoder 4-bit quant — community quantization, incremental
- Qwen3.8-Flash architecture (GDN + QSA + GR + N-gram embedding + Muon optimizer) — incremental

### Developer Experience
- No novel developer experience frameworks beyond Lego-RL (which was created as a new skill)

### Privacy and Trust
- Multiple FL/DP papers — all reinforce existing privacy-and-trust skill cluster

### Marketing and Content
- Multiple neuromarketing papers — all reinforce existing marketing-and-content skill cluster

### Monetization and Revenue
- Revenue-share model evolution (Moonshot K3, Alibaba Qwen3.8-Max) — covered by existing skills

---

## Research Methodology

Today's research used web search across four primary domains:
1. Open-source AI model releases and agentic coding (August 2026)
2. Neuromarketing, behavioral psychology, and AI consumer behavior research
3. Open-source AI business models and monetization trends
4. Privacy-preserving AI, federated learning, and differential privacy

Searches returned 40+ web results across 4 queries. Results were evaluated for:
- **Novelty**: Genuinely new development not covered by existing ~322 skills
- **Open Source Focus**: Alignment with A-Tech's open-source ethos
- **Audience Interest**: Relevance to A-Tech's developer/tech audience
- **Depth Potential**: Ability to sustain well-researched skill content
- **Practical Implementation**: Actionable frameworks, not just theoretical observations

Three developments met all novelty criteria and were synthesized into Agent Skills with proper YAML frontmatter, evidence-base references, and A-Tech alignment analysis. Additional findings were classified as incremental updates to existing skills.

---

## Statistics

| Metric | Value |
|--------|-------|
| Skills created (total today) | 3 |
| Reference files created | 3 |
| Categories touched | 2 (ai-agents-and-workflows: 2, developer-experience-and-flow: 1) |
| Total SKILL.md lines | ~850 |
| Total reference lines | ~650 |
| Research sources evaluated | 40+ web results across 4 queries |
| Skills directory total | ~325 skills across 9 categories |

---

## A-Tech Values Alignment

The 3 new skills align with A-Tech Corporation's core values:

### IBM Granite 4.2
- **Open-source AI**: Apache 2.0 on everything; freely usable for commercial deployment
- **Data privacy**: Self-hostable weights enable air-gapped, SCIF, VPC deployment
- **Financial freedom**: 3B on laptops, 8B on single GPU, eliminates API costs
- **Practical implementation**: vLLM/SGLang recipes, thinking mode control, documented training

### Lego-RL
- **Open-source AI**: Apache 2.0; works with open-weight models (Qwen3.5, GPT-OSS, Nemotron, GLM)
- **Data privacy**: Self-hosted training; code never leaves infrastructure
- **Financial freedom**: Train on own GPUs; no per-token API costs during training
- **Practical implementation**: Working code, live dashboard, Claude Code integration

### SPADE
- **Open-source AI**: MIT license; model-agnostic; works with open-weight models
- **Data privacy**: Self-hosted training; no external API calls during environment generation
- **Financial freedom**: Generate training environments without human task creation
- **Practical implementation**: Working code, Slime/SGLang integration, Tinker support

---

## Report Date
2026-08-29