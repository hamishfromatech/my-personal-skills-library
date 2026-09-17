# Evidence Base: Personalized Federated Diffusion Models Privacy

## Primary Source

**Authors:** Kumar Kshitij Patel (Yale FDS), Bingqing Jiang (HKU), A F M Mahfuzul Kabir (NJIT), Weitong Zhang (UNC), Difan Zou (HKU), Lingxiao Wang (NJIT)
**Publication:** CVPR 2026, pp. 31790-31801
**Code:** Available (see paper appendix)

## Algorithm Details

### PFDM (Personalized Federated Diffusion Model)

**Input:**
- Training datasets {D_m}_{m∈[M]}
- Shared model parameter w
- Client-specific model parameters {θ_m}_{m=1}^M
- Local time step t₀
- Global time step T
- Noise schedule {β_t}_{t=1}^T
- Clipping parameter C

**Stage 1 (Client-side):**
```
for each client m ∈ [M]:
    Train personalized denoiser z_θm = T-DDPM(D_m, T, {β_t}, θ_m)
    Sample n_m data from D_m indexed by B_m
    Set ᾱ_t = ∏_{s=1}^{t} (1 - β_s)
    for i ∈ B_m:
        x̃_{i,m,0} = √ᾱ_{t₀} · CLIP(x_{i,m,0}, C) + √(1-ᾱ_{t₀}) · z
        where z ~ N(0, I), CLIP(x, C) = x · min(1, C/||x||₂)
    Send D̃_m = {x̃_{i,m,0}}_{i∈B_m} to server
```

**Stage 2 (Server-side):**
```
    Obtain D̃ = {D̃_m}_{m∈[M]}
    Train z_w = T-DDPM(D̃, T, {β_t}, w)
```

**Output:** Shared global denoiser z_w

### Sampling Procedure

**Input:** Shared denoiser z_w, personalized denoiser z_θm, T, t₀, {β_t, σ_t}

```
x̃_0 = S-DDPM(z_w, T, {β_t, σ_t})  // Global denoising for T steps
Set ᾱ_t = ∏_{s=1}^{t} (1 - β_s), let x_{t₀} = x̃_0
for t = t₀, ..., 1:
    x_{t-1} = (1/√α_t) · (x_t - (√(1-α_t)/(√(1-ᾱ_t))) · z_θm(x_t, t)) + σ_t · z
    where z ~ N(0, I) if t > 1, else z = 0
Output: x_0
```

## Experimental Results

### FID Scores (Lower is Better)

| Dataset | Method | Major | Minor | Average |
|---------|--------|-------|-------|---------|
| CIFAR-10 | Non-private (Centralized) | 16.27 | 17.62 | 16.95 |
| | Non-private (FedDM) | 18.05 | 19.15 | 18.60 |
| | Non-collaborative | 19.87 | 36.44 | 28.16 |
| | **PFDM (Ours)** | **19.85** | **35.78** | **27.82** |
| Colorized MNIST | Non-private (Centralized) | 1.85 | 1.45 | 1.66 |
| | Non-private (FedDM) | 1.89 | 1.51 | 1.70 |
| | Non-collaborative | 2.19 | 5.99 | 4.09 |
| | **PFDM (Ours)** | **1.72** | **4.79** | **3.26** |
| CelebA | Non-private (Centralized) | 13.72 | 11.70 | 12.71 |
| | Non-private (FedDM) | 14.47 | 11.83 | 13.15 |
| | Non-collaborative | 23.42 | 41.38 | 32.40 |
| | **PFDM (Ours)** | **18.11** | **28.09** | **23.10** |

### Privacy Attack Results

**MIA (Proximal Initialization Attack) on PFDM global model (300 epochs):**

| Dataset | AUC | ASR | TPR@1% FPR |
|---------|-----|-----|------------|
| CIFAR-10 | 50.01 | 50.15 | 0.82 |
| Colorized MNIST | 49.70 | 50.10 | 1.07 |
| CelebA | 50.08 | 50.34 | 0.86 |

(50% = random guessing; lower is better for privacy)

**MIA on standard non-private models (for comparison):**
- CIFAR-10: AUC rises from 53.03% to 82.13% after 1000 epochs
- Colorized MNIST: AUC reaches 99.62%
- CelebA: AUC reaches 99.59%

### Reconstruction Attack Recovery Scores

| Dataset | None (no attack) | Global model attack | Pretrained model attack |
|---------|-------------------|---------------------|------------------------|
| CIFAR-10 | 2.43 | 2.41 | 1.39 |
| Colorized MNIST | 3.77 | 3.66 | 1.20 |
| CelebA | 1.13 | 1.12 | 0.89 |

(Lower recovery score = better reconstruction = worse privacy; higher = worse reconstruction = better privacy)

### DP Training Baseline Comparison (MNIST)

| Method | Major FID | Minor FID | Average FID |
|--------|-----------|-----------|-------------|
| PFDM (ε=10) | 5.40 | 8.51 | 6.96 |
| DPDM federated baseline (ε=10) | 31.06 | 36.40 | 33.73 |

PFDM achieves substantially better image quality than direct DP training of diffusion models in FL.

### Effect of Number of Clients (CIFAR-10)

As M increases from 4 to 128 (total dataset size fixed), the performance gap between PFDM and non-collaborative baseline widens, demonstrating growing value of collaboration under extreme data heterogeneity.

## Privacy Configuration Example

**Parameters:** T=1000, linear noise schedule, C=10, t₀=690
**Result:** (ε=10, δ=10⁻⁵)-LDP guarantee

**Privacy-utility knob:**
- Larger t₀ → more noise injected → stronger privacy → lower utility
- Smaller t₀ → less noise → weaker privacy → higher utility
- σ² = (1-ᾱ_{t₀})/ᾱ_{t₀} determines effective noise level

## Theoretical Guarantees

### Privacy (Theorem 5.1)
For all x ∈ D, output of Algorithm 1 satisfies:
(2C²/σ² + C√(8ln(1/δ)/σ²), δ)-LDP

where σ² = (1-ᾱ_{t₀})/ᾱ_{t₀}

### Utility (Theorem 5.2)
For class k, with sufficient samples (n_k^m = Ω(d log(1/ζ))):

E[W₂²(q; p^m)] = O((2/(2+3σ²)) · d²/N_k + (3σ²/(2+3σ²)) · d²/n_k^m)

with probability ≥ 1-ζ

**Collaboration benefit condition (Eq. 9):**
When N_k = Ω((1+σ²)n_k^m) and n_k^m = Ω((1+σ²)²d log^{3/2}(1/δ)):

E[W₂²(q; p^m_Dm) - W₂²(q; p^m_D̃,Dm)] = Ω((σ²+1)/(2σ²+1)² · d²/n_k^m)

This is positive whenever class k has sufficient overall support across clients.