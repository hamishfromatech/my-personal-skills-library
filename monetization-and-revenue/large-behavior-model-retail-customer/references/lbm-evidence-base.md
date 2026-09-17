# Large Behavior Model Evidence Base

## Primary Source

**Modecrua, W., Pachtrachai, K., & Kraisingkorn, T. (2026). "Large Behavior Model: A Promptable Digital Twin of the Retail Customer." arXiv:2607.06993v2. Amity Research and Application Center (ARAC), Amity AI Holdings.**

### Dataset
- Primary cohort: 1,500 customers, 159,667 train / 40,665 test transactions, 38,516-product catalogue.
- Cross-domain: DMBGN benchmark (SIGKDD'21) — 12,480 Lazada voucher-redemption cases.
- Cross-dataset: Online Shoppers (UCI, Turkey), Tmall (IJCAI-15), Shopee (SEA), Twin-2K-500 (Marketing Science 2025).

### Architecture
- Base models: Qwen3-8B (PoC), Qwen3.5-9B (cross-domain), 4-bit quantisation via Unsloth.
- Personalisation: LoRA (rank 8, α 16, dropout 0, all attention+MLP modules, ~0.42% trainable).
  - Segment-level adapters (lifestage / NielsenIQ persona) for full coverage including cold-start.
  - User-level adapters (where ≥300 behavioural examples).
- RAG: 1,024-dim embeddings → ChromaDB → top-3 SKUs via cosine similarity.
- Training: SFTTrainer (TRL) with Unsloth; GRPO via TRL GRPOTrainer.

### Four-Stage Pipeline
1. **Data collection**: retail transaction logs.
2. **Continual pre-training (CPT)**: verbalised behavioural data → behaviour model.
3. **Supervised fine-tuning (SFT)**: structured decision formats + behavioural reasoning.
4. **GRPO RL**: verifiable YES/NO rewards; NO-class upweighted ×2; Jaccard reward for B3; ~300-500 steps, 8 rollouts.

### In-Domain Results (classic-B, n=5,091)
| Model | B1 | B2 | B3 | B4 | AVG |
|---|---|---|---|---|---|
| LBM (base + SFT) | 84.2 | 97.6 | 71.1 | 76.0 | 82.2 |
| GPT-5.5 | 84.8 | 75.5 | 57.9 | 72.8 | 72.7 |

### Hard Negatives (B-hard v4, n=2,490)
| Model | B1 | B2 | B3 | B4 | AVG |
|---|---|---|---|---|---|
| LBM (GRPO v5 + SFT) | 98.6 | 98.9 | 81.6 | 52.0 | 82.8 |
| GPT-5.5 | 94.5 | 51.4 | 70.9 | 51.6 | 67.1 |

### Cross-Domain (Lazada voucher redemption, zero-shot)
- LBM (trained only on Retailer data): 0.772 AUC (above GPT-5.5).
- LBM (fine-tuned on Lazada): 0.827 AUC (beats prior SOTA).

### Cross-Dataset Transfer
- Online Shoppers (UCI): 0.849 AUC (log-prob); close to GBDT ceiling 0.933.
- Tmall (IJCAI-15): 0.53-0.59 AUC zero-shot → 0.619 with enriched prompts (below SOTA 0.703).
- Shopee (SEA): GPT-5.5 dominates (0.825 vs 0.65) — when signal is in platform IDs, prompt-based generalisation fails.

### Ablation Findings
- CPT is the primary driver of behavioural generalisation.
- SFT primarily improves response formatting.
- Retrieval must be present at both training and inference (not in CPT corpus).
- GRPO improves reliance on explicit behavioural evidence over generic LM priors.
- Reasoning supervision is task-dependent (improves transfer, may reduce fixed-format performance).
- Data packing affects stability (improper multi-turn packing → shortcut learning).

### Central Insight
> "Behavioral simulation is primarily an information problem rather than a model-size problem."

The B-hard experiments show that providing explicit behavioural evidence dramatically improves decision quality, while stronger frontier models without such evidence remain limited by generic priors. The bottleneck is prompt signal, not weights.

## Supporting Literature

### Behavioural Language Models
- Khandelwal, A., et al. (2024). Adobe LCBM — Large content and behaviour models. arXiv:2309.00359.
- Lu, Y., et al. (2026). OPeRA — Can LLM agents simulate multi-turn human behaviour? arXiv:2503.20749.
- Zhang, Y., et al. (2026). Shop-R1 — Rewarding LLMs to simulate shopping behaviour via RL. arXiv:2507.17842.
- Wang, Z., et al. (2025). Customer-R1 — Personalised simulation via RL-based LLM agent. arXiv:2510.07230.

### Persona and Simulation
- Park, J.S., et al. (2024). Generative agent simulations of 1,000 people. arXiv:2411.10109.
- Argyle, L.P., et al. (2023). Out of one, many — LM simulation of human samples. Political Analysis, 31(3).
- Venkit, P.N., et al. (2026). SCOPE — socially-grounded persona framework. arXiv:2601.07110.
- NVIDIA (2026). Nemotron-personas — multilingual synthetic persona datasets.

### Sim2Real Gaps
- Zhou, X., et al. (2026). Mind the sim2real gap in user simulation. arXiv:2603.11245.
- Peng, T., et al. (2026). Digital twins as funhouse mirrors — five key distortions. arXiv:2509.19088.
- Gu, S. (2026). Long context, less focus — scaling gap in LLMs. arXiv:2602.15028.

### Classical Recommenders (contrast)
- Zhai, J., et al. (2024). HSTU — trillion-parameter sequential transducers. arXiv:2402.17152.
- Rajput, S., et al. (2023). TIGER — recommender systems with generative retrieval. NeurIPS.

## Cross-References

- `customer-digital-twin-neuromarketing` — neural+social CDT; the EEG/sentiment complement to this behavioural CDT.
- `ai-consumer-behavior-brand-relationship` — 5-cluster/5-pillar model; LBM can simulate the behavioural pillar.
- `stochastic-reasoning-revshare-llm-model` — outcome-based LLM monetisation; LBM is the simulation engine.
- `ai-agent-memory-architecture` — memory architecture; LBM's Shopping-DNA is a persistent memory.
- `digital-twin-memory-architecture` — cognitive-science digital-twin memory; LBM is the behavioural analogue.

## A-Tech Value Alignment

- **Open-source AI**: Qwen3-8B/3.5-9B are open-weight; Unsloth, TRL, ChromaDB are open-source; LoRA is efficient.
- **Data privacy**: behavioural profiles can be built from on-device transaction histories; no cloud data transmission required for inference.
- **Financial freedom**: LBM enables SMEs to build customer twins from their own transaction data without frontier-model API costs.
- **Practical implementation**: 4-bit quantisation runs on a single H20 96GB; LoRA makes per-segment adaptation cheap; the four-stage pipeline is reproducible.