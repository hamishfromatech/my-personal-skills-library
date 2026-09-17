---
name: veridp-verifiable-dp-sgd
description: Cryptographically enforce and prove correct execution of differentially private stochastic gradient descent (DP-SGD) using zero-knowledge proofs. Use when you need per-iteration verifiable DP training in adversarial or federated settings, when malicious participants might bypass noise addition, or when you need public auditability of privacy guarantees without revealing training data.
---

# VeriDP: Verifiable Differentially Private Training

## Overview

VeriDP is the first framework for verifiable differentially private training that cryptographically enforces and proves the correct execution of DP-SGD in zero knowledge. It combines Zero-Knowledge Proofs (ZKPs) with polynomial commitments, sumcheck and GKR-based proofs, and incrementally verifiable computation (IVC) to generate compact proofs of correct gradient computation, clipping, averaging, and Gaussian noise generation—without revealing private data or randomness. Unlike prior systems that verify only the final privacy budget, VeriDP enables per-iteration verifiability of each model update.

Source: Abdolmaleki, Asadi, Asadi, Köpsell, Mohee, Roustaeifar & Zarezadeh (University of Sheffield/Cambridge/Waterloo/Barkhausen Institut, PoPETs 2026(3)).

## When to Use

- Adversarial or federated settings where participants might bypass DP noise addition
- Regulatory compliance requiring cryptographic proof of privacy guarantees
- Public auditability of AI training privacy claims
- Federated learning where a verifier sees only model updates and cannot check DP compliance
- Long-running training where intermediate deviations could accumulate undetectably
- NOT for: settings where semi-honest participants can be fully trusted

## Core Innovation: Per-Iteration Verifiability

Prior work (Confidential-DPproof) certifies only a single monolithic training run. VeriDP enables continuous, per-iteration verification:
- Each SGD iteration produces a proof recursively merged into a constant-size certificate
- Proof size and verifier time remain constant regardless of training length
- Adversary cannot bias early rounds and compensate later while passing final checks

## The ZK-DPSGD Definition

A protocol Π = (Setup, Prove, Verify) is a Zero-Knowledge Proof of DP-SGD if:
1. **Completeness**: Honest execution → verifier always accepts
2. **Knowledge Soundness**: Accepting proof implies valid gradients, noise, and updates consistent with DP-SGD rules
3. **Zero-Knowledge**: Verifier learns nothing beyond correctness of DP-SGD execution
4. **Privacy Enforcement**: Every iteration satisfies clipping (norm C), Gaussian noise (σ²C²I), and the DP-SGD update rule

## Protocol Architecture

### Phase 1: Data Commitment
- Commit training dataset D via Merkle tree → commitment D
- D becomes public input to every iteration and part of recursive IVC state
- All training iterations cryptographically bound to a single immutable dataset

### Phase 2: Randomness Seed Generation
- Prevents seed grinding (adversary trying multiple seeds for favorable noise)
- Commit-then-derive structure: prover commits to random value before training
- Seed derived as s = r ⊕ H(Com(r) || context) where context includes training ID, iteration, batch index, D
- Context binding: each seed uniquely bound to specific training iteration
- Proof-coupled execution: noise generation, batch membership, and metadata all consistent with derived seed within same proof

### Phase 3: ZKP of DP-SGD Training
Per-iteration proof covering:
1. **Seed correctness**: knowledge of r such that s = r ⊕ H(Com(r) || context)
2. **Dataset membership**: Merkle proofs for all batch samples against committed D
3. **Gradient correctness**: per-example gradients computed correctly
4. **Clipping correctness**: ĝ = g · min(1, ||g||₂/C)
5. **Averaging correctness**: ḡ̂ = (1/m) Σ ĝᵢ
6. **Verifiable noise generation**: Gaussian noise via Box-Muller transform circuit (in-circuit)
7. **Weight update**: W = W_prev - (ḡ̂ + n)
8. **Recursive enforcement**: IVC proof chaining all iterations

### In-Circuit Gaussian Noise (Box-Muller)
- Two uniform values from seed via cryptographically secure PRNG
- Fixed-point representation with public precision parameter
- Box-Muller transform: r = √(-2 ln(u₁)), θ = 2πu₂
- n₁ = r cos(θ)·C·σ, n₂ = r sin(θ)·C·σ
- log and √ approximated via Chebyshev polynomials (degree 5 and 4)
- cos/sin via LUT with 256 entries or Chebyshev approximation

## Performance

| Model | Parameters | Prover Time | Verifier Time | Proof Size | Privacy Budget |
|-------|-----------|-------------|---------------|------------|----------------|
| MLP (MNIST) | 101,770 | 53.25s | 3.21ms | 3.93 KB | ε=2.92 |
| CNN (MNIST) | 421,642 | 217.51s | 2.38ms | 4.08 KB | ε=2.92 |
| ResNet18 (CIFAR-10) | 11,173,962 | 5,162.54s | 4.96ms | 4.08 KB | ε=2.92 |

- Prover time scales linearly with batch size (O(n) complexity)
- Verifier time and proof size effectively constant (2-5ms, 3-4 KB) regardless of model size or training length
- IVC enables constant-size proofs despite per-iteration verification

## Federated Learning Application

Each participating client:
1. Generates ZKP that its local DP-SGD update was computed correctly
2. Aggregator verifies proof BEFORE incorporating update into global aggregation
3. Only privacy-compliant updates contribute to global model
4. Verification occurs prior to model aggregation

Benefits:
- Round-level accountability: detect and localize deviations
- Stronger privacy assurance: every update satisfies DP-SGD
- Support for public and long-term auditing in regulated deployments

## Comparison with Confidential-DPproof

| Feature | Confidential-DPproof | VeriDP |
|---------|---------------------|--------|
| Proof structure | Monolithic (entire run) | Incremental (per-iteration) |
| Verification granularity | Final (ε, δ) only | Per-iteration |
| Proof size | Linear in iterations | Constant (via IVC) |
| Commitment | IT-MAC (private, not hiding) | Polynomial (public, hiding) |
| Randomness | Interactive auditor required | Non-interactive, context-bound |
| Auditability | Private auditor only | Public, long-term |

## A-Tech Alignment

- **Open-source**: implementation available at github.com/BarkhausenInstitut/VeriDP (C++, ~5000 LOC)
- **Data privacy**: zero-knowledge proofs reveal nothing about training data, gradients, noise, or model parameters
- **Financial freedom**: enables small organizations to provably demonstrate privacy compliance
- **Practical implementation**: working implementation with real models (MLP, CNN, ResNet18), real datasets (MNIST, CIFAR-10)