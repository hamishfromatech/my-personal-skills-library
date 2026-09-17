# CHAINFED Evidence Base

## Primary Source

Wu, Y., Li, J., Tian, C., Tam, K., Guo, Z., & Li, L. (2026). *Beyond End-to-End: Dynamic Chain Optimization for Private LLM Adaptation on the Edge*. Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics (ACL 2026), pages 18419–18435.

## The Memory Wall — Quantified

Adapter-based federated fine-tuning memory breakdown (LLaMA2-7B):
- Model parameters: 24.71 GB (91.2%)
- Activations: 1.88 GB (6.9%)
- Adapter (params + gradients): 0.50 GB (1.9%)

LLaMA2-13B: 47.78 GB parameters (94.1%), 2.35 GB activations (4.6%), 0.66 GB adapter (1.3%).

The dominant memory consumer is the base model parameters themselves. Techniques that target activations or trainable parameters yield only marginal savings because those components are a negligible fraction of the total footprint. CHAINFED addresses the primary bottleneck directly by only loading the parameters of the active layer(s).

## Performance Degradation From Memory Constraints

BERT fine-tuning under practical (memory-constrained) vs. idealized (full participation) conditions:
- YELP-P: accuracy drops 8.5% (IID), 11.8% (non-IID)
- The decline stems directly from the memory wall, which systematically excludes low-end devices from training.

## Ablation Results (DistilBERT)

| Component removed | Avg accuracy drop |
|---|---|
| w/o DLCT (Dynamic Layer Co-Tuning) | −11.64% |
| w/o GPO (Globally Perceptive Optimization) | −13.73% |
| w/o FOAT (Function-Oriented Adaptive Tuning) | −6.52% |

All three components are indispensable; removing any one causes a substantial performance drop that persists as model complexity scales (RoBERTa: −12.12%, −11.06%, −5.47% respectively).

## FOAT Threshold Sensitivity (DistilBERT, AGNEWS)

| Threshold T | Accuracy (IID) | Speedup | Communication reduction |
|---|---|---|---|
| 1.0 (all layers) | baseline | ×1 | ×1 |
| 0.9 | +1.7–2.0% | ×1.86–1.94 | ×2.49–3.13 |
| 0.8 | +3.3–4.4% | ×1.97–2.02 | ×2.75–3.47 |

Fine-tuning all layers is suboptimal. Performance peaks at T = 0.8, surpassing the full-chain baseline by up to 4.43% on AGNEWS with 2.02× faster convergence and 3.47× lower communication overhead.

## Co-Tuning Window Size Trade-off (BERT, YELP-P non-IID)

| Q | Accuracy | Memory multiplier |
|---|---|---|
| 2 | ~84% | ×12.8 savings |
| 3 | ~84.5% | ×7.2 |
| 4 | ~85% | ×5.0 |
| 5 | ~85.5% | ×3.8 |
| 6 | ~86% (matches FFT) | ×3.1 |

Larger Q yields consistent performance gains but proportionally raises peak memory. In practice, set Q based on the device with the lowest capacity to ensure inclusive participation.

## Instruction-Tuning Results (LLaMA2-7B, LLaMA3.1-8B)

| Model | Q | MMLU | BBH | DROP | CRASS | Avg | Mem reduction |
|---|---|---|---|---|---|---|---|
| LLaMA2-7B Full Adapters | — | 43.68 | 31.72 | 32.19 | 44.95 | 38.14 | ×1 |
| LLaMA2-7B CHAINFED | 8 | 46.76 | 33.65 | 34.79 | 55.34 | 42.64 | ×3.23 |
| LLaMA3.1-8B Full Adapters | — | 61.15 | 61.28 | 55.72 | 74.63 | 63.20 | ×1 |
| LLaMA3.1-8B CHAINFED | 8 | 68.01 | 70.56 | 63.42 | 93.64 | 73.91 | ×3.45 |

On LLaMA3.1-8B, CHAINFED achieves a 10.71% average accuracy improvement alongside a 3.45× memory reduction. The CRASS counterfactual-reasoning benchmark shows the largest gain (+18.99%), suggesting chain optimization particularly benefits tasks requiring causal reasoning.

## Convergence Analysis

Under standard smoothness, bounded variance, and bounded heterogeneity assumptions, CHAINFED converges to a stationary point of the global objective. The effective gradient at each stage is:

```
g̃_t = g_local + λ · g_global
```

The descent relation is:

```
F(Θ_{t+1}) ≤ F(Θ_t) − η_t ||g̃_t||² − ε_aux − ε_FOAT
```

With appropriate hyperparameter tuning, the approximation errors ε_aux and ε_FOAT can be made arbitrarily small, ensuring F(Θ_t) is monotonically decreasing and lower-bounded → convergence to a stationary point.

## I/O Latency Mitigation

- Loading a transformer layer takes milliseconds; forward/backward propagation for that layer takes seconds. The I/O cost is negligible relative to computation.
- On edge devices with Unified Memory Architecture (UMA), CPU and GPU/NPU share memory pointers, eliminating costly host-device copies.
- CHAINFED uses an asynchronous "Compute-Prefetch-Evict" pipeline: while computing layer Li, the system writes Li−1 back to storage and prefetches Li+1 into a reserved buffer. Since T_comp >> T_I/O, read/write operations complete before computation finishes → near-zero added latency.

## Related Frameworks

- **FEDOT** (Kong et al., CVPR 2026): Federated learning for black-box foundation models via orthogonal transformations applied externally to embeddings. Guarantees dual privacy (client data + server IP) by never accessing FM internals. Orthogonality (κ = 1) minimizes the upper bound on gradient conflict across heterogeneous clients.
- **FedPKDA** (Zeng et al., AAAI 2026): Personalized federated learning with privacy-preserving knowledge dynamic alignment. Uses Laplacian noise on local prototypes + Mahalanobis distance for global prototype generation.
- **Apple Federated Evaluation and Tuning** (Paulik et al., 2022): Production federated system for on-device personalization at scale.
- **Google ODP Federated Compute Server**: TEE-based aggregation with Shamir secret sharing for decryption keys; DP noising in confidential space.