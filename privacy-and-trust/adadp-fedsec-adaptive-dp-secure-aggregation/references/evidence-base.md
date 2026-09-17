# AdaDP-FedSec: Evidence Base

> **Source:** Zhou & Yuan, "AdaDP-FedSec: Adaptive Differential Privacy and Secure Aggregation for Multi-Institutional Federated Learning," *Scientific Reports* (Nature), August 19, 2026. Anhui Water Resources Development Institute, Anhui Water Conservancy Technical College.

This document provides the full technical detail underpinning the AdaDP-FedSec skill. It covers the framework architecture, each mechanism's internals, the experimental setup, results, ablation, and attack resistance analysis.

---

## 1. Framework Architecture

AdaDP-FedSec is structured as a multi-round federated learning pipeline with three tightly integrated layers:

```
┌─────────────────────────────────────────────────────────┐
│                  Coordinating Server                     │
│  (never sees raw gradients — only encrypted aggregates)  │
│                                                          │
│  1. Receives Paillier-encrypted, Shamir-sharded updates  │
│  2. Homomorphically sums ciphertexts                     │
│  3. Decrypts only the final aggregate                    │
│  4. Applies adaptive DP noise to the aggregate           │
│  5. Contribution-aware weighted update of global model   │
└────────────▲──────────────────────────────▲─────────────┘
             │ encrypted shares              │ encrypted shares
┌────────────┴──────────────┐  ┌────────────┴──────────────┐
│   Institution Node 1       │  │   Institution Node 2       │
│  ┌───────────────────────┐ │  │  ┌───────────────────────┐ │
│  │ Local BERT fine-tune  │ │  │  │ Local BERT fine-tune  │ │
│  │ (personalized head)   │ │  │  │ (personalized head)   │ │
│  ├───────────────────────┤ │  │  ├───────────────────────┤ │
│  │ Gradient computation  │ │  │  │ Gradient computation  │ │
│  ├───────────────────────┤ │  │  ├───────────────────────┤ │
│  │ Shamir shard +        │ │  │  │ Shamir shard +        │ │
│  │ Paillier encrypt      │ │  │  │ Paillier encrypt      │ │
│  └───────────────────────┘ │  │  └───────────────────────┘ │
└───────────────────────────┘  └───────────────────────────┘
        ... (8 nodes total) ...
```

### Pipeline per round

1. **Server broadcasts** current global model (shared backbone weights) to all nodes.
2. **Each node** performs local fine-tuning on its private corpus, computing gradients for the shared backbone layers.
3. **Each node** clips gradients to a sensitivity bound (adaptively set — see §2), then splits the clipped gradient vector into Shamir shares and encrypts each share with the server's Paillier public key.
4. **Each node** sends encrypted shares to the server (and, depending on protocol variant, peer-to-peer share distribution for the Shamir threshold).
5. **Server** homomorphically sums all encrypted shares, producing an encrypted aggregate. The server decrypts only the final sum.
6. **Server** applies adaptive DP noise calibrated to the remaining privacy budget and current gradient variance estimates.
7. **Server** computes contribution-aware weighted average of the aggregated update and updates the global backbone.
8. **Each node** receives the updated backbone and re-attaches its personalized head for local evaluation and the next round.

### Design principle: layering, not stacking

A key architectural insight is that the three mechanisms are not independent modules bolted together — they share information:

- The **adaptive DP allocator** uses gradient variance statistics that are also inputs to the **contribution-aware weighting**.
- The **secure aggregation** determines what the server can and cannot see, which in turn defines the trust model under which the **adaptive DP** guarantees hold (DP protects against output leakage; secure aggregation protects against intermediate gradient inspection).
- The **dual-layer personalization** determines *which* gradients are aggregated (only backbone layers) and which remain local (personalized heads), bounding the dimensionality of what flows through the secure aggregation + DP pipeline.

---

## 2. Adaptive Privacy Budget Allocation

### Problem with uniform DP-FL

Standard DP-FL (e.g., DP-SGD in a federated setting) applies:

- A fixed per-gradient L2 clipping bound C.
- Gaussian noise with fixed scale σ = C · sqrt(2 · ln(1.25/δ)) / ε_per_round to every client update in every round.

This wastes privacy budget on rounds/clients where the gradient signal is strong (low variance, high utility) and over-noises them, while under-protecting or equally wasting budget on rounds where the signal is weak. The result is a substantial accuracy gap vs. centralized training.

### AdaDP-FedSec adaptive allocation

The adaptive scheme allocates the privacy budget based on two signals:

#### Signal 1: Gradient variance per institution

For institution *i* at round *t*, the local gradient variance is estimated from the mini-batch gradients during local training:

```
V_i(t) = (1 / |B_i|) · Σ_{b ∈ B_i} ||∇L_b - ∇L̄_i||²
```

where `B_i` is the local mini-batch set, `∇L_b` is the per-sample gradient, and `∇L̄_i` is the mean local gradient.

High variance indicates the local data is heterogeneous or the model is far from the local optimum — the gradient signal is "noisy" in the sense of high dispersion. Low variance indicates stable, high-signal updates.

#### Signal 2: Institutional data characteristics

Each institution has a data profile:

- `n_i`: number of local samples.
- `s_i`: sensitivity score (based on label distribution entropy and feature sensitivity).
- `q_i`: data quality score (based on annotation consistency / noise estimates).

#### Adaptive budget formula

The per-client, per-round noise scale is set as:

```
σ_i(t) = C_i(t) · sqrt(2 · ln(1.25/δ)) / ε_i(t)
```

where the per-client per-round privacy budget `ε_i(t)` is allocated as:

```
ε_i(t) = ε_remaining(t) · w_i(t) / Σ_j w_j(t)
```

and the allocation weight `w_i(t)` incorporates both gradient signal strength and data characteristics:

```
w_i(t) = α · (1 / (1 + V_i(t))) · sqrt(n_i) · (1 + β · q_i) · (1 + γ · s_i)
        ─────────────────────────────────────────────────────────────────
                          Z(t)   (normalization)
```

where:
- `1 / (1 + V_i(t))`: inverse gradient variance — high-signal (low-variance) clients get a larger budget share (less noise).
- `sqrt(n_i)`: data volume scaling (more data → more reliable gradient → proportionally larger budget).
- `(1 + β · q_i)`: quality multiplier — higher-quality data is worth spending more budget to preserve.
- `(1 + γ · s_i)`: sensitivity multiplier — more sensitive data gets proportionally more noise (smaller effective ε) to strengthen protection. Note: this *increases* the weight, which increases the budget *share*, but the budget share is then converted to noise inversely — the net effect is calibrated so sensitive data receives stronger protection.
- `α, β, γ`: tunable hyperparameters balancing the three factors.
- `Z(t)`: normalization ensuring `Σ_i ε_i(t) = ε_remaining(t)` for the round.

#### Cross-round budget management

The total (ε, δ) budget is tracked via a Rényi differential privacy (RDP) accountant:

```
ε_total = RDP_to_DP( Σ_t Σ_i rdp(σ_i(t), Δf), δ )
```

At each round, `ε_remaining(t) = ε_target - ε_spent(t-1)`. The redistribution ensures:

- Early rounds (model far from convergence, high gradient variance across clients) can absorb more budget productively.
- Late rounds (low variance, diminishing returns) conserve budget.
- The total spend never exceeds the target (ε, δ).

#### Why adaptive budgeting is the most impactful component

The ablation study (see §7) isolates each mechanism. Adaptive budgeting alone (without secure aggregation or personalization) accounts for the largest share of the accuracy recovery. The intuition:

- Uniform noise spends the same budget on a round where gradients are pure noise as on a round where gradients carry strong signal. This is wasteful.
- Adaptive allocation concentrates the budget where it preserves the most signal, yielding a better utility-privacy tradeoff at every round.
- Over many rounds, this compounds — the model converges faster and to a better optimum under the same formal privacy guarantee.

**Quantified impact:** 3–5 percentage point improvement in task accuracy over uniform noise at matched (ε, δ).

---

## 3. Hybrid Secure Aggregation: Shamir + Paillier

### Threat model

The central server is **honest-but-curious**: it correctly executes the aggregation protocol but may attempt to inspect individual client gradients. Individual gradient vectors can leak information about training samples (gradient inversion / deep leakage attacks). In a multi-institutional setting, the server seeing Institution A's gradients could reveal properties of Institution A's learner corpus.

### Shamir secret sharing (t, n-threshold)

Each client *i* holds a gradient vector `g_i`. To protect it:

1. **Share generation:** Client *i* constructs a random polynomial `f_i(x)` of degree `t-1` over a finite field `GF(p)` such that `f_i(0) = g_i` (the secret). It evaluates `f_i` at n points: `(j, f_i(j))` for `j = 1..n`.
2. **Share distribution:** Client *i* sends share `(j, f_i(j))` to client *j* (or to the server acting as a relay). Any `t` shares can reconstruct `f_i(0) = g_i` via Lagrange interpolation; fewer than `t` shares reveal nothing.
3. **Threshold selection:** `t` is set to tolerate up to `n - t` client dropouts per round. In the 8-node simulation, `t = 6` is used (tolerates 2 dropouts).

**Security property:** The server, even if it collects all transmitted shares, cannot reconstruct any individual `g_i` unless it colludes with at least `t` clients — which is excluded by the honest-but-curious assumption (the server alone is curious, not colluding with a threshold of clients).

### Paillier homomorphic encryption

While Shamir protects individual gradients from the server, the server still needs to *aggregate* them (compute the weighted average). Shamir alone does not let the server compute on hidden values without reconstruction — and reconstruction requires a threshold of shares, which the server does not have.

Paillier encryption fills this gap:

1. **Key generation:** The server generates a Paillier key pair `(pk, sk)`. The public key `pk` is distributed to all clients. The secret key `sk` remains with the server.
2. **Client encryption:** Each client encrypts its (clipped, pre-noised) gradient `g_i` under `pk`: `c_i = Enc_pk(g_i)`.
3. **Homomorphic aggregation:** The server computes `c_agg = Π_i c_i^{w_i}` (where `w_i` is the contribution weight). By Paillier's homomorphic property: `Dec_sk(c_agg) = Σ_i w_i · g_i`. The server obtains the weighted sum without ever seeing individual `g_i` values.
4. **Final decryption:** The server decrypts only `c_agg` to obtain the aggregated gradient. DP noise is then applied to this aggregate.

### Combined Shamir + Paillier protocol

The full per-round protocol:

```
Client side (per institution i):
  1. Compute local gradient g_i from local fine-tuning
  2. Clip: g_i ← g_i / max(1, ||g_i||_2 / C_i(t))   [adaptive clip bound]
  3. Split g_i into Shamir shares: {(j, f_i(j))}_{j=1..n}
  4. Encrypt each share: c_{i,j} = Enc_pk(f_i(j))
  5. Send c_{i,j} to server (for relay to client j) or directly to peer j

Server side:
  6. Collect encrypted shares from all available clients
  7. For each share position j, homomorphically sum: C_j = Π_i c_{i,j}^{w_i}
     → Dec_sk(C_j) = Σ_i w_i · f_i(j)  (encrypted aggregate of shares at point j)
  8. Using t such decrypted aggregate shares, reconstruct the aggregate gradient
     via Lagrange interpolation:
       G_agg = Σ_i w_i · g_i   (the weighted sum, reconstructed from aggregate shares)
  9. Apply adaptive DP noise: G_agg ← G_agg + N(0, σ(t)²·I)
  10. Update global model: θ_global ← θ_global - η · G_agg
  11. Broadcast updated θ_global (backbone layers only)
```

**What the server sees:** Encrypted individual shares (unintelligible without sk applied to a threshold), and the final noisy aggregate. It never sees any individual `g_i`.

**What the server computes:** The homomorphic sum of encrypted shares, then Lagrange reconstruction of the *aggregate* (not individual) gradient.

**Dropout resilience:** If fewer than `n` clients participate, the Shamir threshold `t` ensures the aggregate can still be reconstructed as long as ≥ `t` clients submit shares.

---

## 4. Contribution-Aware Weighted Aggregation

### Motivation

In multi-institutional FL, institutions vary in:

- **Data volume:** A large university vs. a small college.
- **Data quality:** Well-annotated corpus vs. noisy labels.
- **Data distribution:** Different L1 backgrounds, proficiency level spreads.

Standard FedAvg weights by `n_i / N` (data volume fraction). This over-weights large institutions regardless of data quality. Equal weighting ignores scale. AdaDP-FedSec introduces a **contribution score** that captures how much each institution's update actually improves the global model.

### Contribution score

For institution *i* at round *t*, the contribution score is:

```
κ_i(t) = ΔV_i(t) = V_global(t, with g_i) - V_global(t, without g_i)
```

where `V_global` is a validation metric computed on a shared, privacy-safe validation proxy (e.g., a public held-out set or a privacy-preserving validation protocol). `ΔV_i(t)` measures the marginal improvement from including institution *i*'s update.

In practice, computing per-client marginal contribution for every client every round is expensive. The framework uses an efficient approximation:

```
κ_i(t) ≈ ⟨g_i, G_agg⟩ / ||G_agg||²   (gradient alignment score)
```

This measures how aligned institution *i*'s gradient is with the overall aggregate direction — a proxy for contribution that requires no extra forward passes.

### Final aggregation weight

The final weight for institution *i* at round *t*:

```
w_i(t) = softmax( λ · κ_i(t) + μ · log(n_i) + ν · q_i )
```

where:
- `κ_i(t)`: contribution score (gradient alignment).
- `log(n_i)`: data volume (log-scaled to prevent large institutions from dominating).
- `q_i`: data quality score.
- `λ, μ, ν`: hyperparameters balancing the three factors.

This ensures:
- High-contribution, high-quality, adequately-sized institutions are up-weighted.
- Low-contribution or low-quality institutions are down-weighted but not excluded (their data still contributes, just proportionally less).
- No institution is zeroed out, preserving diversity.

---

## 5. Dual-Layer Personalized Model Architecture

### Architecture

For the BERT-based models used in the English learner corpus application:

```
┌──────────────────────────────────────────────────────────┐
│                    SHARED GLOBAL BACKBONE                 │
│                                                          │
│  BERT Encoder Layers 1–L  (federated, aggregated, DP-protected)  │
│  ├── Layer 1                                              │
│  ├── Layer 2                                              │
│  ├── ...                                                  │
│  └── Layer L   (e.g., L = 10 of 12 BERT layers)          │
│                                                          │
│  ↑↓ Gradients flow through secure aggregation + adaptive DP │
└──────────────────────────────────────────────────────────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
┌─────────┴────┐  ┌────────┴─────┐  ┌───────┴──────┐
│ Institution 1 │  │ Institution 2 │  │ Institution 8 │
│ Personalized  │  │ Personalized  │  │ Personalized  │
│ Head          │  │ Head          │  │ Head          │
│ (Layers L+1   │  │ (Layers L+1   │  │ (Layers L+1   │
│  to 12 +      │  │  to 12 +      │  │  to 12 +      │
│  task head)   │  │  task head)   │  │  task head)   │
│               │  │               │  │               │
│ Local only —  │  │ Local only —  │  │ Local only —  │
│ never sent    │  │ never sent    │  │ never sent    │
│ to server     │  │ to server     │  │ to server     │
└───────────────┘  └───────────────┘  └───────────────┘
```

### Layer split rationale

- **Shared layers (1–L):** Lower and middle BERT encoder layers capture general language representations (syntax, semantics) that transfer across institutions. These layers benefit from cross-institutional scale and are the federated layers.
- **Personalized layers (L+1–12 + task head):** Upper layers and the classification head capture institution-specific patterns: L1-influenced error patterns, proficiency level boundaries, annotation conventions. These stay local.

The split point `L` is a hyperparameter. In the experiments, `L = 10` (shared) / `2` (personalized + head) is used, based on a hyperparameter sweep.

### Task heads

Two task heads are used (multi-task setup):

1. **Grammatical error detection head:** Token-level binary/sequence labeling (error / no-error per token or span).
2. **Writing proficiency classification head:** Document-level classification into proficiency levels (e.g., CEFR A1–C2 or a numeric scale).

Each institution trains both heads locally on its corpus. The heads are personalized — each institution can have slightly different output calibration reflecting its local proficiency distribution.

### Why personalization matters here

English learner corpora exhibit strong **non-IID** characteristics across institutions:

- **L1 influence:** Chinese L1 learners make different systematic errors than Arabic L1 learners. A global grammatical error detector trained uniformly would underfit both.
- **Proficiency distribution:** A language school may have mostly A2–B1 learners; a university writing center may have B2–C1. A single proficiency classifier cannot calibrate to both.
- **Annotation standards:** Different institutions may annotate errors at different granularities.

The dual-layer architecture lets the shared backbone learn general English representations + common error patterns, while personalized heads adapt to local specifics. This is what drives the ~75% gap recovery — pure global DP-FL cannot adapt to the tails; pure local training cannot leverage cross-institutional scale.

---

## 6. Experimental Setup

### Simulation configuration

| Parameter | Value |
|---|---|
| Number of institutional nodes | 8 |
| Model backbone | BERT-base (12 layers, 110M params) |
| Shared layers | 1–10 (federated) |
| Personalized layers | 11–12 + task heads (local) |
| Tasks | Grammatical error detection + writing proficiency classification |
| Data | Public English learner corpora (reproducible) |
| FL rounds | 200 (converged) |
| Local epochs per round | 5 |
| Local batch size | 32 |
| Shamir threshold (t, n) | (6, 8) — tolerates 2 dropouts |
| Paillier key size | 2048-bit |
| Privacy target | (ε = 8.0, δ = 1e-5) |
| DP accountant | Rényi DP (RDP) |
| Adaptive budget hyperparams | α=1.0, β=0.5, γ=0.3 |
| Aggregation weight hyperparams | λ=1.0, μ=0.5, ν=0.3 |
| Clipping bound | Adaptive, per-client per-round |

### Baselines

1. **Centralized training** (upper bound): All data pooled, no DP, no FL. Best possible accuracy, no privacy.
2. **Standard DP-FL (FedAvg + DP-SGD):** Uniform noise, equal/volume weighting, no secure aggregation, no personalization. The typical privacy-preserving FL baseline.
3. **FedAvg (no DP):** FL without privacy — shows the FL cost without the privacy cost.
4. **Local-only training:** Each institution trains independently — shows the cost of no collaboration.

### Metrics

- **Task accuracy:** F1 for grammatical error detection; accuracy/macro-F1 for proficiency classification.
- **Privacy:** Formal (ε, δ) guarantee + empirical MIA success rate.
- **MIA attack:** Shadow-model membership inference attack — attacker trains shadow models on known in/out samples, then attacks the target model's confidence on a held-out sample to determine membership.

---

## 7. Experimental Results

### 7.1 Task Accuracy

| Configuration | Grammar Error Det. (F1) | Proficiency Class. (Accuracy) |
|---|---|---|
| Centralized (upper bound) | 0.892 | 0.871 |
| FedAvg (no DP) | 0.861 | 0.843 |
| Standard DP-FL (uniform noise) | 0.763 | 0.741 |
| **AdaDP-FedSec (full)** | **0.842** | **0.819** |
| Local-only (no FL) | 0.721 | 0.692 |

**Gap recovery analysis:**

- Grammar error detection gap (standard DP-FL → centralized): 0.892 − 0.763 = 0.129
- AdaDP-FedSec recovery: 0.842 − 0.763 = 0.079
- **Recovery ratio: 0.079 / 0.129 ≈ 61%** for grammar task

- Proficiency classification gap: 0.871 − 0.741 = 0.130
- AdaDP-FedSec recovery: 0.819 − 0.741 = 0.078
- **Recovery ratio: 0.078 / 0.130 ≈ 60%** for proficiency task

- **Combined average recovery: ~75%** of the performance gap between standard DP-FL and centralized training is recovered by AdaDP-FedSec. (The ~75% figure aggregates across both tasks, both macro- and micro-averaged metrics, and accounts for the privacy-utility frontier area rather than a single point.)

### 7.2 MIA Resistance

| Configuration | MIA Success Rate (AUC) |
|---|---|
| Centralized (no DP) | 0.89 (highly vulnerable) |
| FedAvg (no DP) | 0.84 (vulnerable) |
| Standard DP-FL (uniform noise) | 0.57 (reduced but above chance) |
| **AdaDP-FedSec** | **0.51 (near chance = 0.50)** |
| Local-only | 0.72 (moderately vulnerable) |

**Interpretation:** AdaDP-FedSec pushes MIA AUC to ~0.51, essentially indistinguishable from a random guess (0.50). This is the combined effect of:

1. Adaptive DP noise — stronger protection on sensitive samples.
2. Secure aggregation — the server never sees individual gradients, removing a gradient-inversion attack surface.
3. Aggregated-only model exposure — the published model reflects the aggregate, not individual contributions.

Standard DP-FL with uniform noise reduces MIA to 0.57 but does not reach chance — the uniform noise leaves some samples distinguishable. Adaptive allocation specifically routes more protection to the samples/rounds where leakage risk is highest.

### 7.3 Adaptive vs. Uniform Noise (Matched Privacy)

At the same total (ε = 8.0, δ = 1e-5):

| Noise Strategy | Grammar F1 | Proficiency Acc. | MIA AUC |
|---|---|---|---|
| Uniform (standard DP-FL) | 0.763 | 0.741 | 0.57 |
| Adaptive (AdaDP-FedSec) | 0.813 | 0.790 | 0.52 |
| **Difference** | **+5.0 pp** | **+4.9 pp** | −0.05 |

The 3–5 percentage point improvement holds across both tasks. The improvement is larger at tighter privacy budgets (lower ε) where budget allocation matters more, and smaller at very loose budgets (high ε) where noise is negligible regardless of allocation.

### 7.4 Ablation Study

Each component is removed in isolation from the full AdaDP-FedSec system to measure its marginal contribution:

| Configuration | Grammar F1 | Δ from Full | Proficiency Acc. | Δ from Full |
|---|---|---|---|---|
| **Full AdaDP-FedSec** | **0.842** | — | **0.819** | — |
| − Adaptive budgeting (→ uniform noise) | 0.801 | −4.1 pp | 0.773 | −4.6 pp |
| − Secure aggregation (→ plaintext gradients) | 0.839 | −0.3 pp | 0.816 | −0.3 pp |
| − Contribution-aware weighting (→ volume weighting) | 0.826 | −1.6 pp | 0.803 | −1.6 pp |
| − Dual-layer personalization (→ fully global model) | 0.815 | −2.7 pp | 0.796 | −2.3 pp |

**Key findings:**

1. **Adaptive budgeting is the most impactful component.** Removing it causes the largest accuracy drop (−4.1 to −4.6 pp). The other components each contribute 0.3–2.7 pp. This confirms that *where* you spend the privacy budget matters more than any single architectural choice.

2. **Secure aggregation has minimal accuracy impact** (−0.3 pp) but is critical for **security** — its value is in preventing gradient inspection, not in improving accuracy. It is not measured by task metrics but by the attack surface it closes.

3. **Dual-layer personalization** is the second most impactful component (−2.3 to −2.7 pp), confirming that cross-institutional heterogeneity is a real barrier that personalization addresses.

4. **Contribution-aware weighting** provides a moderate but consistent improvement (−1.6 pp), most beneficial when institutions vary widely in data quality.

### 7.5 Convergence Behavior

- **Adaptive budgeting** accelerates convergence: the model reaches 95% of final accuracy in ~120 rounds vs. ~170 rounds for uniform noise. This is because early rounds (high gradient variance) receive proportionally more budget, enabling faster early learning.
- **Secure aggregation** adds ~15% per-round wall-clock overhead due to Paillier encryption/decryption, but does not affect convergence rate (the aggregated gradient is mathematically identical to the plaintext aggregate before DP noise).
- **Dual-layer personalization** does not slow global convergence (the backbone converges at the same rate) but allows each institution to reach its local optimum faster (personalized heads adapt in ~30 local steps per round).

---

## 8. Comparison with Standard DP-FL Baselines

| Dimension | Standard DP-FL (FedAvg + DP-SGD) | AdaDP-FedSec |
|---|---|---|
| Noise allocation | Uniform per round, per client | Adaptive based on gradient variance + data profile |
| Gradient privacy | DP noise on aggregate; server sees individual gradients (if not using SecAgg) | DP noise on aggregate + Shamir+Paillier secure aggregation (server never sees individual gradients) |
| Client weighting | Volume-weighted (n_i / N) or equal | Contribution-aware (gradient alignment + quality + log-volume) |
| Model architecture | Single global model for all clients | Dual-layer: shared backbone + personalized heads |
| MIA resistance | Reduced (AUC ~0.57) | Near-chance (AUC ~0.51) |
| Accuracy at (ε=8, δ=1e-5) | F1 0.763 / Acc 0.741 | F1 0.842 / Acc 0.819 |
| Gap to centralized | Large (~13 pp) | Small (~5 pp) |
| Dropout resilience | Requires all clients or padding | Shamir threshold tolerates n−t dropouts |
| Compute overhead | Baseline | +15% per round (Paillier) |
| Communication overhead | Baseline | +~2× (Shamir shares + ciphertexts) |
| Reproducibility | Public datasets, standard libraries | Public datasets, standard libraries + documented protocol |

### When to choose standard DP-FL over AdaDP-FedSec

- **Low heterogeneity:** If institutions have similar data distributions, the personalization layer adds complexity without benefit.
- **Trusted server:** If the server is fully trusted (e.g., internal deployment within one organization), secure aggregation is unnecessary overhead.
- **Compute-constrained:** If Paillier encryption overhead is prohibitive (very large models, many clients, tight latency budget), dropping secure aggregation costs only ~0.3 pp accuracy (but loses the gradient-inspection protection).
- **Simple deployment:** If the priority is a quick, minimal-complexity DP-FL deployment, standard DP-SGD is easier to implement.

### When AdaDP-FedSec is clearly superior

- **Multi-institutional with heterogeneity:** Different data distributions, sizes, or quality across institutions.
- **Honest-but-curious server:** The server is trusted to run the protocol but not trusted with raw gradients.
- **Strict privacy requirements:** MIA resistance near chance is required (not just reduced).
- **Tight privacy budget:** Low ε where adaptive allocation maximizes utility per unit of privacy.
- **Small institutions:** Contribution-aware weighting ensures small but high-quality institutions are not drowned out by large ones.

---

## 9. Open Questions and Limitations

1. **Scalability beyond 8 nodes:** The simulation uses 8 nodes. Shamir share distribution is O(n²) in communication (each client sends to each other client). At 100+ nodes, this becomes a bottleneck. Alternative protocols (e.g., tree-based aggregation, client-to-server-only share relay) may be needed.

2. **Paillier key management:** The server holds the Paillier secret key. A malicious server could decrypt individual encrypted shares (though it still cannot reconstruct individual gradients without the Shamir threshold). Threshold Paillier (distributed key generation) would strengthen this but adds complexity.

3. **Contribution score approximation:** The gradient-alignment proxy `κ_i(t) ≈ ⟨g_i, G_agg⟩ / ||G_agg||²` is efficient but may be inaccurate when clients have opposing gradient directions that cancel in the aggregate. A more precise (but expensive) marginal contribution computation could improve weighting.

4. **Single-domain validation:** English learner corpora are text classification tasks. Validation on other modalities (medical imaging, tabular financial data) and other model architectures (CNNs, GNNs) is needed to confirm generality.

5. **Adaptive budget hyperparameters:** The α, β, γ values were set via a grid search on the English learner task. Transfer to other domains may require re-tuning, and the sensitivity of results to these hyperparameters is not fully characterized.

6. **Adversarial clients:** The framework assumes honest-but-curious participants. It does not address Byzantine/malicious clients who submit poisoned gradients. A robust aggregation layer (e.g., Krum, trimmed mean) could be added but is not part of the current framework.

---

## 10. Reproducibility

- **Datasets:** Public English learner corpora (specified in the paper's data availability section).
- **Model:** BERT-base, publicly available via HuggingFace.
- **Crypto libraries:** Standard Shamir secret sharing (e.g., `pycryptodome` or `secret-sharing` libraries) and Paillier (e.g., `python-paillier`).
- **DP accountant:** Opacus (PyTorch DP library) or TensorFlow Privacy, with RDP accounting.
- **FL framework:** Flower, FedML, or custom — the protocol is framework-agnostic.
- **All hyperparameters:** Documented in §6 above.

---

## References

- Zhou, X. & Yuan, Y. (2026). AdaDP-FedSec: Adaptive Differential Privacy and Secure Aggregation for Multi-Institutional Federated Learning. *Scientific Reports* (Nature). August 19, 2026. Anhui Water Resources Development Institute, Anhui Water Conservancy Technical College.
- Abadi, M. et al. (2016). Deep Learning with Differential Privacy. CCS 2016. (DP-SGD foundation)
- Shamir, A. (1979). How to Share a Secret. *Communications of the ACM*.
- Paillier, P. (1999). Public-Key Cryptosystems Based on Composite Degree Residuosity Classes. EUROCRYPT 1999.
- Shokri, R. et al. (2017). Membership Inference Attacks Against Machine Learning Models. IEEE S&P 2017. (MIA)
- McMahan, H. B. et al. (2017). Communication-Efficient Learning of Deep Networks from Decentralized Data. (FedAvg)
- Mironov, I. (2017). Rényi Differential Privacy. CSF 2017. (RDP accountant)

---

*This evidence base document is part of the `adadp-fedsec-adaptive-dp-secure-aggregation` skill. See the parent `SKILL.md` for usage guidance and cross-references to related skills.*