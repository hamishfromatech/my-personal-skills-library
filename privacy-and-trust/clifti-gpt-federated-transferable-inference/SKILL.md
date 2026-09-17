---
name: clifti-gpt-federated-transferable-inference
description: Applies Clifti-GPT's SMPC-protected federated fine-tuning and transferable inference pattern for foundation models on sensitive clinical data. Use when designing privacy-preserving multi-institution AI collaboration, evaluating federated inference vs federated training, planning GDPR-compatible model deployment across silos, or assessing cryptographic overhead budgets.
---

# Clifti-GPT: Federated Fine-Tuning + Transferable Inference

## Overview

Clifti-GPT (Bakhtiari, Elkjaer, Can, Oubounyt, Baumbach; BioData Mining, Aug 5, 2026, Apache 2.0, code on GitHub) is a privacy-preserving federated framework built on the scGPT single-cell foundation model that solves **both halves** of the sensitive-data collaboration problem:

1. **Federated fine-tuning** with additive secret sharing (SMPC/CrypTen) — model updates split into cryptographic shares, aggregated without reconstructing any client's gradients
2. **Transferable inference** — a new paradigm where zero-shot predictions (reference mapping, annotation transfer) run across decentralized sites by exchanging **securely aggregated low-dimensional statistics** (distances, votes) instead of raw data, embeddings, or models

The distinctive contribution is #2: restricting cross-site exchange to discrete voting statistics "inherently minimizes the information entropy of the results," making it more resistant to reconstruction attacks than sharing gradients or embeddings.

## Threat Model & Design

- **Semi-honest (honest-but-curious) SMPC**: parties follow the protocol but may inspect intermediate values. Secret sharing protects computation-process privacy — distinct from ordinary FL data locality, which does NOT prevent leakage from intermediate updates, embeddings, or outputs.
- Batch effects and metadata are treated as complementary privacy risks: scRNA-seq count matrices are potentially identifiable (donor linkage, disease inference); metadata (rare diagnoses, cohort membership) can directly expose clinical context.
- Explicit limitation, honestly stated: SMPC does not solve output privacy (model inversion, membership inference on final models) or malicious clients — those need Byzantine-robust aggregation, auditing, and governance. The claim is bounded and precise.

## Results

| Metric | Value |
|---|---|
| Performance vs centralized scGPT | Within 4% (accuracy, precision, recall, macro-F1) across 6 datasets (MS, Covid, HP, Lung-Kim, CL, Myeloid) |
| Convergence speed | 99% of centralized accuracy in ≤2 federated rounds (MS, Myeloid); 90% in 1 round |
| Scale | 30 clients with <2% accuracy loss (Top30 scenario, Myeloid) |
| Privacy-preserving reference mapping | Matches centralized baseline on CL; matches or improves HP, Lung-Kim; +0.01 on MS |
| GDPR posture | Compatible by design; data governance requirements met without centralizing |

## Cryptographic Overhead Profile (the practical budget)

Profiling with CrypTen (GPU, 3 parties) — the most useful engineering data in the paper:

| Operation | Absolute time | Overhead vs plaintext |
|---|---|---|
| Secure fine-tuning aggregation (10⁶ params) | ~31–36 ms | ~43–112× |
| Secure weighted-average binning | ~4–19 ms | moderate |
| Secure histogram binning | ~9–19 ms | moderate |
| **Secure KNN (reference mapping)** | 1.4 min → 7.5 min (10³→10⁴ refs) | **~11,000–70,000×** |

**The bottleneck is secure KNN** — exhaustive interactive secure comparisons, not model-weight sharing. Deployment architecture follows directly: clinical sites do local preprocessing + embedding + secret sharing; dedicated high-performance computational parties handle the expensive secure aggregation/KNN kernels. This decoupling removes the crypto burden from data-contributing institutions.

## Key Engineering Details

- **Federated binning**: five strategies evaluated (local quantile, federated weighted-average, SMPC-protected weighted-average, federated histogram, SMPC-protected histogram) — SMPC variants preserve per-client histogram privacy while standardizing bins across sites.
- **Secure KNN + voting pipeline**: secret-shared embeddings → per-client top-k distance/index shares → global neighbor selection via one-hot argmin under secret sharing → per-client label voting → only the final prediction revealed.
- **Transferable inference distinction**: fine-tuning is privacy-aware regarding final weights; transferable inference prioritizes *output* privacy (no raw data, embeddings, or local models exchanged).
- Crypto overhead (43–112× on aggregation) is tolerable in absolute time; the KNN hotspot scales sharply with reference-set size — plan reference atlases accordingly.

## Decision Framework: When to Use Which Privacy Pattern

| Situation | Pattern |
|---|---|
| Multi-institution fine-tuning, curious coordinator | SMPC secure aggregation (Clifti-GPT) or SecAgg+ / Shamir+Paillier (AdaDP-FedSec pattern) |
| Formal output-privacy guarantee needed | DP-SGD layer (DPTrainer pattern) — SMPC alone doesn't provide it |
| Zero-shot inference across silos (no training) | **Transferable inference** (exchange voting stats, not embeddings) — lowest leakage surface |
| Malicious clients suspected | None of the above suffice — need Byzantine-robust + malicious-secure SMPC + audit |
| Extreme KNN scale (>10⁴ references) | Dedicated compute parties or approximate methods; secure exact-KNN is the cost cliff |

## A-Tech Values Alignment

- **Open-source AI**: Apache 2.0, public code, public datasets, reproducible initialized models (Zenodo). Built on scGPT — an open foundation model. A complete open stack.
- **Data privacy**: The core contribution; honest threat-model bounds; GDPR-compatible by design; the transferable-inference pattern is a genuinely novel privacy engineering move (minimize exchanged entropy).
- **Financial freedom**: Clinical sites contribute commodity compute; heavy crypto kernels go to shared computational parties — a cost-sharing architecture small institutions can actually afford, versus each buying the full secure stack.
- **Practical implementation**: Public code, five binning strategies, complete overhead tables, scalability curves to 30 clients — immediately deployable reference architecture.

## Related Skills

- `privacy-and-trust/adadp-fedsec-adaptive-dp-secure-aggregation/` — the DP+secure-aggregation complement (adaptive ε vs SMPC process-privacy)
- `privacy-and-trust/dptrainer-drop-in-differential-privacy/` — the DP layer for output privacy
- `privacy-and-trust/hades-selective-feature-encryption-federated-learning/` — selective encryption alternative
- `privacy-and-trust/` category — federated learning governance, secure aggregation skills
- `ai-agents-and-workflows/` — foundation-model infrastructure context
