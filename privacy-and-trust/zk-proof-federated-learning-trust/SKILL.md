---
name: zk-proof-federated-learning-trust
description: Framework for verifiable, privacy-preserving federated learning using zero-knowledge proofs of training (zkPoTs), zero-trust architectures, and zk-SNARK attestation. Use when designing FL systems that need publicly verifiable training correctness without revealing data, when you need to prove model integrity to an external auditor, or when deploying FL in zero-trust or regulated environments. NOT for basic FL deployments where a trusted central server is acceptable, or for non-verifiable training pipelines.
---

# Zero-Knowledge Proofs of Federated Learning Training

## Overview
A new wave of cryptographic tools makes it possible to prove that federated learning training was executed correctly — from local dataset to final model — without revealing any training data or intermediate model states. These zero-knowledge proofs of training (zkPoTs), combined with zero-trust architectures and decentralized secure aggregation, enable federated learning in adversarial, zero-trust, and regulated environments where a trusted central server cannot be assumed.

## When to Use
- Designing FL systems for regulated industries (finance, healthcare) where training correctness must be auditable
- Deploying FL in zero-trust networks where no central server is trusted
- Requiring external auditors to verify the entire training process without accessing data
- Building verifiable ML pipelines where model provenance and integrity are first-class requirements
- Combining differential privacy with verifiable computation for defense-in-depth
- NOT for basic FL with a trusted aggregator and no audit requirement
- NOT for centralized training (zkPoTs are FL-specific)

## Core Process / Workflow

### 1. Architecture Selection

| Approach | Proof Type | Key Property | Complexity |
|---|---|---|---|
| **Falafel** (Bontekoe et al., 2026) | zkPoT via commit-and-prove | Publicly verifiable training correctness; modular building blocks | ~150s proof generation for LeNet; 70KB proof (10–15× smaller than prior work) |
| **ZT-FL-PE** (Pule et al., 2026) | Zero-trust + FL + DP | Defends against property inference & membership inference attacks | Low-moderate computational overhead; high model accuracy retained |
| **DeSA** (Wang et al., 2026) | Embedded zk-SNARK + dynamic masking | Byzantine-robust decentralized aggregation in D2D networks | Fast verification via embedded proofs; one-time masking eliminates aggregated masks |
| **FLiPD** (Chandran et al., 2026) | MPC + DP + HD filtering | Holistic defense against inference + poisoning attacks; collusion-resistant | Client comm. ≈ same as plaintext FL; 87% accuracy (HAR), 90% (MNIST) |

### 2. Falafel zkPoT Pattern (Modular Commit-and-Prove)
The Falafel scheme enables parties to create a zero-knowledge proof of training for federated learning:
1. **Trusted auditor** verifies input authenticity (dataset attestation)
2. **Local training proof:** each party proves its local training step was executed correctly without revealing data
3. **Centralized update proof:** the central server proves the weight aggregation was correct
4. **External verifier** can check the entire training process — from dataset to final model — without seeing any data
5. **Modular design:** core proof components can be swapped for other building blocks
6. **Relies on well-understood cryptographic assumptions** only (no Fiat-Shamir with arithmetic hash functions)
7. **Performance:** LeNet zkPoT: 70KB proof, ~150s generation, 10–15× smaller than prior work

### 3. Zero-Trust FL (ZT-FL-PE) Pattern
1. Apply ZTA "never trust, always verify" posture to FL
2. Continuous authentication of participating clients
3. Adaptive verification mechanisms constrain adversarial behavior
4. Privacy-preserving update transformations protect against:
   - **Property inference attacks (PIAs)** — infer statistical properties of training data
   - **Membership inference attacks (MIAs)** — infer whether a specific record was in training data
5. Eliminate centralized data aggregation → reduces attack surface

### 4. Decentralized Secure Aggregation (DeSA) Pattern
For device-to-device (D2D) FL with no central server:
1. **Enhanced zk-SNARK** verifies local model training process
2. **Embedded zero-knowledge proofs** ensure aggregation integrity while keeping proofs succinct and verification fast
3. **Byzantine-robust D2D aggregation** withstands malicious nodes disrupting aggregation
4. **One-time masking** eliminates aggregated masks through dynamic aggregation strategy considering adjacency and trust relationships in evolving network topologies

### 5. FLiPD: Holistic Defense (MPC + DP + Filtering)
1. **MPC-based secure aggregation** with two non-colluding servers
2. **Hamming Distance filtering** detects and eliminates poisoned/anomalous updates (oblivious to servers)
3. **Distributed DP noise** generation by both servers (Laplace mechanism) — secure even under collusion between one server and a subset of clients
4. **Seed-based secret sharing** keeps client-server communication ≈ same as plaintext FL
5. **Result:** 87% accuracy (HAR Linear Regression), 90% (MNIST CNN)

## References
- See [references/zk-fl-evidence-base.md](references/zk-fl-evidence-base.md) for detailed evidence on Falafel, ZT-FL-PE, DeSA, FLiPD, and PPCFL (privacy-preserving clustered FL).