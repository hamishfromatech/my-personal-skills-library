# ERIS Evidence Base

## Primary Source

**Fenoglio, D., Polverino, P., Quizi, J., Gjoreski, M., Dhasade, A., & Langheinrich, M. (2026). ERIS: Enhancing Privacy and Scalability in Federated Learning via Federated Shard Aggregation. arXiv:2602.08617v2.**

### Datasets
- Image: MNIST (LeNet-5, 62K params), CIFAR-10 (ResNet-9, 1.65M), ImageNet (ResNet-18, 11.7M)
- Text: IMDB (DistilBERT, 67M), CNN/DailyMail (GPT-Neo, 1.3B), LFW (reconstruction)
- 5-fold cross-validation, IID and non-IID (Dirichlet α ∈ {0.2, 0.5})

### Baselines (6 SOTA)
1. **Ako** (SoCC '16) — decentralized, partial gradient exchange
2. **Shatter** — privacy-preserving distributed learning, gossip protocol
3. **SoteriaFL** (NeurIPS '22) — centralized shifted compression + DP
4. **PriPrune** (ACM TOMPECS '24) — pruning strategy, withholds informative gradients
5. **LDP** — local differential privacy
6. **FedAvg** (AISTATS '17) — standard baseline, no defenses

### Privacy Attacks (6 attacks, 2 categories)
**Membership Inference Attacks (MIA)**: Steinke et al. privacy auditing framework; SPV-MIA for text
**Data Reconstruction Attacks (DRA)**: DLG, iDLG, ROG (obfuscated gradients), GGL (generative gradient inversion, adaptive)

### Key Results

**Utility-Privacy Trade-off (CIFAR-10, 50 clients)**:
| Method | Test Acc | MIA Acc | Gap from FedAvg |
|---|---|---|---|
| FedAvg | 34.86% | 68.46% | — |
| FedAvg-LDP (ε=10) | 19.00% | 63.35% | -15.86pp acc |
| SoteriaFL (ε=10) | 17.18% | 58.83% | -17.68pp acc |
| PriPrune (p=0.01) | 26.30% | 65.67% | -8.56pp acc |
| Shatter | 12.40% | 63.00% | -22.46pp acc |
| **ERIS (FSA)** | **34.84%** | **63.02%** | **-0.02pp acc** |
| **ERIS (+DSC)** | **34.68%** | **60.48%** | **-0.18pp acc** |

**Scalability (distribution time per round)**:
- FedAvg: 33s (CIFAR-10), 5200s (CNN/DailyMail)
- SoteriaFL: 17.32s, 2730s
- ERIS (A=2): 0.65s, 468s (4× / 11× speedup)
- ERIS (A=50): 0.02s, 236s (1650× / 22× speedup)

**Communication (per-client, CNN/DailyMail GPT-Neo 1.3B)**:
- FedAvg: 5.2 GB upload + 5.2 GB download = 10.4 GB
- ERIS (+DSC, ω≈8000): 30.87 KB upload + 257.28 MB download = 257.31 MB (40× reduction)

**Robustness**:
- Aggregator dropout: accuracy unchanged up to 70% dropout
- Link failures: accuracy close to no-failure up to 50%
- Collusion: MIA accuracy increases smoothly but remains below FedAvg even at 50% collusion

### Theoretical Comparison (asymptotic utility bounds)

| Algorithm | Privacy | Utility Bound |
|---|---|---|
| Distributed DP-SRM | (ε,δ)-DP | Õ(n/(√Kmε)) |
| SDM-DSGD | (ε,δ)-LDP | Õ(n/(√Kmε)) |
| CDP-SGD | (ε,δ)-LDP | Õ((1+ω)n/(√Kmε)) |
| SoteriaFL-SGD | (ε,δ)-LDP | Õ((1+ω)n/(√Kmε)(1+√τ)) |
| **ERIS-SGD (+DSC)** | **No DP noise** | **Õ(√(1+ω)/(m√K))** |

ERIS achieves faster convergence under same optimization assumptions because it doesn't inject DP noise.

## Supporting Literature

- **McMahan et al. (2017)** — FedAvg foundational paper
- **Zhu et al. (2019)** — Deep Leakage from Gradients (DLG attack)
- **Zhao et al. (2020)** — iDLG improved attack
- **Li et al. (2022)** — SoteriaFL: shifted compression + DP
- **Chu et al. (2024)** — PriPrune: pruning-based privacy
- **Bonawitz et al. (2017)** — Practical secure aggregation
- **Damgård et al. (2012)** — MPC foundations for additive secret sharing
- **Mohassel & Zhang (2017)** — SecureML: two-party MPC
- **Wagh et al. (2019)** — SecureNN: 3-party secure computation

## Implementation

- **Code**: MIT license, available on GitHub
- **Stack**: Python 3.13, PyTorch 2.6 (BSD), Flower 1.12 (Apache 2.0), Opacus 1.5 (Apache 2.0)
- **Hardware**: 4× NVIDIA RTX A6000 (48GB), dual AMD EPYC 7513, 512GB RAM
- **Reproducibility**: Fixed seeds, 5-fold CV, all hyperparameters documented

## Cross-References

- `adaptive-verifiable-federated-learning-2026` — adaptive DP methods (HEAD-FL, PPCFL, DP-FedAdamW)
- `zk-proof-federated-learning-trust` — cryptographic verification (Falafel, ZT-FL-PE, DeSA, FLiPD)
- `chain-federated-fine-tuning` — memory-constrained FL (CHAINFED)
- `sheld-fl-self-learning` — heterogeneous DP framework
- `chain-federated-fine-tuning` — edge device FL

## A-Tech Alignment

- **Open-source AI**: MIT implementation; PyTorch, Flower open-source; public datasets
- **Data privacy**: Information-theoretic; no TEE/HE; data stays local; collusion-bounded
- **Financial freedom**: No specialized hardware; 105× communication reduction; commodity infrastructure
- **Practical implementation**: Docker-deployable; reproducible; robust to real-world failure modes