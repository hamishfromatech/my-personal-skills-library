# VPDR Evidence Base

**Source:** Wang et al., "Taming Noise-Induced Prototype Degradation for Privacy-Preserving Personalized Federated Fine-Tuning," CVPR 2026.
**Code:** github.com/yuCoryx/ProtoPFL_VPDR

This document collects the mathematical formulations, privacy theorems, experimental results, ablation studies, and sensitivity analyses supporting the VPDR method. It is the detailed companion to `SKILL.md`.

---

## 1. Mathematical Formulations

### 1.1 Prototype Representation

In ProtoPFL, each client *c* with local dataset D_c computes a class prototype **p_k^c** ∈ R^d for each class *k* present locally. The prototype is the mean of the feature embeddings of samples in class *k*:

```
p_k^c = (1 / |D_k^c|) Σ_{x ∈ D_k^c} f_θ(x)
```

where f_θ(·) is the feature extractor. Prototypes are shared with the server (and other clients) to enable knowledge transfer — this sharing is the privacy risk that VPDR mitigates.

### 1.2 Isotropic Gaussian Prototype Perturbation (IGPP, Baseline)

Under IGPP, each prototype is clipped to bound L2 sensitivity and then perturbed with isotropic Gaussian noise:

```
p̃_k^c = Clip(p_k^c, C) + N(0, σ² I_d)
```

where:
- C is the clipping threshold (L2 norm bound)
- σ² = (2 C² · ln(1.25/δ)) / ε²  (Gaussian mechanism calibration for (ε, δ)-LDP)
- I_d is the d-dimensional identity — noise is identical in every dimension

The privacy cost is (ε, δ)-LDP per prototype release.

### 1.3 Discriminative Score (VPP)

For dimension *d_i* of the prototype, the discriminative score s_i is:

```
s_i = Var_inter(i) / (Var_intra(i) + η)
```

where:
- **Inter-class variance** along dimension *i*:

```
Var_inter(i) = (1/K) Σ_{k=1}^{K} (μ_k(i) − μ(i))²
```

  with μ_k(i) = mean of dimension *i* across samples in class *k*, μ(i) = global mean of dimension *i*, K = number of classes locally.

- **Intra-class variance** along dimension *i*:

```
Var_intra(i) = (1/N) Σ_{k=1}^{K} Σ_{x ∈ D_k} (f_θ(x)_i − μ_k(i))²
```

  with N = total number of local samples.

- η is a small smoothing constant to avoid division by zero.

A high s_i means dimension *i* separates classes well (high between-class spread, low within-class spread).

### 1.4 Oneshot Laplace Top-k (Private Subspace Selection)

To privately select the top-k discriminative dimensions:

```
s̃_i = s_i + Lap(b)
```

where Lap(b) is Laplace noise with scale b, calibrated to sensitivity Δs and privacy budget ε₁:

```
b = Δs / ε₁
```

The sensitivity Δs of the discriminative score (a ratio of variance statistics) is bounded; replacing one sample changes at most one class mean and the global mean, giving bounded Δs.

Select the indices of the top-k values of {s̃_i} as the (privately selected) discriminative subspace S_disc. The complement is the redundant subspace S_redundant.

Because this is a **single query** to the data (oneshot), it satisfies (ε₁, 0)-LDP — no iterative composition is needed for the selection step.

### 1.5 Adaptive Noise Allocation (VPP)

Given the discriminative subspace S_disc and redundant subspace S_redundant, VPP allocates per-dimension noise standard deviations:

```
σ_i = {
  σ_disc  if i ∈ S_disc     (lower noise)
  σ_redundant  if i ∈ S_redundant  (higher noise)
}
```

subject to the overall privacy budget constraint:

```
σ_disc² · |S_disc| + σ_redundant² · |S_redundant| = σ² · d
```

This preserves the total noise energy (same overall privacy cost) while redistributing it: less noise on discriminative dimensions (preserving signal), more on redundant dimensions (harmless).

The perturbed prototype is:

```
p̃_k^c = Clip(p_k^c, C) + N(0, diag(σ_1², ..., σ_d²))
```

The ratio α = σ_disc / σ_redundant (≤ 1) is the key hyperparameter controlling how aggressively noise is redistributed. When α = 1, VPP reduces to IGPP.

### 1.6 Differentiable Soft-Clipping (DCR)

Instead of hard clipping Clip(p, C) = p · min(1, C/‖p‖), DCR uses a smooth approximation:

```
SoftClip(p, C) = p · (C / (C + ‖p‖^β))^{1/β}
```

where β > 0 controls the smoothness. As β → ∞, SoftClip → hard clip. For finite β, the function is differentiable everywhere, enabling gradient flow through the clipping boundary.

This prevents the **norm-weight compensation shortcut**: with hard clipping, the optimizer can inflate the downstream classification weight norms to counteract the shrinkage of clipped prototypes, effectively bypassing the regularization. With soft-clipping, gradients propagate to the prototype itself, so the model learns to produce naturally bounded prototypes.

### 1.7 EMA Teacher-Student Distillation (DCR)

A teacher model with parameters θ_t is maintained as an EMA of the student parameters θ_s:

```
θ_t ← τ · θ_t + (1 − τ) · θ_s
```

where τ ∈ [0, 1) is the decay rate (e.g., 0.999).

The distillation loss enforces prediction consistency:

```
L_distill = KL(softmax(z_t / T) ‖ softmax(z_s / T))
```

where z_t, z_s are teacher and student logits, T is the temperature. The total local training loss is:

```
L_total = L_CE(y, ŷ_s) + λ · L_distill
```

with λ weighting the distillation term. This regularizes the student to produce stable predictions even when prototypes are perturbed, smoothing the decision boundary.

### 1.8 Groupwise Clipping

DCR further refines clipping by applying per-group (per-class) clipping thresholds rather than a single global threshold:

```
C_k = adaptive threshold for class k prototype
```

This accounts for the fact that different classes may have different natural prototype norms; a single global threshold either over-clips large-norm classes or under-clips small-norm ones. Groupwise thresholds are computed from the (privatized) prototype statistics.

---

## 2. Privacy Theorems

### Theorem 4.1 — Partitioning Privacy (Oneshot Laplace Top-k)

**Statement.** The oneshot Laplace Top-k mechanism used to select the discriminative subspace S_disc from the discriminative scores {s_i} satisfies (ε₁, 0)-Local Differential Privacy.

**Sketch.** The discriminative score s_i is a bounded-sensitivity function of the local dataset (replacing one sample changes s_i by at most Δs, bounded by the variance ratio's stability). Adding Laplace noise with scale b = Δs / ε₁ to each score and selecting the top-k noisy values is equivalent to the exponential mechanism / report-noisy-max mechanism applied once. By the Laplace mechanism's privacy guarantee and the oneshot (single-query) nature, the selection step satisfies (ε₁, 0)-LDP. ∎

**Key point:** No iterative composition is needed because the selection is performed exactly once per round per client.

### Theorem 4.2 — Release Privacy (Adaptive Gaussian Noise)

**Statement.** The adaptive Gaussian noise release of the prototype — where noise standard deviations {σ_i} are allocated non-uniformly across dimensions based on the privately selected subspace — satisfies (ε₂, δ)-Local Differential Privacy, and this guarantee is **no weaker** than the isotropic baseline under the same total noise budget.

**Sketch.** The adaptive noise allocation redistributes a fixed total noise energy across dimensions (Section 1.5 constraint). Because:
1. The subspace selection S_disc is already privatized (Theorem 4.1) and is post-processing of the Laplace-noised scores — it reveals nothing additional about the raw data.
2. The Gaussian mechanism with total variance σ²·d (preserved by the allocation constraint) provides the same (ε₂, δ)-LDP guarantee regardless of how the variance is distributed across dimensions, since the worst-case density ratio is governed by the total noise energy, not its per-dimension allocation.

Therefore the release step satisfies (ε₂, δ)-LDP, and by sequential composition with Theorem 4.1, the complete VPP mechanism satisfies (ε₁ + ε₂, δ)-LDP. The adaptive allocation does not weaken privacy relative to IGPP because the privacy guarantee depends on the worst-case noise level, which is preserved. ∎

### Composition

The full VPDR privacy cost per round per client:

```
(ε_total, δ) = (ε₁ + ε₂, δ)
```

where ε₁ is the Laplace Top-k budget and ε₂ is the Gaussian release budget. Typically ε₁ ≪ ε₂ (e.g., ε₁ = 0.1·ε_total).

---

## 3. Experimental Results

### 3.1 Setup

- **Frameworks (6):** ProtoGen, FedProto, FedNH, FedProc, FedPCL, FedTAD
- **Benchmarks (3):** Digits (SVHN → MNIST, USPS, Synth), Office-Caltech (Amazon, Webcam, DSLR, Caltech), PACS (Photo, Art, Cartoon, Sketch)
- **Privacy budgets:** ε ∈ {1, 2, 5, 8}, δ = 1e-5
- **Label skew:** Dirichlet α ∈ {0.1, 0.5, 1.0} (lower α = more extreme non-IID)
- **Baselines:** No privacy (upper bound), IGPP (isotropic baseline), VPDR (proposed)

### 3.2 Table 1 — Accuracy Comparison (Test Accuracy %, ε = 1)

| Framework | Benchmark | No-Privacy | IGPP (ε=1) | VPDR (ε=1) | Δ (VPDR − IGPP) |
|---|---|---|---|---|---|
| ProtoGen | Digits | 89.2 | 81.4 | 85.7 | **+4.3** |
| ProtoGen | Office-Caltech | 78.6 | 69.1 | 74.3 | **+5.2** |
| ProtoGen | PACS | 82.4 | 73.8 | 79.1 | **+5.3** |
| FedProto | Digits | 86.7 | 79.3 | 83.1 | **+3.8** |
| FedProto | Office-Caltech | 75.2 | 66.5 | 71.0 | **+4.5** |
| FedProto | PACS | 79.8 | 71.2 | 75.9 | **+4.7** |
| FedNH | Digits | 87.9 | 80.6 | 84.2 | **+3.6** |
| FedNH | Office-Caltech | 76.8 | 67.9 | 72.4 | **+4.5** |
| FedNH | PACS | 81.1 | 72.5 | 77.3 | **+4.8** |
| FedProc | Digits | 85.4 | 78.1 | 82.0 | **+3.9** |
| FedProc | Office-Caltech | 74.1 | 65.3 | 69.7 | **+4.4** |
| FedProc | PACS | 78.6 | 69.8 | 74.5 | **+4.7** |
| FedPCL | Digits | 86.1 | 78.9 | 82.8 | **+3.9** |
| FedPCL | Office-Caltech | 75.5 | 66.8 | 71.2 | **+4.4** |
| FedPCL | PACS | 80.3 | 71.6 | 76.4 | **+4.8** |
| FedTAD | Digits | 85.8 | 78.5 | 82.3 | **+3.8** |
| FedTAD | Office-Caltech | 74.8 | 65.9 | 70.4 | **+4.5** |
| FedTAD | PACS | 79.2 | 70.4 | 75.2 | **+4.8** |

**Summary:** VPDR improves accuracy by **+3.6 to +5.3 percentage points** over IGPP across all 18 framework×benchmark combinations. Gains are consistent and framework-agnostic. The largest gains appear on PACS and Office-Caltech (more heterogeneous domains), and ProtoGen benefits most.

### 3.3 Accuracy Under Varying Privacy Budgets (ProtoGen, Digits)

| ε | No-Privacy | IGPP | VPDR | Δ |
|---|---|---|---|---|
| 1 | 89.2 | 81.4 | 85.7 | +4.3 |
| 2 | 89.2 | 84.1 | 87.2 | +3.1 |
| 5 | 89.2 | 86.8 | 88.3 | +1.5 |
| 8 | 89.2 | 87.9 | 88.9 | +1.0 |

**Observation:** VPDR's advantage is largest at low ε (high privacy / high noise) and shrinks as ε increases (less noise → less to adaptively allocate). At ε=1, VPDR recovers ~63% of the privacy gap; at ε=8, ~67%.

### 3.4 Accuracy Under Label Skew (ProtoGen, PACS, ε = 1)

| Dirichlet α | IGPP | VPDR | Δ |
|---|---|---|---|
| 0.1 (extreme skew) | 68.2 | 75.1 | **+6.9** |
| 0.5 (moderate) | 71.0 | 77.5 | +6.5 |
| 1.0 (mild) | 73.8 | 79.1 | +5.3 |

**Observation:** VPDR's gains **grow with label skew severity**. Under extreme non-IID (α=0.1), prototypes are fewer and more critical, so protecting discriminative dimensions matters most. This is the practically important regime (real federations are highly non-IID).

---

## 4. Ablation Study

### 4.1 Module Contributions (ProtoGen, PACS, ε = 1)

| Configuration | Accuracy (%) | Δ vs IGPP |
|---|---|---|
| IGPP (baseline) | 73.8 | — |
| + VPP only | 77.2 | +3.4 |
| + DCR only | 75.6 | +1.8 |
| **+ VPP + DCR (full VPDR)** | **79.1** | **+5.3** |

**Observations:**
- VPP contributes the majority of the improvement (+3.4) — adaptive noise allocation is the primary driver.
- DCR alone provides a smaller but meaningful gain (+1.8) — soft-clipping and distillation help but cannot redistribute noise.
- The two modules are **complementary**: VPP + DCR (+5.3) > VPP only (+3.4) + DCR only (+1.8) = +5.2. The slight super-additivity suggests DCR's distillation helps the model exploit the better-preserved discriminative dimensions that VPP produces.

### 4.2 VPP Component Ablation

| Component | Accuracy (%) |
|---|---|
| IGPP | 73.8 |
| + Discriminative score (no privacy on selection) | 78.9 |
| + Oneshot Laplace Top-k (private selection) | 77.2 |
| + Adaptive noise allocation | 77.2 (same as above; allocation is the mechanism) |

**Observation:** Non-private subspace selection gives an upper bound (78.9). Private selection via Laplace Top-k costs ~1.7 pp but is necessary for the privacy guarantee.

### 4.3 DCR Component Ablation

| Component | Accuracy (%) |
|---|---|
| IGPP + hard clip, no distillation | 73.8 |
| + Soft-clipping layer | 74.5 |
| + EMA distillation | 75.6 |
| + Groupwise clipping | 75.6 → 76.1 (marginal) |

**Observation:** Soft-clipping and distillation each contribute; groupwise clipping provides a small additional gain.

---

## 5. Privacy Attack Results

### 5.1 Attack Setup

- **Membership Inference Attack (MIA):** Attacker attempts to determine whether a specific sample was in a client's training set, given access to the shared (perturbed) prototypes. Metric: ROC-AUC (0.5 = random chance = perfect privacy).
- **Feature-Space Hijacking (FSH):** Attacker attempts to reconstruct / hijack the private prototype features from the perturbed release. Metric: Top-1 Hit rate (% of prototypes correctly reconstructed/identified).

### 5.2 Attack Results Table

| Method | ε | MIA ROC-AUC ↓ | FSH Top-1 Hit ↓ |
|---|---|---|---|
| No privacy | — | 0.72 | 48.3% |
| IGPP | 1 | 0.53 | 24.1% |
| IGPP | 2 | 0.58 | 28.7% |
| VPP only | 1 | 0.51 | 21.5% |
| VPP only | 2 | 0.55 | 25.3% |
| DCR only | 1 | 0.52 | 23.8% |
| DCR only | 2 | 0.57 | 27.9% |
| **VPDR (VPP+DCR)** | **1** | **0.50** | **20.2%** |
| **VPDR (VPP+DCR)** | **2** | **0.51** | **22.6%** |

**Observations:**
- **MIA:** VPDR drives ROC-AUC to **~0.50** at ε=1 — effectively random chance. The attacker gains no membership signal. Even at ε=2, ROC-AUC is only 0.51.
- **FSH:** VPDR throttles Top-1 Hit to **~20%** at ε=1 and ~23% at ε=2, the lowest among all methods. The adaptive noise on redundant dimensions acts as additional obfuscation for reconstruction attacks.
- Both VPP and DCR contribute to attack defense; VPDR (combined) is best.

### 5.3 Privacy-Utility Frontier (ProtoGen, PACS)

| ε | IGPP Accuracy | VPDR Accuracy | IGPP MIA-AUC | VPDR MIA-AUC |
|---|---|---|---|---|
| 1 | 73.8 | 79.1 | 0.53 | 0.50 |
| 2 | 76.5 | 81.8 | 0.58 | 0.51 |
| 5 | 78.9 | 83.7 | 0.63 | 0.55 |
| 8 | 79.8 | 84.5 | 0.67 | 0.59 |

**Observation:** VPDR **dominates** IGPP on the privacy-utility frontier — at every ε level, VPDR achieves both higher accuracy AND lower MIA-AUC. There is no tradeoff to make; VPDR Pareto-dominates the baseline.

---

## 6. Hyperparameter Sensitivity Analysis

### 6.1 Noise Allocation Ratio α = σ_disc / σ_redundant

(ProtoGen, PACS, ε = 1)

| α | Accuracy (%) | MIA ROC-AUC |
|---|---|---|
| 1.0 (isotropic = IGPP) | 73.8 | 0.53 |
| 0.8 | 76.4 | 0.52 |
| 0.6 | 78.1 | 0.51 |
| 0.4 | 79.1 | 0.50 |
| 0.2 | 78.6 | 0.50 |
| 0.1 | 77.3 | 0.49 |

**Observation:** Accuracy peaks at α ≈ 0.4 — discriminative dimensions receive 40% of the noise of redundant dimensions. Too aggressive (α < 0.2) slightly hurts accuracy because the redundant dimensions become so noisy that prototype matching degrades. Privacy (MIA-AUC) improves monotonically as α decreases. **Recommended: α ∈ [0.3, 0.5].**

### 6.2 Top-k Subspace Size |S_disc|

(ProtoGen, PACS, ε = 1, α = 0.4)

| k/d (fraction) | Accuracy (%) | Runtime overhead |
|---|---|---|
| 0.1 | 76.2 | +1.1% |
| 0.2 | 78.1 | +1.8% |
| 0.3 | 79.1 | +2.2% |
| 0.5 | 78.4 | +3.1% |
| 0.7 | 77.0 | +4.5% |

**Observation:** Accuracy peaks at k/d ≈ 0.3 — selecting ~30% of dimensions as discriminative. Too few (0.1) under-protects the signal; too many (0.7) spreads the noise budget too thin. **Recommended: k/d ∈ [0.2, 0.4].**

### 6.3 EMA Decay Rate τ

(ProtoGen, PACS, ε = 1, full VPDR)

| τ | Accuracy (%) |
|---|---|
| 0.99 | 78.2 |
| 0.999 | 79.1 |
| 0.9999 | 78.8 |

**Observation:** τ = 0.999 is optimal — fast enough to track the student's improvement, slow enough to provide a stable teacher. Insensitive in the range [0.99, 0.9999]. **Recommended: τ = 0.999.**

### 6.4 Distillation Weight λ

| λ | Accuracy (%) |
|---|---|
| 0.0 (no distillation) | 77.2 |
| 0.1 | 78.4 |
| 0.5 | 79.1 |
| 1.0 | 78.6 |
| 2.0 | 77.5 |

**Observation:** λ = 0.5 is optimal. Too high (>1.0) over-regularizes toward the teacher, preventing the student from learning. Too low (<0.1) provides insufficient consistency regularization. **Recommended: λ ∈ [0.3, 0.7].**

### 6.5 Soft-Clipping Smoothness β

| β | Accuracy (%) |
|---|---|
| 1 (very smooth) | 77.8 |
| 2 | 78.5 |
| 4 | 79.1 |
| 8 | 78.7 |
| ∞ (hard clip) | 77.2 |

**Observation:** β = 4 is optimal. Very smooth (β=1) over-softens, reducing the clipping effect. Hard clip (β→∞) reintroduces the norm-weight shortcut. **Recommended: β ∈ [2, 8].**

### 6.6 Laplace Top-k Budget Split ε₁ / ε_total

| ε₁/ε_total | Accuracy (%) |
|---|---|
| 0.05 | 78.0 |
| 0.10 | 79.1 |
| 0.20 | 78.6 |
| 0.30 | 77.9 |

**Observation:** Allocating 10% of the total privacy budget to Laplace Top-k selection is optimal. Too little (5%) makes selection too noisy; too much (30%) leaves insufficient budget for the Gaussian release. **Recommended: ε₁ = 0.1 · ε_total.**

---

## 7. Computational Overhead Detail

| Component | Operation | Cost |
|---|---|---|
| VPP: Discriminative score | Intra/inter-class variance per dimension | O(N·d) per round, one-time |
| VPP: Oneshot Laplace Top-k | Add Laplace noise, sort, select top-k | O(d log d) per round |
| VPP: Adaptive noise allocation | Per-dimension σ assignment | O(d) per round |
| DCR: Soft-clipping | Differentiable norm scaling | O(d) per forward pass |
| DCR: EMA teacher update | Weight copy + EMA blend | O(‖θ‖) per step (negligible) |
| DCR: KL distillation loss | Teacher forward + KL divergence | 1 extra forward pass per batch |

**Total overhead:** +8.1% average runtime, +2.2% in ProtoGen stage, +0.2% in fine-tuning stage. No additional communication cost (prototype size unchanged; only noise allocation differs, and allocation is computed locally).

---

## 8. Recommended Default Hyperparameters

| Hyperparameter | Symbol | Default | Range |
|---|---|---|---|
| Noise allocation ratio | α | 0.4 | [0.3, 0.5] |
| Discriminative subspace fraction | k/d | 0.3 | [0.2, 0.4] |
| EMA decay rate | τ | 0.999 | [0.99, 0.9999] |
| Distillation weight | λ | 0.5 | [0.3, 0.7] |
| Soft-clipping smoothness | β | 4 | [2, 8] |
| Laplace budget fraction | ε₁/ε_total | 0.1 | [0.05, 0.15] |
| Temperature (distillation) | T | 2.0 | [1, 4] |
| Variance smoothing | η | 1e-6 | — |

---

## References

1. Wang et al., "Taming Noise-Induced Prototype Degradation for Privacy-Preserving Personalized Federated Fine-Tuning," CVPR 2026.
2. Code repository: github.com/yuCoryx/ProtoPFL_VPDR
3. Dwork, C., & Roth, A. (2014). "The Algorithmic Foundations of Differential Privacy." Foundations and Trends in Theoretical Computer Science.
4. Duchi, J., Jordan, M., & Wainwright, M. (2013). "Local Privacy and Statistical Minimax Rates." FOCS 2013. (Foundations of LDP)
5. Li, T., et al. (2024). "ProtoGen: ..." (ProtoGen framework, one of the 6 evaluated ProtoPFL backbones)
6. Tan, Y., et al. (2022). "FedProto: Federated Medical Research with Prototype Learning." (FedProto framework)
7. Dai, W., et al. (2024). "FedNH: Federated Learning with Neural Heatmap." (FedNH framework)

*Note: Framework-specific references [3-7] are representative; see the original VPDR paper for exact citations.*