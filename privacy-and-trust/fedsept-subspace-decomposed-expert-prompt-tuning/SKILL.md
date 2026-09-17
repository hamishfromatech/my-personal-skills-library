---
name: fedsept-subspace-decomposed-expert-prompt-tuning
description: Applies privacy-preserving federated multi-expert prompt tuning with subspace decomposition for heterogeneous vision-language model clients. Use when designing federated prompt tuning systems, addressing data heterogeneity in FL, or reducing DP noise in prompt-based PEFT.
---

# FedSEPT: Federated Subspace-decomposed Expert Prompt Tuning

> **Source:** Wang et al., Beihang University, ACM MM 2026. Code: [github.com/yuCoryx/FedSEPT](https://github.com/yuCoryx/FedSEPT). A privacy-preserving federated prompt-tuning framework that decomposes prompt experts into a low-rank subspace, so that only a tiny, data-independent factor is communicated and DP-perturbed — cutting communication 8×, slashing DP noise dimensionality, and preserving strong defense against both membership-inference and gradient-inversion attacks.

## What This Skill Does

This skill applies FedSEPT, a federated learning method for tuning prompt-based parameter-efficient fine-tuning (PEFT) adapters on vision-language models (VLMs) such as CLIP. The defining innovation is **subspace decomposition of the prompt experts**: each expert's full parameter tensor is never transmitted. Instead, it is factored into (1) a low-dimensional, data-independent expert factor that *is* shared and DP-perturbed, (2) a fixed public basis that never changes and never needs protection, and (3) a client-private residual that stays local. This restructuring simultaneously collapses the communication cost, the DP noise dimensionality, and the privacy-leakage surface — without sacrificing personalization or generalization.

**Use this skill when:**
- Designing federated prompt-tuning or PEFT systems for CLIP and other VLMs
- Addressing data heterogeneity (label skew, domain skew, pathological non-IID) in federated vision learning
- Reducing the dimensionality of DP noise injected into prompt gradients (the dominant utility cost of FL+DP)
- Building multi-expert federated systems where clients specialize in complementary sub-skills
- Deploying on-device personalization that must coexist with strong formal privacy guarantees (ε=1.0)
- Cutting per-round communication to a few kilobytes for bandwidth-constrained edge clients

**Do NOT use this skill for:**
- Full-model federated fine-tuning of LLMs — use `dp-lac-lightweight-adaptive-clipping` or `chain-federated-fine-tuning`
- Federated learning service architecture / business model — use `federated-learning-as-a-service-2026`
- Byzantine-robust aggregation against adversarial clients — use `federated-byzantine-robust-partial-participation`
- Distributed shard aggregation as a structural-privacy alternative to DP — use `eris-federated-shard-aggregation`

## The Privacy-Efficiency Challenge

Federated prompt tuning for heterogeneous VLM clients faces three compounding pressures, and FedSEPT's design is essentially an integrated answer to all three.

### 1. Heterogeneous clients need *different* prompts
Label skew (each client holds only a subset of classes), domain skew (medical vs. satellite vs. consumer photos), and pathological non-IID distributions mean a single global prompt is a poor fit for everyone. The natural fix is **multiple experts** per client, but naively scaling the number of experts multiplies the parameters that must be communicated and DP-perturbed every round.

### 2. DP noise scales with the dimension of the communicated update
Local differential privacy (LDP) adds Gaussian noise calibrated to the L2 sensitivity of each client's update. The noise variance grows with the **dimensionality** of the vector being protected. A full prompt of length L and embedding dimension d has MLd parameters per expert (M experts, L tokens, d dims). At d=512, L=16, M=4, that is 32,768 parameters — and the DP noise injected into each one erodes the signal. This is the single biggest reason FL+DP underperforms: the DP noise is high-dimensional and drowns out the actual learning signal.

### 3. Communication cost compounds across rounds
Transmitting a full MLd update per client per round is expensive. At the default settings above, that is roughly 64 KiB per client per round for full-prompt methods — unsustainable for bandwidth-constrained edge devices and for long federated engagements.

### The FedSEPT answer
FedSEPT reframes the problem: instead of protecting and transmitting the full expert, **decompose it** so that the only part that ever leaves the device — and the only part DP noise is added to — is a tiny low-rank factor. The basis provides a shared coordinate system for free, the residual stays private, and the DP budget is spent on a dimension so small the noise no longer dominates the signal.

## Subspace-Decomposed Expert Modeling (SEM)

SEM is the first of FedSEPT's two innovations and the heart of the privacy-efficiency gains.

### The decomposition

Each prompt expert for client *k* and expert index *m* is parameterized as:

```
P_k^m = A_k^m · B_0 + R_k
```

| Component | Shape | Properties | Communicated? | DP-perturbed? |
|-----------|-------|-----------|---------------|---------------|
| `A_k^m` | L × r | Low-dimensional **expert factor** (client-specific, data-dependent) | **Yes** | **Yes** (Gaussian LDP) |
| `B_0` | r × d | Fixed **public basis**, data-independent, shared across all clients and all experts | No (static) | No (no privacy needed) |
| `R_k` | L × d | Client-private **residual**, captures purely local personalization signal | **No** (stays on-device) | No (never leaves device) |

With defaults M=4 experts, r=16 subspace rank, L=16 prompt length, d=512 embedding dim:

- **Communicated + DP-perturbed dimension:** M × L × r = 4 × 16 × 16 = **1,024 parameters** (the `A_k^m` factors)
- **Full-prompt baseline dimension:** M × L × d = 4 × 16 × 512 = **32,768 parameters**
- **Reduction:** 32× fewer dimensions to protect, 8× less data to transmit per round

### Why this works

- **B_0 is data-independent and public.** Because the basis never depends on any client's data, revealing it leaks nothing — no DP protection is required, no noise is added, and it never changes so it is never re-sent. It provides a common coordinate system that lets heterogeneous experts from different clients be aggregated meaningfully: every client's experts are expressed in the same low-rank subspace.
- **A_k^m is small and is the only thing DP touches.** By collapsing the protected dimension from MLd to MLr (with r ≪ d), the DP noise is injected into a 32× smaller vector. At fixed ε, the noise per coordinate stays the same, but the *total* noise injected — and the fraction of the signal it corrupts — drops dramatically. This is the core utility win.
- **R_k stays local.** The residual captures whatever a client cannot express in the shared r-dimensional subspace. It never leaves the device, so it is perfectly private and never consumes communication budget. It is the personalization layer that survives even under strong DP.

### Communication and DP cost reductions

| Quantity | Full-prompt FL | FedSEPT (SEM) | Reduction |
|-----------|---------------|---------------|-----------|
| Per-round per-client transmission | 64 KiB | 8 KiB | **8×** |
| DP noise dimensionality | MLd = 32,768 | MLr = 1,024 | **32×** |
| Privacy budget | ε=1.0, δ=10⁻⁵ | ε=1.0, δ=10⁻⁵ | matched |

The 8× communication reduction (64 KiB → 8 KiB) and the 32× DP-dimensionality reduction (32,768 → 1,024) are the headline efficiency numbers, and they come from the same structural change: only `A_k^m` travels and only `A_k^m` is noised.

## Instance-Aware Expert Fusion (IEF)

SEM defines the experts; IEF defines how a client uses them at inference time. Without a fusion mechanism, a multi-expert system risks either ignoring most experts (winner-takes-all collapse) or blindly averaging them (destroying specialization). IEF is an on-device router that picks a per-input blend.

### Per-instance routing

For each input image, the router computes input-dependent fusion weights over the M experts. The combination is a **logit-level fusion**: instead of re-running the text encoder for every expert on every input (expensive), FedSEPT caches each expert's text features once and combines the cached logits with the learned weights. This is what makes the multi-expert path cheap at inference.

- **Cached inference latency:** 2.346 ms — the lowest among all compared methods
- **Cached inference compute:** 35.1 GFLOPs — the lowest among all compared methods
- **No repeated text-encoder passes:** the text encoder runs once per expert per class, ever; subsequent inputs reuse the cached features

### Diversity and load-balancing regularization

Two regularizers prevent the multi-expert system from degenerating:

- **Expert diversity regularization** pushes experts to cover semantically complementary subspaces, preventing *expert collapse* (all experts learning the same thing).
- **Load-balancing regularization** prevents *winner-takes-all* routing, where one expert dominates and the others are starved of training signal and atrophy.

Together they ensure the M experts remain genuinely complementary, which is what makes the per-instance fusion meaningful rather than decorative.

## Federated Private Optimization

### Per-round workflow

1. **Server broadcasts** the fixed public basis `B_0` (once, at initialization — it never changes) and the current global expert factors `{Ā^m}`.
2. **Each client k** initializes its local experts as `P_k^m = A_k^m · B_0 + R_k`, trains locally on its private data, and updates `A_k^m` and `R_k`. Only `A_k^m` will be sent; `R_k` is updated but kept local.
3. **DP perturbation:** Before transmission, each client adds Gaussian noise calibrated to the L2 sensitivity of its `A_k^m` factors, satisfying local (ε, δ)-DP. Because the dimension is MLr (not MLd), the noise is injected into a 32× smaller vector.
4. **Server aggregation:** The server averages the noisy `A_k^m` factors across participating clients to produce the next round's global `Ā^m`. The aggregation is a simple mean — no heavy cryptography, no secure aggregation protocol required, because DP already provides the formal guarantee.
5. **Repeat** until convergence. `B_0` is never updated; `R_k` is never transmitted.

### Privacy guarantee

- Local differential privacy with Gaussian mechanism, (ε=1.0, δ=10⁻⁵) per the paper's strong-privacy setting
- Privacy amplification by subsampling (standard in FL-DP composition) tightens the per-round bound
- The only quantity released per client per round is the noised `A_k^m ∈ R^{L×r}` — a 1,024-dimensional vector at the default settings

### Default hyperparameters (from the paper)

| Hyperparameter | Symbol | Value |
|----------------|--------|-------|
| Number of experts | M | 4 |
| Subspace rank | r | 16 |
| Prompt length | L | 16 |
| Embedding dimension | d | 512 |
| Privacy budget | ε | 1.0 |
| Privacy δ | δ | 10⁻⁵ |

## Experimental Results

### Benchmarks and heterogeneity settings

FedSEPT is evaluated across **11 benchmarks** in three heterogeneity regimes, making it one of the more broadly tested federated prompt-tuning methods:

| Setting | Datasets | Heterogeneity type |
|---------|----------|--------------------|
| Pathological label skew | Food101, Caltech101, Flowers102, DTD, OxfordPets | Each client holds only a subset of classes |
| Practical label skew | CIFAR-10, CIFAR-100 | Dirichlet partition with β ∈ {0.1, 0.3, 0.5} |
| Domain + label skew | PACS, Office31, OfficeHome, DomainNet | Both distribution shift across domains *and* label imbalance |

### Metrics

FedSEPT explicitly measures the personalization-generalization trade-off rather than reporting a single accuracy:

- **In-Client accuracy** — performance on data from distributions the client has seen (personalization)
- **Cross-Client accuracy** — performance on unseen client distributions (generalization)
- **Harmonic Mean** — the trade-off metric; a method that is great on one and terrible on the other scores poorly here

### Efficiency headline numbers

| Metric | FedSEPT | Full-prompt baselines |
|--------|---------|----------------------|
| Per-round communication | 8 KiB | 64 KiB |
| Communication reduction | — | **8×** |
| Inference latency (cached) | 2.346 ms | higher |
| Inference compute | 35.1 GFLOPs | higher |
| DP noise dimensionality | MLr = 1,024 | MLd = 32,768 |

The cached-inference numbers (2.346 ms, 35.1 GFLOPs) are the lowest among all methods the paper compares against, confirming that the logit-level fusion with cached expert text features is not just theoretically cheaper but empirically the cheapest at inference time.

## Defense Assessment

FedSEPT is evaluated against the two canonical privacy attacks on federated updates, under the strong DP setting (ε=1.0, δ=10⁻⁵). The results quantify how much the subspace decomposition + LDP defend against concrete adversaries.

### Membership inference attack (MIA)

MIA tries to determine whether a specific sample was in a client's training set. Lower AUC is better (0.5 is random guessing).

| Adversary access | MIA AUC range (under FedSEPT + DP) |
|------------------|-----------------------------------|
| Update-only | 0.4487 – 0.5021 |
| Update + query | 0.5521 – 0.5790 |

The update-only range straddles 0.5 (random), meaning the attacker gains essentially no signal from the released `A_k^m` alone. With query access the attacker does better, but stays near-random — far from the leakage of unprotected updates.

### Gradient inversion attack (GIA)

GIA tries to reconstruct input data from the released gradient. FedSEPT measures defense by how much the reconstructed image resembles the real one under CLIP.

| Setting | CLIP cosine similarity | Top-1 agreement |
|---------|------------------------|-----------------|
| Without DP | 0.529 | 0.938 |
| With DP (FedSEPT) | **-0.013** | **0** |

This is the most striking defense result: under DP, the reconstructed image is essentially uncorrelated with the real image (cosine similarity ≈ 0, in fact slightly negative), and top-1 agreement drops from 93.8% to 0%. The GIA adversary cannot recover recognizable content at all. This is the concrete payoff of injecting DP noise into a 32× smaller vector — the noise is concentrated enough to fully blind the inversion attack without destroying utility.

### Interpretation

The MIA and GIA results together show that FedSEPT's privacy is not just a formal ε guarantee but a *practically effective* defense: the released `A_k^m` factors are too small and too noised for either an inference or inversion adversary to extract useful information. The subspace decomposition is what makes this affordable — protecting the full MLd tensor at ε=1.0 would destroy utility, while protecting the MLr factor preserves it.

## A-Tech Applications

| A-Tech product | Application |
|----------------|-------------|
| **Builder's Club** | Open-source reference implementation of FedSEPT-style subspace-decomposed PEFT for the community; the decomposition pattern (public basis + private low-rank factor + local residual) is a reusable design for any federated prompt-tuning project |
| **Be Practical** | Curriculum module on *dimensionality as a privacy lever* — teaching that reducing what you transmit also reduces what you must protect, with FedSEPT as the worked example (8 KiB vs 64 KiB, 32× DP-dimension reduction, GIA cosine 0.529 → -0.013) |
| **A-Tech Data Center in a Box / Project Infra** | On-device personalization path for sovereign AI containers: clients tune prompts locally under LDP, only the tiny `A_k^m` factor leaves the device, the residual stays sovereign. The 8 KiB/round footprint fits bandwidth-constrained edge and solar-powered deployments |
| **OpenAdapter Enterprise** | Privacy-preserving federated prompt tuning for VLM-based classification across heterogeneous customer deployments (medical imaging, industrial inspection, retail) where each customer's label/domain distribution differs and formal DP is contractually required |

## Cross-References

- `privacy-and-trust/dp-lac-lightweight-adaptive-clipping/` — Lightweight adaptive clipping for DP-FL on LLMs; complementary privacy-efficiency lever (clipping threshold) for the full-model-fine-tuning case FedSEPT does not target
- `privacy-and-trust/sheld-fl-self-learning-heterogeneous-dp-framework/` — Heterogeneous per-client DP; FedSEPT's SEM is a structural complement (reduce the protected dimension) to SHELD-FL's adaptive-ε approach (spend budget where the risk is)
- `privacy-and-trust/eris-federated-shard-aggregation/` — Information-theoretic structural privacy via shard aggregation; an alternative to DP-based protection that could compose with FedSEPT's subspace factor as an additional aggregation-privacy layer
- `privacy-and-trust/federated-learning-for-privacy-preserving-ai/` — FL foundations
- `privacy-and-trust/federated-llm-on-device-personalization/` — Federated LLM personalization (text modality; FedSEPT is the vision-language counterpart)
- `privacy-and-trust/separable-expert-architecture-deletable-personalization/` — Separable expert architectures for deletable personalization; FedSEPT's multi-expert + residual decomposition is structurally related (experts as separable components)
- `privacy-and-trust/federated-byzantine-robust-partial-participation/` — Byzantine-robust aggregation; orthogonal to FedSEPT's DP defense, composable for deployments with untrusted clients
- `privacy-and-trust/slaclip-adaptive-clipping-dp-sgd/` — Adaptive clipping for centralized DP-SGD; the non-federated clipping counterpart
- `privacy-and-trust/google-gboard-private-fl-dp/` — Production FL+DP system reference (different modality, same privacy-efficiency tension)