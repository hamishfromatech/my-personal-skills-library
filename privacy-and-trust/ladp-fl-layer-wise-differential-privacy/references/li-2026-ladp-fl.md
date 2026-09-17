# LaDP-FL: Local Layer-wise Differential Privacy in Federated Learning — Reference

## Bibliographic Information
- **Authors**: Li, Gui, Meng, Wu
- **Affiliation**: Shanghai Jiao Tong University
- **Source**: arXiv, 2026
- **Title**: LaDP-FL: Local Layer-wise Differential Privacy in Federated Learning

---

## 1. Problem Statement

Traditional differentially private federated learning (DP-FL) methods inject noise uniformly across the entire model. This one-size-fits-all approach suffers from a fundamental limitation: it treats all layers as equally sensitive, even though:

1. **Layers contribute unevenly to model utility** — some layers are critical for prediction accuracy, while others carry little predictive value.
2. **Layer-wise privacy leakage varies** — the amount of private information exposed differs across layers depending on how much a client's local model has diverged from the global model.

Uniform noise injection therefore over-protects non-sensitive layers (wasting privacy budget and degrading accuracy) while potentially under-protecting highly sensitive layers. LaDP-FL addresses this by introducing a layer-wise adaptive noise injection mechanism.

---

## 2. Key Insights

### Insight 1: Uneven Layer Contribution to Utility
Neural network layers do not contribute equally to model accuracy. Layers with larger weight norms are more influential in prediction, while layers with small weight norms have minimal impact on the global model's predictive capability. This means:
- Large-weight layers are **crucial for utility** → should receive less noise to preserve accuracy
- Small-weight layers contribute little to prediction → adversaries gain little from attacking them → can be pruned from noise injection entirely, preserving utility at no meaningful privacy cost

### Insight 2: Quantifiable Layer-Wise Privacy Leakage
The privacy risk of each layer can be quantified using **KL divergence** between the local model's parameter distribution and the global model's parameter distribution for that layer:
- **Low KL divergence** (local ≈ global): The layer has not learned much client-specific information, meaning it closely mirrors the global aggregate. Paradoxically, this represents **higher privacy risk** because the layer's parameters are more "predictable" and an adversary could more easily infer the direction of private updates. → Needs **more noise**.
- **High KL divergence** (local ≠ global): The layer has diverged significantly from the global model, meaning it encodes substantial client-specific (potentially non-private) learning. This divergence means the layer carries **less directly extractable private information** relative to its deviation. → Needs **less noise** to preserve the useful learned information.

This inverse relationship is the core innovation of LaDP-FL.

---

## 3. Methodology

### 3.1 Layer Selection Module

**Goal**: Identify which layers are important enough to warrant noise injection.

**Process**:
1. For each layer j in client i's model, compute the L₂ norm of the weight matrix: ||w^i_j||₂
2. Compare against a threshold R:
   - If ||w^i_j||₂ > R → layer is **selected** for noise injection (important for prediction)
   - If ||w^i_j||₂ ≤ R → layer is **pruned** from noise injection (low importance, low attack value)
3. R is a hyperparameter that controls the selectivity of the mechanism.

**Rationale**: Adversaries attempting model inversion or gradient leakage attacks rely on the predictive capability of model parameters. Layers with small weights offer little predictive value and thus minimal attack surface. By excluding them from noise injection, LaDP-FL preserves model utility without meaningfully increasing privacy risk.

### 3.2 Privacy Estimation Module

**Goal**: Quantify the privacy sensitivity of each selected layer.

**Process**:
1. Extract parameter matrices for layer j from:
   - Local model: w^i_j (client i's updated weights)
   - Global model: w^g_j (aggregated global weights)
2. Flatten both matrices into vectors
3. Normalize using softmax to convert to probability distributions
4. Compute KL divergence: KL(w^i_j || w^g_j)
5. Apply clipping boundary: P_i,j = min(KL(w^i_j || w^g_j), B)

**Clipping boundary B**: Prevents P_i,j from becoming unbounded, which would drive noise to zero (eliminating privacy protection). B ensures a minimum noise floor for all selected layers.

**Interpretation of P_i,j**:
- P_i,j represents the estimated private information content of layer j in client i
- Small P_i,j → layer is similar to global → higher privacy risk → needs more noise
- Large P_i,j → layer diverges from global → lower privacy risk → needs less noise

### 3.3 Adaptive Noise Injection

**Formula**:
```
σ_i,j = c_i × Δf_i,j / (ε × P_i,j)
```

Where:
- **σ_i,j**: Standard deviation of Gaussian noise injected into layer j of client i
- **c_i**: Client-specific scaling parameter (tuned to satisfy DP guarantee, see Theorem 3)
- **Δf_i,j**: L₂ sensitivity of layer j (bounded by Theorem 2)
- **ε**: Privacy budget parameter
- **P_i,j**: Privacy estimation from KL divergence module

**Inverse relationship** (the key design principle):
- Low P_i,j (similar to global) → denominator is small → σ_i,j is **large** → stronger privacy protection
- High P_i,j (dissimilar from global) → denominator is large → σ_i,j is **small** → preserves useful divergent information

**Sensitivity bound (Theorem 2)**:
```
Δf_i,j ≤ 2 × η × E × G_c
```
Where:
- η = learning rate
- E = number of local epochs
- G_c = gradient clipping bound

This bounds the maximum sensitivity any layer can have, ensuring noise does not become unbounded.

### 3.4 Ensuring DP Guarantee (Theorem 3)

For the mechanism to satisfy (ε, δ)-differential privacy, the scaling parameter c_i must be chosen to satisfy specific conditions based on:
- The privacy budget ε
- The failure probability δ
- The clipping boundary B

**Two cases** based on the value of δ relative to a threshold:

**Case 1** (δ below threshold): c_i must be at least a value derived from ε, δ, and B to ensure the tail probability of the Gaussian noise mechanism remains bounded.

**Case 2** (δ above threshold): A different (generally less restrictive) lower bound on c_i applies.

The theorem provides the explicit mathematical conditions under which the adaptive noise injection satisfies (ε, δ)-DP, giving practitioners a concrete formula to compute the minimum c_i for their desired privacy parameters.

### 3.5 Convergence Guarantee (Theorem 4)

Under the bounded noise assumption (||n_i,j|| ≤ N_c, where n_i,j is the injected noise):

**Learning rate condition**:
```
2JN_c / G_c  >  η  >  2JN_c / (G_c - μ_L) × L × G_c / μ
```

Where:
- J = number of selected layers
- N_c = noise bound
- G_c = gradient clipping bound
- μ = strong convexity parameter (or μ_L for the L-smoothness regime)
- L = Lipschitz constant
- η = learning rate

**Guarantee**: With the learning rate in the specified range, LaDP-FL converges to a neighborhood of the optimal solution. The neighborhood size depends on the noise bound N_c and the problem parameters, providing a formal accuracy-privacy tradeoff characterization.

---

## 4. Experimental Results

### 4.1 Setup
- **Datasets**: CIFAR-10, CIFAR-100
- **Architectures**: ResNet-18, CNN
- **Data distribution**: Non-IID (including extreme non-IID scenarios)
- **Baselines compared**:
  - Full DP (uniform noise across all layers)
  - Time-Varying DP (noise varies over time but not across layers)
  - Sensitive DP (noise based on sensitivity, but not layer-wise adaptive)
  - DPA LDP (Differentially Private Adaptive Local DP)
  - AdapLDP (Adaptive Local Differential Privacy)

### 4.2 Accuracy Improvement

LaDP-FL achieves a **102.99% average improvement** across all experimental scenarios compared to baselines.

| Baseline | Accuracy Improvement |
|----------|---------------------|
| Full DP | +236.32% |
| Time-Varying DP | +144.93% |
| Sensitive DP | +102.42% |
| DPA LDP | +25.18% |
| AdapLDP | +6.10% |

**Key takeaways**:
- The largest gains are against Full DP (uniform noise), confirming that layer-wise adaptation provides massive utility benefits
- Even against the strongest baseline (AdapLDP), LaDP-FL still provides a meaningful 6.1% improvement
- Under extreme non-IID conditions, LaDP-FL provides a **25.21% accuracy improvement** over baselines, demonstrating robustness in the hardest FL scenarios

### 4.3 Noise Reduction

LaDP-FL achieves a **46.14% average noise reduction** compared to SOTA methods.

| Baseline | Noise Reduction |
|----------|----------------|
| Full DP | 69.32% reduction |
| Time-Varying DP | 51.35% reduction |
| DPA LDP | 40.54% reduction |
| AdapLDP | 68.31% reduction |

**Key takeaway**: By selectively injecting noise only into important layers and scaling it adaptively, LaDP-FL dramatically reduces the total noise injected into the model while maintaining or improving privacy guarantees.

### 4.4 Privacy Budget Efficiency

LaDP-FL achieves **63.3% lower ε accumulation** compared to Full DP.

This means that for the same number of communication rounds, LaDP-FL consumes significantly less of the privacy budget, enabling more training rounds before the budget is exhausted — a critical advantage in long-running FL deployments.

### 4.5 Defense Against Model Inversion Attacks

On CIFAR-10 (ResNet-18):
- **No protection**: FID = 52.77 (adversary can reconstruct high-quality images)
- **LaDP-FL**: FID = 121.02 (reconstructions are significantly degraded)
- **Full DP**: FID = ~102.75 (estimated from "17.58% greater protection than Full DP")

LaDP-FL provides **17.58% greater protection** against model inversion attacks than Full DP, despite using significantly less noise. This validates the core thesis: adaptive, targeted noise provides better defense than uniform noise.

---

## 5. Key Design Principles

1. **Not all layers need equal protection** — Layers with small weights carry less private information and have lower attack value. Pruning them from noise injection preserves utility at minimal privacy cost.

2. **Privacy risk varies by layer** — KL divergence between local and global model parameter distributions quantifies how much private information each layer leaks. This provides a principled, data-driven privacy estimation rather than a uniform heuristic.

3. **Adaptive noise based on privacy sensitivity** — The inverse relationship between KL divergence and required noise (low KL → more noise, high KL → less noise) ensures that protection is concentrated where it's most needed, while preserving useful divergent parameters.

4. **Preserve critical model parameters while protecting sensitive ones** — By combining importance-based layer selection (preserving large-weight layers' utility) with KL-based privacy estimation (protecting high-risk layers), LaDP-FL achieves a Pareto-optimal privacy-utility tradeoff.

---

## 6. Theoretical Summary

| Theorem | Statement | Significance |
|---------|-----------|-------------|
| Theorem 2 | Δf_i,j ≤ 2ηEG_c | Bounds layer sensitivity, preventing unbounded noise |
| Theorem 3 | Conditions on c_i for (ε,δ)-DP | Provides concrete formula for DP guarantee |
| Theorem 4 | Convergence under bounded noise | Learning rate conditions for convergence to optimal neighborhood |

---

## 7. Comparison Summary

| Metric | vs Full DP | vs Time-Varying DP | vs Sensitive DP | vs DPA LDP | vs AdapLDP |
|--------|-----------|--------------------|-----------------|-----------|------------|
| Accuracy improvement | +236.32% | +144.93% | +102.42% | +25.18% | +6.10% |
| Noise reduction | 69.32% | 51.35% | — | 40.54% | 68.31% |
| ε accumulation | -63.3% | — | — | — | — |
| Defense (FID) | +17.58% better | — | — | — | — |

---

## 8. Practical Considerations

### When to deploy LaDP-FL:
- Federated learning with neural networks (CNN, ResNet, Transformers)
- Non-IID data distributions (especially extreme non-IID)
- Privacy-utility tradeoff is critical and uniform DP is too costly
- Long training runs where ε budget management matters

### When NOT to deploy:
- Non-neural-network models (linear regression, decision trees, gradient boosting) — the layer abstraction doesn't apply
- Very small models where per-layer analysis overhead is disproportionate to the model
- Scenarios where computational overhead of KL divergence computation per layer per round is prohibitive (though this is typically small relative to gradient computation)
- When strict per-sample DP (rather than per-client DP) is required — LaDP-FL operates at the client/layer granularity

### Hyperparameters to tune:
- **R** (layer selection threshold): Controls how many layers receive noise. Higher R = fewer layers protected, more utility preserved.
- **B** (clipping boundary): Prevents noise from approaching zero for high-KL layers. Must be set to ensure minimum protection.
- **c_i** (scaling parameter): Computed from Theorem 3 conditions based on desired ε and δ.
- **ε** (privacy budget): Controls overall privacy level. Lower ε = more noise, more privacy, less utility.
- **η** (learning rate): Must satisfy Theorem 4 convergence conditions.

---

## 9. Related Work Context

LaDP-FL builds on and improves several lines of DP-FL research:

- **Full DP**: Uniform Gaussian noise across all parameters. LaDP-FL's largest improvements are here (+236% accuracy, -69% noise).
- **Time-Varying DP**: Noise varies across training rounds but not layers. LaDP-FL adds the layer dimension (+145% accuracy).
- **Sensitive DP**: Noise scaled by parameter sensitivity. LaDP-FL adds importance-based selection and KL-based privacy estimation (+102% accuracy).
- **DPA LDP**: Adaptive local DP with some layer awareness. LaDP-FL adds KL-divergence-based estimation (+25% accuracy).
- **AdapLDP**: State-of-the-art adaptive LDP. LaDP-FL's closest competitor — still provides +6.1% accuracy and -68% noise.

---

## 10. Citation

```
Li, Gui, Meng, Wu. "LaDP-FL: Local Layer-wise Differential Privacy in Federated Learning."
Shanghai Jiao Tong University, arXiv, 2026.
```