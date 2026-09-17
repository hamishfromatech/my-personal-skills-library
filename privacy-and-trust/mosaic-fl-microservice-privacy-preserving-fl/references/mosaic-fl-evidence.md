# MOSAIC-FL Evidence Base

## Source
Largillier, Paygambar, Gouy-Pailler, Meyer, Mziou, Stan. "MOSAIC-FL, a micro-service based privacy-preserving framework with application to genomics." arXiv:2607.25107, July 2026. Université Paris-Saclay, CEA LIST, CNRGH.

## Key Architecture

Microservice-based: each federated node = cluster of Orchestrator + ML Engine + Crypto Provider containers. Strict separation of concerns. gRPC + Protobuf communication. Passive "pull" mode for ML and Crypto components.

## Threshold HE Details

- Based on Mouchet et al. (2023) RLWE-based ThHE with t-of-N Shamir access structure
- CKKS scheme: cyclotomic ring R_q = Z_q[X]/(X^n+1), n=power of 2
- Security: 128-bit, polynomial degree N=2^14, scaling factor Δ=2^45, PQ size 438 bits
- Setup: single communication round; public key reusable for unlimited aggregations
- Key renewed every round to counter de Verdière et al. (2026) key-recovery attack (collect partial decryption shares across decryptor sets → CRT → secret key recovery)
- Noise flooding counters Checri et al. (2024) CPA-D attacks

## Security Model

- Aggregation server: honest-but-curious (semi-adaptive adversary)
- At least t out of N clients: honest-but-curious
- Non-collusion between participants assumed
- Public authentication channels required for setup

## Genomic Application — BRCA Subtyping

- Dataset: TCGA breast cancer samples with PAM50 subtypes
- Model: Transformer optimized for BRCA subtyping (679k params)
- Embedding: 23×20531 dimension reduction → 23×92 channel expansion
- 4 vanilla transformer blocks with 4×23-sized heads
- Lasso + Ridge regularization (λ=10^-5), 20% dropout
- Adam optimizer, lr=0.0005, 50 local epochs, 10-epoch patience
- Non-IID setup via target standard deviation parameter
- Client allocation: target std dev for non-IID simulation

## Experimental Results

### Communication Overhead (CNN, EMNIST)
- Baseline FL: <0.1s latency, <2% of total 5.17s per round
- ThHE FL: up to 14.2s for N=16, t=9 (P2P crypto dominates)

### Accuracy (100 rounds, N=4, t=3)
- CNN: ThHE mode matches baseline exactly (lossless CKKS)
- Transformer: ThHE matches baseline (training dominates)

### Time Breakdown (N=4, t=3)
| Exp | Train | Eval | Enc | Dec | Agg | Sync | Total |
|---|---|---|---|---|---|---|---|
| CNN-Baseline | 4.40 | 0.99 | - | - | 0.002 | 5.17 | 10.56 |
| CNN-ThHE | 4.38 | 1.00 | 0.60 | 2.09 | 0.09 | 9.14 | 17.30 |
| Trans-Baseline | 70.96 | 0.34 | - | - | 0.004 | 87.87 | 159.17 |
| Trans-ThHE | 69.95 | 0.24 | 0.97 | 3.26 | 0.28 | 92.35 | 167.05 |

## Framework Comparison

MOSAIC-FL is the only framework with native threshold HE support, polyglot microservice architecture, and t-of-N fault-tolerant decryption. Flower, FATE, and PySyft all lack threshold HE and rely on retry-based fault tolerance.

## Future Work
- More aggregation rules (robust, fair)
- DP integration
- Verifiable Computing (VC) module
- Stronger adversary models