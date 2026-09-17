# FedFG Evidence Base

## Source
Wang, Pan, Yao. "FedFG: Privacy-Preserving and Robust Federated Learning via Flow-Matching Generation." arXiv:2603.27986, March 2026. Sun Yat-sen University. Code: https://github.com/rywangcn/FedFG

## Core Innovation
First framework to use flow-matching as generative backbone for unified FL privacy + robustness. Prior work (FedCG, GAN-Filter) used GANs — flow-matching provides smoother distributions, better privacy, and simulation-free training.

## Attack Models Tested

### Gradient Inversion (Privacy)
- **DLG** (Deep Leakage from Gradients): 10,000 iterations, Adam optimizer, TV regularization
- **IG** (Inverting Gradients): 5,000 iterations, cosine-similarity-based objective
- Results: FedFG produces near-noise reconstructions with lower PSNR/SSIM than FedCG

### Poisoning (Robustness)
- **SF** (Sign Flipping): Malicious clients reverse sign of local updates
- **IPM** (Inner Product Manipulation): Make inner product with true gradient small/negative
- **MPAF** (Model Poisoning Attack with Fake clients): Steer toward low-accuracy base model, scale updates

## Robustness Results (IID)

### MNIST
| Method | SF 10% | IPM 10% | MPAF 10% | SF 30% | IPM 30% | MPAF 30% |
|---|---|---|---|---|---|---|
| FedAvg | 0.174 | 0.895 | 0.096 | 0.083 | 0.096 | 0.095 |
| Median | 0.936 | 0.942 | 0.942 | 0.817 | 0.938 | 0.936 |
| FedFG | **0.949** | **0.950** | **0.956** | **0.947** | **0.946** | **0.955** |

### CIFAR10
| Method | SF 10% | IPM 10% | MPAF 10% | SF 30% | IPM 30% | MPAF 30% |
|---|---|---|---|---|---|---|
| FedAvg | 0.095 | 0.447 | 0.103 | 0.096 | 0.103 | 0.106 |
| Median | 0.489 | 0.510 | 0.508 | 0.284 | 0.503 | 0.487 |
| FedFG | **0.561** | **0.546** | **0.533** | **0.523** | **0.563** | **0.557** |

## Non-IID Results (Dirichlet β=0.5)

### MNIST-0.5
| Method | SF 10% | IPM 10% | MPAF 10% | SF 30% | IPM 30% | MPAF 30% |
|---|---|---|---|---|---|---|
| Median | 0.909 | 0.100 | 0.105 | 0.383 | 0.839 | 0.852 |
| DPR-PPFL | 0.921 | 0.391 | 0.927 | 0.093 | 0.090 | 0.093 |
| GAN-Filter | 0.917 | 0.924 | 0.102 | 0.863 | 0.835 | 0.074 |
| FedFG | **0.924** | **0.924** | **0.924** | **0.898** | **0.899** | **0.897** |

Key observation: FedFG maintains ~90% accuracy across all attacks at 30% malicious, while baselines collapse under specific attacks.

## Outlier Detection Mechanism

- Hellinger distance between client predictive distributions on synthetic probes
- Hampel rule: threshold = median + γ × 1.4826 × MAD (γ=3, classical 3-sigma cutoff)
- Accuracy threshold: κ = 1/(2N)
- Dynamic threshold adapts to evolving score distribution across rounds

## Convergence Theorem Summary

Theorem IV.1: Under standard FL assumptions + verification mechanism assumptions:
- Rate: O(1/√(QR)) with constant step size η = Θ(1/√(QR))
- Five error terms: initialization, noise/heterogeneity, drift, verification failure, probe estimation
- As weights stabilize (Δ→0), verification succeeds (δ_f→0), probes grow (S→∞), recovers canonical rate

## Why Flow-Matching > GAN for FL Privacy

1. **Smoother distribution**: Flow-matching ODE produces smoother conditional manifolds than GAN's concentrated distributions → harder for gradient inversion to match
2. **Simulation-free**: No adversarial training instability
3. **Stable under non-IID**: Flow-matching handles heterogeneous client distributions better
4. **Privacy advantage**: Lower PSNR/SSIM on both DLG and IG attacks vs FedCG