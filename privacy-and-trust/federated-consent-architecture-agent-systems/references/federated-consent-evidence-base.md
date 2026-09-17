# Federated Consent Architecture — Evidence Base

## Primary Sources

### IETF Internet-Draft: Privacy-Preserving Federated Learning for Agent Systems
- **Document**: draft-kale-agntcy-federated-privacy-02
- **Author**: Nik Kale (Cisco Systems)
- **Published**: July 2026
- **Status**: Informational (expires January 2027)
- **Scope**: Cross-tenant and cross-organization federated learning for agent systems
- **Key contribution**: Architecture separating agent communication from learning coordination; defines privacy/security requirements for the learning layer

### Federated Consent System for Training Data (describe.cloud, 2026)
- Canonical consent record per creator/content pair
- Signed, portable consent tokens (JWT or W3C Verifiable Credential)
- Event-driven propagation (webhooks, message buses, pub/sub)
- Privacy-preserving identifiers (content fingerprints, keyed hashes)
- Auditability via Merkle trees with selective disclosure via ZKPs
- Federated governance for dispute resolution, escrow, arbitration
- SLO targets from 2025-2026 production pilot: median propagation 12s, 99th percentile 90s, 100% revocation compliance within 5 minutes for checkpointed jobs
- API spec: POST /consents, GET /consents/{id}, POST /consents/{id}/revoke, GET /consents/verify, POST /subscriptions
- Three propagation patterns: PULL-first, PUSH-first, Hybrid (recommended)

### Federated Learning Systematic Reviews (2026)

#### Sum et al. (2026) — "A systematic review on privacy preservation in federated learning" (Int. J. Inf. Security 25:65)
- 153 papers from 392 initial (90%+ Scopus Q1)
- Privacy mechanisms: differential privacy, secure multi-party computation, homomorphic encryption, secure aggregation, SecureBoost, secure shuffling, split learning, zero-knowledge proofs
- Applications: healthcare, government, education, finance, IoT, retail/e-commerce, transportation, telecommunications
- Key finding: shared model updates can leak sensitive client information despite data localization; FL introduces distinct attack surface not fully mitigated by standard mechanisms

#### Baduwal et al. (2026) — "Federated Learning: A Survey of Core Challenges, Current Methods, and Opportunities" (Computers 15(3):155)
- Six core challenges: heterogeneity, computation overhead, communication bottlenecks, client selection, aggregation/optimization, privacy preservation
- Challenge-centric taxonomy with cross-layer interactions
- Co-design formulation: min E[L(wT)] s.t. B_total ≤ B_max, C_total ≤ C_max, τ_round ≤ τ_max, ε ≤ ε_max
- Open-source frameworks: Flower, TensorFlow Federated, FedML, PySyft, FATE
- Federated foundation models: FedLLM-Bench, PEFT/LoRA for LLM fine-tuning, FedMoE (60% communication reduction, 98.2% ROUGE-L retention)

#### Waref et al. (2026) — "Unfederated: Open Challenges, Deployment Gaps, and Emerging Directions in Federated Learning" (Arch. Computat. Methods Eng.)
- Introduces "U-score" (unfederated score): number of federation properties violated
- Four properties: Data Locality, Decentralized Participation, Update Privacy, Incentive Alignment
- U=0 = fully federated; U=4 = complete violation
- Real-world assessment: NVIDIA Clara U=2-3, Owkin MELLODDY U=1, Gboard U=2, no production deployment achieves U=0
- Three-level threat taxonomy: TM-1 (honest-but-curious server), TM-2 (colluding clients), TM-3 (active Byzantine)
- Key insight: structural failures (not algorithmic) explain why FL hasn't scaled
- Incentive alignment: four mechanism classes — game-theoretic (Stackelberg), Shapley value, blockchain/tokenomics, reputation systems

### Privacy-Preserving ML Deep Dive (youngju.dev, 2026)
- Five PPML stacks: FL, DP, HE, MPC, TEE
- Frameworks: Flower 1.13 (5,500 GitHub stars, framework-agnostic), NVIDIA FLARE 2.5 (enterprise, healthcare/finance), TFF (research/simulation), PySyft 0.9 (structured transparency)
- DP libraries: Opacus 1.5 (Meta, PyTorch DP-SGD), OpenDP 0.10 (Harvard, Rust-backed), Google DP Library, JAX-Privacy (DeepMind)
- FHE libraries: Microsoft SEAL 4.1, OpenFHE 1.2, Zama Concrete/Concrete-ML (FHE compiler), TFHE-rs
- MPC frameworks: CrypTen (Meta), MP-SPDZ (Bristol), EzPC (Microsoft)
- TEE: Intel SGX (vulnerability crisis post-2018), AMD SEV-SNP, Apple Private Cloud Compute (2024), AWS Nitro Enclaves, NVIDIA H100/H200 confidential compute
- Real deployments: Google Gboard, Apple Siri + PCC, BraTS Federated (71 institutions, 6% accuracy improvement), MELLODDY (10 pharma companies), Korean banking (Shinhan/Kookmin/Woori via KFTC/NICE), Japanese banking (NTT/MUFG/Mizuho PoCs)
- Market projection: combined PPML/Confidential Computing ~$10B by 2026, CAGR >50%

## Key Technical Details

### LoRA in Federated Settings
- Full fine-tuning of Llama-3-7B: ~28 GB VRAM at bf16 (exceeds mobile devices)
- Single gradient update for 7B: ~28 GB (vs. ~200 MB for ResNet-50)
- LoRA reduces trainable parameters to <0.1% of model
- FedMoE (Feng et al. 2025): client-specific LoRA adapters (rank r∈{4,8}); on Llama-2-7B with 200 clients: 60% communication reduction, 98.2% ROUGE-L retention
- FFA-LoRA / FLoRA: aggregation methods correct for adapter structure (freeze one factor, or aggregate reconstructed update)
- Heterogeneous adapter ranks: element-by-element averaging undefined; need rank-aware aggregation

### Privacy Accounting Worked Example
- Privacy unit: tenant; 25 from 250 (sampling 0.1); 100 rounds; L2 clip 1.0; delta 1e-6
- sigma=1.1 → ε≈7.5; sigma=1.5 → ε≈4.4; sigma=2.0 → ε≈2.9
- Per-round intuition (sigma=1.1 common in literature for one-shot) exceeds budget of ε=3.0 more than twofold when composed over 100 rounds
- Composed accounting is mandatory; per-round intuition is insufficient

### Threat Model Mismatch
- Applying Krum (TM-3 defense) in TM-1 setting: up to 15% accuracy reduction for no privacy benefit
- Secure aggregation provides no guarantee under TM-2 (colluding clients can compute g_k = Σg_k - Σ_{j≠k} g_j to isolate individual updates)
- Homomorphic encryption: numerical approximation errors (~2^-40 for CKKS at 128-bit security) accumulate over iterations

### Coordinator Compromise Attack
- Compromised coordinator selects minimum-size cohort where every participant except target is controlled by attacker
- Attacker's contributions known → target's contribution recoverable by subtraction
- Repeating across rounds defeats secure aggregation without breaking cryptography
- Mitigations: verifiable cohort selection (committed random seed), participant-set commitments, Sybil controls, distributed noise

### Deletion and Unlearning
- Retention policies do NOT remove influence from trained models
- Four approaches: (1) retrain from checkpoint (only full removal, costly), (2) approximate unlearning (FedEraser, reduces not eliminates), (3) DP as bound (formal but legal sufficiency unclear), (4) contractual handling
- Approach MUST be recorded in task configuration and disclosed before enrollment

## Regulatory Context

| Regulation | Region | PPML-Relevant Articles |
|------------|--------|----------------------|
| GDPR | EU | Art 25 (by design), Recital 26 (anonymisation) |
| HIPAA | US | Safe Harbor, Expert Determination |
| EU AI Act | EU | Art 10 (data governance), high-risk AI |
| CCPA/CPRA | California | Sensitive Personal Information |
| PIPA | Korea | Pseudonymised information, combination institutions |
| APPI | Japan | Anonymised / pseudonymised processed information |
| PIPL | China | Separate consent, cross-border security assessment |

## Federated Learning Market Data (2026)
- Only 5.2% of FL research reaches production deployment
- Market projection: $0.1B (2025) → $1.6B (2035)
- Cross-silo FL: 63.7% enterprise market share in 2026
- Communication cost: can be 10x-100x more than standard methods at scale
- Accuracy gap: FL typically 1-5pp below centralized learning
- Communication reduction potential: up to 94.89% while maintaining comparable performance