# EHDS FL Compliance Framework — Evidence Base

## Source
Liberti, F. (2026). "FL-EHDS: A Privacy-Preserving Federated Learning Framework for the European Health Data Space." IEEE 2nd International Conference on Federated Learning and Intelligent Computing Systems (FLICS 2026), Valencia, Spain, June 9–12, 2026. GitHub: FabioLiberti/FL-EHDS-FLICS2026. License: MIT.

## Algorithm Catalogue (17 algorithms, 6 categories)

| Algorithm | Venue | Category | Key Mechanism |
|---|---|---|---|
| FedAvg | AISTATS 2017 | Baseline | Weighted model averaging |
| FedProx | MLSys 2020 | Non-IID | Proximal regularisation (μ) |
| SCAFFOLD | ICML 2020 | Non-IID | Control variates for drift correction |
| FedNova | NeurIPS 2020 | Non-IID | Normalised averaging for unequal local steps |
| FedDyn | ICLR 2021 | Non-IID | Dynamic regularisation |
| FedAdam | ICLR 2021 | Adaptive | Server-side Adam momentum |
| FedYogi | ICLR 2021 | Adaptive | Controlled adaptive learning rate |
| FedAdagrad | ICLR 2021 | Adaptive | Server-side gradient accumulation |
| Per-FedAvg | NeurIPS 2020 | Personalisation | MAML-based meta-learning |
| Ditto | ICML 2021 | Personalisation | L2-regularised personal models |
| FedLC | ICML 2022 | Label skew | Logit calibration |
| FedSAM | ICML 2022 | Generalisation | Sharpness-aware flat minima |
| FedDecorr | ICLR 2023 | Representation | Decorrelation against dimensional collapse |
| FedSpeed | ICLR 2023 | Efficiency | Fewer communication rounds |
| FedExP | ICLR 2023 | Server-side | POCS-based step size |
| **FedLESAM** | **ICML 2024 Spotlight** | **Generalisation** | **Globally-guided sharpness-aware optimisation** |
| **HPFL** | **ICLR 2025** | **Personalisation** | **Shared backbone + personalised classifiers** |

Byzantine resilience (6 methods): Krum, Multi-Krum, Trimmed Mean, Coordinate-wise Median, Bulyan, FLTrust.

## Dataset Coverage (8 evaluated, 19 supported)

### Evaluated
| Dataset | Samples | Type | Classes | FL Partition | EHDS Category |
|---|---|---|---|---|---|
| PTB-XL ECG | 21,799 | Tabular | 5 | Natural (52 EU sites) | SCP-ECG diagnostics |
| Cardiovascular Disease | 70,000 | Tabular | 2 | Dirichlet (α=0.5) | Vitals, lab, risk factors |
| Diabetes 130-US | 101,766 | Tabular | 2 | Dirichlet (α=0.5) | EHR, ICD-9, medications |
| Heart Disease UCI | 920 | Tabular | 2 | Natural (4 hospitals) | Vitals, ECG, lab |
| Breast Cancer Wisconsin | 569 | Tabular | 2 | Dirichlet (α=0.5) | Pathology (FNA cytology) |
| Chest X-ray | 5,856 | Imaging | 2 | Dirichlet (α=0.5) | Radiology (DICOM) |
| Brain Tumor MRI | 7,023 | Imaging | 4 | Dirichlet (α=0.5) | Neuro-imaging (DICOM) |
| Skin Cancer | 3,297 | Imaging | 2 | Dirichlet (α=0.5) | Dermatology (DICOM) |

## Primary Benchmark — 7 Algorithms × 3 Datasets

| Algorithm | PTB-XL Acc (%) | PTB-XL Jain | CV Acc (%) | CV Jain | BC Acc (%) | BC Jain |
|---|---|---|---|---|---|---|
| FedAvg | 91.9 ± 0.5 | 0.999 | 71.1 ± 1.8 | 0.981 | 52.3 ± 17.9 | 0.608 |
| FedProx | 91.6 ± 0.7 | 0.999 | 71.5 ± 1.2 | 0.986 | 52.3 ± 17.9 | 0.608 |
| Ditto | 91.8 ± 0.3 | 0.999 | **82.5 ± 4.7** | 0.980 | **79.1 ± 12.5** | 0.606 |
| FedLC | 91.9 ± 0.5 | 0.999 | 71.1 ± 1.6 | 0.982 | 52.1 ± 18.1 | 0.606 |
| FedExP | 92.0 ± 0.2 | 0.999 | 71.1 ± 1.8 | 0.981 | 52.3 ± 17.9 | 0.608 |
| FedLESAM | 91.9 ± 0.5 | 0.999 | 71.1 ± 1.8 | 0.981 | 52.3 ± 17.9 | 0.608 |
| **HPFL** | **92.5 ± 0.3** | 0.999 | 82.3 ± 4.5 | 0.984 | 74.1 ± 20.9 | **0.867** |

## Medical Imaging — ResNet-18 + GroupNorm + FedBN

| Algorithm | Chest X-ray | Brain Tumor | Skin Cancer |
|---|---|---|---|
| FedAvg | 87.3% | 53.8% | 65.0% |
| Ditto | 80.0% | **77.3%** (+23.5 pp) | **90.5%** (+25.5 pp) |
| FedLESAM | **87.8%** | — | — |
| HPFL | 69.1% | 50.0% | 60.9% |

## Privacy–Utility Tradeoff (PTB-XL ECG, central DP)

| Algorithm | ε=1 | ε=10 | No DP |
|---|---|---|---|
| FedAvg | 52.3 | 92.4 | 91.9 |
| Ditto | 89.2 | 91.6 | 91.8 |
| HPFL | 87.1 | 92.4 | 92.5 |

**Key insight**: Personalised methods are remarkably DP-robust. At ε=1, FedAvg collapses (−39.6 pp) while Ditto and HPFL retain > 87% accuracy. At ε=10, privacy imposes negligible utility cost.

## Centralised vs Federated (Heart Disease UCI)

| Approach | Accuracy | Gap |
|---|---|---|
| Centralised (upper bound) | 79.6 ± 5.0% | — |
| Local-Only | 78.6 ± 2.9% | −1.0 pp |
| FL — Ditto | 76.0 ± 2.3% | −3.6 pp |
| FL — HPFL | 75.0 ± 2.8% | −4.6 pp |
| FL — FedAvg | 64.8 ± 7.9% | −14.8 pp |

## Statistical Significance (10-seed, Wilcoxon signed-rank)

| Algorithm | vs FedAvg (PTB-XL) | vs FedAvg (CV) | vs FedAvg (BC) | Pooled |
|---|---|---|---|---|
| Ditto | p=0.492 | p=0.002 | p=0.016 | p<0.001 |
| **HPFL** | **p=0.004** | **p=0.002** | **p=0.031** | **p<0.001** |

HPFL is the only algorithm significantly outperforming FedAvg on all three datasets individually.

## Framework Comparison

| Dimension | FL-EHDS | Flower | NVIDIA FLARE | TFF |
|---|---|---|---|---|
| FL Algorithms | 17 built-in | 12+ strategies | 5 built-in | 3 built-in |
| Byzantine Resilience | 6 methods | 4 methods | — | — |
| Differential Privacy | Central + Local | Central + Local | Built-in | Adaptive clip |
| Secure Aggregation | Pairwise + HE | SecAgg+ | Built-in + HE | Mask-based |
| EHDS Governance | Full | — | — | — |
| HDAB Integration | Yes | — | — | — |
| Data Permits (Art. 53) | Yes | — | — | — |
| Opt-out (Art. 71) | Yes | — | — | — |
| Healthcare Standards | FHIR R4 + OMOP | MONAI | MONAI | — |

## Key Barrier Finding

Evidence synthesis of 47 PRISMA documents (847 screened; GRADE-CERQual confidence assessment) identified that **unresolved regulatory questions** — gradient data classification under GDPR, cross-border privacy budget harmonisation — constitute the critical adoption blocker, NOT technical limitations.

## EHDS Regulatory Context

- Regulation (EU) 2025/327 on the European Health Data Space
- Mandates cross-border secondary use of health data across 27 EU Member States by 2029
- Fewer than 1 in 4 FL implementations achieve sustained production deployment in healthcare (Fröhlich et al., JMIR 2025)
- The 3-layer compliance framework is designed for incremental deployment during the 2025–2031 EHDS transition