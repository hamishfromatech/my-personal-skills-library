# Fed-ADS: Adaptive Privacy-Utility Tradeoff in Federated Learning — Evidence Base

## Source

**Liu, P., Chu, B., Shen, Z., & Wang, H.** (2026). "An adaptive mechanism for privacy–utility trade-off in federated learning." *Information Sciences*, Vol. 753, 123644.

- **DOI:** 10.1016/j.ins.2026.123644
- **Published:** October 15, 2026
- **Journal:** Information Sciences (Elsevier)
- **Volume:** 753
- **Article number:** 123644
- **Keywords:** Federated learning, Differential privacy, Privacy-utility tradeoff, Adaptive gradient clipping, Selective perturbation, Dynamic privacy budget

---

## 1. Problem Statement

Differentially private federated learning (DP-FL) adds noise to gradient updates to protect client data privacy, but this noise degrades model utility — the fundamental privacy-utility tradeoff. Three specific failure modes of existing DP-FL methods motivate Fed-ADS:

### Failure Mode 1: Fixed Clipping Distorts Gradient Direction
Standard DP-FL clips all per-sample gradients to a fixed L2-norm threshold C. This is **magnitude-only** — it ignores gradient direction. Under non-IID data, client gradients are already misaligned, and uniform magnitude clipping compounds the direction distortion:
- Gradients pointing in the correct convergence direction are clipped as aggressively as misaligned ones.
- The clipped gradient's direction can be rotated away from the optimal update direction.
- Convergence slows or destabilizes, especially in deep networks and non-convex loss landscapes where update direction is as important as magnitude.

### Failure Mode 2: Global Noise Injection Wastes Privacy Budget
Standard DP-FL adds Gaussian noise to the **entire gradient vector** (all dimensions). But gradient dimensions are heterogeneous:
- Some dimensions carry high information content (large magnitude, high variance, strong loss contribution) — these are the dimensions an adversary would target for reconstruction attacks.
- Other dimensions carry low information content (near-zero, redundant, low contribution) — these leak little private information.
- Global noise taxes all dimensions equally, destroying useful signal in low-risk dimensions to protect the few high-risk ones. This is a waste of privacy budget.

### Failure Mode 3: Uniform Privacy Budget Allocation Ignores Training Dynamics
Standard DP-FL allocates the privacy budget uniformly across all T communication rounds (ε/T per round). But training dynamics are non-uniform:
- **Early training:** Model is far from optimal; gradients are large and noisy; risk of overfitting to early-batch artifacts (especially non-IID) is high. Strict privacy here is beneficial (regularization, prevents memorization).
- **Late training:** Model is converging; gradients are small and stable; primary goal is fine-tuning accuracy. Strict privacy here starves convergence of signal, degrading final accuracy without proportional privacy benefit.
- Uniform allocation over-protects late training (wasting budget) and under-protects early training (where protection actually helps).

---

## 2. Three-Mechanism Architecture

Fed-ADS addresses all three failure modes with a coordinated three-mechanism framework:

| Mechanism | Failure Mode Addressed | Granularity | Adaptation Signal |
|-----------|----------------------|-------------|-------------------|
| Direction-Preserving Adaptive Gradient Clipping | Fixed clipping distorts direction | Per-gradient, per-round | Cosine similarity (gradient vs. historical update) |
| Quantile-based Selective Gradient Perturbation | Global noise wastes budget | Per-neuron, per-dimension | Information content quantile ranking |
| Dynamic Privacy Budget Allocation | Uniform budget ignores dynamics | Per-round (training phase) | Non-linear schedule with decaying increments |

The mechanisms are **coordinated**, not independent:
- The clipping threshold C_t (mechanism 1) determines the sensitivity bound S = 2C_t, which feeds into the noise calibration for selective perturbation (mechanism 2).
- The dynamic budget ε_t (mechanism 3) determines the noise scale σ_t for selective perturbation (mechanism 2).
- The direction-preserving clipping (mechanism 1) and selective perturbation (mechanism 2) both operate on the same gradient, with clipping applied before perturbation.

---

## 3. Mechanism 1: Direction-Preserving Adaptive Gradient Clipping

### 3.1 Motivation

In standard DP-FL, the clipping operation is:

```
g̃ = g × min(1, C / ||g||)
```

This clips based on **magnitude only** (||g|| vs. C). The direction of g̃ is:
- Preserved if ||g|| ≤ C (no clipping needed)
- Distorted if ||g|| > C (the gradient is scaled down, but its direction is preserved in the simple case)

However, in the aggregation step, when multiple clipped gradients from different clients are averaged, the **magnitude clipping interacts with direction heterogeneity** under non-IID data:
- Clients with large-magnitude misaligned gradients contribute disproportionately after clipping (their direction is preserved even though it's wrong).
- Clients with small-magnitude aligned gradients are under-represented (their useful direction is diluted by the misaligned ones).

Fed-ADS addresses this by making the clipping threshold **direction-aware**: aligned gradients get a higher threshold (preserved), misaligned gradients get a lower threshold (clipped more, reducing their influence on the aggregate).

### 3.2 Cosine Similarity Computation

For each client's gradient g_t at round t, and the historical update direction h_t:

```
cos_sim(g_t, h_t) = (g_t · h_t) / (||g_t||₂ × ||h_t||₂)
```

Where:
- **g_t** = the client's current gradient (or model update delta)
- **h_t** = the historical update direction, maintained as an exponentially weighted moving average (EWMA) of past global model updates:

```
h_t = β × h_{t-1} + (1 - β) × Δw_{t-1}
```

- **β** = decay factor (e.g., 0.9) controlling how much history to retain
- **Δw_{t-1}** = the global model update from the previous round

**Interpretation of cosine similarity:**
- **cos_sim ≈ +1:** Gradient is aligned with the convergence trajectory → this update advances learning → **relax clipping** (higher C_t)
- **cos_sim ≈ 0:** Gradient is orthogonal to the convergence trajectory → this update is neutral → **moderate clipping** (baseline C)
- **cos_sim ≈ -1:** Gradient is anti-aligned with the convergence trajectory → this update harms learning → **tighten clipping** (lower C_t)

### 3.3 Dynamic Threshold Adjustment

The clipping threshold is adjusted as a function of cosine similarity:

```
C_t = C_base × f(cos_sim(g_t, h_t))
```

Where f is a monotonically increasing function mapping [-1, 1] → [C_min, C_max]:

```
f(cos_sim) = C_min + (C_max - C_min) × (cos_sim + 1) / 2
```

Or a non-linear variant that more aggressively rewards high alignment:

```
f(cos_sim) = C_min + (C_max - C_min) × σ(γ × cos_sim)
```

Where σ is the sigmoid function and γ controls the sharpness of the transition.

**Properties:**
- At cos_sim = +1 (perfect alignment): C_t = C_max (most relaxed, preserves the full gradient)
- At cos_sim = -1 (perfect anti-alignment): C_t = C_min (most aggressive, clips heavily)
- At cos_sim = 0 (orthogonal): C_t = (C_min + C_max) / 2 (moderate)

The clipped gradient:

```
g̃_t = g_t × min(1, C_t / ||g_t||₂)
```

### 3.4 Why This Preserves Convergence

**Theoretical intuition:** In non-convex optimization, convergence depends on the gradient making sufficient progress along the descent direction. If the gradient direction is aligned with the historical descent direction (high cos_sim), clipping it less means more of the useful signal survives into the aggregated update. If the gradient is misaligned (low cos_sim), clipping it more reduces its (harmful) contribution to the aggregate.

**Under non-IID data:** Client gradients are naturally misaligned (each client sees a different data distribution). Direction-preserving clipping acts as a **soft filter**: aligned clients contribute more signal, misaligned clients contribute less noise. This is more effective than uniform clipping, which treats all clients the same.

### 3.5 Sensitivity Bound

After direction-preserving clipping, the L2 sensitivity of the gradient (for privacy analysis) is:

```
S_t = 2 × C_t
```

The factor of 2 arises because adding or removing one sample changes the gradient by at most 2C_t (one gradient with the sample, one without, each clipped to C_t).

**Note:** Because C_t varies per round (and potentially per client), the sensitivity is **adaptive**. The privacy analysis must account for this — the worst-case sensitivity is 2 × C_max, but the typical sensitivity is lower.

---

## 4. Mechanism 2: Quantile-based Selective Gradient Perturbation

### 4.1 Motivation

Standard DP-FL adds Gaussian noise to the **entire** gradient vector:

```
g̃ = ḡ + N(0, σ²I)
```

This applies noise to all d dimensions equally. But the d dimensions are not equally risky:
- **High-information dimensions:** Large magnitude, high variance across samples, strong contribution to loss reduction. These are the dimensions an adversary would target for gradient leakage or model inversion attacks. They **need** noise.
- **Low-information dimensions:** Near-zero magnitude, low variance, minimal contribution. These carry little extractable private information. Noise on them is **wasted privacy budget**.

Fed-ADS adds noise only to the high-information dimensions, identified via quantile analysis.

### 4.2 Per-Neuron Dimension Selection

For each neuron in the model, the gradient dimensions are ranked by an **information content score**. The information content of dimension j in neuron n is computed as:

```
I(n, j) = |g_{n,j}| × V(n, j)
```

Where:
- **|g_{n,j}|** = magnitude of the gradient at dimension j of neuron n
- **V(n, j)** = variance of g_{n,j} across the per-sample gradients in the batch (high variance = this dimension varies a lot per sample = high information content about individual samples = high leakage risk)

Alternative formulations:
- Magnitude-only: I(n, j) = |g_{n,j}|
- Loss contribution: I(n, j) = |∂L/∂g_{n,j}| × |g_{n,j}|
- Combined: weighted sum of magnitude, variance, and loss contribution

### 4.3 Quantile Threshold

For each neuron n, sort dimensions by I(n, j) in descending order and select the top-q quantile:

```
S_q(n) = { j : I(n, j) ≥ Q_q(I(n, ·)) }
```

Where Q_q is the q-th quantile of the information content distribution for neuron n.

**Example:** If q = 0.2 (top 20%), only the highest-information 20% of dimensions in each neuron receive noise. The remaining 80% are left noise-free.

### 4.4 Selective Noise Application

```
g̃_{n,j} = ḡ_{n,j} + N(0, σ²)   if j ∈ S_q(n)   (selected, high-risk)
g̃_{n,j} = ḡ_{n,j}              if j ∉ S_q(n)   (not selected, low-risk)
```

**Key property:** The noise is applied **only** to the selected dimensions. The unselected dimensions are transmitted and aggregated without any noise, preserving their full signal.

### 4.5 Privacy Implication of Selective Perturbation

The privacy guarantee is computed over the **selected subspace** only:

- The sensitivity of the selected dimensions is S_selected = 2C_t (same bound, but now over fewer dimensions).
- The Gaussian mechanism on the selected subspace with noise scale σ provides (ε_t, δ_t)-DP for the selected dimensions.
- The unselected dimensions are not perturbed, but they carry low information content → low leakage risk → the privacy loss from not perturbing them is bounded and small.

**Formally:** The selective perturbation mechanism satisfies (ε_t, δ_t)-DP if:

```
σ ≥ (S_selected × sqrt(2 × ln(1.25/δ_t))) / ε_t
```

This is the **same** noise scale as global perturbation, but applied to fewer dimensions. The result:
- **Same privacy guarantee** on the protected (selected) dimensions.
- **Zero noise** on the unprotected (unselected) dimensions — strict utility improvement.
- The overall privacy guarantee holds because the unprotected dimensions leak negligible information (by construction — they were selected out precisely because they have low information content).

### 4.6 Quantile Parameter Selection

The quantile threshold q is a key hyperparameter:
- **q = 1.0:** All dimensions perturbed (equivalent to global DP-FL, no selectivity benefit).
- **q = 0.0:** No dimensions perturbed (no privacy protection — degenerate case).
- **q = 0.1–0.3:** Typical range — perturb the top 10-30% of dimensions, protect the high-risk subset, preserve 70-90% of the signal.

The optimal q depends on:
- **Gradient sparsity:** If gradients are naturally sparse (many near-zero dimensions), low q is safe (few dimensions need protection).
- **Model architecture:** Deeper models with more neurons may allow lower q per neuron.
- **Privacy budget:** Tighter ε may require higher q (more dimensions protected) to maintain the overall guarantee.

---

## 5. Mechanism 3: Dynamic Privacy Budget Allocation

### 5.1 Motivation

The privacy budget ε is a finite resource. How it is allocated across T communication rounds affects both privacy and utility:
- **Uniform allocation** (ε/T per round): Simple but ignores training dynamics. Over-protects late training (wasting budget on a converged model), under-protects early training (where overfitting risk is highest).
- **Front-loaded allocation** (more ε early, less late): Good for preventing early overfitting, but starves late training of signal.
- **Back-loaded allocation** (less ε early, more late): Good for final accuracy, but leaves early training vulnerable to memorization.

Fed-ADS uses a **non-linear dynamic allocation** that is strict early and relaxed late, with rapidly decaying increments — capturing the benefits of both front-loaded (early protection) and back-loaded (late signal) without their drawbacks.

### 5.2 Allocation Schedule

```
ε_t = ε_min + (ε_max - ε_min) × (1 - exp(-α × t / T))

where:
  ε_t     = privacy budget allocated to round t (t = 0, 1, ..., T-1)
  ε_min   = minimum budget at t = 0 (strict privacy, early training)
  ε_max   = asymptotic maximum budget as t → T (relaxed privacy, late training)
  α       = decay rate (controls how quickly the budget relaxes)
  T       = total number of communication rounds
```

**Schedule properties:**

| Round | ε_t | Phase | Rationale |
|-------|-----|-------|-----------|
| t = 0 | ε_min | Early (strict) | Maximum noise, prevents overfitting to early batches, regularizes |
| t = T/4 | ε_min + (ε_max - ε_min) × (1 - exp(-α/4)) | Early-mid | Relaxing, but still strict |
| t = T/2 | ε_min + (ε_max - ε_min) × (1 - exp(-α/2)) | Mid | Balanced |
| t = 3T/4 | ε_min + (ε_max - ε_min) × (1 - exp(-3α/4)) | Mid-late | Mostly relaxed |
| t → T | → ε_max | Late (relaxed) | Minimal noise, accelerates convergence, improves accuracy |

### 5.3 Decaying Increments

The per-round increment is:

```
Δε_t = ε_{t+1} - ε_t = (ε_max - ε_min) × exp(-α × t / T) × (α / T)
```

**Key property:** The increment Δε_t decays exponentially with t:
- **Early rounds:** Large increments (budget relaxes quickly from the strict minimum).
- **Late rounds:** Small increments (budget is near ε_max, further relaxation is marginal).

This means:
- The transition from strict to relaxed is **front-loaded within the relaxation** — the budget relaxes quickly after the initial strict phase, giving mid-to-late training the signal it needs.
- The late rounds see minimal further relaxation, avoiding a sudden noise cliff at the end.

### 5.4 Non-Linear Allocation

The schedule is **non-linear** (exponential approach to ε_max):
- **Linear allocation** (ε_t = ε_min + (ε_max - ε_min) × t/T) would give constant increments — too gradual early, too slow late.
- **Exponential allocation** front-loads the relaxation, which is better matched to training dynamics (the model needs signal most in the mid-to-late phase, not uniformly).

### 5.5 Total Budget Composition

The total privacy guarantee is computed via composition of the per-round budgets:

```
Total ε = Σ_{t=0}^{T-1} ε_t  (basic composition)
```

Or tighter via advanced composition / RDP:

```
Total (ε_total, δ) where ε_total ≤ Σ ε_t (advanced composition gives a sqrt-T improvement)
```

**Constraint:** The parameters (ε_min, ε_max, α, T) must be chosen such that the composed total does not exceed the desired overall privacy budget ε_total.

**Practical approach:** Given a target ε_total and T, solve for (ε_min, ε_max, α) such that:
1. Σ ε_t ≤ ε_total (composition constraint)
2. ε_min is small enough for meaningful early protection
3. ε_max is large enough for meaningful late signal
4. α produces the desired relaxation speed

---

## 6. Privacy Analysis

### 6.1 (ε, δ)-Differential Privacy Guarantee

**Definition:** A randomized mechanism M satisfies (ε, δ)-differential privacy if for all datasets D and D' differing in one record, and for all S ⊆ Range(M):

```
Pr[M(D) ∈ S] ≤ exp(ε) × Pr[M(D') ∈ S] + δ
```

### 6.2 Sensitivity Bound

After direction-preserving adaptive clipping with threshold C_t:

```
S_t = 2C_t
```

The factor of 2 arises because adding or removing one sample changes the gradient by at most 2C_t (the gradient with the sample, clipped to C_t, minus the gradient without, clipped to C_t).

**Worst case:** S_max = 2 × C_max (when cos_sim = +1, the most relaxed threshold).
**Typical case:** S_t < S_max for most rounds (when cos_sim < 1).

For privacy accounting, the worst-case sensitivity S_max is used to ensure the guarantee holds for all possible C_t values.

### 6.3 Gaussian Mechanism

For round t with budget (ε_t, δ_t), the Gaussian mechanism adds noise:

```
σ_t ≥ (S_t × sqrt(2 × ln(1.25/δ_t))) / ε_t
```

This ensures the perturbation step satisfies (ε_t, δ_t)-DP for that round.

### 6.4 Composition

Across T rounds, the total privacy guarantee is:

**Basic composition:**
```
(Σ ε_t, Σ δ_t)-DP
```

**Advanced composition:** For T rounds of (ε_t, δ_t)-DP:
```
(Σ ε_t + sqrt(2T × ln(1/δ')) × max(ε_t), T × δ_t + δ')-DP
```

**RDP-based composition (tighter):** If each round satisfies (α, ε_t)-RDP, the total is (α, Σ ε_t)-RDP, converted to (ε_total, δ)-DP:

```
ε_total = Σ ε_t + log(1/δ) / (α - 1)
```

The dynamic budget allocation (mechanism 3) means ε_t varies per round, but the composition theorems apply regardless — the total is bounded by the sum (or tighter RDP bound).

### 6.5 Selective Perturbation Privacy

For the quantile-based selective perturbation:
- **Selected dimensions (S_q):** Protected by the Gaussian mechanism with noise σ_t, satisfying (ε_t, δ_t)-DP.
- **Unselected dimensions (complement of S_q):** Not perturbed. Privacy loss is bounded because these dimensions have low information content (low magnitude, low variance) → low sensitivity to any individual sample → the unperturbed output leaks negligible private information.

**Formal argument:** The unselected dimensions have gradients bounded by the quantile threshold Q_q, which is low by construction. The sensitivity of these dimensions is small, and without noise, the output on these dimensions is a deterministic function of the (already clipped) gradient. The privacy loss is bounded by the sensitivity-to-signal ratio, which is small for low-information dimensions.

### 6.6 Overall Guarantee

Fed-ADS satisfies (ε_total, δ)-DP where:
- ε_total is the composed budget across all rounds (bounded by the dynamic allocation schedule).
- δ is the composition of per-round δ_t values.
- The guarantee holds for both the selected (perturbed) and unselected (unperturbed) dimensions.

---

## 7. Experimental Setup

### 7.1 Datasets

| Dataset | Model | Classes | Image Size | Notes |
|---------|-------|---------|------------|-------|
| MNIST | CNN (2 conv + 2 FC) | 10 | 28×28 | Standard DP-FL benchmark |
| Fashion-MNIST | CNN (same architecture) | 10 | 28×28 | Harder than MNIST, clothing categories |
| CIFAR-10 | CNN / ResNet variant | 10 | 32×32×3 | Significantly harder, color images |

### 7.2 Federated Learning Configuration

| Parameter | Value |
|-----------|-------|
| Number of clients | 20 |
| Client participation | 100% (all clients participate every round) |
| Optimizer | Momentum SGD |
| Data distribution | Non-IID (Dirichlet partitioned and/or label-skew) |
| Communication rounds | Varies by dataset and privacy setting |
| Local epochs | Standard (1-5 per round) |
| Batch size | Standard DP-FL batch sizes |

### 7.3 Privacy Settings

Multiple (ε, δ) configurations tested, spanning:
- **Strict privacy:** Low ε (e.g., ε = 1-2), testing the framework under tight budgets where the privacy-utility tradeoff is most challenging.
- **Moderate privacy:** Medium ε (e.g., ε = 5-8), typical deployment settings.
- **Relaxed privacy:** Higher ε (e.g., ε = 10+), for comparison and to show the convergence behavior.

δ is set to a standard small value (e.g., δ = 10⁻⁵, calibrated to dataset size).

### 7.4 Baselines

Fed-ADS is compared against:

| Baseline | Description | Why Included |
|----------|-------------|--------------|
| FedAvg | Standard federated averaging, no DP | Non-private upper bound on accuracy |
| FedProx | FL with proximal term for non-IID robustness | Non-IID-aware FL without DP |
| DP-SGD (fixed noise) | Standard DP-FL with fixed C and uniform ε/T | The default DP-FL baseline |
| AdaClip / Adap-Clip | Adaptive clipping (magnitude-based, Adap-Clip from Andrew et al. 2021) | State-of-the-art adaptive clipping (no direction awareness) |
| Adap-Clip | Alternative adaptive clipping variant | Another adaptive clipping comparison |
| Dynamic DP-SGD | DP-SGD with dynamic (time-varying) noise schedule | Tests dynamic budget alone (without direction-preserving clipping or selective perturbation) |

### 7.5 Evaluation Metrics

- **Test accuracy** at target ε (primary metric)
- **Accuracy vs. ε curve** (privacy-utility frontier)
- **Convergence speed** (rounds to reach X% accuracy)
- **Communication efficiency** (rounds needed, bytes transmitted)
- **Ablation:** accuracy with each mechanism individually and in pairs, to isolate contributions

---

## 8. Results

### 8.1 Accuracy at Equivalent Privacy

Fed-ADS achieves superior test accuracy at equivalent (ε, δ) privacy budgets across all three datasets:

| Dataset | FedAvg (no DP) | DP-SGD (fixed) | AdaClip | Dynamic DP-SGD | **Fed-ADS** |
|---------|---------------|----------------|---------|----------------|-------------|
| MNIST (CNN) | ~99% (upper bound) | Degraded | Moderate | Moderate | **Best DP-FL accuracy** |
| Fashion-MNIST | ~90% (upper bound) | Significantly degraded | Moderate | Moderate | **Best DP-FL accuracy** |
| CIFAR-10 | ~85% (upper bound) | Severely degraded | Low | Low-moderate | **Best DP-FL accuracy** |

**Key findings:**
- The largest gains are on CIFAR-10 (the hardest dataset), where fixed-noise DP-FL fails most severely — Fed-ADS's adaptive mechanisms provide the most benefit where the privacy-utility tradeoff is most challenging.
- On MNIST, all methods achieve reasonable accuracy, but Fed-ADS still leads due to the dynamic budget allocation improving late-stage convergence.
- The gap between Fed-ADS and baselines widens as ε tightens (stricter privacy → more benefit from adaptive mechanisms).

### 8.2 Non-IID Robustness

Under non-IID data distributions (label-skew, Dirichlet partitioning):
- **Fixed-noise DP-FL:** Accuracy collapses under severe non-IID (gradient misalignment + noise = convergence failure).
- **Fed-ADS:** Direction-preserving clipping mitigates gradient misalignment, and selective perturbation preserves signal in the dimensions that matter → significantly better non-IID robustness.

### 8.3 Ablation Study (Mechanism Contributions)

| Configuration | Clipping | Perturbation | Budget | Relative Accuracy |
|---------------|----------|-------------|--------|-------------------|
| Full Fed-ADS | Direction-preserving | Quantile-selective | Dynamic | **100% (baseline)** |
| – No direction-preserving | Fixed C | Quantile-selective | Dynamic | Reduced |
| – No selective perturbation | Direction-preserving | Global | Dynamic | Reduced |
| – No dynamic budget | Direction-preserving | Quantile-selective | Uniform | Reduced |
| – Only clipping | Direction-preserving | Global | Uniform | Reduced |
| – Only perturbation | Fixed C | Quantile-selective | Uniform | Reduced |
| – Only budget | Fixed C | Global | Dynamic | Reduced |
| Fixed-noise DP-SGD | Fixed C | Global | Uniform | Lowest |

**Key ablation findings:**
- All three mechanisms contribute; removing any one degrades accuracy.
- The **direction-preserving clipping** contributes most under non-IID settings (where direction misalignment is the primary problem).
- The **selective perturbation** contributes most under tight ε (where budget efficiency is critical).
- The **dynamic budget** contributes most in long training runs (where phase-aware allocation has time to compound).
- The mechanisms are **synergistic**: the full framework outperforms the sum of individual mechanism contributions, because they operate at complementary granularities.

### 8.4 Convergence Speed

Fed-ADS converges faster than fixed-noise DP-FL:
- Direction-preserving clipping reduces the number of rounds needed for convergence (aligned gradients make more progress per round).
- Dynamic budget allocation accelerates late-stage convergence (relaxed noise → larger effective learning rate).
- Fewer communication rounds → lower communication cost → better for bandwidth-constrained deployments.

---

## 9. Related Work

### 9.1 Federated Learning Foundations

- **FedAvg** (McMahan et al., 2017): The foundational FL algorithm — clients train locally, server averages model updates. Fed-ADS uses FedAvg-style aggregation but with adaptive DP protection.
- **FedProx** (Li et al., 2020): Adds a proximal term to the local objective for non-IID robustness. Fed-ADS addresses non-IID via direction-preserving clipping rather than a proximal term — complementary approaches.

### 9.2 Differential Privacy in FL

- **DP-SGD** (Abadi et al., 2016): The foundational DP deep learning method — per-sample gradient clipping + Gaussian noise. Fed-ADS extends this to FL with adaptive, direction-aware clipping.
- **Dynamic DP-SGD**: Time-varying noise schedules in DP-SGD. Fed-ADS's dynamic budget allocation is a more principled version — non-linear with decaying increments, coordinated with the other two mechanisms.

### 9.3 Adaptive Clipping Methods

- **AdaClip / Adap-Clip** (Andrew et al., NeurIPS 2021): Adaptive clipping threshold based on the unclipped gradient fraction. Magnitude-only, no direction awareness. Fed-ADS's direction-preserving clipping is direction-aware.
- **SlaClip** (Zou et al., ICML 2026): Adaptive clipping for centralized DP-SGD using a free Slack Indicator (binned CDF estimate). Zero extra privacy budget. Designed for centralized DP-SGD, not FL. See `slaclip-adaptive-clipping-dp-sgd`.
- **DP-LAC** (2026): Lightweight adaptive clipping for DP-FL LLM fine-tuning, using server-side validation loss. Zero extra hyperparameters. Adapts C based on server-side loss, not client-side gradient direction. See `dp-lac-lightweight-adaptive-clipping`.

### 9.4 Layer-wise and Dimension-wise DP

- **LaDP-FL** (Li et al., 2026): Layer-wise adaptive noise injection using KL divergence between local and global model distributions. Operates at the layer granularity. Fed-ADS operates at the dimension granularity (finer). See `ladp-fl-layer-wise-differential-privacy`.
- **SHELD-FL** (2026): Self-learning heterogeneous DP with per-client noise based on data sensitivity. Operates at the client granularity. Fed-ADS operates at the direction/dimension/phase granularity. See `sheld-fl-self-learning-heterogeneous-dp-framework`.

### 9.5 Secure Aggregation + DP

- **HEAD-FL** (Seeydi et al., 2026): Round-adaptive DP (RDP-based) with verifiable homomorphic aggregation. Adds cryptographic guarantees that Fed-ADS does not provide. See `head-fl-adaptive-dp-homomorphic-aggregation`.

### 9.6 Comparison Table

| Method | Setting | Clipping Adaptation | Noise Scope | Budget Allocation | Granularity | Extra Privacy Cost |
|--------|---------|---------------------|-------------|-------------------|-------------|-------------------|
| FedAvg | FL | None | None | N/A | N/A | None |
| DP-SGD | Centralized/FL | Fixed | Global | Uniform | Full gradient | Standard |
| Adap-Clip | FL | Adaptive (magnitude) | Global | Uniform | Full gradient | Yes (extra queries) |
| SlaClip | Centralized | Adaptive (CDF-based) | Global | Uniform | Full gradient | None |
| DP-LAC | FL (LLM) | Adaptive (loss-based) | Global | Uniform | Full gradient | None |
| LaDP-FL | FL | Fixed | Layer-wise (KL) | Uniform | Per-layer | Standard |
| SHELD-FL | FL | Fixed | Per-client | Dynamic (per-client) | Per-client | Standard |
| HEAD-FL | FL | Fixed | Global | Dynamic (RDP) | Full gradient | Standard |
| **Fed-ADS** | **FL** | **Adaptive (direction)** | **Selective (quantile)** | **Dynamic (non-linear)** | **Per-direction, per-dimension, per-phase** | **Standard** |

---

## 10. Limitations and Future Directions

### 10.1 Limitations

1. **100% client participation assumed:** The experimental setup uses 20 clients with 100% participation. In real deployments, client availability is partial and dynamic. The direction-preserving clipping's historical update direction h_t may be less reliable when different clients participate each round (the aggregated update direction is noisier).

2. **CNN architectures only:** Experiments use CNN models. Large-scale transformer architectures (LLMs, ViTs) are not tested. The direction-preserving clipping and quantile-based perturbation should in principle transfer, but the optimal quantile parameter q and decay rate α may differ for transformers.

3. **Quantile parameter sensitivity:** The quantile threshold q is a key hyperparameter. Too low → insufficient privacy protection. Too high → no selectivity benefit. The optimal q may vary by dataset, model, and privacy setting, requiring some tuning.

4. **Historical direction maintenance overhead:** The EWMA of past updates (h_t) requires storing a model-sized vector and updating it each round. For very large models, this is a non-trivial memory overhead on the server.

5. **No cryptographic guarantees:** Fed-ADS provides statistical privacy (DP) but not cryptographic privacy (secure aggregation, homomorphic encryption). A malicious server could still observe perturbed gradients. For settings requiring both, combine with HEAD-FL-style secure aggregation.

6. **Composition tightness:** The total privacy guarantee depends on the composition theorem used. Basic composition is conservative; RDP-based composition is tighter but requires RDP-compatible noise calibration. The dynamic budget allocation complicates RDP accounting slightly (per-round ε_t varies).

7. **Selective perturbation privacy argument:** The claim that unselected (low-information) dimensions leak negligible information is intuitive but relies on the information-content score being a good proxy for leakage risk. Adversaries with auxiliary information might extract more from "low-information" dimensions than the score predicts.

### 10.2 Future Directions

1. **Partial participation extension:** Extend Fed-ADS to handle partial and dynamic client participation, with robustified historical direction maintenance (e.g., weighting h_t by participating client count).

2. **Transformer / LLM validation:** Test Fed-ADS on transformer architectures (BERT, GPT-style LLMs, ViTs) in FL settings, with adapted quantile parameters for attention heads and embedding layers.

3. **Adaptive quantile selection:** Make the quantile threshold q adaptive (per-round or per-neuron) rather than fixed — e.g., relax q in late training (fewer dimensions need protection as the model converges) or tighten q for neurons with high activation variance.

4. **Combination with secure aggregation:** Integrate Fed-ADS with secure aggregation protocols (SMPC, HE) to provide both statistical (DP) and cryptographic privacy — a Fed-ADS + HEAD-FL hybrid.

5. **Per-layer Fed-ADS + LaDP-FL hybrid:** Combine Fed-ADS's direction/dimension/phase adaptation with LaDP-FL's layer-wise KL-divergence-based privacy estimation — a multi-granularity adaptive DP-FL framework.

6. **RDP accounting for dynamic budget:** Develop a tighter RDP-based privacy accountant specifically for the non-linear dynamic budget allocation schedule, potentially achieving a better composed ε_total than basic composition.

7. **Communication-aware quantile selection:** Tie the quantile threshold q to communication constraints — when bandwidth is limited, select fewer dimensions (lower q) to reduce the perturbed payload size, trading some privacy for communication efficiency.

---

## 11. Mathematical Summary

### 11.1 Key Formulas

| Component | Formula | Description |
|-----------|---------|-------------|
| Cosine similarity | cos_sim(g_t, h_t) = (g_t · h_t) / (\|\|g_t\|\| × \|\|h_t\|\|) | Direction alignment between current gradient and history |
| Historical direction | h_t = β × h_{t-1} + (1-β) × Δw_{t-1} | EWMA of past global updates |
| Adaptive threshold | C_t = C_base × f(cos_sim) | Direction-aware clipping threshold |
| Clipped gradient | g̃_t = g_t × min(1, C_t / \|\|g_t\|\|) | Direction-preserving clip |
| Sensitivity | S_t = 2C_t | L2 sensitivity after clipping |
| Selective perturbation | g̃_j = ḡ_j + N(0, σ²) if j ∈ S_q; else ḡ_j | Noise only on selected dimensions |
| Dynamic budget | ε_t = ε_min + (ε_max - ε_min) × (1 - exp(-αt/T)) | Non-linear phase-aware allocation |
| Budget increment | Δε_t = (ε_max - ε_min) × exp(-αt/T) × (α/T) | Decaying per-round increment |
| Noise scale | σ_t ≥ S_t × sqrt(2 ln(1.25/δ_t)) / ε_t | Gaussian mechanism calibration |
| Total privacy | (Σ ε_t, δ)-DP (basic) or tighter (RDP) | Composed guarantee across rounds |

### 11.2 Theorem Summary

| Theorem | Statement | Significance |
|---------|-----------|-------------|
| Sensitivity bound | S_t = 2C_t ≤ 2C_max | Bounds gradient sensitivity after adaptive clipping |
| Gaussian mechanism | σ_t ≥ S_t √(2 ln(1.25/δ_t)) / ε_t | Ensures (ε_t, δ_t)-DP per round |
| Composition | Total (ε, δ) via basic/advanced/RDP composition | Aggregates per-round guarantees |
| Selective perturbation | (ε_t, δ_t)-DP on selected dims; negligible leak on unselected | Justifies dimension-selective noise |
| Convergence | Direction-preserving clipping maintains convergence under non-IID | Clipping aligned gradients less preserves the descent direction |

---

## 12. Citation

```
Liu, P., Chu, B., Shen, Z., & Wang, H. (2026). An adaptive mechanism for
privacy–utility trade-off in federated learning. Information Sciences,
Vol. 753, 123644. DOI: 10.1016/j.ins.2026.123644.
```

---

## 13. Cross-References to Existing A-Tech Privacy Skills

| Skill | Relationship | Key Difference |
|-------|-------------|----------------|
| `slaclip-adaptive-clipping-dp-sgd` | Adaptive clipping analog for centralized DP-SGD | SlaClip is for centralized DP-SGD (zero extra privacy budget, CDF-based); Fed-ADS is for FL (direction-aware, cosine similarity-based) |
| `ladp-fl-layer-wise-differential-privacy` | Layer-wise adaptive DP for FL | LaDP-FL adapts at layer granularity (KL divergence); Fed-ADS adapts at dimension/direction/phase granularity |
| `head-fl-adaptive-dp-homomorphic-aggregation` | Round-adaptive DP + homomorphic aggregation for FL | HEAD-FL adds cryptographic guarantees (HE); Fed-ADS adds direction/dimension selectivity without crypto overhead |
| `dp-lac-lightweight-adaptive-clipping` | Lightweight adaptive clipping for DP-FL LLM fine-tuning | DP-LAC adapts C via server-side validation loss (zero hyperparameters); Fed-ADS adapts C via client-side gradient direction |
| `sheld-fl-self-learning-heterogeneous-dp-framework` | Self-learning heterogeneous DP for FL | SHELD-FL adapts per-client (data sensitivity, BOA); Fed-ADS adapts per-direction/dimension/phase |
| `differential-privacy-synthetic-data` | DP for synthetic data generation | Different application (data generation vs. FL training) |
| `federated-learning-for-privacy-preserving-ai` | General FL privacy overview | Fed-ADS is a specific advanced mechanism within this space |
| `google-gboard-private-fl-dp` | Production FL+DP system (Google) | Fed-ADS could enhance Gboard's DP component with adaptive mechanisms |
| `federated-llm-on-device-personalization` | FL for on-device LLM personalization | Fed-ADS's adaptive DP could improve privacy-utility for federated LLM fine-tuning |
| `bitnet-on-device-training-framework` | 1-bit on-device training | Fed-ADS + BitNet = privacy-preserving on-device FL with quantized weights |
| `ftte-federated-tiny-training-engine` | Tiny training engine for FL on constrained devices | Fed-ADS's communication efficiency (selective perturbation) complements FTTE's resource efficiency |