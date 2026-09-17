---
name: vpdr-variance-adaptive-prototype-perturbation
description: Implements variance-adaptive noise allocation for privacy-preserving prototype-based personalized federated learning. Use when building federated learning systems with differential privacy guarantees, optimizing privacy-utility tradeoffs in prototype sharing, or deploying PFL under Local Differential Privacy constraints. NOT for centralized DP or gradient-based FL privacy.
---

# VPDR: Variance-Adaptive Prototype Perturbation & Distillation-Guided Clipping Regularization

**Source:** Wang et al., "Taming Noise-Induced Prototype Degradation for Privacy-Preserving Personalized Federated Fine-Tuning," CVPR 2026.
**Code:** github.com/yuCoryx/ProtoPFL_VPDR

## Problem: Isotropic Gaussian Prototype Perturbation (IGPP) Limitations

Prototype-based Personalized Federated Learning (ProtoPFL) frameworks protect privacy by perturbing shared class prototypes with isotropic Gaussian noise under Local Differential Privacy (LDP). The baseline approach — **Isotropic Gaussian Prototype Perturbation (IGPP)** — has two critical weaknesses:

1. **Over-perturbation of discriminative dimensions.** IGPP applies identical noise magnitude across every dimension of the prototype. However, the informative signal is concentrated in a small subset of discriminative dimensions. By flooding all dimensions equally, IGPP disproportionately degrades exactly the features that matter most for classification, while wasting noise budget on redundant dimensions that carry little class-distinguishing information.

2. **Clipping-threshold dilemma.** Before noise injection, prototypes are clipped to bound sensitivity. A high clipping threshold preserves signal but requires large noise (destroying utility); a low threshold controls noise but truncates informative prototype magnitudes. There is no isotropic threshold that escapes this tradeoff, and naive soft-clipping introduces a **norm-weight compensation shortcut**: the model inflates weight norms to counteract clipping's shrinkage, undermining the regularization intent.

## VPDR Solution

VPDR (Variance-adaptive Prototype perturbation with Distillation-guided clipping Regularization) addresses both weaknesses through two complementary modules:

### Module 1: VPP — Variance-adaptive Prototype Perturbation

VPP replaces isotropic noise with **dimension-wise adaptive noise allocation** based on how discriminative each dimension is.

**Discriminative Score Computation.**
For each dimension *d* of a class prototype, VPP computes a discriminative score from local data using intra-class and inter-class variance:

- **Intra-class variance** (`Var_intra`): spread of samples within the same class along dimension *d* — lower is better (tight clusters).
- **Inter-class variance** (`Var_inter`): spread of class means along dimension *d* — higher is better (well-separated classes).
- **Discriminative score** ≈ `Var_inter / Var_intra` (high score = dimension separates classes well).

**Private Discriminative Subspace Selection.**
Naively selecting the top-k discriminative dimensions leaks information about the local data distribution. VPP uses **oneshot Laplace Top-k**:
- Add Laplace noise calibrated to the privacy budget to the discriminative scores.
- Select the top-k noisy scores as the "discriminative subspace."
- This is a single-shot mechanism (no iterative queries), so it composes cleanly under LDP.
- The remaining dimensions form the "redundant subspace."

**Adaptive Noise Allocation.**
- **Discriminative subspace:** allocated *less* noise (lower standard deviation) — preserves the signal that matters.
- **Redundant subspace:** allocated *more* noise — spends the privacy budget where signal loss is harmless.
- The total noise budget is preserved (same overall privacy cost), but redistributed to improve utility.

### Module 2: DCR — Distillation-guided Clipping Regularization

DCR fixes the clipping-threshold dilemma and the norm-weight compensation shortcut.

**Differentiable Soft-Clipping Layer.**
- Replaces hard clipping with a smooth, differentiable soft-clipping function applied to prototype norms.
- Gradients flow through the clipping layer during training, so the model learns to produce prototypes that lie within the clip region rather than fighting against it.
- Prevents the norm-weight compensation shortcut: because clipping is differentiable, the optimizer can adjust prototype magnitudes directly instead of inflating downstream weight norms.

**EMA Teacher-Student Knowledge Distillation.**
- A teacher model (Exponential Moving Average of the student's weights) provides stable target predictions.
- The student is regularized to match the teacher's prediction distribution (KL divergence on soft labels), enforcing **prediction consistency** across the perturbation boundary.
- This smooths the decision boundary and makes the model robust to the residual noise that passes through the discriminative subspace.

## Privacy Guarantees

VPDR operates under **(ε, δ)-Local Differential Privacy (LDP)**:

- **Sequential composition:** VPP (Laplace Top-k + adaptive Gaussian noise) and DCR (clipping + distillation, which operate on already-perturbed prototypes) compose sequentially. The total privacy cost is the sum of the individual costs, bounded by the target (ε, δ).
- **Theorem (Partitioning Privacy):** The oneshot Laplace Top-k selection over the discriminative scores satisfies (ε₁, 0)-LDP.
- **Theorem (Release Privacy):** The adaptive Gaussian noise release of the prototype satisfies (ε₂, δ)-LDP, where the adaptive allocation does not weaken the guarantee relative to isotropic baseline.
- **Key result:** VPP provides LDP **no weaker than the isotropic baseline** — the adaptive noise redistribution is privacy-neutral because the total sensitivity budget is unchanged; only the allocation across dimensions differs.

See `references/vpdr-evidence-base.md` for full theorem statements and proofs sketch.

## Experimental Results

VPDR was evaluated as a plug-in module across **6 ProtoPFL frameworks** on **3 benchmarks**:

**Frameworks:** ProtoGen, FedProto, FedNH, FedProc, FedPCL, FedTAD
**Benchmarks:** Digits (digit recognition), Office-Caltech (object recognition), PACS (domain generalization)

**Headline findings:**

- **Consistent accuracy improvements** across all 6 frameworks and 3 benchmarks — VPDR is framework-agnostic.
- **Largest gains under extreme label skew** (non-IID settings where clients hold few classes). This is the regime where prototype noise hurts most, and where adaptive allocation helps most.
- VPP and DCR each contribute independently; combining them yields the best results (see ablation in references).

**Privacy attack resistance:**

- **Membership Inference Attack (MIA):** ROC-AUC driven to **~0.50** (random chance), meaning the attacker gains no signal about whether a specific sample was in training.
- **Feature-Space Hijacking (FSH) Top-1 Hit:** throttled to **~20%**, indicating prototype reconstruction attacks largely fail.

See `references/vpdr-evidence-base.md` for full results tables, ablation, and hyperparameter sensitivity.

## Computational Overhead

VPDR is deliberately lightweight:

| Stage | Runtime Increase |
|---|---|
| ProtoGen (prototype generation) | +2.2% |
| Fine-tuning | +0.2% |
| **Overall average** | **+8.1%** |

The discriminative score computation and oneshot Laplace Top-k add a small one-time cost per round; the soft-clipping layer and EMA distillation add negligible overhead during training.

## Practical Implementation

**Repository:** github.com/yuCoryx/ProtoPFL_VPDR

Integration points:

1. **Prototype generation stage** — insert VPP after prototype computation, before sharing with the server. Compute discriminative scores from local data, run oneshot Laplace Top-k, allocate noise per-dimension.
2. **Fine-tuning stage** — insert DCR's soft-clipping layer around prototype normalization; add EMA teacher-student KL loss to the local training objective.
3. **Privacy budget allocation** — split (ε, δ) between VPP's Laplace Top-k (ε₁) and Gaussian release (ε₂). Typical split: small ε₁ for selection (e.g., 0.1ε), remainder for release.

### When to use VPDR

- ✅ Prototype-based PFL under LDP constraints
- ✅ Non-IID / label-skew federated settings where prototype noise degrades accuracy
- ✅ Need to defend against MIA and FSH attacks on shared prototypes
- ✅ Framework-agnostic plug-in for existing ProtoPFL systems

### When NOT to use VPDR

- ❌ Centralized differential privacy (DP-SGD on a single server) — VPDR targets the LDP prototype-sharing setting
- ❌ Gradient-based FL privacy (e.g., DP-SGD in FedAvg) — VPDR perturbs prototypes, not gradients
- ❌ Non-prototype FL frameworks that never share class representatives
- ❌ Settings where prototypes are never shared between clients (no privacy risk to mitigate)

## Key Terminology

| Term | Meaning |
|---|---|
| ProtoPFL | Prototype-based Personalized Federated Learning |
| IGPP | Isotropic Gaussian Prototype Perturbation (baseline) |
| VPP | Variance-adaptive Prototype Perturbation (VPDR module 1) |
| DCR | Distillation-guided Clipping Regularization (VPDR module 2) |
| LDP | Local Differential Privacy |
| MIA | Membership Inference Attack |
| FSH | Feature-Space Hijacking (prototype reconstruction attack) |
| EMA | Exponential Moving Average (teacher model weights) |
| Discriminative subspace | Top-k dimensions with highest (private) discriminative scores |
| Redundant subspace | Remaining dimensions, receive higher noise allocation |

## References

- Wang et al., "Taming Noise-Induced Prototype Degradation for Privacy-Preserving Personalized Federated Fine-Tuning," CVPR 2026.
- Code: github.com/yuCoryx/ProtoPFL_VPDR
- Full mathematical formulations, theorems, and experimental tables: `references/vpdr-evidence-base.md`