---
name: eris-federated-shard-aggregation
description: Applies the ERIS framework for privacy-preserving federated learning that achieves FedAvg-equivalent utility, information-theoretic privacy amplification, and scalable distributed aggregation without heavy cryptography or utility-degrading perturbations. Use when designing federated learning systems that need simultaneous privacy, scalability, and accuracy for billion-parameter models including LLMs. NOT for centralized training or when cryptographic hardware (TEE) is available.
---

# ERIS: Federated Shard Aggregation

## Core Concept

ERIS (Fenoglio, Polverino, Quizi, Gjoreski, Dhasade & Langheinrich, arXiv:2602.08617, May 2026) is the first FL framework that simultaneously provides:
1. **FedAvg-equivalent utility** — the distributed protocol produces the exact same update as centralized aggregation
2. **Information-theoretic privacy amplification** — leakage bounded by observable fraction, decreasing with number of aggregators
3. **Scalable distributed aggregation** — removes the central server bottleneck

All without relying on heavy cryptography (no TEE, no homomorphic encryption) or utility-degrading perturbations (no mandatory differential privacy noise).

## The Federated Shard Aggregation (FSA) Mechanism

FSA is the core building block. Instead of sending a complete client update to a central server:

1. **Shard-wise partitioning**: Each client partitions its update `v_k` into `A` disjoint shards using binary masks `{m(a)}` that are disjoint and complete
2. **Distributed aggregation**: Each shard is sent to a different aggregator, which independently combines assigned shards
3. **Reassembly**: Aggregators broadcast updated shards; clients reassemble the full model

Because masks are disjoint and complete, `sum(shards) = original_update`, so FSA preserves the exact centralized FL update after reassembly. No single aggregator ever observes a full client update.

### Privacy Bound

Mutual information leakage over T rounds: `I(D_k; observations) ≤ n*T*p/A * C_max`

- `n` = model size, `T` = rounds, `p` = compression retention probability, `A` = number of aggregators
- FSA provides privacy amplification through the `1/A` factor
- DSC (Distributed Shifted Compression) further reduces leakage through `p`

### Collusion Extension

Under `A_c` colluding aggregators: `I ≤ n*T*p*A_c/A * C_max`
- Leakage grows linearly with coalition size
- Mitigation: increase shard count `A → A * A_max_c` or decrease retention `p → p/A_max_c`

## Optional DSC Integration

Distributed Shifted Compression (DSC) reduces transmitted and exposed coordinates:
- Each client maintains reference vector `s_k`, sends compressed shifted update
- Aggregator maintains shard-level reference, compensates for shift
- DSC amplifies both scalability and privacy by shrinking the observable subset

## Architecture Selection

| Requirement | Configuration | Privacy | Utility | Scalability |
|---|---|---|---|---|
| Trusted environment | FSA only (no DSC) | 1/A amplification | FedAvg-equivalent | Good |
| Bandwidth-constrained | FSA + DSC | 1/(A*p) amplification | Near-FedAvg | Excellent |
| Strong privacy needed | FSA + DSC + optional LDP | Strongest | Slight DP cost | Excellent |
| Collusion risk | Increase A, decrease p | Bounded by A_c/A | Preserved | Good |

## Convergence Guarantees

**Theorem (Utility of ERIS with DSC)**: Under standard smoothness and unbiased estimator assumptions, ERIS satisfies:
`1/T * sum(||∇f(x_t)||²) ≤ 2Φ₀/(λT) + 3βΓ₂(1+ω)Lλ`

- Asymptotic utility governed by gradient estimator variance Γ₂ (vanishes for SVRG/SAGA)
- No term growing with T (unlike SoteriaFL)
- Dimension-free non-private utility bound: `Õ(√(1+ω)/(m√K))`

## Empirical Results (vs 6 SOTA baselines)

| Method | Utility (CIFAR-10) | MIA Accuracy | Communication |
|---|---|---|---|
| FedAvg (no defense) | 34.86% | 68.46% | 100% |
| FedAvg-LDP | 19.00% | 63.35% | 100% |
| SoteriaFL | 17.18% | 58.83% | 5% |
| ERIS (FSA only) | 34.84% | 63.02% | 98% |
| ERIS (+DSC) | 34.68% | 60.48% | 0.6% |

Key findings:
- ERIS matches non-private FedAvg utility while significantly reducing privacy leakage
- ERIS achieves 105× speedup over FedAvg with A=50 aggregators
- Robust up to 70% aggregator dropout and 50% link failures
- Effective against DLG, iDLG, ROG, and GGL reconstruction attacks

## A-Tech Applications

- **A-Coder**: Federated code intelligence with adaptive privacy (ERIS + DSC for developer code patterns)
- **Be Practical**: ERIS curriculum for privacy-preserving FL architecture course
- **Builder's Club**: Open-source ERIS implementation benchmark (MIT license, PyTorch + Flower)

## Cross-References

- `adaptive-verifiable-federated-learning-2026` — ERIS complements adaptive DP; can combine FSA with round-adaptive perturbation
- `zk-proof-federated-learning-trust` — ERIS provides structural privacy; zkPoTs provide verifiable correctness; combinable
- `chain-federated-fine-tuning` — ERIS addresses scalability for billion-parameter models that CHAINFED targets for memory-constrained devices
- `sheld-fl-self-learning` — ERIS's distributed aggregation extends DP-FL frameworks with serverless architecture

## A-Tech Alignment

| Value | Alignment |
|---|---|
| Open-source AI | MIT-licensed implementation; PyTorch, Flower (Apache 2.0); all datasets public |
| Data privacy | Information-theoretic guarantees; no heavy cryptography; local data never leaves device |
| Financial freedom | Eliminates TEE hardware costs; runs on commodity infrastructure; 105× communication reduction |
| Practical implementation | Docker-deployable; reproducible 5-fold CV; robust to dropout and link failures |