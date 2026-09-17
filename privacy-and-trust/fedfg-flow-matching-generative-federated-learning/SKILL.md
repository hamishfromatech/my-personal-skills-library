---
name: fedfg-flow-matching-generative-federated-learning
description: Unified federated learning framework using flow-matching generators to jointly preserve client privacy (via split learning with private extractors) and resist poisoning attacks (via synthetic sample verification). Use when needing simultaneous privacy + robustness in FL, defending against gradient inversion AND model poisoning, or exploring generative approaches to FL security. First framework to use flow-matching as generative backbone for FL.
---

# FedFG: Flow-Matching Generative Federated Learning

## Overview
FedFG (Flow-matching Generative Federated learning) is the first FL framework to use flow-matching generators as a unified backbone for both client-side privacy protection and server-side robustness. It decouples each client model into a private feature extractor (kept local) and a public classifier (shared), with a flow-matching generator that replaces the extractor during server communication — protecting private features while enabling server-side verification via synthetic samples.

## When to Use
- Needing simultaneous privacy preservation AND poisoning attack resistance in FL
- Defending against gradient inversion attacks (DLG, IG, iDLG) in federated settings
- Building robust FL systems that resist sign flipping, inner product manipulation, and multi-round model poisoning attacks
- Exploring generative approaches (flow-matching vs GAN) to FL security
- Working in non-IID federated environments where traditional robust aggregators fail
- Needing server-side update verification without auxiliary clean datasets

## NOT For
- Centralized training (FL-specific design)
- Environments with strict communication constraints (generator training adds overhead)
- Pure privacy without robustness needs (simpler split learning suffices)

## Core Process / Workflow

### 1. Architecture — Model Decoupling

Each client model is split into three components:

| Component | Visibility | Role |
|---|---|---|
| **Private Feature Extractor (E_i)** | Local only — never shared | Transforms raw inputs → features |
| **Flow-Matching Generator (FG_i)** | Shared with server | Approximates feature distribution; replaces extractor during communication |
| **Public Classifier (C_i)** | Shared with server | Maps features → logits |

Only (FG_i, C_i) are uploaded. The extractor E_i never leaves the client, so gradient inversion cannot access raw data representations.

### 2. Client-Side Privacy Mechanism

1. Train E_i and C_i locally on private data (classification loss)
2. Train FG_i to approximate conditional feature distribution (flow-matching loss)
3. Upload only (FG_i, C_i) to server — extractor stays local

**Flow-matching generator** trained via conditional ODE:
- h(t) transported from noise h(0) → real feature h(1) via vector field v_θ(h,t,y)
- Label-conditioned: y injected via learnable embedding
- Loss: MSE between predicted and target flow: ||v_θ(h_t, t, y) - (h_1 - h_0)||²

### 3. Server-Side Robustness Mechanism

Server uses the aggregated global generator FG_g to produce **synthetic feature probes** (class-conditional), then evaluates all client classifiers on these probes:

**Two detection signals:**
1. **Outlier score** (o_i): Average Hellinger distance between client i's predictive distribution and all others on synthetic probes
2. **Accuracy score** (α_i): Fraction of synthetic probes classified correctly by client i's classifier

**Filtering:**
- Hampel rule (median + MAD-based threshold) detects outlier clients
- Accuracy threshold filters low-quality updates
- Combined by set union → remaining benign set B_r

**Aggregation:**
- Accuracy-aware reweighting: w_i = α_i / Σ(α_j for j in B_r)
- Only benign clients aggregated

### 4. Why Flow-Matching Over GANs

| Property | Flow-Matching (FedFG) | GAN (FedCG, GAN-Filter) |
|---|---|---|
| Training stability | Simulation-free, ODE-based | Mode collapse, instability |
| Distribution smoothness | Smoother conditional manifold | Concentrated, more invertible |
| Privacy (gradient inversion) | Lower PSNR/SSIM (harder to invert) | Higher PSNR/SSIM (easier to invert) |
| Non-IID robustness | Maintains accuracy | Can fail under severe heterogeneity |
| Server-side verification | Synthetic probes in plaintext | Often needs clean data |

### 5. Performance Highlights

**Privacy (Gradient Inversion):**
- DLG and IG attacks produce near-noise reconstructions
- Lower PSNR and SSIM than FedCG (GAN-based) due to smoother learned distribution

**Robustness (MNIST, IID):**
| Attack | ε=10% | ε=20% | ε=30% |
|---|---|---|---|
| Sign Flipping | 0.949 | 0.949 | 0.947 |
| Inner Product | 0.950 | 0.949 | 0.946 |
| MPAF | 0.956 | 0.956 | 0.955 |

FedFG maintains >94% accuracy even at 30% malicious clients across all attack types, outperforming Median, TrimmedMean, FoolsGold, DPR-PPFL, and GAN-Filter.

**Non-IID (Dirichlet β=0.5):**
- FedFG maintains ~90% accuracy across attacks
- Baselines collapse (Median → 0.10 under IPM, Geometric → 0.10 across all)

**Extreme non-IID (β=0.2):**
- FedFG sustains high accuracy with small fluctuations
- Several baselines collapse to low accuracy or remain unstable

### 6. Convergence Guarantee

First-order stationarity guarantee under nonconvex objectives. Bound decomposes into:
1. Initialization gap
2. Noise & heterogeneity (standard FL)
3. Objective drift (evolving aggregation weights)
4. Verification failure penalty
5. Probe estimation error

With constant step size η = Θ(1/√(QR)), recovers canonical O(1/√(QR)) rate.

## References
- See [references/fedfg-evidence.md](references/fedfg-evidence.md) for attack details, experimental tables, and convergence proof summary.