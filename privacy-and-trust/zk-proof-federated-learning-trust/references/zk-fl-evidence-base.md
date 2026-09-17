# Zero-Knowledge Proofs of Federated Learning Training — Evidence Base

## Falafel: Modular zkPoT (Bontekoe et al., 2026, ePrint 2026/1335)
- **Authors:** Bontekoe, Bootsma, Dunning, Sijpesteijn, Attema (RUG/TNO)
- **Core innovation:** Modular commit-and-prove scheme for zero-knowledge proofs of training (zkPoT) in federated learning
- **Guarantees:** Active security during the federated training process + publicly verifiable correctness of the final trained model
- **Privacy:** No additional information revealed about local datasets or intermediate local model states
- **Scope:** FL of (deep) neural networks with a centralized server for weight updates
- **Attestation:** Proves local training steps, centralized weight update, and input authenticity (via trusted auditor)
- **External verifier** can check the entire training process from dataset to final model
- **Cryptographic assumptions:** Well-understood only; no Fiat-Shamir with arithmetic hash functions
- **Performance:** LeNet zkPoT: 70KB proof, ~150s generation; 10–15× smaller proof than prior work
- **Parallelizable and modular:** core proof components swappable

## ZT-FL-PE: Zero-Trust Driven FL (Pule, Nleya, Sibiya, 2026, Appl. Sci. 16(8):3872)
- Integrates Zero Trust Architecture "never trust, always verify" with FL
- Eliminates centralized data aggregation; reduces attack surface
- Targets property inference attacks (PIAs) and membership inference attacks (MIAs)
- Adaptive verification mechanisms + privacy-preserving update transformations
- Continuous authentication constrains adversarial behavior
- Result: substantial privacy protection, high model accuracy, low-moderate computational overhead

## DeSA: Decentralized Secure Aggregation (Wang et al., 2026, IEEE TIFS)
- Byzantine-robust decentralized secure aggregation for D2D FL in zero-trust networks
- Enhanced zk-SNARK proof system verifies local model training
- Framework embeds multiple zero-knowledge proofs for aggregation integrity
- Succinct proofs, fast verification (embedded proof verification significantly faster than multiple individual proofs)
- Byzantine-robust D2D aggregation withstands malicious nodes
- One-time masking eliminates aggregated masks via dynamic aggregation strategy
- Dynamic strategy considers adjacency and trust relationships in evolving network topologies
- Accuracy robust against malicious nodes on real-world datasets

## FLiPD: MPC + DP + Filtering (Chandran, Önen, Schneider, 2026, ePrint 2026/324)
- Holistic FL defense: simultaneously defends against inference AND poisoning attacks
- MPC-based secure aggregation with two non-colluding servers (ABY2.0)
- Hamming Distance filtering (oblivious to servers) eliminates anomalous/poisoned updates
- Distributed Differential Privacy noise generation by both servers (Laplace mechanism)
- Collusion-resistant: secure even when majority of clients collude with one server (requires ≥1 honest server + ≥1 honest client)
- Client-server communication ≈ same as unprotected plaintext FL (seed-based secret sharing)
- Server-server communication 11% lower than Prio+ (state-of-the-art)
- Accuracy: 87% (HAR, Linear Regression), 90% (MNIST, CNN)
- Handles client dropouts at all stages

## PPCFL: Privacy-Preserving Clustered FL (Zhan, Jiang, Liu, 2026, Sci. Rep.)
- Split-stream framework: adaptive Gaussian perturbation + threshold Paillier encrypted aggregation
- Backbone updates: adaptive Gaussian perturbation before plaintext aggregation
- Clustering signatures and cluster-head updates: stream-specific adaptive Gaussian + threshold Paillier encryption
- Server performs ciphertext-domain aggregation; threshold-decryption by qualified client subset (server has no decryption capability)
- Round-wise budget growth, utility-aware refinement, adaptive clipping-threshold updates
- Improves final accuracy over DP-FedAvg by 0.33–2.62 pp and over IFCA by 0.98–10.24 pp across MNIST, Fashion-MNIST, CIFAR-10

## MIT FTTE: Federated Tiny Training Engine (Tenison et al., 2026, MIT News)
- Enables privacy-preserving AI training on resource-constrained edge devices (sensors, smartwatches)
- Three innovations: (1) subset of model parameters broadcast (memory reduction), (2) asynchronous server updates, (3) time-weighted updates
- 81% faster training than standard FL; 80% memory overhead reduction; 69% communication payload reduction
- Brings FL benefits to developing countries with less powerful devices

## Implications for A-Tech

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | zkPoTs make open-weight model training auditable — anyone can verify training was correct without seeing data |
| **Data privacy** | zkPoTs + DP + zero-trust = strongest possible privacy posture; auditable without data exposure |
| **Financial freedom** | Verifiable FL enables privacy-preserving AI in regulated finance — banks can collaborate on models without sharing data or trusting a central server |
| **Practical implementation** | Falafel proofs are 70KB and generate in ~150s — practical for real deployments; FLiPD keeps client communication ≈ plaintext |