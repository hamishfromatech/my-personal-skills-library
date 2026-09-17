# Synthetic Data Validation Stack

## Overview

The validation stack is the trust foundation of synthetic data monetization. Without reproducible utility and privacy scores, synthetic data is unsellable. This reference documents the full methodology, metric formulas, and open-source tool mapping.

## Utility Metrics

### 1. Statistical Fidelity

Compare the synthetic data's statistical properties to the real data's.

**Marginal distributions:**
- Kolmogorov-Smirnov (KS) test for continuous variables: `KS = max |F_real(x) - F_synth(x)|`
- Chi-square test for categorical variables
- Threshold: KS statistic < 0.05 indicates good marginal fidelity

**Joint distributions / correlations:**
- Correlation delta: `Δ_corr = max |corr_real[i,j] - corr_synth[i,j]|` across all variable pairs
- Propensity Mean-Squared Error (MSE): train a classifier to distinguish real from synthetic; `propensity_MSE = E[(p_real - p_synth)^2]`. Lower is better (0 = indistinguishable).

**Coverage (mode collapse detection):**
- `coverage = |{x ∈ support(real) : ∃ s ∈ synthetic, d(x,s) < τ}| / |support(real)|`
- Low coverage indicates the generator collapsed to a subset of the real distribution.

### 2. Downstream-Task Utility (TSTR)

The gold standard: train a model on synthetic data, test on held-out real data.

```
TSTR_accuracy = accuracy(model trained on synthetic, tested on real_test)
TRTR_accuracy = accuracy(model trained on real_train, tested on real_test)  # baseline
utility_ratio = TSTR_accuracy / TRTR_accuracy
```

- `utility_ratio > 0.90` → high utility
- `0.70 < utility_ratio < 0.90` → moderate utility (check downstream use case)
- `utility_ratio < 0.70` → low utility; generator needs improvement

### 3. Open-Source Utility Validation Tools

| Tool | Language | Capability |
|---|---|---|
| SDMetrics (SDV project) | Python | Full utility + privacy metrics suite; the reference implementation |
| TableEvaluator | Python | Pandas-based statistical comparison |
| Synthea eval modules | Java | Healthcare-specific synthetic data evaluation |

## Privacy Metrics

### 1. Membership Inference Attack Resistance

An attacker attempts to determine whether a specific record was in the training set.

**Attack protocol:**
1. Split real data into `train` (used to train generator) and `holdout`.
2. Train a membership inference classifier on generator outputs + shadow models.
3. `MIA_success_rate = P(attacker correctly identifies train vs holdout membership)`
4. `privacy_score = 1 - (MIA_success_rate - 0.5) * 2`  (normalized; 1.0 = perfect privacy)

**Threshold:** `MIA_success_rate < 0.55` (close to random chance) indicates strong privacy.

### 2. Distance-Based Disclosure Risk

```python
def disclosure_risk(real_data, synthetic_data):
    risks = []
    for r in real_data:
        nearest_synth = min(distance(r, s) for s in synthetic_data)
        nearest_real = min(distance(r, r2) for r2 in real_data if r2 != r)
        # If synthetic record is closer to real record than 
        # the nearest *other* real record, disclosure risk is high
        risks.append(1 if nearest_synth < nearest_real * 0.5 else 0)
    return sum(risks) / len(risks)
```

**Threshold:** `disclosure_risk < 0.05` (fewer than 5% of records at risk).

### 3. Formal Differential Privacy Guarantee

When the generator uses DP (e.g., DP-SGD, DP mechanisms on statistics):

- `epsilon (ε)` — the privacy budget. Lower = stronger privacy.
- `ε ≤ 1`: strong privacy (commands 30-50% pricing premium)
- `1 < ε ≤ 10`: moderate privacy
- `ε > 10`: weak formal guarantee (rely on empirical metrics)

Publish `ε` with every DP-guaranteed dataset. This is the formal compliance anchor.

### 4. Open-Source Privacy Validation Tools

| Tool | Capability |
|---|---|
| PrivacyMeter | Membership inference attack evaluation library |
| SDMetrics privacy report | Disclosure risk + MIA metrics |
| Opacus / TF-Privacy | DP training frameworks (for epsilon computation) |

## Provenance Manifest

Every monetizable synthetic dataset must ship with a provenance manifest:

```yaml
dataset_id: syn-medical-imaging-v3
generator: sdv-ctgan-v1.2
generator_hash: sha256:abc123...
training_data_stats:  # NOT the training data itself
  n_records: 50000
  schema_hash: sha256:def456...
  marginal_distributions: [attached]
epsilon: 1.5  # if DP; null if not
utility_scores:
  ks_max: 0.031
  correlation_delta: 0.072
  tstr_utility_ratio: 0.93
privacy_scores:
  mia_success_rate: 0.52
  disclosure_risk: 0.02
generated_at: 2026-07-19T00:00:00Z
validation_tool_versions:
  sdmetrics: 0.12.0
  privacymeter: 0.3.1
license: CC-BY-4.0  # or commercial license
```

The manifest is the trust contract between seller and buyer. Open-source the manifest format; charge for the validated datasets and enterprise pipeline that produces compliant manifests.

## Privacy-First Generation Architecture

### On-Prem Training Pattern

```
Customer Environment (data never leaves)
┌─────────────────────────────────────────┐
│  Real Data → Profile → Train Generator  │
│                        ↓                │
│              Synthetic Data ←───┐       │
│              Validation Scores  │       │
└──────────────────────────────────┼───────┘
                                   │
                        Only synthetic + scores leave
                                   ↓
┌──────────────────────────────────────────┐
│  A-Tech Platform (marketplace / API)      │
│  Receive synthetic → validate → publish   │
│  → deliver to buyer with manifest         │
└──────────────────────────────────────────┘
```

### Federated Generation Pattern (for multi-party synthetic data)

Multiple parties collaboratively train a synthetic data generator without sharing their real data, using federated learning. Only generator updates are shared; the resulting synthetic data benefits from all parties' distributions. This is the highest-value, highest-trust pattern for regulated consortia (e.g., hospitals sharing disease data patterns).

## Regulatory Alignment

- **GDPR:** Synthetic data that cannot be linked to an identified individual falls outside GDPR's personal data scope (confirmed by EDPB 2025 guidance). This is the legal foundation for monetization.
- **HIPAA:** Synthetic healthcare data that passes Safe Harbor / Expert Determination de-identification can be shared without restrictions.
- **DPDP (India, May 2027):** Non-personal synthetic data is outside scope; DP-guaranteed synthetic data is defensible even for personal-data-adjacent use cases.
- **CCPA/CPRA:** Synthetic data that cannot be linked to a consumer or household is not "personal information."

The regulatory alignment is the moat: synthetic data products are compliant-by-design, while surveillance-based data products face growing restriction.