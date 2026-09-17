---
name: fedgsa-grassmann-manifold-dp-federated-lora
description: Aggregate differentially private federated LoRA updates using geometry-consistent subspace methods on the Grassmann manifold. Use when federated fine-tuning with LoRA under differential privacy produces aggregation mismatch, when factor-wise Euclidean averaging distorts low-rank updates, or when you need basis-invariant aggregation for noisy heterogeneous client updates.
---

# FedGSA: Geometry-Consistent Subspace Aggregation for DP Federated LoRA

## Overview

FedGSA is a geometry-consistent aggregation framework for differentially private federated LoRA that represents each privatized client update as a basis-invariant subspace on the Grassmann manifold. Instead of factor-wise Euclidean averaging (which is sensitive to sign, scale, and basis choices), FedGSA extracts dominant update directions, aggregates them geometrically, and reconstructs global LoRA factors within the consensus subspace. It improves average accuracy by 2.17% (ε=6) and 2.27% (ε=3) over the strongest baseline across GLUE tasks.

Source: Zheng, Hu, Zhang, Cheng & Shen (arXiv:2608.03267, 2026).

## When to Use

- Federated LoRA fine-tuning with differential privacy (DP-SGD)
- When factor-wise Euclidean averaging causes aggregation mismatch and quadratic noise amplification
- When different clients encode identical update directions using different bases (rotational ambiguity)
- Cross-client subspace misalignment under non-IID data and DP noise
- Privacy-preserving federated NLU or NLG with LoRA adapters
- NOT for: centralized fine-tuning, full-parameter FL, or settings without privacy constraints

## Core Problem

Three challenges in DP federated LoRA:
1. **Aggregation mismatch**: averaging local LoRA factors A and B separately is not equivalent to averaging the updates BA
2. **Quadratic noise amplification**: DP noise in A and B interacts through the matrix product, amplifying effective noise
3. **Basis ambiguity**: for any invertible Q, BA = (BQ)(Q⁻¹A) — same update, different factors — making Euclidean averaging basis-dependent

## The Grassmann Manifold Solution

### From LoRA Updates to Grassmann Points

1. Client optimizes only B (A fixed) using DP-SGD with per-sample clipping and Gaussian noise
2. Form the privatized update matrix M_k = B_k × A (rank ≤ r)
3. Extract dominant column space via truncated SVD: M_k = U_k Σ_k V_k^T
4. Represent as projection matrix: P_k = U_k U_k^T (a point on Gr(r, d_out))

### Geometry-Consistent Subspace Aggregation

1. Compute weighted average of projection matrices: P̄ = Σ w_k P_k
2. Eigenvalue decomposition of P̄ reveals dominant eigenspace
3. Take leading r eigenvectors as global output-direction subspace U_g

### Preconditioned Reconstruction

1. Project aggregated update onto consensus subspace: M̃ = U_g^T M̄
2. Thin SVD of reduced matrix: M̃ = Ũ Σ V^T
3. Reconstruct factors: A^(t) = V^T, B^(t) = U_g Ũ Σ
4. Result: B^(t) A^(t) = U_g Ũ Σ V^T (rank-r reconstruction constrained to global Grassmann subspace)

## Privacy Guarantee

- Server-side operations (subspace extraction, geometric aggregation, reconstruction) operate on already-privatized client updates
- These are post-processing operations → no additional privacy loss (Theorem 1)
- Privacy guarantee determined solely by client-side DP-SGD: (ε, δ)-DP with Gaussian noise variance σ² = O(q²D·m·qK·T·log(2/δ)·log(2TqK/δ) / (ε²·K))

## Key Results

### Natural Language Understanding (GLUE, RoBERTa-base, DP-SGD)

| Privacy Budget | Best Baseline (FedSVD) | FedGSA | Improvement |
|----------------|----------------------|-------|-------------|
| ε=6 | 80.13% | 82.30% | +2.17% |
| ε=3 | 78.27% | 80.54% | +2.27% |

- FedGSA ranks first across all five evaluation tasks in both settings
- Without DP: 88.81% average, +2.02% over FedSVD

### Natural Language Generation (GPT-2, E2E NLG)

- Best on 4 of 5 metrics: 30.32 BLEU, 54.56 METEOR, 53.45 ROUGE-L, 2.40 CIDEr

### Heterogeneity Robustness

- Advantage grows with more severe non-IID (smaller Dirichlet α)
- At α=0.1 (severe): 2.16%–11.82% improvement over strongest baseline

### Ablation

- Single-factor DP-SGD (freeze A, update B) avoids multiplicative cross-noise
- Grassmann aggregation + frozen A: +13.06 points vs Euclidean aggregation + frozen A
- Grassmann aggregation alone (both factors updated): +1.12 points

## Design Principles

1. **Single-factor private optimization**: Freeze A, apply DP-SGD only to B → eliminates quadratic noise
2. **Basis-invariant aggregation**: Grassmann manifold → immune to rotational ambiguity
3. **Subspace preconditioning**: Project onto consensus subspace before reconstruction → filters heterogeneous drift
4. **Complementary roles**: noise suppression (single-factor) + directional consensus (Grassmann)

## A-Tech Alignment

- **Open-source**: reproducible, standard libraries (PyTorch)
- **Data privacy**: formal (ε, δ)-DP with no trusted server requirement; server-side operations are post-processing
- **Financial freedom**: enables privacy-preserving collaborative fine-tuning for small organizations
- **Practical implementation**: works with RoBERTa-base and GPT-2, standard GLUE benchmarks, open DP-SGD framework