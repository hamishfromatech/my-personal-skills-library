---
name: ddp-sa-distributed-dp-secure-aggregation
description: Scalable FL framework combining local differential privacy with additive secret sharing across intermediate servers for end-to-end (ε,δ)-DP. Use when designing scalable privacy-preserving FL with both statistical and cryptographic protection, or needing multi-server architectures for bandwidth optimization. NOT for centralized learning or when single-server FL is sufficient.
---

# DDP-SA Distributed DP Secure Aggregation

## Overview
DDP-SA combines client-side local differential privacy (Laplace noise) with full-threshold additive secret sharing across intermediate servers, achieving end-to-end (ε,δ)-DP with zero additional privacy loss from the cryptographic layer. Scales linearly with participants.

## When to Use
- Designing scalable privacy-preserving federated learning systems
- Requiring both LDP (statistical) and ASS (cryptographic) privacy protection
- Building multi-server FL architectures for bandwidth optimization
- Needing formal end-to-end DP guarantees with cryptographic security
- NOT for centralized machine learning
- NOT when single-server FL without intermediate servers is sufficient

## Core Process / Workflow

### 1. Implement Two-Stage Protection
**Stage 1 — Client-side LDP**:
- Each client computes local gradients
- Clips per-sample gradients with ℓ₁ threshold Δ
- Adds calibrated Laplace noise with scale Δ/ε
- Averages noisy gradients locally

**Stage 2 — Additive Secret Sharing**:
- Encode noisy gradients using fixed precision
- Decompose into m additive secret shares
- Distribute one share to each intermediate server
- No single share reveals information about the secret

### 2. Deploy Multi-Server Architecture
- n clients + m intermediate servers + 1 parameter server
- Each intermediate server receives one share per client
- Servers aggregate shares locally, forward combined result
- Parameter server reconstructs only the aggregated noisy gradient
- Communication scales linearly with m (m << n)

### 3. Guarantee End-to-End Privacy
- **Theorem 1**: DDP-SA satisfies (ε,δ)-DP end-to-end (LDP preserved via post-processing invariance)
- **Corollary 1**: ASS introduces ZERO additional privacy loss
- **Theorem 2**: Multi-round composition: basic (Tε, Tδ) or advanced (ε√(2T ln(1/δ')) + Tε(e^ε-1))

### 4. Manage Privacy Budget Across Rounds
- **Uniform allocation**: ε_per_round = ε_total / T
- **Adaptive allocation**: Exponential decay, more budget early when gradients larger
- Advanced composition preferred for long-running systems

### 5. Understand Overhead Distribution
- LDP computation: 92.12% (gradient clipping dominates, O(d))
- MPC computation: 7.88% (share transmission, O(d·m))
- Communication: ASS adds factor of m overhead vs plaintext

### 6. Verify Defense Against Attacks
- Membership inference: LDP noise masks record contributions ✓
- Property inference: Perturbed gradients hide patterns ✓
- Data/label inference: Secure aggregation prevents individual access ✓
- Class representative: Combined LDP + ASS prevents reconstruction ✓

## References
- See [references/wei-2026-ddp-sa.md](references/wei-2026-ddp-sa.md) for full paper details.
- **2026-09 update:** the static clipping threshold is now superseded by round-wise, layer-wise median-based adaptive clipping ("DDP-SA-adaptive", arXiv:2608.15153v1, same group). Client-side only — encoding, secret sharing, aggregation and the privacy analysis are unchanged. Reported: 4× stronger ε at equal utility (ε≈0.1 vs 0.4 at R²=0.99), −98.7% test loss, −19.2% training time, −6.8% communication rounds vs static DDP-SA on a federated linear-regression benchmark. Caveats: regression-only benchmark, IID data, Laplace mechanism, no non-IID or deep-network evidence yet. See [ddp-sa-addendum-2026-09.md](ddp-sa-addendum-2026-09.md).