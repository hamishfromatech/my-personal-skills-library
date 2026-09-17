---
name: mosaic-fl-microservice-privacy-preserving-fl
description: Modular, microservice-based federated learning framework with threshold homomorphic encryption (ThHE-CKKS), finite state machine orchestration, and fault-tolerant secure aggregation for sensitive domains like genomics. Use when designing privacy-preserving FL deployments for healthcare/genomics, needing language-agnostic polyglot FL architecture, requiring fault-tolerant threshold decryption (t-of-N), or evaluating microservice vs monolithic FL framework trade-offs.
---

# MOSAIC-FL: Microservice Privacy-Preserving FL

## Overview
MOSAIC-FL (Modular and Secure Federated Learning via Microservice Orchestration) is a privacy-preserving FL framework using a microservice architecture with threshold fully homomorphic encryption (ThHE-CKKS), enabling blind model aggregation by an orchestration server that requires only t-out-of-N active clients for decryption. Validated on EMNIST image classification and TCGA breast cancer genomic subtyping.

## When to Use
- Deploying privacy-preserving FL in healthcare, genomics, or other sensitive domains
- Needing language-agnostic, polyglot FL architecture (Python + Rust + C++ components)
- Requiring fault-tolerant secure aggregation where client dropouts are expected
- Evaluating microservice vs monolithic FL framework architecture
- Building modular FL systems where cryptographic primitives must be swappable
- Integrating genomic classification (cancer subtyping) with privacy guarantees

## NOT For
- Single-organization FL without privacy requirements
- Environments where gRPC/Protobuf overhead is unacceptable
- Scenarios requiring differential privacy (not included in v1)

## Core Process / Workflow

### 1. Microservice Architecture — Three Core Entities per Node

Each federated node is a cluster of isolated containers:

| Component | Role | Mode |
|---|---|---|
| **Orchestrator** | Central intelligence, sole gateway, proxy, FSM synchronization | Active (manages communication) |
| **ML Engine** | Tensor operations, model training | Passive pull (never initiates requests) |
| **Crypto Provider** | Key generation, homomorphic operations | Passive pull (only executes when prompted) |

Design principle: strict separation of concerns (SoC). No direct communication between sub-services. Internal attack surface minimized via container-level isolation.

### 2. Communication — gRPC + Protocol Buffers

- Reduces serialization overhead vs REST/JSON
- Enforces type safety via `.proto` service contracts
- Native binary payload support for large model weights and ciphertexts
- Enables polyglot implementation (Rust for crypto, Python for ML)

### 3. Threshold CKKS Homomorphic Encryption

Based on Mouchet et al. (2023) RLWE-based construction:
- **Setup**: Parties jointly generate collective public key via DistKeyGen
- **ShareReshare**: Shamir secret sharing over ciphertext ring → t-of-N access structure
- **PartDec**: Each party computes partial decryption share
- **CombDec**: Any t shares recover the plaintext via Lagrange interpolation

Key properties:
- Single communication round during setup
- Public key reusable for unlimited aggregations
- IND-CPA-D security with noise flooding against known attacks
- Key material renewed every round (counter to de Verdière et al. 2026 key-recovery attack)

### 4. Secure Training Round Protocol

1. **Setup phase**: All clients jointly execute Setup + DistKeyGen + ShareReshare
2. **Round start**: Server selects K participants; others await
3. **Local training**: Each client trains locally, encrypts weights with collective public key
4. **Aggregation**: Server computes FedAvg homomorphically: W = Σ (n_k/n) · Enc(w_k)
5. **Broadcast**: Encrypted global model W sent to all participants
6. **Threshold decryption**: Each node computes PartDec share; exchange shares; any t nodes recover global model via CombDec
7. **Pipeline**: Server opens next round registration while decryption proceeds

### 5. Fault Tolerance

- Requires only t-out-of-N clients online for decryption (not all N)
- Decrypting subset T can be static (pre-configured) or dynamic (first t ready)
- Lost clients don't block the round — threshold property handles dropouts

### 6. Performance Profile

| Model | Baseline (s/round) | ThHE (s/round) | Overhead | Notes |
|---|---|---|---|---|
| CNN (EMNIST) | 10.56 | 17.30 | 64% | Crypto overhead significant for small models |
| Transformer (BRCA) | 159.17 | 167.05 | 5% | Training dominates; crypto negligible |

For complex/long training tasks, cryptographic overhead becomes a minor fraction. The framework is "lossless" — CKKS noise managed at Δ=2^45, accuracy matches baseline.

### 7. Comparison with Popular FL Frameworks

| Feature | MOSAIC-FL | Flower | FATE | PySyft |
|---|---|---|---|---|
| Threshold HE | ✓ (CKKS/BGV/BFV) | ✗ | ✗ | ✗ |
| Extensibility | Very High (microservices) | High | Medium | High |
| Languages | Polyglot | Python/Swift/Kotlin/TS | Python/Java/C++ | Python |
| Fault tolerance | t-of-N decryption | Retry-based | Retry-based | Retry-based |

## References
- See [references/mosaic-fl-evidence.md](references/mosaic-fl-evidence.md) for cryptographic details, experimental results, and genomic application specifics.