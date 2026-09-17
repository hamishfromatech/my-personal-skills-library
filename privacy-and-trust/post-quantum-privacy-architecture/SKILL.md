---
name: post-quantum-privacy-architecture
description: Future-proof privacy-first AI systems against quantum threats using NIST-standardized post-quantum cryptography, crypto-agile architectures, and hybrid key exchange. Use when designing long-term privacy infrastructure, enterprise compliance roadmaps, or federated learning systems that must survive the quantum transition.
---

# Post-Quantum Privacy Architecture

## Overview

NIST finalized its first post-quantum cryptography (PQC) standards in 2024–2025: ML-KEM (FIPS 203) for key encapsulation, ML-DSA (FIPS 204) for digital signatures, and SLH-DSA (FIPS 205) for stateless hash-based signatures. By 2026, the U.S. National Security Memorandum NSM-10 mandates federal agencies begin PQC migration, and the private sector is following. For privacy-first AI systems, this is not a distant threat — it is an immediate architectural requirement.

Quantum computers capable of breaking RSA-2048 and ECC P-256 may arrive within 10–15 years. But "harvest now, decrypt later" attacks are happening today: adversaries collect encrypted data now to decrypt once quantum computers become available. Any AI system processing sensitive data with classical cryptography is already vulnerable to future exposure.

This skill provides a practical framework for building crypto-agile AI privacy architectures that survive the quantum transition.

## When to Use

- Designing federated learning systems with multi-year data lifetimes
- Building enterprise AI products for regulated industries (healthcare, finance, government)
- Evaluating current cryptography stacks for quantum vulnerability
- Planning migration roadmaps from classical to post-quantum algorithms
- Communicating quantum risk to non-technical stakeholders

## The Quantum Threat Model

### What Quantum Computers Break
- **RSA** (all key sizes): Shor's algorithm solves integer factorization in polynomial time
- **ECC** (all curves): Shor's algorithm solves discrete logarithm problems
- **Finite field DH/DSA**: Broken by Shor's algorithm
- **Current TLS 1.3 handshakes**: Hybrid ECDH + RSA authentication is fully vulnerable

### What Quantum Computers Do NOT Break (Yet)
- **Symmetric encryption** (AES-256): Grover's algorithm reduces effective key length by half; AES-256 becomes AES-128-equivalent — still secure with adequate key sizes
- **Hash functions** (SHA-256, SHA-3): Grover's algorithm affects collision resistance but does not break preimage resistance for properly sized hashes
- **Lattice-based cryptography**: ML-KEM and ML-DSA are designed to resist both classical and quantum attacks

### The "Harvest Now, Decrypt Later" Risk
The most urgent threat is not future quantum computers — it is today's adversaries storing encrypted traffic for later decryption.

**High-risk data types:**
- Medical records with 50+ year retention requirements
- Financial transaction logs with 7+ year regulatory retention
- Biometric templates used for lifetime identity verification
- Government classified communications with 25+ year classification periods
- AI model weights trained on proprietary datasets

**Rule:** If data must remain confidential for more than 5 years, it should be encrypted with post-quantum algorithms now.

## The NIST Post-Quantum Standards (2024–2025)

| Standard | Algorithm Family | Purpose | Key Size (public) | Security Level |
|----------|-----------------|---------|-------------------|----------------|
| **FIPS 203 (ML-KEM)** | Lattice-based (Kyber) | Key encapsulation / key exchange | 1,568 bytes (ML-KEM-768) | NIST Level 3 |
| **FIPS 204 (ML-DSA)** | Lattice-based (Dilithium) | Digital signatures | 1,952 bytes (ML-DSA-65) | NIST Level 3 |
| **FIPS 205 (SLH-DSA)** | Hash-based (SPHINCS+) | Stateless digital signatures | 8–49 KB signatures | NIST Level 1/3/5 |
| **FIPS 206 (FN-DSA)** | Lattice-based (FALCON) | Digital signatures (smaller sigs) | 897 bytes | NIST Level 5 |

**Practical notes:**
- ML-KEM and ML-DSA are the primary standards for most use cases
- SLH-DSA has very large signatures (8–49 KB) — suitable for high-security, low-bandwidth contexts
- FN-DSA (FALCON) offers smaller signatures but more complex implementation; still in draft as of early 2026
- All three are patent-free and open-source implementations exist (liboqs, BoringSSL, OpenSSL 3.2+)

## The Crypto-Agile Architecture

Crypto-agility is the ability to switch cryptographic algorithms without redesigning the entire system. This is the foundational principle for quantum-safe AI privacy architecture.

### Layer 1: Algorithm Abstraction
Never hard-code algorithm identifiers. Use a negotiation layer that can upgrade algorithms as standards evolve.

```
Current: TLS 1.3 + X25519 Kyber768 hybrid
Future:  TLS 1.3 + ML-KEM-1024
Future+: TLS 2.0 + [next NIST standard]
```

**Implementation:**
- Use protocol libraries that support algorithm negotiation (OpenSSL 3.2+, BoringSSL with PQC patches)
- Store algorithm identifiers alongside ciphertext, not in application logic
- Build automated testing for algorithm fallback and negotiation paths

### Layer 2: Hybrid Key Exchange
Combine classical and post-quantum algorithms so the system remains secure even if one is broken.

**Pattern:**
- Key exchange: ECDH (X25519) + ML-KEM-768 hybrid
- Authentication: ECDSA (P-256) + ML-DSA-65 hybrid
- Rationale: If ML-KEM is broken, ECDH still protects. If ECDH is broken by quantum computers, ML-KEM still protects.

**A-Tech application:**
- A-Coder cloud sync: Hybrid key exchange for all data in transit
- Builder's Club federation: Hybrid signatures for model update attestations
- Be Practical distribution: Hybrid signing for playbook authenticity verification

### Layer 3: Key Rotation and Forward Secrecy
Post-quantum algorithms do not eliminate the need for forward secrecy. Design key rotation into the architecture from day one.

**Requirements:**
- Session keys rotated every 24 hours maximum
- Long-term identity keys rotated annually
- Compromised key recovery path with zero data re-encryption time
- Automated key rotation without user intervention

### Layer 4: Migration Roadmap
Every system using classical cryptography needs a documented migration path.

**Phase 1: Inventory (Month 1)**
- Catalog all cryptographic dependencies: libraries, protocols, key sizes, certificate authorities
- Classify data by confidentiality lifetime: <1 year, 1–5 years, 5–20 years, >20 years
- Identify "cryptographic single points of failure"

**Phase 2: Hybrid Deployment (Months 2–6)**
- Enable hybrid key exchange on all TLS connections
- Add ML-DSA signatures alongside ECDSA for code signing and attestation
- Test performance impact: ML-KEM is often faster than ECDH; ML-DSA signing is slower but verification is fast

**Phase 3: Post-Quantum Default (Months 6–18)**
- Switch default key exchange to ML-KEM-only for new connections
- Maintain hybrid mode for backward compatibility during transition
- Deprecate RSA for all new deployments

**Phase 4: Quantum-Safe Mode (Year 2+)**
- Remove classical algorithms for high-security contexts
- Maintain hybrid mode for general-purpose contexts until quantum threat is realized
- Continuous monitoring for cryptanalytic advances against lattice-based schemes

## A-Tech Applications

### A-Coder (IDE)
- **Local-first, quantum-safe:** All local data encrypted at rest with AES-256-GCM (quantum-resistant symmetric encryption)
- **Cloud sync hybrid:** TLS with X25519Kyber768 hybrid key exchange for all sync operations
- **Plugin signature verification:** ML-DSA signatures for plugin authenticity; fallback to ECDSA during transition
- **Enterprise key escrow:** Post-quantum key escrow for recovery without exposing plaintext to cloud providers

### Be Practical (Playbooks)
- **"The Quantum Privacy Playbook"** — guide to PQC migration for solo founders and small teams
- **Chapter:** "Why your AI data is already at risk (and how to fix it)"
- **Template:** Crypto-agility assessment checklist for any AI product

### Builder's Club
- **Privacy-First Certified 2.0:** Updated badge requiring post-quantum readiness
- **Open-source PQC tools:** liboqs wrappers, hybrid TLS configurations, key rotation scripts
- **Federated learning security:** Post-quantum secure aggregation for federated model updates

## Federated Learning + Post-Quantum

Federated learning is uniquely exposed to quantum threats because model updates are aggregated across many parties over long time periods.

**Threats:**
- **Eavesdropping on model updates:** Adversary collects encrypted updates, decrypts later with quantum computer
- **Participant deanonymization:** Quantum attacks on aggregation protocols reveal which parties contributed
- **Model inversion:** Quantum-enhanced attacks reconstruct training data from aggregated updates

**Defenses:**
- **Post-quantum secure aggregation:** Replace classical homomorphic encryption with lattice-based schemes (BFV/CKKS with PQC parameters)
- **Hybrid differential privacy:** Combine classical DP noise with post-quantum encryption of noise parameters
- **Short-lived session keys:** Rotate federation session keys every update round, not annually

## Measurement Framework

| Metric | Target | Measurement |
|--------|--------|-------------|
| Crypto-agility coverage | 100% of cryptographic dependencies | Dependency inventory |
| Hybrid deployment rate | 100% of external-facing TLS | SSL Labs scan + internal audit |
| Key rotation compliance | 100% of keys rotated on schedule | Key management system logs |
| Migration blockers resolved | 0 critical blockers | Risk register review |
| PQ algorithm performance | < 20% latency increase | Benchmark suite |
| Community PQC tooling | 5+ open-source tools published | GitHub repository tracking |

## A-Tech Values Alignment

| Value | How Post-Quantum Architecture Serves It |
|-------|----------------------------------------|
| **Open-Source AI** | All PQC implementations are open-source and patent-free; A-Tech contributes crypto-agility tooling to the community |
| **Data Privacy** | Post-quantum encryption ensures data remains private for decades, not just years |
| **Financial Freedom** | Early PQC adoption becomes competitive differentiator in regulated markets; enables premium enterprise pricing |
| **Practical Implementation** | Migration is phased, measurable, and compatible with existing infrastructure |

## Cross-References
- `privacy-and-trust/privacy-first-competitive-differentiator` — Trust reserve framework and privacy positioning
- `privacy-and-trust/federated-learning-for-privacy-preserving-ai` — Federated learning architecture
- `privacy-and-trust/differential-privacy-synthetic-data` — Mathematical privacy guarantees
- `ai-agents-and-workflows/agent-reputation-identity-framework` — DID and verifiable credentials with post-quantum signatures

## Sources
- NIST — FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA) standards (2024–2025)
- NIST — Post-Quantum Cryptography Standardization Project (nist.gov/pqcrypto)
- NSA — Commercial National Security Algorithm Suite 2.0 (CNSA 2.0) guidance (2022–2025)
- ENISA — "Post-Quantum Cryptography: Current State and Quantum Mitigation" (2025)
- Open Quantum Safe (liboqs) — Open-source post-quantum cryptography library (openquantumsafe.org)
- Cloudflare — Post-quantum TLS deployment experience (blog.cloudflare.com, 2024–2025)
- Google — "Protecting Chrome Traffic with Hybrid Kyber" (blog.google, 2024)
