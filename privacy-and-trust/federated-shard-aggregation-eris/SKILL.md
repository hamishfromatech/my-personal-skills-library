---
name: ERIS Federated Shard Aggregation
description: Applies the ERIS framework for privacy-preserving federated learning that achieves FedAvg-equivalent utility, information-theoretic privacy amplification, and scalable distributed aggregation without heavy cryptography or utility-degrading perturbations. Use when designing federated learning systems, privacy-preserving ML pipelines, or distributed AI training infrastructure. NOT for centralized ML (different threat model).
---

# ERIS Federated Shard Aggregation

## Overview

Based on Fenoglio et al. (arXiv:2602.08617v2, May 2026). First FL framework that simultaneously provides FedAvg-equivalent utility, information-theoretic privacy amplification, and scalable distributed aggregation.

## Key Innovation

**Federated Shard Aggregation (FSA):** Partitions each client update into $A$ disjoint shards sent to different aggregators. Because masks are disjoint and complete, reassembly produces the exact centralized update.

**Distributed Shifted Compression (DSC):** Optional layer that reduces transmitted and exposed coordinates, further enhancing scalability and privacy.

## Privacy Guarantee

$$I(D_k; \{v_{t,k,(a)}\}_{t=0}^{T-1}) \leq \frac{nT}{A} \cdot p \cdot C_{max}$$

Where:
- $n$ = model size
- $T$ = rounds
- $A$ = number of aggregators (shards)
- $p$ = compression retention probability (DSC)
- $C_{max}$ = per-coordinate mutual information bound

**Key insight:** Privacy amplification through $1/A$ factor. Each aggregator observes only $1/A$ of each client update.

## Utility Guarantee

$$\frac{1}{T} \sum_{t=0}^{T-1} \|\nabla f(x_t)\|^2 \leq \frac{2\Phi_0}{\lambda T} + \frac{3\beta\Gamma_2}{(1+\omega)L\lambda}$$

- No term growing with $T$
- Governed by gradient estimator variance $\Gamma_2$
- $\Gamma_2 = 0$ with SVRG/SAGA (variance-reduced estimators)

## Collusion Extension

$$I(D_k; \{v_{t,k}^{col}\}_{t=1}^T) \leq \frac{nT}{A} \cdot p \cdot A_c \cdot C_{max}$$

Where $A_c$ = number of colluding aggregators. **Remains substantially lower than centralized FL** even under collusion.

## ERIS vs. Alternatives

| Property | FedAvg | DP | Secure Agg. | ERIS |
|----------|--------|-----|-------------|------|
| Utility | ✓ | ✗ (noise) | ~ | ✓ (exact) |
| Privacy | ✗ | ✓ | ~ | ✓ (info-theoretic) |
| Scalability | ✗ (bottleneck) | ~ | ✗ (crypto) | ✓ (distributed) |
| Communication | Full model | Full + noise | Full + crypto | Sharded (1/A) |

## Communication Speedup

With $A=50$ aggregators: **105× speedup** over FedAvg, **55× over SoteriaFL.**

## Attack Resistance

| Attack | FedAvg | ERIS (A=2) | ERIS (A=50) |
|--------|--------|------------|-------------|
| DLG | Perfect reconstruction | Distorted | Unrecognizable |
| iDLG | Perfect | Distorted | Unrecognizable |
| ROG | High quality | Moderate | Near-random |
| GGL | High quality | Moderate | Near-random |

**Even with A=2 (weakest config), reconstructions are highly distorted and no longer preserve meaningful features.**

## A-Tech Applications

### For A-Coder
- **Federated code intelligence** — train code models across distributed repositories with privacy
- **Adaptive privacy** — tune $A$ and $p$ for desired privacy-utility trade-off
- **No heavy crypto** — FSA requires no homomorphic encryption or TEEs

### For Be Practical
- **Privacy-preserving FL curriculum** — ERIS as teaching example for FL design
- **Information-theoretic privacy** — formal privacy without noise injection
- **Distributed systems design** — sharded aggregation architecture

### For Builder's Club
- **Open-source FL framework** — ERIS implementation available (MIT, PyTorch, Flower)
- **Benchmark suite** — reproducible privacy-utility evaluation
- **Practical FL deployment** — 105× communication speedup enables real-world use

## Cross-References
- `adaptive-verifiable-federated-learning-2026` — adaptive DP-FL
- `ehds-federated-learning-compliance-framework` — EU compliance
- `federated-consent-architecture-agent-systems` — consent architecture
- `privacy-first-ai-pipeline-defense` — privacy-first pipeline

## Source
Fenoglio, D. et al. (May 2026). "ERIS: Enhancing Privacy and Scalability in Federated Learning via Federated Shard Aggregation." arXiv:2602.08617v2.
