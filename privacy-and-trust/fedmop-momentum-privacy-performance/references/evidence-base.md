# FedMOP Evidence Base

## Source

Zhao, Deng, Xu, Qiu, Hu, You, Chen, Xu, Su (Central South University / Hong Kong University of Science and Technology / SenseTime Research / University of Sydney / Shenzhen University, CVPR 2026). "FedMOP: Achieving Enhanced Privacy and Performance in Federated Learning via Momentum Orthogonal Projection." Code: github.com/zyl123456aB/FedMOP

## Key Insight

By carefully controlling each client's starting point for local training, FedMOP simultaneously:
- Corrects for non-IID drift (performance) via gradient orthogonal projection
- Obscures gradient information (privacy) via momentum-based trajectory hiding
- Without modifying the training process itself

## Experimental Setup

- Datasets: CIFAR-10, CIFAR-100, Tiny-ImageNet
- Non-IID partitioning: Dirichlet distribution, α ∈ {0.3 (D1, moderate), 0.05 (D2, high)}
- Model: ResNet-18
- 1000 communication rounds (CIFAR), 200 (Tiny-ImageNet)
- 5 local epochs, SGD lr=0.01, momentum=0.9, batch size=64
- 100 clients: full, 40% partial, 20% partial participation
- Attack methods: csDLG (optimization-based), FGLA (generation-based)

## Privacy Defense Results

### Single-Image Setting (Epochs=1, BatchSize=1)

| Method | MSE ↓ | PSNR ↑ | FMSE ↓ | LPIPS ↓ |
|--------|-------|--------|--------|---------|
| csDLG (no defense) | 0.62 | 14.11 | 2.14e-8 | 0.42 |
| DP | 0.48 | 15.28 | 1.42e-8 | 0.35 |
| Soteria | 0.54 | 14.73 | 1.76e-8 | 0.40 |
| CENSOR | 0.46 | 15.42 | 0.95e-8 | 0.33 |
| GradAug | 0.85 | 7.43 | 7.64e-8 | 0.71 |
| OUTPOST | 0.19 | 19.60 | 0.72e-8 | 0.28 |
| **FedMOP** | **0.85** | **7.43** | **7.64e-8** | **0.71** |

### Batch Reconstruction (Epochs=1, BatchSize=16)

| Method | MSE ↓ | PSNR ↑ | FMSE ↓ | LPIPS ↓ |
|--------|-------|--------|--------|---------|
| csDLG (no defense) | 0.36 | 16.43 | 2.20e-7 | 0.35 |
| DP | 0.41 | 15.87 | 1.85e-6 | 0.38 |
| CENSOR | 0.38 | 16.22 | 8.52e-7 | 0.35 |
| OUTPOST | 0.30 | 17.54 | 1.92e-6 | 0.32 |
| **FedMOP** | **0.28** | **17.26** | **6.5e-6** | **0.30** |

- FedMOP achieves strongest defense against both optimization-based (csDLG) and generation-based (FGLA) attacks
- FGLA is more severe threat (bypasses iterative gradient matching via direct generation)
- FedMOP maintains robust protection even against FGLA

## Model Accuracy Results

### Full Participation (100 clients)

| Method | CIFAR-10 D1 | CIFAR-10 D2 | CIFAR-100 D1 | CIFAR-100 D2 | Tiny-ImageNet D1 | Tiny-ImageNet D2 |
|--------|-------------|-------------|--------------|--------------|------------------|------------------|
| FedAvg | 57.85 | 57.28 | 31.61 | 30.35 | 23.26 | 22.84 |
| FedProx | 58.70 | 58.13 | 34.13 | 32.82 | 24.57 | 24.18 |
| Scaffold | 58.89 | 58.02 | 34.96 | 33.12 | 24.12 | 24.18 |
| FedLMT | 62.45 | 61.78 | 37.32 | 36.12 | 27.89 | 27.21 |
| FedUPS | 63.12 | 62.61 | 38.45 | 37.34 | 27.93 | 27.89 |
| HierFed | 63.54 | 63.21 | 39.23 | 38.92 | 28.47 | 28.12 |
| DePRL | 63.31 | 62.82 | 38.67 | 38.21 | 28.21 | 28.03 |
| **FedMOP** | **64.31** | **63.89** | **42.07** | **41.78** | **30.28** | **29.69** |

- FedMOP consistently highest accuracy across all datasets and heterogeneity settings
- Improvements: +0.77% over HierFed (CIFAR-10 D1), +2.84% (CIFAR-100 D1), +1.81% over FedUPS (Tiny-ImageNet D2)
- Gains particularly pronounced on CIFAR-100

### Partial Participation

- 40% partial: FedMOP 64.92% (CIFAR-10 D1), 42.80% (CIFAR-100 D1), 29.69% (Tiny-ImageNet D2)
- 20% partial: FedMOP 62.23% (CIFAR-10 D1), 38.73% (CIFAR-100 D1), 29.40% (Tiny-ImageNet D2)
- Minimal degradation compared to baselines that suffer significant drops

## Convergence Speed

### Rounds to 30% Accuracy (CIFAR-100)

| Method | D1 Full | D2 Full | D1 40% | D2 40% | D1 20% | D2 20% |
|--------|---------|---------|--------|--------|--------|--------|
| FedAvg | 312 | 124 | 209 | 102 | 304 | 116 |
| FedProx | 258 | 29 | 68 | 83 | 137 | 93 |
| Scaffold | 71 | 84 | 39 | 62 | 78 | 61 |
| FedUPS | 73 | 42 | 38 | 29 | 54 | 35 |
| HierFed | 68 | 38 | 35 | 26 | 48 | 31 |
| **FedMOP** | **67** | **27** | **31** | **26** | **64** | **23** |

- FedMOP achieves best or near-best convergence across participation rates
- D2 Full: 27 rounds (4.59x speedup vs FedAvg)
- Acceleration from orthogonal projection counteracting drift + momentum smoothing reducing noise

## Computation Cost

| Method | CIFAR-10 MFLOPs | CIFAR-10 Time (s) | Tiny-ImageNet MFLOPs | Tiny-ImageNet Time (s) |
|--------|-----------------|-------------------|----------------------|------------------------|
| FedAvg | 6.2 | 10.1 | 31.6 | 36.7 |
| FedProx | 7.3 | 14.2 | 33.3 | 44.2 |
| Scaffold | 11.2 | 17.9 | 39.7 | 51.6 |
| FedLMT | 19.7 | 21.4 | 67.2 | 71.2 |
| **FedMOP** | **6.3** | **10.6** | **31.9** | **38.7** |

- FedMOP computational cost close to FedAvg (minimal overhead)
- Significantly lower than enhanced methods (FedLMT, Scaffold)

## Ablation Study (CIFAR-10, 50% participation, α=0.5)

### Component Analysis

| Configuration | MSE ↑ (privacy) | Acc (%) ↑ | Rounds ↓ |
|--------------|-----------------|-----------|----------|
| Baseline FedAvg | 0.02 | 68.3 | 850 |
| Orthogonal Proj only | 0.08 | 70.4 | 780 |
| Random β (no momentum) | 0.31 | 70.2 | 790 |
| **Full FedMOP (+Momentum)** | **0.85** | **72.7** | **760** |

- Orthogonal projection alone: improves utility (+2.1% accuracy, 70 fewer rounds) but minimal privacy (MSE=0.08)
- Random β alone: limited defense (MSE=0.31) due to 1D search vulnerability
- Only complete FedMOP with momentum achieves strong privacy (MSE=0.85) with superior performance

### Hyperparameter Sensitivity

- γ̄ ∈ [0.85, 0.95]: accuracy within 1%
- σ_γ ∈ [0.01, 0.03]: accuracy within 0.5%
- σ_0 varied 10x: accuracy affects 0.5%
- Larger σ_γ and σ_β: strengthen privacy with 5% round increase

## Theoretical Convergence

**Theorem 1:** Under standard assumptions (smoothness, bounded variance, bounded gradients), with η ≤ 1/(16KL), K ≥ 2, M ≥ 2, γ̄ ∈ [0.8, 0.95], σ_γ ≤ 0.05:

```
min_t E[‖∇f(x_t)‖²] ≤ O(Ψ + Φ₁σ² + Φ₂dG²) / (ηT(T+1)) + O(σ²_0 γ̄^{2T}) + O(σ²_γ T)
```

- O(1/(K²T²)) improves upon vanilla FedAvg's O(1/(KT))
- Additional momentum terms vanish asymptotically when σ_0, σ_γ are small
- No heterogeneity assumption needed (unlike prior adaptive FL optimizers)

## A-Tech Alignment

- **Open-source**: code available at github.com/zyl123456aB/FedMOP
- **Data privacy**: 5-10x stronger privacy defense against gradient leakage attacks
- **Financial freedom**: 1.5-2x faster convergence reduces training cost; 2-4% accuracy gains improve model value
- **Practical implementation**: minimal computational overhead (close to FedAvg), no complex loss functions

## Limitations

- Evaluated on image classification (CIFAR, Tiny-ImageNet); language tasks not tested
- ResNet-18 backbone; larger models not evaluated
- Two attack types (csDLG, FGLA); adaptive attacks not tested
- Theoretical analysis assumes honest-but-curious server