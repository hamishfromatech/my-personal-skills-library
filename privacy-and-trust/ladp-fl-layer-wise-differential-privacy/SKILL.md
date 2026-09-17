---
name: ladp-fl-layer-wise-differential-privacy
description: Layer-wise adaptive noise injection for federated learning that reduces noise by 46% while improving accuracy by 103% via KL-divergence-based privacy estimation. Use when designing DP-FL systems needing better privacy-utility tradeoffs, or when uniform noise injection degrades model performance excessively. NOT for non-neural-network FL or when computational overhead of layer analysis is prohibitive.
---

# LaDP-FL Layer-wise Differential Privacy

## Overview
LaDP-FL introduces layer-wise adaptive noise injection for federated learning, using KL divergence between local and global model distributions to estimate per-layer privacy sensitivity. Achieves 46% noise reduction and 103% accuracy improvement over SOTA methods.

## When to Use
- Designing differentially private federated learning with better privacy-utility tradeoffs
- When uniform noise injection degrades model performance excessively
- Needing layer-aware privacy protection in neural network FL
- Building adaptive DP mechanisms that respect layer importance
- NOT for non-neural-network FL (linear models, tree-based)
- NOT when computational overhead of per-layer analysis is prohibitive

## Core Process / Workflow

### 1. Layer Selection (Importance Based)
- Calculate L₂ norm of each layer's weights
- Layers exceeding threshold R = crucial for model prediction
- Small-weight layers pruned from noise injection (low private info, preserve utility)
- Key insight: adversaries rely on predictive capability; layers with small weights offer little attack value

### 2. Privacy Estimation (KL-Divergence-Based)
For each selected layer j in client i:
- Extract parameter matrices from local (w^i_j) and global (w^g_j) models
- Flatten and normalize using softmax
- Calculate P_i,j = min(KL(w^i_j || w^g_j), B)
- B = clipping boundary preventing noise from approaching zero

**Interpretation**:
- Small KL → layer closely matches global → HIGHER privacy risk (needs MORE noise)
- Large KL → significant deviation from global → LOWER privacy risk (needs LESS noise)

### 3. Adaptive Noise Injection
σ_i,j = c_i × Δf_i,j / (ε × P_i,j)

Inverse relationship:
- Low P_i,j (similar to global) → Larger noise (stronger privacy)
- High P_i,j (dissimilar) → Smaller noise (preserves useful information)

Theorem 2 (Sensitivity bound): Δf_i,j ≤ 2ηEG_c (learning rate × epochs × gradient clip bound)

### 4. Ensure DP Guarantee (Theorem 3)
Parameter c_i must satisfy:
- c_i ≥ [formula based on ε, δ, B] for given privacy budget
- Two cases based on δ value relative to threshold

### 5. Verify Convergence (Theorem 4)
Under bounded noise assumption (||n_i,j|| ≤ N_c):
- Learning rate: 2JN_c/G_c > η > 2JN_c/(G_c - μ-L) × L × G_c/μ
- Converges to neighborhood of optimal solution

### 6. Compare Performance
| Metric | LaDP-FL vs Full DP | vs Time-Varying DP | vs AdapLDP |
|--------|-------------------|-------------------|-----------|
| Accuracy | +236% | +145% | +6.1% |
| Noise reduction | 69% | 51% | 68% |
| ε accumulation | -63% | — | — |

## References
- See [references/li-2026-ladp-fl.md](references/li-2026-ladp-fl.md) for full paper details.