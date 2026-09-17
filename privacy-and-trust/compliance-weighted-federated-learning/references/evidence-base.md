# Compliance-Weighted Federated Learning — Evidence Base

## Primary Source

Parampottupadam, S., Coşğun, M., Pati, S., Zenk, M., Roy, S., Bounias, D., Hamm, B., Sav, S., Maier-Hein, K., & Floca, R. (2026). "Inclusive Federated Learning Through Compliance-Weighted Noise Allocation in Healthcare AI." arXiv:2505.22108v4. German Cancer Research Center (DKFZ) / Heidelberg University / Bilkent University / MLCommons.

Code: github.com/santhoshcameo/inclusive-privacy-with-compliance-fl

## Study Design

### Datasets
- **PneumoniaMNIST**: n=5,856 (chest radiograph classification)
- **BreastMNIST**: n=780 (breast ultrasound classification)
- Image size: 128×128
- 16 clients, 50 FL rounds, 5 random seeds per configuration (reference implementation)

### Architecture
- ResNet-18 with GroupNorm (32 groups) replacing BatchNorm (incompatible with per-sample gradient computation in Opacus DP-SGD)
- Batch size: 32
- Learning rate: 0.001
- 3 local epochs per client per round
- 1 DP-SGD epoch on aggregator dataset per client per round

### Aggregator Dataset
- 1/16 of source training pool, stratified by class label
- PneumoniaMNIST: n_agg ≈ 366
- BreastMNIST: n_agg ≈ 49
- Fixed and reused across all 50 rounds and 16 clients
- Sampling rate q = batch_size / n_agg

### Privacy Accounting
- Opacus 1.5.4 RDP accountant
- Subsampled Gaussian mechanism
- δ = 10⁻⁵
- Full sequential composition across all DP-SGD steps

## Compliance Scoring Tool

### 12 Compliance Factors

| Factor | Standards/Options |
|---|---|
| Data Encryption | AES-256 (NIST), AES-128 (Healthcare Minimum) |
| Ethical AI Policies | EU AI Act, US FDA Guidelines |
| Privacy Regulations | HIPAA, GDPR |
| Data Quality | DICOM Standard, Partially Validated Data |
| Anonymization | ISO/TS 25237:2017, Pseudonymization |
| Interoperability | HL7/FHIR |
| Secure Network | NIST Cybersecurity Framework |
| Authentication | MFA, RBAC |
| Audit Logs | SOC 2 Type II |
| Patient Consent | HL7 CDA Compliant |
| TEEs | Intel SGX, AMD SEV |
| Local Training Quality | High (>95%), Moderate (85-95%) |

### Default Weights
- Data quality: 0.35
- Local training quality: 0.25
- Audit logs: 0.10
- Interoperability: 0.10
- Anonymization: 0.10
- Secure network: 0.10
- Cap: Data quality + local training quality ≤ 0.5 combined

### Noise Scale Mapping
```
σ_i = σ_min + (1 - S_c) × (σ_max - σ_min)
σ_min = 0.4, σ_max = 1.0
```

| Compliance Score | σ_i | Per-Step Contribution |
|---|---|---|
| 1.00 (fully compliant) | 0.40 | Largest per-step RDP contribution |
| 0.75 | 0.55 | Above-mean contribution |
| 0.50 | 0.70 | Mid-range |
| 0.25 | 0.85 | Below-mean contribution |
| 0.00 (minimal) | 1.00 | Smallest per-step contribution |

## Experimental Configurations

| Experiment | Compliant Clients | Non-Compliant | Compliance DP | Minimum DP |
|---|---|---|---|---|
| 1 | 4 | 12 | Yes | Yes |
| 2 | 10 | 6 | Yes | Yes |
| 3 | 16 | 0 | Yes | Yes |
| 4 | 4 | 0 | No | Yes |
| 5 | 16-Vanilla | 0 | No | No |
| 6 | 16 | 0 | Yes (Uniform) | - |

## Key Results (Reference Implementation, 5 Seeds)

### Inclusivity: Experiment 1 vs Experiment 4 (BreastMNIST)

| Strategy | E1 Mean ± SD | E4 Mean ± SD | Paired Diff (pp) | p-value |
|---|---|---|---|---|
| FedAvg | 61.54 ± 8.38 | 57.09 ± 4.05 | +4.45 | 0.329 |
| FedMedian | 64.22 ± 7.45 | 57.42 ± 4.49 | +6.80 | 0.165 |
| FedYogi | 51.62 ± 3.61 | 50.00 ± 0.00 | +1.62 | 0.374 |
| FedProx | 62.02 ± 7.93 | 56.85 ± 4.11 | +5.16 | 0.219 |
| FedAdam | 50.00 ± 0.00 | 54.14 ± 4.76 | -4.14 | 0.124 |

**Pooled**: Mean +2.78 pp; Sign test p=0.359; Wilcoxon p=0.107

### Mechanism: Experiment 1 vs Experiment 6 (BreastMNIST)

| Strategy | E1 Mean ± SD | E6 Mean ± SD | Paired Diff (pp) | p-value |
|---|---|---|---|---|
| FedAvg | 61.54 ± 8.38 | 61.72 ± 8.43 | -0.18 | 0.782 |
| FedMedian | 64.22 ± 7.45 | 63.51 ± 6.93 | +0.71 | 0.646 |
| FedYogi | 51.62 ± 3.61 | 50.00 ± 0.00 | +1.62 | 0.374 |
| FedProx | 62.02 ± 7.93 | 63.68 ± 7.94 | -1.67 | 0.365 |
| FedAdam | 50.00 ± 0.00 | 50.06 ± 0.14 | -0.06 | 0.374 |

**Pooled**: Mean +0.09 pp; Sign test p=1.000 (statistical equivalence)

### First-Round Noise Effect

| Dataset | Strategy | Without σ_min | With σ_min=0.4 | Effect (pp) |
|---|---|---|---|---|
| BreastMNIST | FedAvg | 58.54 | 56.41 | -2.13 |
| BreastMNIST | FedMedian | 61.17 | 58.15 | -3.03 |
| PneumoniaMNIST | FedAvg | 82.39 | 79.93 | -2.46 |

### Aggregator-Dataset Sensitivity (BreastMNIST)

| Aggregator Size | ε (δ=10⁻⁵) | FedAvg Acc (%) |
|---|---|---|
| 49 (default) | 1434 | 58.54 |
| 98 | 1074 | 62.51 |
| 156 | 717 | 50.90 |

### Compliance-Weight Sensitivity

| Scale | FedAvg Acc (%) | ε |
|---|---|---|
| ×0.9 | 58.15 | 1177 |
| ×1.0 | 58.54 | 1434 |
| ×1.1 | 58.15 | 1477 |

## Cumulative Privacy Budgets

| Configuration | Cumulative ε (δ=10⁻⁵) |
|---|---|
| BreastMNIST, Exp 1 (n_agg=49, 50 rounds) | ≈ 1434 |
| BreastMNIST, Exp 4 (n_agg default, 50 rounds) | ≈ 899 |
| PneumoniaMNIST, Exp 1 (n_agg=366, 50 rounds) | ≈ 513 |

## Threat Model

**Semi-honest (honest-but-curious) aggregator**:
- Server trusted to execute protocol correctly
- Server may attempt to learn about client data from information it legitimately observes
- DP guarantees on released global model w.r.t. aggregator dataset records
- Does NOT protect raw client updates in transit against malicious aggregator
- Client-level formal DP requires secure aggregation composition (Future Work)

## Adversarial Misreporting Analysis

| Quantity | Honest (S_c=0.3) | Over-Report (claims S_c=1.0) |
|---|---|---|
| σ_i | 0.82 | 0.40 |
| Protection of own data | Stronger | Weaker |
| Per-step ε contribution | Smaller | Larger (accelerates cumulative ε) |
| Net effect | — | Weaker own protection + looser shared bound |

**Self-limiting property**: Over-reporting reduces own protection AND accelerates shared budget consumption. But not sufficient — requires verification mechanisms (Future Work item 2).

## Future Work (13 Concrete Items)

1. Central DP with secure aggregation (client-level formal DP)
2. Dynamic compliance verification (scanners, attestation, audits, game-theoretic incentives)
3. Higher-resolution and multi-modal clinical validation
4. Tighter privacy accounting (PRV accountant)
5. Compliance-weight and noise-mapping sensitivity ablations
6. Larger-seed Monte Carlo with per-strategy paired tests
7. Head-to-head personalized-DP comparison
8. Expert validation of compliance scoring tool
9. Empirical privacy attacks (membership inference, gradient inversion)
10. Clinical-grade reporting (AUROC, calibration, per-class)
11. Fairness and inclusivity operationalized (per-site equity)
12. Systems-level benchmarking (wall-clock, compute, communication)
13. Aggregator-dataset size and reuse-policy ablation

## A-Tech Alignment

- **Open-source AI**: Framework and tool released open source; Flower, PyTorch, Opacus
- **Data privacy**: Compliance-weighted noise; client data stays local; formal DP on aggregator dataset
- **Financial freedom**: Enables resource-constrained institutions to participate → broader data diversity
- **Practical implementation**: 65 experiments, 5 strategies, 2 datasets, reproducible code