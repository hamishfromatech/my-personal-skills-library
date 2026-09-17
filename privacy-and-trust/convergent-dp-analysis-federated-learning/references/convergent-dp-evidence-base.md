# Convergent DP Analysis for Federated Learning — Evidence Base

This document collects the technical evidence, per-result analysis, related work, and cross-references supporting the `convergent-dp-analysis-federated-learning` skill.

---

## 1. The Composition Theorem Problem

Standard FL-DP privacy accounting relies on the **composition theorem**: the privacy loss of a multi-round mechanism is bounded by summing (or composing) the per-round privacy losses.

- **Tight for few rounds.** When the number of communication rounds `T` is small, composition is essentially tight — the composed bound closely matches the true privacy leakage.
- **Divergent for many rounds.** As `T` grows, the composed bound grows without bound (linearly or worse in `T`). The bound becomes **arbitrarily loose** — it no longer reflects the true privacy profile.
- **Discrepancy with experiments.** Empirical privacy audits of FL-DP systems consistently show **stable, non-divergent** privacy leakage over long training horizons. This contradicts the composition-theorem prediction and creates a counterintuitive judgment that long-term FL-DP may be unsafe — a judgment that practitioners know from experience is wrong, but could not previously refute with a tight bound.

This discrepancy is the core problem the convergent DP analysis resolves.

## 2. The f-DP Framework

**f-DP** (Dong, Roth & Su, 2022) characterizes privacy via a **tradeoff function** `f: [0,1] → [0,1]`, where `f(α)` is the minimum type-II error achievable when distinguishing two neighboring datasets subject to type-I error `≤ α`. A mechanism is `f`-DP if this tradeoff is at least `f`.

### Advantages over (ε,δ)-DP
- A tradeoff function captures the **complete shape** of the privacy loss; (ε,δ) is a single point approximation.
- Composition under f-DP is **exact** (the tradeoff function of the composition is the infimal convolution / tensor product of the component tradeoff functions), avoiding the compounding looseness of (ε,δ) composition.

### Advantages over Rényi-DP (RDP)
- RDP summarizes privacy via the Rényi divergence at a chosen order `α`; f-DP keeps the full curve.
- f-DP composes without choosing a single order, and converts losslessly to RDP.

### Why f-DP is the right tool for FL
FL is inherently a **many-round** setting, so the tightness of composition matters enormously. f-DP's exact composition plus the shifted interpolation technique (below) is what makes a convergent bound provable where composition-theorem accounting diverges.

## 3. Shifted Interpolation Technique

The key technical innovation of Sun et al. (2026). To prove that Noisy-FedAvg's cumulative privacy leakage converges, the authors construct a **shifted interpolation** between the tradeoff functions of successive communication rounds.

- **Setup.** Each round's mechanism produces a per-round tradeoff function. Naively composing these (the composition-theorem route) yields a product that degrades unboundedly.
- **Shift.** Rather than composing directly, the analysis applies a carefully chosen **shift** to the interpolation between successive tradeoff functions. The shift is constructed so that the per-round contribution is **absorbed** into a convergent geometric-like series rather than an additive one.
- **Convergence.** The shifted cumulative tradeoff function converges to a **fixed limit** as `T → ∞`. This limit is the convergent privacy bound.
- **Tightness.** The bound is shown to be **tight** — it matches the true privacy profile (confirmed experimentally), not merely an upper bound.

This technique is the bridge between "composition diverges" and "the true leakage converges."

## 4. Noisy-FedAvg Result — Tight Convergent Privacy Bound

**Noisy-FedAvg** is the standard DP variant of Federated Averaging: each client clips and noise-adds its model update before sending it to the server, which averages.

**Result.** Under the f-DP framework with shifted interpolation, Noisy-FedAvg's cumulative privacy leakage over `T` rounds has a **tight convergent bound** — a finite limit `f∞` that the tradeoff function approaches as `T → ∞`.

**Significance.**
- Resolves the counterintuitive judgment: long-term FL-DP *does* provide adequate privacy; the composition theorem was simply loose, not the mechanism unsafe.
- The bound is **tight**, not just an upper bound, so it can be used for privacy budgeting without excessive conservatism.
- Applies to **non-convex, smooth objectives** — the regime of modern deep learning FL.

## 5. Noisy-FedProx Result — Stable Constant Lower Bound

**Noisy-FedProx** adds a **proximal regularization term** (the "proxy" term) to each client's local objective, pulling local updates toward the global model.

**Result.** The proxy regularization ensures a **stable constant lower bound** on privacy leakage — a bound that does not degrade with the number of rounds `T`.

**Mechanism.** The proximal term constrains the update trajectory, which the f-DP analysis leverages to bound the tradeoff function from below by a constant. Intuitively, the regularization limits how much any single client's data can shift the global model over many rounds, capping the cumulative privacy leakage.

**Practical implication.** For long-horizon FL-DP deployments where a non-degrading privacy floor is desirable, Noisy-FedProx offers stronger formal guarantees than Noisy-FedAvg.

## 6. Conversion to Other Frameworks

All f-DP results convert **losslessly**:

- **f-DP → (ε,δ)-DP.** A mechanism is `(ε,δ)`-DP iff its tradeoff function `f` satisfies `f(α) ≥ max(0, 1 - δ - e^ε α)` for all `α`. This gives the tightest (ε,δ) pair implied by `f`.
- **f-DP → RDP.** Via the relationship between the tradeoff function and the Rényi divergence: a bound on the tradeoff function yields a bound on the Rényi divergence at each order, and vice versa.

This means practitioners can reason and prove in f-DP (where the convergent analysis lives), then **report and compose** in whichever framework their tooling, audit, or regulation requires — with no loss of tightness.

## 7. Non-Convex and Smooth Objectives

The analysis is developed for **non-convex and smooth** loss objectives, covering the deep-learning FL setting rather than only the convex case common in earlier DP-FL theory. This broadens applicability to realistic neural network training.

## 8. Implications for FL-DP System Design

- **Privacy budgeting.** Convergent bounds enable a *fixed* privacy budget that does not explode with `T`, removing the need to artificially cap rounds or over-inject noise.
- **Algorithm choice.** When a stable privacy floor matters most, Noisy-FedProx is preferable; when tight accounting of the convergent profile suffices, Noisy-FedAvg is suitable.
- **Noise calibration.** Because the true bound is convergent (not divergent), the noise required to hit a target (ε,δ) is **lower** than composition-theorem accounting suggests — recovering utility previously spent on compensating for a loose bound.
- **Auditability.** Tight, convergent bounds are more defensible in privacy audits than divergent worst-case bounds.

## 9. Related Work

- **Composition-based FL-DP analyses.** Prior work accounts for FL-DP privacy via the composition theorem (often via RDP or (ε,δ) composition). These are tight for few rounds but diverge — the problem this work resolves.
- **RDP accounting for DP-SGD / FL.** Rényi-DP-based accountants (e.g., Abadi et al. for DP-SGD; FL extensions) provide tighter composition than naive (ε,δ) but still diverge linearly in `T`.
- **f-DP framework origins.** Dong, Roth & Su (2022) introduced f-DP and its exact composition; this work is the first to apply it with shifted interpolation to obtain *convergent* bounds for general FL.
- **Proximal FL.** FedProx (Li et al., 2020) introduced proximal regularization for FL convergence/stability; this work reuses the proximal term as a privacy-stabilizing proxy.

## 10. Cross-References to Existing A-Tech Privacy Skills

- `fed-ads-adaptive-privacy-utility-tradeoff` — adaptive privacy/utility tradeoffs in federated advertising; complementary on the utility side.
- `slaclip-adaptive-clipping-dp-sgd` — adaptive clipping for DP-SGD; an accounting-side technique that pairs with convergent bounds for tighter per-round contributions.
- `head-fl-adaptive-dp-homomorphic-aggregation` — adaptive DP with homomorphic aggregation in FL; aggregation-layer complement to the analysis-layer convergent bounds here.
- `dp-fedadamw-dpfl-large-model-optimizer` — DP-aware optimizer for large federated models; convergent accounting improves the privacy budget available to such optimizers.
- `fednewton-statistical-limits-dp-federated-estimation` — statistical limits of DP federated estimation; foundational limits that interact with the convergent accounting bounds.

## 11. Citation

Sun, Y., Zhang, Q., Shen, L., & Tao, D. (2026). *"Convergent Differential Privacy Analysis for General Federated Learning."* ICLR 2026.