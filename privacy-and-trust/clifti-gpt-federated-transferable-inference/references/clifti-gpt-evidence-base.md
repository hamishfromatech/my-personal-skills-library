# Clifti-GPT — Full Evidence Base

## Citation

Bakhtiari, M., Elkjaer, M.L., Can, A.O., Oubounyt, M., Baumbach, J. "Clifti-GPT: privacy-preserving federated fine-tuning and transferable inference of foundation models on clinical single-cell data." BioData Mining 19:58 (published Aug 5, 2026). doi:10.1186/s13040-026-00582-w. Code: github.com/Mohammad-Bakhtiari/clifti-GPT (Apache 2.0). Preprocessed datasets: Zenodo 20491148; initialized PyTorch models: Zenodo 20489646.

Affiliations: University of Hamburg, Copenhagen University Hospital (Rigshospitalet), Helmholtz Munich, TUM, University of Southern Denmark. Funded by EU Horizon CVDLINK (101137278), BMFTR NetMap (031L0309B), IHI JU (101219324).

## Privacy Problem Framing

1. scRNA-seq count matrices are **not anonymous**: they encode donor-specific molecular signals (genotype–phenotype correlations, eQTL-driven states, disease signatures); identity and sensitive traits inferable via linking to quasi-identifiers/metadata/external genomic resources; private information demonstrably leaks from count matrices (Cell 2024).
2. Metadata is often the bigger risk: rare diagnoses, tissue sources, treatment arms, cohort membership directly narrow the donor set.
3. Institutional governance prohibits centralization (IP, publication priority, regulatory compliance).
4. FL alone is insufficient: "interrogation" attacks — model-inversion reconstructs gene-expression profiles from outputs; gradient-leakage recovers count data from shared updates.

## Architecture

- Base model: scGPT (pre-trained on 33M+ cells, CELLxGENE).
- **Federated fine-tuning**: additive secret sharing (SMPC/CrypTen); clients secret-share local model updates; coordinator computes FedAvg or FedProx in secret-shared domain; standard semi-honest threat model.
- **Federated binning** (privacy-preserving preprocessing): five strategies — local quantile; federated weighted-average; SMPC-protected weighted-average; federated secure histogram (global max → public grid → secret-shared local histograms → secure cumulative sums → quantile cuts in SMPC domain); SMPC-protected histogram. Result: harmonized bins across sites without centralizing raw data; annotation accuracies broadly similar across strategies.
- **Transferable inference** (the novel paradigm): zero-shot reference mapping and annotation transfer without exchanging raw data, embeddings, or models. Pipeline: secret-shared query embeddings → per-client squared-Euclidean distance matrices and top-k selection (one-hot argmin under secret sharing, Λ-suppression of selected minima) → concatenated global distance matrix → global k-nearest-neighbor selection → per-client label voting under secret sharing → only final predictions revealed.
- Design principle: restrict cross-site exchange to **low-dimensional, discrete voting statistics** — inherently minimizes information entropy of results, more resistant to reconstruction than gradient/embedding sharing.

## Threat Model (precise bounds)

- Protected: intermediate client-level quantities during computation (updates, embeddings, distances, votes) under semi-honest assumptions.
- NOT protected (explicitly acknowledged): output privacy (model inversion, membership inference on final models/predictions), malicious clients, collusion, poisoning, Byzantine behavior. Those require malicious-secure SMPC, Byzantine-robust aggregation, auditing, governance. The paper's honesty here is a model for the field.

## Datasets & Scenarios

- Multiple Sclerosis: 21,312 cells / 18 types / 3 brain regions; 4 reference clients.
- Covid-19: 19,922 cells / 36 types / 9 cohorts; 6 reference clients (batch-effect analysis).
- Human Pancreas: 14,746 cells / 11 types / 5 studies; 4 clients.
- Lung-Kim: 30,472 cells / 10 types / 14 tumors; 10 clients.
- Cell Line: 9,531 cells / 2 types / 3 batches; 2 clients.
- Myeloid: 13,178 cells / 16 types / 84 batches; scalability scenarios Top5/10/20/30 clients.

## Results Detail

- Fine-tuning: matches centralized on MS, Lung-Kim, CL, Myeloid; HP trails slightly until extended rounds (156 rounds → within 1%). FedProx-SMPC (μ=0.01) often better for heterogeneous cohorts.
- Communication efficiency: 90% of centralized accuracy in 1 round; 99% in ≤2 rounds (MS, Myeloid); 99% of centralized metrics within 4 rounds on several datasets.
- Per-class TPR preserved; low-represented cell types (25–98 cells) show minor fluctuation (Mast cells 0.71 federated; Phagocytes −0.08) — the inherent trade-off of structurally imbalanced federated cohorts.
- Reference mapping: matches centralized on CL (all metrics), matches/improves HP & Lung-Kim, +0.01 MS.
- Worst-case margin across all datasets/scenarios: −0.03 accuracy; within 2% accuracy and 4% other metrics overall.
- Scalability: Top30 (most heterogeneous) needs 180–186 rounds but matches centralized; "rest" client lacks data in Top30.
- Batch effects: federated more sensitive than centralized; scGen correction closes the gap (0.69→0.93 accuracy on Covid-19 with 61 rounds).

## Cryptographic Overhead (CrypTen, GPU, 3 parties)

| Workflow | Wall-clock | Overhead |
|---|---|---|
| Secure aggregation of fine-tuning (|θ|=10⁶) | ~31–36 ms (C=2,3,5) | ~43–112× |
| Secure weighted-average binning | ~4–19 ms (C=2,5,10) | moderate |
| Secure histogram binning | ~9–19 ms | moderate |
| Secure KNN, n_q=2000, k=10, n_r=10³, C=5 | ~1.4 min | ~11k× |
| Secure KNN, n_r=10⁴ | ~7.5 min | ~70k× |

- Interpretation: model-weight sharing and statistical binning are local, non-interactive operations on shares (cheap); secure KNN demands interactive encrypted distance computation, secure comparison, masked min-selection, index extraction, suppression (expensive). **Secure cell-to-atlas KNN is the system bottleneck, not weight sharing.**
- Deployment consequence: clinical sites run local preprocessing/embedding/secret-sharing; dedicated high-performance computational parties run secure aggregation + KNN. Decoupled architecture removes crypto burden from data-contributing hospitals.

## Comparison Points (library context)

- vs Tabula (federated foundation model for scRNA-seq): Tabula does NOT integrate SMPC or comparable PETs — Clifti-GPT is among the first privacy-preserving federated frameworks for scRNA-seq foundation models.
- vs DP approaches (AdaDP-FedSec, DPTrainer): DP adds noise for formal output guarantees; SMPC protects process without noise but no formal output guarantee. Complementary layers, not substitutes.
- vs HADES (selective encryption): HADES encrypts features at the data level; Clifti-GPT protects the computation on model updates/statistics.
- vs FedScGen (same group, prior): federated batch-effect correction → Clifti-GPT extends to full foundation-model fine-tuning + transferable inference.

## A-Tech Applications

- Reference architecture for any multi-institution sensitive-data AI collaboration (health, legal, finance).
- The transferable-inference pattern generalizes: anywhere zero-shot inference across silos is needed (e.g., federated RAG over private document corpora — exchange secure votes/similarities, not documents or embeddings).
- Overhead budgeting template: cheap (aggregation/binning) vs cliff (secure KNN) — plan reference-set sizes and compute-party topology before committing.
- Honest threat-model disclosure as a trust design principle: bounded claims > inflated claims (echoes the trust-by-verification principle from the agent-economy skills).
