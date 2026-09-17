---
name: convergent-dp-analysis-federated-learning
description: Applies the first convergent differential privacy analysis for general federated learning using the f-DP framework, proving that Noisy-FedAvg has a tight convergent privacy bound and Noisy-FedProx has a stable constant lower bound. Use when evaluating long-term privacy guarantees in FL-DP systems, when composition-theorem-based privacy bounds diverge, or when designing FL systems that need reliable privacy accounting over many rounds.
---

# Convergent Differential Privacy Analysis for General Federated Learning

## Overview

This skill encodes the **first convergent differential privacy (DP) analysis for general federated learning (FL)**, resolving a long-standing discrepancy between theoretical and experimental privacy bounds in FL-DP systems.

Existing FL-DP analyses rely on the **composition theorem**, which is tight for a small number of communication rounds but yields **arbitrarily loose, divergent bounds** as the number of rounds grows. This produces a counterintuitive judgment: that FL-DP may fail to provide adequate privacy over long-term training — even though experiments consistently show **stable privacy leakage**.

This work uses the **f-DP framework** (based on tradeoff functions rather than (ε,δ)-pairs) combined with a novel **shifted interpolation technique** to prove two key results:

1. **Noisy-FedAvg** has a **tight convergent privacy bound** — the privacy leakage does not diverge, contradicting the composition-theorem-based prediction.
2. **Noisy-FedProx** has a **stable constant lower bound** on privacy, guaranteed by proxy regularization.

All results convert **losslessly** to the standard (ε,δ)-DP and Rényi-DP (RDP) frameworks, so they are directly usable by practitioners working in either accounting tradition.

### Key Insight

> Existing FL-DP analyses rely on the composition theorem, which is tight for few rounds but yields arbitrarily loose, divergent bounds eventually. This creates a counterintuitive judgment that FL-DP may not provide adequate privacy during long-term training — but experiments show stable privacy. This work resolves that discrepancy with the first convergent DP analysis for general FL.

## When to Use

- **Long-term FL-DP training** where the composition theorem yields divergent privacy bounds and the true privacy behavior must be characterized.
- When **privacy accounting must be reliable over hundreds of rounds** — e.g., production FL deployments, longitudinal medical or financial model training.
- When **designing FL systems that need formal privacy guarantees** and a defensible, convergent privacy budget rather than a worst-case-divergent one.
- When evaluating the privacy properties of **Noisy-FedAvg** or **Noisy-FedProx** specifically.
- When you need to convert f-DP results into **(ε,δ)-DP** or **RDP** terms for integration with existing privacy accounting pipelines.

## NOT For

- **Short-term FL with few rounds** — the composition theorem is already tight there; convergent analysis adds no benefit.
- **Non-FL differential privacy analysis** — the results are specific to federated learning architectures.
- When **only (ε,δ)-DP accounting is needed** and there is no requirement for f-DP reasoning or long-round convergence — standard composition suffices.

## Core Process

### 1. The f-DP Framework

Instead of expressing privacy as an (ε,δ)-pair, f-DP expresses privacy as a **tradeoff function** `f(α)` that captures the tradeoff between type-I and type-II error rates in distinguishing two neighboring datasets. This gives a **complete, tighter** characterization of privacy leakage and composes in a way that is far more amenable to long-round analysis than scalar ε-accounting.

**Advantages over (ε,δ)-DP and RDP for FL:**
- Captures the *full shape* of privacy loss, not a single scalar.
- Composition under f-DP is exact (via tensor product of tradeoff functions), avoiding the looseness that compounds in (ε,δ) composition.
- Results are **losslessly convertible** back to (ε,δ)-DP and RDP, so nothing is lost by working in f-DP first.

### 2. Shifted Interpolation Technique

A novel mathematical technique introduced in this work. It enables **proving convergent bounds** for Noisy-FedAvg by constructing a carefully shifted interpolation between the tradeoff functions of successive rounds. The shift absorbs the per-round privacy contribution in a way that the cumulative tradeoff function **converges to a fixed limit** rather than diverging — the core technical innovation that resolves the theory-experiment discrepancy.

### 3. Noisy-FedAvg Convergent Bound

Under the f-DP framework with shifted interpolation, Noisy-FedAvg's privacy leakage is shown to have a **tight convergent bound**. Concretely, the cumulative tradeoff function over `T` rounds converges as `T → ∞`, rather than degrading unboundedly as composition-theorem accounting predicts. This matches experimental observations and removes the counterintuitive judgment that long-term FL-DP is unsafe.

### 4. Noisy-FedProx Stable Constant Lower Bound

Noisy-FedProx, which adds a proximal regularization term to local updates, enjoys an even stronger guarantee: a **stable constant lower bound** on privacy leakage. The proxy (proximal) term regularizes the update trajectory, which the analysis leverages to bound privacy from below by a constant that does not degrade with the number of rounds. This makes Noisy-FedProx especially attractive for long-horizon FL-DP deployments.

### 5. Conversion to Other Frameworks

All f-DP results convert **losslessly** to:
- **(ε,δ)-DP** — via the standard tradeoff-function-to-(ε,δ) mapping.
- **Rényi-DP (RDP)** — via the Rényi divergence / tradeoff-function relationship.

This means practitioners can reason and prove in the tighter f-DP space, then report and compose in whichever framework their tooling or regulation requires.

## Analyzed Settings

The analysis covers **non-convex and smooth objectives**, making it applicable to modern deep-learning-based FL rather than only convex settings.

## Based On

Sun, Y., Zhang, Q., Shen, L., & Tao, D. (2026). *"Convergent Differential Privacy Analysis for General Federated Learning."* ICLR 2026.

## A-Tech Applications

- **A-Coder** — Long-term federated code intelligence with reliable privacy accounting. Convergent DP bounds let A-Coder's FL deployments train over many rounds without the privacy budget (theoretically) exploding, and without over-injecting noise to compensate for a divergent bound.
- **Be Practical** — A convergent DP curriculum module that teaches practitioners *why* composition bounds diverge, *how* f-DP resolves it, and *when* to prefer convergent analysis over composition.
- **Builder's Club** — An open-source convergent DP accounting library implementing the f-DP shifted-interpolation accounting for Noisy-FedAvg / Noisy-FedProx, with lossless (ε,δ)-DP and RDP export.

## A-Tech Alignment

- **Open-source** — Built on ICLR-published, peer-reviewed work and standard ML frameworks; the intended accounting library is open source.
- **Data privacy** — Core contribution: reliable, convergent long-term privacy guarantees for FL-DP, directly advancing the privacy mission.
- **Financial freedom** — Reliable privacy accounting prevents *over-spending* on unnecessary noise. A divergent bound forces practitioners to add far more noise (or use far more rounds/samples) than the true privacy profile requires, wasting compute and degrading utility. Convergent bounds restore efficiency.
- **Practical implementation** — Theoretical proofs are paired with experimental validation; results are convertible to the frameworks practitioners already use.

## Cross-References

- `fed-ads-adaptive-privacy-utility-tradeoff` — adaptive privacy/utility tradeoffs in federated advertising.
- `slaclip-adaptive-clipping-dp-sgd` — adaptive clipping for DP-SGD, complementary accounting-side technique.
- `head-fl-adaptive-dp-homomorphic-aggregation` — adaptive DP with homomorphic aggregation in FL.
- `dp-fedadamw-dpfl-large-model-optimizer` — DP-aware optimizer for large federated models.
- `fednewton-statistical-limits-dp-federated-estimation` — statistical limits of DP federated estimation.

## See Also

- `references/convergent-dp-evidence-base.md` — detailed technical evidence, related work, and per-result analysis.