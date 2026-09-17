---
name: ehds-federated-learning-compliance-framework
description: Implement privacy-preserving federated learning compliant with the European Health Data Space (EHDS, Regulation EU 2025/327) — a three-layer architecture mapping governance (HDAB integration, Art. 53 data permits, Art. 71 opt-out registries), FL orchestration (17 algorithms, DP-SGD, secure aggregation, Byzantine resilience), and data holders (FHIR R4, OMOP-CDM). Use when building cross-border health AI, EHDS-compliant federated systems, healthcare FL deployments, or governance-aware federated architectures for regulated domains. NOT for general federated learning without EHDS governance, non-health domains, or FL without regulatory compliance requirements.
---

# EHDS Federated Learning Compliance Framework

## The Problem

The EHDS mandates cross-border secondary use of health data across 27 EU Member States by 2029. Yet fewer than 1 in 4 FL implementations achieve sustained production deployment in healthcare (Fröhlich et al., JMIR 2025). The dominant barriers are **not technical** — they are legal: gradient data classification under GDPR, cross-border privacy budget harmonization, controller/processor allocation.

No existing framework jointly addresses: systematic barrier evidence + technical implementation with state-of-the-art algorithms + EHDS governance operationalization.

## The Three-Layer Architecture

### Layer 1: Governance
- **Health Data Access Body (HDAB) integration**: OAuth2/mTLS authentication, HDAB API
- **Data Permit Manager (Art. 53)**: lifecycle PENDING → ACTIVE → EXPIRED; purpose limitation module
- **Opt-Out Registry (Art. 71)**: record/patient/dataset-level filtering
- **Cross-Border Coordinator (Arts. 46, 50)**: multi-HDAB coordination, 10 EU country profiles
- **GDPR Art. 30 audit trail**: immutable records of processing activities, 7-year retention

### Layer 2: FL Orchestration (Secure Processing Environment)
- **17 FL algorithms** (2017–2025): FedAvg, FedProx, SCAFFOLD, FedNova, FedDyn, FedAdam/Yogi/Adagrad, Per-FedAvg, Ditto, FedLC, FedSAM, FedDecorr, FedSpeed, FedExP, FedLESAM (ICML 2024 Spotlight), HPFL (ICLR 2025)
- **Differential privacy**: DP-SGD with RDP accounting, 5–6× tighter bounds than naive composition; ε configurable per data permit
- **Secure aggregation**: pairwise masking (ECDH SECP384R1), Shamir secret sharing, homomorphic encryption (CKKS via TenSEAL)
- **6 Byzantine resilience methods**: Krum, Multi-Krum, Trimmed Mean, Coordinate-wise Median, Bulyan, FLTrust (defends against f < n/3 adversarial clients)

### Layer 3: Data Holders
- **Adaptive local training engine**: CUDA/MPS/CPU; never exports raw data
- **FHIR R4 preprocessing**: 6 resource types
- **OMOP-CDM harmonization**: SNOMED CT, ICD-10, LOINC, ATC, UCUM
- **Secure gradient communication**: AES-256-GCM, mTLS

## EHDS Compliance Mapping

| EHDS Article | Requirement | Framework Component |
|---|---|---|
| Art. 33 | Secondary use authorisation | HDAB API + Permit validation |
| Art. 46 | Cross-border processing | Multi-HDAB coordinator (10 EU profiles) |
| Art. 50 | Secure Processing Environment | All aggregation within SPE boundary |
| Art. 53 | Permitted purposes | Purpose limitation module, permit lifecycle |
| Art. 71 | Citizen opt-out | Registry filtering (record/patient/dataset level) |
| GDPR Art. 30 | Records of processing | Immutable audit trail, 7-year retention |

## Algorithm Selection Guide

| EHDS Scenario | Recommended | Rationale |
|---|---|---|
| Homogeneous Member States | FedAvg | Simplicity, well-studied convergence |
| Heterogeneous Member States | SCAFFOLD | Variance reduction under client drift |
| Resource-limited institutions | FedAdam | Fast convergence, fewer rounds |
| Privacy-critical studies | FedAvg + DP | Well-studied DP composition bounds |
| Sparse participation / dropout | FedProx | Proximal term, dropout resilience |
| Label-imbalanced populations | FedLC | Class-frequency logit calibration |
| Communication-constrained | FedSpeed | Fewer communication rounds |
| Per-hospital personalisation | HPFL (ICLR 2025) | Shared backbone + local decision boundaries |

## Key Experimental Findings (6,004+ experiments)

| Finding | Evidence |
|---|---|
| Personalisation gains up to 26.8 pp | Breast Cancer: Ditto 79.1% vs FedAvg 52.3% |
| Best-FL gap to centralised ≤ 2.4 pp | PTB-XL: HPFL 92.5% vs centralised 92.6% |
| HPFL outperforms FedAvg on all 3 tabular datasets | p = 0.004, 0.002, 0.031 (Wilcoxon, 10-seed); pooled p < 0.001 |
| DP at ε=10 imposes negligible cost | < 2 pp accuracy cost across PTB-XL and Cardiovascular |
| DP noise acts as regularisation | FedAvg ε=5 → 78.7% vs 52.3% without DP on Breast Cancer (+26.4 pp) |
| Art. 71 opt-out at 30% is negligible | < 1 pp drop on adequately sized datasets |
| Full EHDS compliance costs −0.7 pp | Ditto under simultaneous data minimisation + opt-out + DP (p < 0.001) |
| Compound stress: personalisation wins 81% | Ditto outperforms FedAvg in 81% of conditions (+9.6 pp mean) |
| Cross-border heterogeneous DP: −0.9 pp | Per-client budgets vs no-DP; mixed > strictest-wins (+3.8 pp) |
| Hyperparameter-insensitive: ≤1.44 pp | Lambda 100× variation; model-invariant across MLP and TabNet |
| Governance overhead < 1.1% per round | Ditto +1.0%, HPFL −0.3% (within noise) |
| PTB-XL validates European FL | 92.5% accuracy (HPFL), 5-class ECG, 52 sites, Jain fairness 0.999 |

## Privacy & Security

### Differential Privacy
- **Mechanism**: Gaussian noise with L2 gradient clipping (max norm = 1.0)
- **Accounting**: RDP → (ε, δ)-DP conversion with optimal order selection (5–6× tighter than naive)
- **Budget**: configurable per data permit (default ε=1.0, δ=10⁻⁵)
- **Enforcement**: BudgetExhaustedError terminates training at HDAB-approved threshold
- **Tracking**: per-round cumulative expenditure with audit logging

### Threat Model
| Adversary | Capability | Defence |
|---|---|---|
| A1: Honest-but-curious server | Follows protocol, infers from gradients | Central DP + Secure aggregation |
| A2: Malicious clients (< n/3) | Arbitrary protocol deviation | 6 Byzantine-resilient aggregation rules |
| A3: External attacker | Black-box model access | Art. 71 output filtering + HDAB permit control |

## A-Tech Value Alignment

| Value | Alignment |
|---|---|
| Open-source AI | Reference implementation is open-source Python (~40K lines, 159 modules, MIT license); 17 algorithms reproducible |
| Data privacy | Raw health data never leaves institutional boundaries; only encrypted gradients exchanged within SPE; DP + secure aggregation + opt-out registry |
| Financial freedom | Reduces compliance cost of cross-border health AI; governance layer is configurable without architectural changes for production binding (2027–2029) |
| Practical implementation | 6,004+ experiments validated across 8 datasets; interactive dashboard; full reproducibility outputs (JSON, CSV, LaTeX, PNG) |

## Implementation Notes

- **Governance layer**: fully functional simulation backend (OAuth2/mTLS, permit CRUD, LRU-cached registry lookups) — requires only endpoint configuration for production binding to HDAB services (expected 2027–2029)
- **FHIR R4**: 6 resource types supported; HL7 FHIR R4 transformation pipeline
- **OMOP-CDM**: SNOMED CT, ICD-10, LOINC, ATC, UCUM harmonization
- **Scalability**: each component scales independently (Task Assignment, Aggregator, Model Updater instances)
- **Disaster recovery**: Spanner change history + GCS per-round storage; service resumes from last round

## What This Skill Is NOT

- NOT a general FL framework (Flower, NVIDIA FLARE, TFF cover general FL without EHDS governance)
- NOT a legal opinion on GDPR/EHDS compliance (the framework operationalizes requirements; legal review still needed)
- NOT limited to healthcare (the 3-layer pattern — governance / orchestration / data holders — generalizes to other regulated domains by swapping the governance layer)
- NOT production-deployed today (governance layer is simulation; production HDAB binding expected 2027–2029)

## References

See `references/ehds-fl-evidence-base.md` for the full algorithm catalogue, dataset coverage, experimental results tables, privacy-utility tradeoff data, and threat model details.