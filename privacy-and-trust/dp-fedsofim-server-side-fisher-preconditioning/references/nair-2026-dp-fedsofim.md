# Nair, Sen, Sen, Banerjee (2026) — DP-FedSOFIM: Differentially Private Federated Stochastic Optimization using Regularized Fisher Information Matrix

**Authors:** Nair, Sen, Sen, Banerjee
**Affiliations:** IIT Delhi / ISI Kolkata / IIT Hyderabad / IIM Indore
**Source:** arXiv preprint, 2026

---

## Summary

DP-FedSOFIM is a server-side second-order optimization method for differentially private federated learning (DP-FL). Its central contribution is relocating all curvature estimation to the server, where it is constructed solely from already-privatized aggregated gradients. This yields three simultaneous benefits:

1. **O(d) client memory** — the client protocol is identical to standard DP-FedGD; no Hessian or curvature buffer is needed on-device.
2. **Zero additional privacy cost** — by the post-processing theorem of differential privacy, any deterministic function of an already-privatized quantity inherits the same (ε,δ)-DP guarantee. The Fisher proxy is such a function, so DP-FedSOFIM consumes no extra privacy budget beyond the underlying gradient release.
3. **4-5x communication speedup** — the second-order curvature information dramatically accelerates convergence, reducing the number of communication rounds needed to reach a target accuracy.

---

## Method

### Client Side (Identical to DP-FedGD)

Each client `i` at round `t`:

1. Computes per-example gradients on its local dataset `D_i`.
2. Clips each per-example gradient to norm `C_g` (gradient clipping).
3. Sums clipped gradients and adds Gaussian noise:
   - `E_i,t ~ N(0, (C_g σ_g)² / n I_d)` where `n = |D_i|`.
4. Normalizes and sends the privatized update:
   - `g_i,t = (S_i,t + E_i,t) / |D_i|`
   - where `S_i,t` is the sum of clipped gradients.

**Memory cost:** O(d). **No additional client computation** beyond standard DP-FedGD.

### Server Side — Curvature Proxy Construction

The server maintains a momentum buffer and constructs a rank-one Fisher Information Matrix proxy:

1. **Aggregate** privatized client updates:
   - `G_t = (1/n) Σ_i g_i,t`

2. **Momentum buffer** (exponential moving average of aggregated gradients):
   - `M_t = β M_{t-1} + (1-β) G_t`
   - `β ∈ (0,1)` is the EMA momentum parameter.

3. **Fisher Information Matrix proxy** (rank-one + regularization):
   - `Î_t = M_t × M_t^T + ρ I_d`
   - `ρ > 0` is the regularization parameter ensuring positive definiteness.
   - This is a rank-one perturbation of a scaled identity — structured enough to invert cheaply.

4. **Preconditioner via Sherman-Morrison formula**:
   - `H_t = (1/ρ) I_d − M_t M_t^T / (ρ² + ρ ||M_t||²)`
   - This is the exact inverse of `Î_t` computed in O(d) time — no O(d³) matrix inversion required.
   - Only requires `||M_t||²` (a scalar) and the rank-one outer product `M_t M_t^T`.

5. **Server-side parameter update**:
   - `θ_{t+1} = θ_t − η_t H_t G_t`
   - `η_t` is the learning rate at round `t`.

---

## Key Properties

### Zero Additional Privacy Cost

The preconditioner `H_t` is a deterministic function of the privatized aggregated gradient `G_t` (through the momentum buffer `M_t`). Since `G_t` is already (ε,δ)-DP (guaranteed by the client-side Gaussian mechanism), applying a deterministic post-processing function to it cannot weaken the privacy guarantee. Therefore:

> **DP-FedSOFIM preserves the same (ε,δ)-DP guarantee as the underlying gradient release, at no additional privacy cost.**

This is a direct consequence of the post-processing theorem of differential privacy. It is the defining advantage over client-side second-order DP methods, which must release Hessian/Jacobian information and therefore consume additional privacy budget.

### O(d) Complexity

The Sherman-Morrison formula exploits the rank-one structure of the Fisher proxy:
- Computing `||M_t||²`: O(d)
- Computing `M_t M_t^T G_t` (rank-one update applied to gradient): O(d)
- No O(d³) matrix inversion; no O(d²) matrix storage beyond the implicit rank-one representation.

Client side remains O(d) memory (identical to DP-FedGD). Server side is O(d) per round for the preconditioner computation and update.

### Momentum Buffer Noise Suppression

The EMA momentum buffer `M_t = β M_{t-1} + (1-β) G_t` accumulates signal while averaging out the per-round DP Gaussian noise. The variance of the DP noise in `M_t` is reduced by a factor of `(1-β)/(1+β)` relative to the per-round noise in `G_t`:

| β | Noise variance reduction factor |
|---|--------------------------------|
| 0.9 | ~19x |
| 0.95 | ~39x |
| 0.99 | ~199x |

This is critical under tight privacy budgets where the per-round DP noise is large. The EMA acts as a low-pass filter that preserves the signal (curvature direction) while suppressing the high-frequency noise injected by the Gaussian mechanism.

### Anisotropic Rescaling

The preconditioner `H_t` rescales the gradient anisotropically: it applies stronger contraction along the dominant gradient direction (captured by `M_t`) and weaker contraction in orthogonal directions (scaled by `1/ρ`). This curvature-aware step sizing accelerates convergence in ill-conditioned problems where the gradient is dominated by a few principal directions.

---

## Warm-Started Variant (Tight Privacy: ε ≤ 0.5)

Under very tight privacy budgets, the early-round aggregated gradients `G_t` are noise-dominated because the DP Gaussian noise must be large to satisfy the budget. Applying curvature preconditioning to noise-dominated gradients can be harmful. The warm-started variant addresses this:

- **Phase 1 (Warmup, ~20 rounds):** Disable preconditioning; use first-order updates `θ_{t+1} = θ_t − (η_t/ρ) G_t`. This lets the momentum buffer `M_t` accumulate a stable signal.
- **Phase 2 (Gradual activation):** Interpolate between first-order and second-order updates:
  - `q_t = (1 − λ_t)(1/ρ) G_t + λ_t H_t G_t`
  - where `λ_t ↑ 1` (increasing schedule, e.g., linear ramp from 0 to 1 over subsequent rounds).
- **Phase 3 (Full preconditioning):** Once `λ_t = 1`, use pure second-order updates.

This ensures the Fisher proxy is constructed from a momentum buffer that has stabilized, avoiding amplification of early-round noise.

---

## Convergence Guarantees

DP-FedSOFIM provides theoretical convergence guarantees under three problem classes:

### Strong Convexity (Theorem 4.21)
- **Assumption:** Objective is μ-strongly convex and L-smooth.
- **Guarantee:** Linear (geometric) convergence to a neighborhood of the optimum.
- **Neighborhood size:** Determined by three error sources:
  1. **DP noise** — injected by the Gaussian mechanism; scales with σ_g, clipping norm C_g, and inverse of n.
  2. **Clipping bias** — gradient clipping introduces bias when unclipped gradients exceed C_g.
  3. **Same-step coupling penalty** — the momentum buffer and preconditioner use the same step, creating a coupling between curvature estimation and the update direction.

### PL Condition (Theorem 4.29)
- **Assumption:** Objective satisfies the Polyak-Łojasiewicz (PL) condition: `(1/2)||∇f||² ≥ μ(f − f*)`. This is weaker than strong convexity (allows non-convex functions with a unique minimum).
- **Guarantee:** Extended linear convergence to a neighborhood, analogous to the strongly convex case.

### Non-Convex Smooth (Theorem 4.31)
- **Assumption:** Objective is L-smooth, possibly non-convex, with bounded gradient norm.
- **Guarantee:** O(1/T) convergence to a stationary point (min squared gradient norm decays as O(1/T)).

### Error Floor Decomposition

The asymptotic error floor has three additive sources:

```
Error floor = DP noise contribution + Clipping bias + Same-step coupling penalty
```

- **DP noise:** Decreases with more clients (averaging), increases with tighter ε (larger σ_g).
- **Clipping bias:** Decreases with larger clipping norm C_g, but larger C_g requires larger σ_g for the same ε — a fundamental DP-FL tradeoff.
- **Same-step coupling:** A structural artifact of using the current gradient both to update the momentum buffer and to compute the preconditioned step. Can be mitigated by the warm-start variant.

---

## Experimental Setup

- **Datasets:** CIFAR-10 (natural images), PathMNIST (medical histopathology)
- **Backbones:** ResNet-20, VGG-16
- **Clients:** 20 clients per experiment
- **Privacy budgets:** ε ∈ {0.5, 1, 5, 10} (with fixed δ)
- **Baselines compared:**
  - DP-FedGD (first-order, no preconditioning)
  - DP-FedAdam (adaptive learning rate)
  - DP-SCAFFOLD (variance reduction via control variates)
  - DP-FTRL (follow-the-regularized-leader)

---

## Experimental Results

### Early Convergence (Round-10 Accuracy)
- DP-FedSOFIM achieves the **best round-10 accuracy in 7 out of 8 ResNet-20 configurations**.
- The early-convergence advantage is consistent across architectures and privacy budgets, driven by the curvature-aware preconditioning accelerating the initial descent.

### Communication Savings
- **4-5x reduction in rounds-to-target** compared to DP-FedGD.
- This is the primary practical benefit: fewer communication rounds means lower bandwidth cost, lower latency, and lower client participation burden — critical in cross-device FL.

### Final Accuracy
- DP-FedSOFIM achieves the **best final-round accuracy in 12 out of 16 dataset/backbone/privacy configurations**.
- In the remaining 4 configurations, it saturates early and is matched or marginally exceeded — but reaches competitive accuracy in far fewer rounds.

### Final-Round Gains over DP-FedGD
| Configuration | Gain |
|---|---|
| CIFAR-10, ε=10 | +4.55% |
| PathMNIST, ε=10 | +5.16% |

### Graceful Degradation
- DP-FedSOFIM **retains its advantage down to ε=0.5** (the tightest privacy budget tested).
- The warm-started variant is specifically designed for this regime and maintains stable convergence where naive preconditioning would fail.

### PathMNIST Sweep
- DP-FedSOFIM **swept all 8 PathMNIST configurations** (both backbones × all four ε values).
- This is attributed to the **curvature concentration effect** (see phenomenon 5 below): PathMNIST has a more concentrated curvature spectrum, making the rank-one Fisher proxy a better approximation.

---

## Five Experimental Phenomena

### 1. Superior Early Convergence Across Architectures
DP-FedSOFIM consistently outperforms all baselines in the first ~10-20 rounds across both ResNet-20 and VGG-16. The curvature-aware preconditioning provides a "head start" that first-order methods cannot match. This is the most robust finding — it holds in every tested configuration.

### 2. Final-Round Wins in Majority of Regimes; Early Saturation in Remainder
In 12/16 configurations, DP-FedSOFIM has the best final accuracy. In the remaining 4, it saturates early (reaches its asymptotic accuracy before the final round) and is marginally exceeded by a baseline that continues to improve slowly. This suggests the rank-one proxy captures the dominant curvature but misses secondary directions that matter for the final few percentage points in some settings.

### 3. DP-FedAdam — Strongest Adaptive Baseline but Steep Early-Round Penalty
DP-FedAdam (adaptive per-coordinate learning rates via Adam-style momentum) is the strongest baseline in the final rounds of several configurations. However, it pays a steep penalty in the early rounds — its adaptive learning rates take many rounds to calibrate under DP noise, leading to slow initial progress. DP-FedSOFIM's early-convergence advantage over DP-FedAdam is its most practically significant win, since early rounds are the most expensive in cross-device FL.

### 4. DP-SCAFFOLD Degrades Under Tight Privacy; DP-FTRL Cold-Start Instability
- **DP-SCAFFOLD** relies on control variates for variance reduction. Under tight privacy (small ε), the control variates must be privatized, and the DP noise corrupts them — degrading or even reversing the variance-reduction benefit. DP-FedSOFIM does not suffer this because it has no client-side state to privatize.
- **DP-FTRL** exhibits cold-start instability — the regularized-leader framework takes many rounds to stabilize under DP noise, leading to erratic early-round behavior. DP-FedSOFIM's momentum buffer provides natural smoothing.

### 5. Dataset-Dependent Curvature Concentration
- **PathMNIST (medical histopathology):** Curvature is concentrated — the loss landscape has a few dominant directions, making the rank-one Fisher proxy an excellent approximation. This explains the complete sweep across all 8 PathMNIST configurations.
- **CIFAR-10 (natural images):** Curvature is more diffuse — the loss landscape has significant curvature in multiple directions, so the rank-one proxy captures only the dominant direction. This explains why DP-FedSOFIM saturates early in some CIFAR-10 configurations.

**Practical implication:** DP-FedSOFIM is likely to be most effective on tasks with concentrated curvature (e.g., medical imaging, structured/tabular data) and somewhat less effective on tasks with diffuse curvature (e.g., large-scale natural image classification). A spectral analysis of the loss landscape can predict effectiveness before training.

---

## Hyperparameter Tuning Guide

### ρ (Regularization Parameter)
- Controls how isotropic the preconditioner is: large ρ → H_t ≈ (1/ρ) I_d (first-order); small ρ → H_t captures the rank-one curvature.
- **Tight privacy (ε ≤ 1):** Higher ρ (more isotropic, more robust to noise).
- **Relaxed privacy (ε ≥ 5):** Lower ρ (more anisotropic, more curvature exploitation).
- **Rule of thumb:** Start with ρ ≈ ||M_t||² at convergence and tune ±1 order of magnitude.

### β (EMA Momentum)
- Controls noise suppression vs. responsiveness: high β → more smoothing but slower adaptation; low β → more responsive but noisier.
- **Tight privacy:** Higher β (0.95) for maximum noise suppression.
- **Relaxed privacy:** Lower β (0.9) for faster curvature adaptation.
- **Noise variance reduction factor:** (1-β)/(1+β).

### η (Learning Rate)
- Must be grid-searched per privacy regime — no universal default.
- Generally lower than DP-FedGD's learning rate because the preconditioner rescales gradients.
- Start at η ≈ ρ × (DP-FedGD learning rate) and tune.

### Warmup Rounds (for ε ≤ 0.5)
- ~20 rounds of first-order-only updates before activating preconditioning.
- Monitor the momentum buffer norm ||M_t|| for stability before activating.

---

## When NOT to Use DP-FedSOFIM

1. **Non-DP federated learning:** Without the privacy constraint, standard second-order methods (e.g., SOFIM, FedNew, Newton-FL) with full curvature computation are strictly better. The rank-one proxy is a compromise necessitated by the DP post-processing constraint.

2. **Client-side second-order computation is acceptable:** If clients can compute and release Hessian/Jacobian information under the privacy budget (e.g., DP-FedNew for low-dimensional problems), full second-order methods will outperform the rank-one proxy.

3. **Highly diffuse curvature:** If the loss landscape has significant curvature in many directions (e.g., very large models with diverse feature hierarchies), the rank-one proxy captures only the dominant direction and may not provide sufficient acceleration. Consider spectral analysis to check.

4. **Single-round or very few rounds:** The momentum buffer needs several rounds to stabilize. If the training budget is < 10 rounds, the warmup phase dominates and the preconditioning benefit is minimal.

---

## Related Methods

| Method | Curvature Location | Client Memory | Privacy Cost | Complexity |
|---|---|---|---|---|
| **DP-FedSOFIM** | Server (rank-one Fisher proxy) | O(d) | 0 (post-processing) | O(d)/round |
| DP-FedGD | None (first-order) | O(d) | 0 | O(d)/round |
| DP-FedAdam | Server (diagonal adaptive) | O(d) | 0 (post-processing) | O(d)/round |
| DP-SCAFFOLD | None (variance reduction) | O(d) | +1 release (control variates) | O(d)/round |
| DP-FTRL | Server (regularized leader) | O(d) | 0 | O(d)/round |
| DP-FedNew | Client (Hessian) | O(d²) | +1 release (Hessian) | O(d²)/round |
| Standard SOFIM (non-DP) | Client (full Fisher) | O(d²) | N/A | O(d²)/round |

---

## Citation

```bibtex
@article{nair2026dpfedsofim,
  title={DP-FedSOFIM: Differentially Private Federated Stochastic Optimization using Regularized Fisher Information Matrix},
  author={Nair and Sen and Sen and Banerjee},
  journal={arXiv preprint},
  year={2026},
  note={IIT Delhi / ISI Kolkata / IIT Hyderabad / IIM Indore}
}
```