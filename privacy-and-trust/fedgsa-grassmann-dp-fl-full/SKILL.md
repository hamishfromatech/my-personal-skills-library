---
name: fedgsa-grassmann-dp-fl-full
description: Use when aggregating differentially private federated LoRA updates, fixing basis misalignment in federated fine-tuning, choosing a DP federated LoRA method for heterogeneous non-IID clients, or deciding between single-factor freeze alternating and subspace aggregation approaches.
---

# FedGSA: Grassmann Subspace Aggregation for DP Federated LoRA

**Source:** Zheng, Hu, Zhang, Cheng & Shen, "FedGSA: Geometry-Consistent Subspace Aggregation for Differentially Private Federated LoRA," arXiv:2608.03267 (Aug 2026). A geometry-level fix to the DP federated LoRA aggregation problem — the successor lineage to the library's existing `fedgsa-grassmann-manifold-dp-federated-lora` skill; this SKILL.md carries the full mechanism extraction.

## The Problem

DP federated LoRA has two structural failure modes: (1) **aggregation mismatch** — independently perturbing and averaging the two low-rank matrices A and B is not equivalent to averaging the update ΔW = BA, and cross-client subspace misalignment distorts the global update; (2) **quadratic noise amplification** — independently perturbing both factors creates a noise×noise cross-term in the product. Prior fixes (FFA-LoRA freeze-one-factor, RoLoRA/LA-LoRA alternating, FedSVD reparameterization) either restrict capacity or add overhead.

## The Mechanism

FedGSA reframes the aggregation target geometrically:
1. **Single-factor private optimization** — freeze A client-side, run DP-SGD only on B. This alone eliminates the quadratic cross-noise term.
2. **Basis-invariant representation** — each privatized client update M = B·A is a low-rank matrix whose *column space* is the meaningful object. Extract the dominant r-dim subspace via truncated SVD → projection matrix P = U·Uᵀ, which is invariant to sign/scale/basis changes.
3. **Grassmann aggregation** — server averages projection matrices (weighted), eigen-decomposes, takes the leading r eigenvectors as the consensus output subspace. Averaging projectors cannot suffer the sign-cancellation failure that plagues Euclidean factor averaging.
4. **Preconditioned reconstruction** — project the weighted matrix update onto the consensus subspace, thin-SVD, rebuild global LoRA factors inside the aggregated subspace. This filters heterogeneous drift and DP noise components before reconstruction.

**Privacy:** server-side subspace operations are post-processing on already-privatized updates — no additional privacy loss beyond client-side DP-SGD. Formal convergence proof under standard assumptions.

## Results

- GLUE (RoBERTa-base, non-IID Dirichlet α=0.5): **+2.17% average accuracy over the strongest baseline (FedSVD) at ε=6; +2.27% at ε=3**; first on all five tasks in both regimes.
- Ablation isolation: Grassmann aggregation alone adds +13.06 points when A is frozen (77.36 vs 64.30 with Euclidean aggregation); the interaction is the point — single-factor DP optimization suppresses multiplicative noise, Grassmann aggregation recovers the expressiveness that freezing would otherwise lose.
- Without DP: +2.02% over FedSVD (88.81% avg) — the geometry helps even in non-private FL.
- Generation (GPT-2, E2E NLG): best on 4/5 metrics under DP — generalizes beyond classification.
- Convergence speed: reaches 70% MNLI accuracy by round 14 vs round 96 for the Euclidean-ablation variant.

## Decision Guide

| Situation | Use |
|---|---|
| DP federated LoRA, heterogeneous non-IID clients | FedGSA (subspace aggregation + frozen A) |
| DP federated LoRA, tighter compute, moderate heterogeneity | LA-LoRA (alternating updates + low-pass filter) |
| Severe non-IID skew (α ≤ 0.3) | FedGSA's advantage widens (2.16–11.82% over baselines) |
| Non-private FL | Either; geometry still helps (+2%) |

## Cross-Links

- `fedgsa-grassmann-manifold-dp-federated-lora` — the original skill entry this cycle's sweep surfaced (README entry 2026-08-26); this SKILL.md is the completed full-depth version
- `la-lora-privacy-preserving-federated-large-models` — the alternating-update sibling
- `fedgsa-grassmann-manifold-dp-federated-lora` vs `dp-fedadamw-dpfl-large-model-optimizer` — optimizer-side vs aggregation-side axes of the DP-FL stack
- `adaptive-verifiable-federated-learning-2026` — verifiability axis

## A-Tech Fit

- **Open source:** pure math on top of standard LoRA/DP-SGD — implementable in HuggingFace PEFT + Opacus without new infrastructure.
- **Privacy:** strengthens the privacy-preserving federated fine-tuning recommendation for multi-institution clients (healthcare, education corpora — the paper's own domain).
- **Financial freedom:** aggregation quality at the same privacy budget means fewer rounds and less GPU spend for federated deployments.
- **Practical:** the decision table above is directly usable when advising clients on DP-FL method selection.