---
name: google-parfait-open-privacy-ai-stack
description: Applies the Google Parfait open-source privacy-preserving AI technology stack for federated learning, analytics, and differential privacy. Use when building privacy-first AI systems, implementing federated learning pipelines, or designing on-device personalization with formal privacy guarantees.
---

# Google Parfait: Open-Source Privacy-Preserving AI Stack

## Overview

**Parfait** — "Private Aggregation & Retrieval, Federated, Analytics, Inference, & Training" — is the open-source privacy-preserving AI technology stack maintained by Google Research and published under the `google-parfait` GitHub organization (launched January 2025). It is the most complete, production-validated open-source ecosystem for federated learning (FL), federated analytics (FA), and differential privacy (DP), comprising seven repositories, 100+ contributors, and an Apache 2.0 license. Parfait is the open-source spine behind Google's production privacy-preserving deployments: Gboard next-word prediction (30+ language models across 7+ languages and 15+ countries, including the first production DP model with ε < 1), Android Private Compute Core, and Google Maps.

For A-Tech, Parfait is the canonical reference for building privacy-first AI systems that are open-source, formally private, and production-proven. It collapses the gap between research-grade privacy technology and deployable engineering: every component is public, every algorithm has formal guarantees, and every claim is backed by planetary-scale production evidence.

---

## When to Use This Skill

- Building federated learning systems that need formal differential privacy guarantees
- Implementing differential privacy in production ML training or fine-tuning pipelines
- Designing on-device personalization with the Android On-Device Personalization (ODP) APIs
- Creating privacy-preserving analytics pipelines (federated analytics, heavy hitters, histograms)
- Evaluating open-source privacy stacks against proprietary alternatives
- Building verifiable, TEE-based federated computation workflows
- Architecting scalable group-structured dataset pipelines for FL training
- Implementing secure aggregation or consensus for distributed privacy systems

---

## The Four Privacy Pillars

Parfait is organized around four pillars that together define a complete privacy-preserving AI lifecycle:

### 1. Transparency
All code, algorithms, configurations, and parameters are open-source and publicly auditable. Privacy guarantees are not asserted in marketing — they are expressed in code, formally proven, and reviewable by anyone. This is the foundational differentiator from proprietary privacy stacks.

### 2. Data Minimization
Raw user data never leaves the device. Parfait achieves this through:
- **Federated Learning (FL):** On-device model training; only aggregated model updates are transmitted.
- **Federated Analytics (FA):** On-device computation of aggregate statistics (counts, histograms, heavy hitters) without collecting raw records.
- **Secure Aggregation (SecAgg):** Cryptographic protocols ensuring the server only sees the sum of participant updates, never any individual contribution.

### 3. Data Anonymization
Even after data minimization, aggregated outputs can leak individual contributions. Parfait applies differential privacy (DP) to provide formal, mathematical guarantees:
- **DP for training:** Noise injected into model updates during FL training (DP-FTRL, DP-SGD).
- **DP for fine-tuning:** Privacy-preserving personalization and adaptation.
- **DP for heavy hitters:** Privately discovering the most frequent items (e.g., trending emojis, phrases) across a population.
- **DP for histograms:** Private aggregate distributions over user data.

### 4. External Verifiability
Privacy guarantees must be verifiable by third parties, not just trusted on faith. Parfait provides this through Trusted Execution Environment (TEE) workflows:
- Confidential federated compute components run inside hardware enclaves.
- Enclave computations are attested and sealed, producing auditable evidence.
- External auditors can verify that DP was correctly applied without accessing raw data.

---

## The Seven Open-Source Repositories

All repositories live under the `google-parfait` GitHub organization and are Apache 2.0 licensed.

### 1. `federated-language`
The core language for expressing federated algorithms. Platform-independent — it defines federated computations abstractly, with backends for both JAX and TensorFlow. This is the foundational abstraction layer that makes Parfait portable across ML frameworks.

### 2. `tensorflow-federated`
High-level interfaces for federated learning and federated analytics, built on TensorFlow and JAX. Provides the `tff.simulation` toolkit for local experimentation, `tff.learning` for FL training APIs, and `tff.analytics` for private analytics. This is the most widely used entry point for developers.

### 3. `federated-compute`
The cross-device execution system. Includes the Federated Compute Server (production-grade orchestration), Android client libraries for on-device participation, and a reference demo (the "federated learning of emojis" example). This is what runs in production at Google scale.

### 4. `confidential-federated-compute`
TEE-based verifiable components. Extends federated-compute with confidential computing primitives so that aggregation and DP noise injection execute inside attested enclaves. This is the implementation of Pillar 4 (External Verifiability).

### 5. `trusted-computations-platform`
Secure enclave platform for stateful computations. Provides the runtime for executing multi-step, stateful computations inside TEEs with sealing, attestation, and remote verification. Underpins confidential-federated-compute and broader confidential workloads.

### 6. `raft-rs`
A Rust implementation of the Raft consensus algorithm. Used within the trusted-computations-platform and federated-compute infrastructure to provide fault-tolerant, replicated state for orchestration services. Enables reliable, crash-recoverable coordination across the distributed compute fabric.

### 7. `dataset_grouper`
Scalable group-structured dataset pipeline tooling. Produces federated datasets partitioned by user/group (rather than IID shuffles), which is essential for realistic FL experimentation and for evaluating DP guarantees under non-IID data distributions — the real-world condition for on-device personalization.

---

## Production Deployments

Parfait is not a research artifact. Its components power production systems serving hundreds of millions of users:

### Gboard (Next-Word Prediction)
- **30+ language models** trained with FL + DP
- **7+ languages, 15+ countries**
- **ε values range from 0.994 to 13.69** (δ = 10⁻¹⁰)
- **First production DP model with ε < 1** — the Portuguese (Brazil) and Spanish (Latin America) models, achieved via Matrix Factorization DP-FTRL (MF-DP-FTRL)
- **12,000+ devices per training round** for the strongest DP models
- Algorithms: DP-FTRL and the newer MF-DP-FTRL variant

### Android Private Compute Core
- An isolated execution environment on Android for privacy-sensitive ML features
- Parfait's federated-compute Android client libraries run inside this core
- Provides the on-device substrate for federated learning and analytics at Android scale

### Google Maps
- Uses Parfait's federated analytics and DP infrastructure for privacy-preserving aggregate insights (e.g., popular times, route improvements) derived from user location data without collecting raw location traces

---

## Gboard DP-FTRL & MF-DP-FTRL

The Gboard deployment uses **DP-FTRL** (Differentially Private Follow-The-Regularized-Leader), a streaming optimization algorithm well-suited to the non-IID, sequential nature of federated rounds. The newer **Matrix Factorization DP-FTRL (MF-DP-FTRL)** variant achieves tighter privacy-utility trade-offs by factorizing the update sequence, enabling **ε ≤ 1** for the Portuguese (Brazil) and Spanish (Latin America) language models — a milestone for production DP.

See the evidence base for the full per-language-locale ε guarantee table and algorithm details.

---

## Android On-Device Personalization (ODP)

ODP is Google's Privacy Sandbox initiative for on-device, privacy-preserving personalization. It is the application-layer embodiment of Parfait's principles on Android.

### Paired-Process Architecture
- **ManagingProcess:** Handles lifecycle, networking, and non-sensitive orchestration. Cannot access model data directly.
- **IsolatedProcess:** Sandboxed process that holds the model, executes inference, and performs on-device personalization. Communicates with the ManagingProcess via a narrow, policy-enforced IPC boundary.

### Policy Engine
- Enforces rules on **data ingress** (what data may enter the IsolatedProcess) and **data egress** (what may leave).
- Ensures that only permitted, privacy-safe operations occur, blocking exfiltration of sensitive inputs.

### TEE-Based Aggregation
- Federated aggregation and DP noise injection execute inside a Trusted Execution Environment.
- Central DP is achieved via TEE sealing: the enclave applies DP to aggregated outputs and seals the result, so the server only ever sees the post-DP, sealed output.
- Attestation follows RFC 9334 (Remote ATtestation procedureS — RATS), enabling external verification of the enclave's integrity and the DP computation's correctness.

### ODP Privacy Model: "Privacy via Confidentiality"
ODP models computation as a **consumer-producer DAG** (directed acyclic graph). DP analysis is performed via **post-processing** and **advanced composition** over the DAG's outputs. Central DP is enforced by the TEE sealing mechanism, ensuring the only observable output satisfies the formal DP guarantee.

---

## Federated Compute Server Architecture

The `federated-compute` server orchestrates cross-device FL/FA at production scale. Its core components:

| Component | Role |
|-----------|------|
| **Task Management** | Defines, schedules, and tracks federated computations (training tasks, analytics tasks) across their lifecycle. |
| **Task Assignment** | Selects eligible devices for each round, manages device selection criteria (population, eligibility, sampling rate), and dispatches task specs. |
| **Aggregator** | Receives device updates, applies secure aggregation (SecAgg), and (in confidential deployments) routes through TEE for DP noise injection. |
| **Model Updater** | Applies aggregated updates to the global model, manages server-side optimizer state, and prepares the next round's model checkpoint. |

The server is designed for fault tolerance (via `raft-rs` consensus for critical state) and horizontal scalability to millions of concurrent device participants.

---

## Differential Privacy Configuration Parameters

Parfait supports multiple DP mechanisms, configurable per task:

| Mechanism | Description |
|-----------|-------------|
| **FIXED_GAUSSIAN** | Standard Gaussian mechanism with fixed noise σ calibrated to the privacy budget (ε, δ). Simple, predictable, widely used. |
| **ADAPTIVE_GAUSSIAN** | Gaussian mechanism with adaptive noise scaling — σ adjusts based on observed round statistics to better utilize the privacy budget across rounds. |
| **TREE** | Tree-based aggregation (hierarchical) for streaming/continual release, supporting DP-FTRL-style algorithms. Enables efficient DP for sequential updates. |
| **ADAPTIVE_TREE** | Adaptive variant of tree aggregation that tunes the tree structure and noise allocation per round, improving utility for non-stationary data. |

These parameters allow practitioners to trade off privacy strength, utility, and computational cost depending on the deployment context.

---

## A-Tech Alignment

- **Open-source:** All seven repositories are public, Apache 2.0 licensed, with 100+ contributors. No proprietary lock-in; fully auditable.
- **Data privacy:** Formal DP guarantees with production-proven ε values, including the first ε < 1 production DP model. δ = 10⁻¹⁰ across Gboard deployments.
- **Practical implementation:** Gboard production evidence (30+ models, 12,000+ devices/round), Android ODP APIs, Federated Compute Server architecture — this is deployable engineering, not just theory.
- **Verifiability:** TEE-based confidential federated compute with RFC 9334 attestation enables external, third-party auditing of privacy guarantees.

---

## Practical Triggers

Use this skill when:
- Building federated learning systems that need formal privacy guarantees
- Implementing differential privacy in production ML pipelines
- Designing on-device personalization (Android ODP or equivalent)
- Creating privacy-preserving analytics pipelines (heavy hitters, histograms)
- Evaluating open-source alternatives to proprietary privacy stacks
- Architecting TEE-based verifiable computation workflows
- Building scalable, group-structured federated datasets
- Implementing secure aggregation or consensus for distributed privacy systems

---

## Cross-References

- `google-gboard-private-fl-dp` — Gboard-specific FL+DP production deep dive
- `federated-learning-for-privacy-preserving-ai` — FL fundamentals
- `privacy-by-design-generative-ai` — Privacy-by-design principles for generative models
- `dataguard-hardware-dp-guarantee` — Hardware-enforced DP (complementary to TEE approach)
- `federated-learning-as-a-service-2026` — FL-as-a-service architectures
- `privacy-first-ai-pipeline-defense` — Software-level DP enforcement

---

## Sources

- Google Research, `google-parfait` GitHub organization (https://github.com/google-parfait), launched January 2025.
- Parfait repositories: `federated-language`, `tensorflow-federated`, `federated-compute`, `confidential-federated-compute`, `trusted-computations-platform`, `raft-rs`, `dataset_grouper`.
- Gboard production FL+DP deployments (Google AI blog posts and accompanying papers on DP-FTRL / MF-DP-FTRL).
- Android On-Device Personalization documentation (Privacy Sandbox initiative).
- RFC 9334 — Remote ATtestation procedureS (RATS) architecture.
- Apache 2.0 license; 100+ contributors across the Parfait organization.

See `references/evidence-base.md` for full repository descriptions, the Gboard DP guarantee table, MF-DP-FTRL algorithm details, ODP architecture internals, Federated Compute Server component design, and DP configuration parameter reference.