---
name: hades-selective-feature-encryption-federated-learning
description: Applies the HADES selective feature encryption framework for privacy-preserving federated learning to design hybrid encrypted-plaintext FL systems that encrypt only the most privacy-sensitive features while processing the rest in plaintext. Use when building privacy-preserving FL with reduced computational overhead, designing hybrid encrypted-plaintext model fusion, protecting against gradient inversion attacks with selective encryption, or needing FL privacy without full homomorphic encryption costs.
---

# HADES: Selective Feature Encryption for Privacy-Preserving Federated Learning

## Overview
A hybrid FL framework that selectively encrypts only the most privacy-sensitive features (identified via PCA) using multiparty homomorphic encryption, while training a plaintext network on the remaining features — achieving privacy protection comparable to fully encrypted FL at a fraction of the computational cost through a novel fusion mechanism.

## When to Use
- Building privacy-preserving FL with computational efficiency constraints
- Designing hybrid encrypted-plaintext model architectures for federated training
- Protecting against gradient inversion (iDLG) attacks without full encryption overhead
- Needing FL privacy when full homomorphic encryption is too expensive (hours to days)
- Collaborative training across organizations with mixed privacy requirements per feature
- Reducing reconstruction attack success while maintaining model utility
- NOT for: applications requiring all features to be encrypted; real-time inference (training-focused); non-FL centralized learning

## Core Process / Workflow

### The HADES Dual-Model Architecture

```
Input Features (F)
        ↓
PCA Feature Selection
    ├── F_HE (top-i principal components → encrypted)
    └── F_P  (remaining features → plaintext)
        ↓                    ↓
   W_HE (encrypted)      W_P (plaintext)
   MHE training          Standard training
        ↓                    ↓
   z_HE (encrypted logits)  z_P (plaintext logits)
        ↓                    ↓
        └─── FUSION ─────────┘
              ↓
        Fused prediction
```

### Five-Step Implementation

1. **Feature Selection via PCA**
   - Apply PCA to identify most privacy-sensitive features (top-i principal components)
   - Split: F_HE (encrypted subset) and F_P (plaintext subset)
   - Selection criteria: variance explained, ciphertext capacity, desired model utility
   - HADES is agnostic to feature-selection method — PCA is illustrative

2. **Dual Network Initialization**
   - Encrypted sub-network W_HE: input dimension |F_HE|, trained entirely under MHE
   - Plaintext sub-network W_P: input dimension |F_P|, standard training
   - Both networks never decrypted during training (W_HE); W_P trains independently

3. **Federated Training Loop**
   - Forward pass: z_HE from encrypted branch, z_P from plaintext branch
   - Fusion: fused_HE = α·z_HE + (1-α)·HE(z_P) — weighted ciphertext-plaintext combination
   - Separate losses: L_HE (fused, encrypted) and L_P (plaintext only)
   - Backward pass: gradients computed for both branches
   - Server aggregation: ΔW_HE and ΔW_P aggregated separately
   - Weight update: both branches updated; W_HE bootstrapped after each update

4. **Score-Level Fusion**
   - Fusion weight α ∈ [0,1] controls relative contribution (default: 0.5)
   - Intermediate fusion possible: concatenate/add hidden representations
   - Plaintext branch provides "free" capacity (no HE cost) while preserving utility

5. **Privacy Preservation**
   - Encrypted parameters never decrypted during training
   - PCA transformation adds layer: adversary needs inverse PCA (encrypted) + MHE decryption
   - Prevents reconstruction attacks even with gradient access

### Privacy Analysis: Reconstruction Attack Mitigation

**Attack model:** Improved Deep Leakage from Gradients (iDLG) — strongest at batch size B=1

| Encrypted Features | MNIST SSIM | MNIST LPIPS | SVHN SSIM | SVHN LPIPS |
|-------------------|------------|-------------|-----------|------------|
| 0 (no encryption) | 1.00 | 0.00 | 1.00 | 0.00 |
| 16 | 0.48 | 0.17 | 0.53 | 0.16 |
| 64 | 0.26 | 0.29 | 0.22 | 0.26 |
| 256 | 0.11 | 0.49 | 0.04 | 0.53 |
| All-1 | 0.04 | 0.64 | 0.01 | 0.87 |

**Key finding:** Encrypting only 256 of 784 MNIST features (33%) reduces SSIM from 1.00 to 0.11 — approaching the near-worst-case (0.04) without encrypting all features.

### Utility Preservation

| Dataset | CE (no encryption) | HADES (FL, approx) | Reduction in encrypted params |
|---------|-------------------|--------------------|-------------------------------|
| BCD | 94.71% | 97.08% | 2x |
| MNIST | 95.00% | 94.99% | 4x |
| SVHN | 63.1% | 70.4% | 16x |

**Key finding:** HADES matches or exceeds vanilla FL accuracy while encrypting 2-16x fewer parameters. SVHN improvement (63.1% → 70.4%) shows hybrid design recovers signal when input space is high-dimensional.

### Computational Efficiency

- Training time scales linearly with |F_HE| (number of encrypted features)
- Up to 28% runtime reduction by reducing encrypted feature dimension
- Minimum ~7% reduction even in worst-case configurations
- Single-ciphertext mini-batching: entire mini-batch encoded in one ciphertext
- General packing scheme eliminates redundant rotations by considering entire network architecture

### Comparison to Existing Approaches

| Approach | Privacy Level | Computational Cost | Limitation |
|----------|--------------|-------------------|------------|
| Secure aggregation | Server can't see updates | Low | Client-side model still vulnerable |
| Full HE (POSEIDON) | Complete | Very high (hours to days) | Impractical for most FL |
| HADES (selective HE) | Strong (selective) | Moderate (proportional to F_HE) | First hybrid encrypted-plaintext FL |
| DP (differential privacy) | Statistical | Low | Accuracy degradation |

**HADES is the first system to perform fully encrypted model training while incorporating a fusion mechanism between encrypted and plaintext model components.**

### Decision Framework: Selecting |F_HE|

| Priority | Recommended |F_HE| | Trade-off |
|----------|---------------------|-----------|
| Maximum privacy | |F| - 1 (all but one) | Highest computational cost |
| Strong privacy, moderate cost | Top 30-40% of features | SSIM < 0.15, good utility |
| Balanced | Top 20% of features | SSIM ~0.2-0.3, best speed |
| Minimum viable privacy | Top 10% of features | Some reconstruction possible |
| Speed critical | Top 5% of features | Minimal privacy protection |

### Packing Strategy

**Alternating packing scheme:**
- Odd layers: column-major packing
- Even layers: row-major packing
- Padding to power-of-2 dimensions
- Multiplication area μ calculated globally across network

**Operation counts per n-layer network (per client):**
| | Rotations | MultPT | MultCT |
|---|-----------|--------|--------|
| Forward | 2n-1 | 2n | n |
| Backward | 2n-1 | 2n+1 | 2n+1 |
| Total | 4n-2 | 4n+1 | 3n+1 |

## References
- See [references/evidence-base.md](references/evidence-base.md) for full evidence: system and threat model, PCA feature selection details, MHE background, fusion mechanism mathematics, training algorithm, experimental setup, reconstruction attack results (iDLG), utility preservation across datasets, ablation against PCA-only baseline, scalability analysis, runtime performance, packing strategy details, A-Tech applications.