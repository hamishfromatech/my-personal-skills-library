---
name: large-behavior-model-retail-customer
description: Builds a promptable Large Behavior Model (LBM) that simulates individual retail customer decisions (purchase, basket completion, promotion response, survey answering) by grounding a language model in longitudinal transaction histories (Shopping-DNA profile) and retrieval-augmented product context, outperforming frontier GPT models on in-domain retail tasks and generalising zero-shot across retailers and domains. Use when simulating individual customer behaviour from transaction data, building a customer digital twin from behavioural (not neural) evidence, or when a single model must answer diverse retail questions via prompting. NOT for neural/EEG-based consumer models (use customer-digital-twin-neuromarketing), for recommendation-only systems (use token-based recommenders), or when predictive signal is encoded in platform-specific IDs rather than natural-language-expressible behaviour.
---

# Large Behavior Model: A Promptable Digital Twin of the Retail Customer

## Overview

The Large Behavior Model (LBM) treats customer simulation as language-conditioned decision-making: a single language model, grounded in a Shopping-DNA behavioural profile (derived from longitudinal transactions) and retrieval-augmented product context, simulates purchase prediction, basket completion, promotion response, and survey-style reasoning without task-specific architectures. Trained via continual pre-training + supervised fine-tuning + GRPO reinforcement learning, LBM outperforms GPT-5.5 on in-domain retail tasks (+9.5pp average) and generalises zero-shot across retailers and decision domains, demonstrating that behavioural evidence is a stronger foundation for personalised simulation than generic language-model priors.

## When to Use

- Simulating individual customer decisions from transaction history (not neural data)
- Building a customer digital twin that answers diverse retail questions via prompting (no per-task models)
- When a single model must handle purchase prediction, basket completion, promotion response, and survey reasoning
- Evaluating whether behavioural grounding outperforms foundation-model priors for consumer simulation
- Extending ai-consumer-behavior-brand-relationship or customer-digital-twin-neuromarketing with a behavioural (non-neural) twin
- NOT for EEG/neural consumer models — use customer-digital-twin-neuromarketing
- NOT for recommendation-only systems — use token-based sequential recommenders (HSTU, TIGER)
- NOT when predictive signal is in platform-specific IDs or high-dimensional engineered features (LBM degrades; use feature-engineered GBDT)

## Core Process / Workflow

### 1. Person–Environment decomposition

Model behaviour as B = f(P, E):
- **Person (P)**: persistent behavioural representation from longitudinal transactions (Shopping-DNA profile + LoRA adapter).
- **Environment (E)**: dynamic product context retrieved via RAG at inference time.

### 2. Shopping-DNA profile construction

From training-period transactions, derive:
- Demographic/lifestage information (when available)
- Price sensitivity
- Category preferences
- Repeat purchasing patterns
- Basket statistics
- Temporal shopping behaviour
- Channel usage
- Promotion affinity

No evaluation-period data is included (prevents future leakage).

### 3. Four-stage training pipeline

| Stage | Purpose | Key finding |
|---|---|---|
| 1. Data collection | Large-scale retail transaction histories | — |
| 2. Continual pre-training (CPT) | Adapt LM into a behaviour model | **Primary driver of generalisation** |
| 3. Supervised fine-tuning (SFT) | Teach decision formats | Primarily improves formatting, not generalisation |
| 4. GRPO reinforcement learning | Calibrate evidence-over-prior | Improves reliance on explicit behavioural evidence |

### 4. Retrieval-augmented context

- Embed all products in dense vector space.
- Retrieve top-3 semantically similar products via cosine similarity (ChromaDB).
- Inject retrieved products into the model prompt at decision time.
- **Critical**: retrieval must be present at both training AND inference (not in CPT corpus).

### 5. Evaluation tasks

| Task | Description |
|---|---|
| B1 Purchase (positive) | Actual purchased items with retrieved context |
| B2 Purchase (negative) | Hard negatives from same/different sections |
| B3 Basket completion | Predict next item given partial basket |
| B4 Promotion response | Evaluate response to discounts vs counterfactuals |

### 6. Performance

- **In-domain (classic-B, n=5,091)**: LBM 82.2% avg vs GPT-5.5 72.7% (+9.5pp).
- **Hard negatives (B-hard v4, n=2,490)**: LBM 82.8% vs GPT-5.5 67.1% (+15.7pp).
- **Cross-domain (Lazada voucher redemption, zero-shot)**: LBM 0.772 AUC vs GPT-5.5 (below).
- **Cross-domain (Lazada, fine-tuned)**: LBM 0.827 AUC (beats prior SOTA).

### 7. Key insight: evidence over prior

> "Behavioral simulation is primarily an information problem rather than a model-size problem."

The largest gains come from improving how customer behaviour is represented (Shopping-DNA, RAG, prompt construction), not from scaling the model. Providing explicit behavioural evidence dramatically improves decision quality; stronger frontier models without such evidence remain limited by generic priors.

## A-Tech Application Matrix

### A-Coder (developer tool)
- Developer behaviour twin: simulate developer tool adoption decisions from longitudinal usage telemetry (experimental).

### Be Practical (education)
- Curriculum: "Large Behavior Models for Customer Simulation" — CPT + SFT + GRPO pipeline.

### Builder's Club (community)
- Open-source LBM benchmark; community challenge for cross-domain transfer.

## References

- See [references/lbm-evidence-base.md](references/lbm-evidence-base.md) for the full evidence base.