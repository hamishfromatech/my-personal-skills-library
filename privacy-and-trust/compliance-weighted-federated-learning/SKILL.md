---
name: compliance-weighted-federated-learning
description: Adapts differential privacy noise allocation in federated learning based on institutional compliance scores, enabling inclusive participation from resource-constrained and lower-compliance institutions without degrading model performance. Use when designing federated learning systems for heterogeneous clinical/enterprise consortia, when compliance-gated participation is needed, or when allocating DP noise proportionally to institutional readiness.
---

# Compliance-Weighted Federated Learning

## Overview

Applies a compliance-aware federated learning framework that adapts differential privacy (DP) noise allocation based on each client institution's compliance score, enabling inclusive participation from both highly regulated and resource-constrained sites. Based on Parampottupadam et al. (DKFZ/Heidelberg/Bilkent, arXiv:2505.22108v4, 2026).

The framework addresses a structural exclusion problem in clinical FL: standard DP applies uniform noise to all clients, disproportionately penalizing well-compliant or under-resourced institutions. The solution allocates noise proportional to institutional compliance — lower-compliance sites receive larger noise scales, higher-compliance sites incur less noise — expanding federation participation without relaxing protection for the most-exposed participants.

## The Structural Exclusion Problem

In healthcare FL across countries and resource tiers, the institutions that benefit most from cross-site collaboration (regional hospitals, public-health programs in LMICs, emerging-market clinics, specialist centers treating under-represented populations) are often the same institutions whose compliance infrastructure lags behind mature academic medical centers.

A uniform-DP regime forces a binary choice:
- **Include** the site under the same noise scale → potentially insufficient for higher-risk sites
- **Exclude** the site → sacrificing the diversity of its patient population

Either choice replicates structural exclusion documented in clinical FL reviews (5.2% of 612 studies involved real-world clinical applications) and produces models that under-represent the populations most likely to encounter them.

## The Compliance-Weighted Solution

### Compliance Scoring Tool
A web-based, PI-configurable tool aligned with healthcare and security standards:
- **HIPAA, GDPR, NIST, ISO, HL7/FHIR**
- 12 compliance factors with configurable weights
- Two-tier participation process:
  1. **Eligibility Gate (mandatory)**: HIPAA/GDPR compliance, patient consent management, encryption (AES-128+, TLS 1.2+)
  2. **Modulating Factors (weighted)**: Data quality, local training quality, audit logs, interoperability, anonymization, secure network, ethical AI policies, authentication, TEEs

### Compliance Score Formula
```
S_c = Σ(w_i × s_i) / Σ(w_i),  S_c ∈ [0, 1]
```
Where: n = number of selected factors, w_i = weight of factor i, s_i = option score for factor i

**Default weights**: Data quality (0.35), Local training quality (0.25), Audit logs/Interoperability/Anonymization/Secure network (0.10 each). Data quality + local training quality capped at combined 0.5 to prevent correlated-dimension overweighting.

### Noise Scale Mapping
```
σ_i = σ_max − (σ_max − σ_min) × S_c
    = σ_min + (1 − S_c)(σ_max − σ_min)
```
- **σ_min = 0.4** (fully compliant site → minimum noise)
- **σ_max = 1.0** (minimal compliance → maximum noise)
- Linear interpolation: highly compliant sites (→1) get lower noise; every client uses at least σ_min

### Analytical Sensitivity Bound
A perturbation of any single weight w_i by Δ ∈ [−0.10, +0.10] induces |ΔC_i| ≤ Δ, propagating to |Δσ_i| ≤ Δ × (σ_max − σ_min) = 0.6Δ. A 10% single-weight perturbation moves σ_i by at most 0.06 on the [0.4, 1.0] scale.

## Server-Side DP-SGD Architecture

### The Aggregator Dataset
- 1/16 of source training pool reserved as aggregator dataset
- Fixed and reused across all 50 rounds and 16 clients
- DP-SGD runs on this dataset at the server (not on client data)
- Stratified to match class-label distribution

### Algorithm
1. **Client Training**: Each client trains locally for 3 epochs
2. **First-Round Noise**: Round 1 client updates clipped (C=1.0) and perturbed with σ_min Gaussian noise before transmission
3. **DP Processing**: Server runs 1 DP-SGD epoch on aggregator dataset with per-client σ_i (from compliance score)
4. **Aggregation**: FedAvg/FedMedian/FedProx/FedYogi/FedAdam on noisy updates
5. **Broadcast**: Global model returned to clients

### Privacy Accounting
- Subsampled Gaussian mechanism via Opacus RDP accountant
- Cumulative ε under full sequential composition:
  - BreastMNIST: ε ≈ 1434 (n_agg=49, q=0.65)
  - PneumoniaMNIST: ε ≈ 513 (n_agg=366, q=0.087)
  - Per-client-epoch cost: ε ∈ [2, 6]
- **Scope**: Formal (ε,δ)-DP bound applies to the aggregator dataset, NOT to client data. Client-level formal DP requires composition with secure aggregation (Future Work).

## Key Empirical Results

### Inclusivity (Experiment 1 vs 4)
Including 12 lower-compliance clients (4 compliant + 12 non-compliant) vs compliant-only:
- **Pooled mean**: +2.8 pp accuracy improvement
- **Per-strategy**: Positive for 4/5 strategies (FedAvg +4.5, FedMedian +6.8, FedProx +5.2, FedYogi +1.6; FedAdam −4.1)
- **Individual max gain**: +17.0 pp (FedMedian)
- **Significance**: No per-strategy test reached significance at n=5 seeds; pooled sign test p=0.359
- **Interpretation**: Directionally consistent but not statistically significant at this seed count

### Mechanism (Experiment 1 vs 6)
Compliance-weighted allocation vs uniform DP at same mean noise scale:
- **Pooled difference**: +0.09 pp (statistical equivalence)
- **Interpretation**: The compliance-weighted mechanism neither improves nor degrades utility relative to uniform noise — its value is governance (auditable, compliance-justified per-site noise allocation) at zero utility cost

### Strategy Robustness (Reference Implementation)
The per-client server-side DP mechanism reverses the legacy strategy ranking:
- **FedMedian**: Most robust under per-client noise (coordinate-wise median suppresses per-client DP noise)
- **FedAvg/FedProx**: Stable under heavy per-client noise
- **FedYogi/FedAdam**: Fragile under heavy per-client noise (adaptive optimizers lock onto noise-dominated directions)

### First-Round Noise Cost
Applying σ_min=0.4 to first-round updates:
- BreastMNIST: −1.3 pp average across strategies
- PneumoniaMNIST: −2.5 pp (FedAvg)
- **Interpretation**: First-round protection obtained at small utility cost

## SMPC Compatibility

| Strategy | SMPC Compatible | Notes |
|---|---|---|
| FedAvg | ✓ | Sum-based secure aggregation directly compatible |
| FedProx | ✓ | Sum-based, compatible |
| FedYogi | ✓ | Server-side adaptive optimizer, no individual update visibility needed |
| FedAdam | ✓ | Server-side adaptive optimizer, compatible |
| FedMedian | ✗ | Requires per-coordinate median of individual values; needs robust-SMPC variants |

4/5 strategies are architecturally ready for secure aggregation integration.

## Deployment Decision Matrix

| Use Case | Recommended Strategy | Compliance Distribution | Notes |
|---|---|---|---|
| Mixed-compliance clinical consortium | FedMedian or FedAvg | Heterogeneous (4+ low-compliance) | FedMedian suppresses per-client noise best |
| Homogeneous high-compliance federation | FedAvg or FedProx | Mostly compliant | Simpler implementation; noise is low |
| Research consortium with DP focus | FedProx | Moderate heterogeneity | Handles client heterogeneity well |
| Production enterprise FL | FedAvg | Moderate | Simplest to deploy and maintain |
| Avoid in high-noise settings | FedYogi, FedAdam | High heterogeneity | Adaptive optimizers degrade under per-client noise |

## A-Tech Application Matrix

### A-Coder
- **Application**: Federated code intelligence across organizations with varying compliance postures
- **Compliance factors**: Code security practices, data handling policies, encryption standards, audit logging
- **Benefit**: Enables participation from smaller development teams with less mature compliance infrastructure
- **Privacy property**: Code patterns stay local; only model updates are shared with compliance-weighted noise

### Be Practical
- **Application**: Cross-institutional learning analytics with compliance-gated participation
- **Compliance factors**: Student data privacy practices, FERPA/GDPR compliance, learning record security
- **Benefit**: Enables smaller educational institutions to contribute to federated learning models
- **Privacy property**: Student behavioral data stays on-device; only model updates with compliance-calibrated noise

### Builder's Club
- **Application**: Community federated model training across organizations with diverse governance maturity
- **Compliance factors**: Open-source governance practices, contribution transparency, security practices
- **Benefit**: Lowers barrier for smaller organizations to participate in community AI model training
- **Privacy property**: Each organization's contribution is noise-calibrated to their governance maturity

## When to Use This Skill

- Designing FL systems for heterogeneous clinical or enterprise consortia
- When compliance-gated participation is needed to include resource-constrained sites
- Allocating DP noise proportionally to institutional readiness rather than uniformly
- Evaluating whether compliance-weighted noise allocation is appropriate vs uniform DP
- Building inclusive FL ecosystems that don't structurally exclude lower-compliance institutions
- Selecting aggregation strategies for mixed-compliance federations

## Limitations

1. **Formal DP scope**: (ε,δ) bound applies to aggregator dataset, not client data. Client-level formal DP requires secure aggregation (Future Work).
2. **Honest self-reporting**: Compliance scores assumed accurately reported. Misreporting risk is partially self-limiting (over-reporting → less noise → weaker protection + larger ε contribution) but requires verification mechanisms for adversarial settings.
3. **Benchmark scale**: Evaluated on MedMNIST (128×128). Full-resolution clinical imaging, EHR, genomic data validation is future work.
4. **Seed count**: Per-strategy inference at n=5 seeds has limited power. Directionally consistent but not statistically significant.
5. **Cumulative ε is large**: ε ≈ 513-1434 under full composition. Tighter accounting (PRV accountant) was numerically unstable at this composition depth.
6. **Aggregator dataset is public benchmark data**: In production, aggregator dataset privacy properties differ.

## Cross-References

- `federated-learning-for-privacy-preserving-ai` — Foundational FL skill this extends with compliance-aware noise
- `sheld-fl-self-learning-heterogeneous-dp-framework` — Adaptive DP approach (sensitivity-driven vs compliance-driven)
- `dp-lac-lightweight-adaptive-clipping` — Adaptive clipping for DP-FL (complementary technique)
- `eris-federated-shard-aggregation` — Distributed aggregation with privacy amplification
- `federated-consent-architecture-agent-systems` — Consent layer for cross-tenant FL
- `federated-byzantine-robust-partial-participation` — Byzantine robustness for FL
- `federated-llm-on-device-personalization` — On-device personalization stack
- `google-gboard-private-fl-dp` — Production FL+DP blueprint

## A-Tech Alignment

- **Open-source AI**: Framework and compliance scoring tool released as open source; uses Flower, PyTorch, Opacus
- **Data privacy**: Core principle — compliance-weighted noise allocation; client data never leaves institution; formal DP on aggregator dataset
- **Financial freedom**: Enables resource-constrained institutions to participate in FL → broader data diversity → better models for under-represented populations
- **Practical implementation**: 65 experiments, 5 aggregation strategies, 2 datasets, reproducible code (github.com/santhoshcameo/inclusive-privacy-with-compliance-fl)