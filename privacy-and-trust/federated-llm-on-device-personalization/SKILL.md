---
name: federated-llm-on-device-personalization
description: Build privacy-first personalization for large language models and AI agents using federated learning and on-device adaptation — so models personalize to each user's context, vocabulary, and workflow without any user data leaving the device. Covers the federated LLM architecture (parameter-efficient fine-tuning on-device, secure aggregation, differential privacy noise injection, the heterogeneity challenge), the on-device personalization stack (adapter/LoRA modules, local RAG, user-specific decoding), the privacy-utility-efficacy tradeoff, the edge-vs-cloud compute economics, and the regulatory alignment (GDPR data minimization, EU AI Act, on-device as consent-free processing). Use when building personalized AI features that must respect data sovereignty, designing federated training for LLM-based products, implementing on-device adaptation for a local-first AI tool, or evaluating the privacy-first personalization architecture vs cloud-personalization alternatives. NOT for general federated learning (use federated-learning-for-privacy-preserving-ai), federated learning as a service (use federated-learning-as-a-service-2026), or local-first web architecture (use local-first-web-architecture-2026).
---

# Federated LLMs & On-Device Personalization

## Overview

Federated learning (FL) lets multiple parties collaboratively train a shared model without exchanging raw data — each party trains locally on its own data and shares only model updates (gradients or parameters), which are aggregated into an improved global model. Applied to large language models, FL enables a privacy-first personalization architecture: the model personalizes to each user's context, vocabulary, and workflow while the user's data never leaves the device. This is the architecture A-Tech's local-first values require for genuinely private AI — not just "your data stays on your device" but "the model adapts to you without anyone ever seeing your data."

This skill covers the production architecture for federated LLMs and on-device personalization: parameter-efficient fine-tuning on-device (LoRA/adapter modules), secure aggregation protocols, differential privacy noise injection, the data-heterogeneity challenge (non-IID data across users), the privacy-utility-efficacy tradeoff, edge-vs-cloud compute economics, and regulatory alignment. It extends the existing privacy skills (`federated-learning-for-privacy-preserving-ai`, `federated-learning-as-a-service-2026`, `local-first-web-architecture-2026`) from the general FL concept and the local-first web concept to the **LLM-specific architecture and on-device personalization stack**.

## When to Use

- Building personalized AI features that must respect data sovereignty (the model adapts to the user without the user's data leaving the device)
- Designing federated training or federated fine-tuning for an LLM-based product
- Implementing on-device adaptation for a local-first AI coding assistant, learning tool, or community platform
- Evaluating the privacy-first personalization architecture vs cloud-personalization alternatives (and the tradeoffs)
- Solving the data-heterogeneity (non-IID) problem across a federated user base
- Aligning AI personalization with GDPR data-minimization, EU AI Act risk classification, and consent-free on-device processing
- Designing the privacy-utility-efficacy tradeoff for a specific product (how much personalization, how much privacy, how much model quality)
- Building the open-source reference architecture for privacy-preserving personalization

NOT for:
- General federated learning concepts and protocols — use `federated-learning-for-privacy-preserving-ai`
- Federated learning as a managed service / commercial offering — use `federated-learning-as-a-service-2026`
- Local-first web architecture (CRDTs, sync, offline-first data) — use `local-first-web-architecture-2026`
- Differential privacy synthetic data generation — use `differential-privacy-synthetic-data`
- On-device training frameworks (BitNet) — use `bitnet-on-device-training-framework`

## Core Process / Workflow

### Step 1: Understand Why LLMs Change the Federated Learning Problem

Classical FL (Google's Gboard, 2017+) trained small models on mobile devices — next-word prediction, emoji suggestion. The updates were small and the models were shallow. LLMs break several assumptions:

| Classical FL (Gboard era) | Federated LLMs |
|---|---|
| Small models (millions of params) | Large models (billions of params) — full fine-tuning is infeasible on-device |
| Single-purpose (next-word) | General-purpose + personalization layers |
| Tiny updates (KB) | Large updates (MB-GB for full fine-tuning) |
| Homogeneous tasks | Heterogeneous user contexts (non-IID) |
| Cloud aggregation cheap | Aggregation bandwidth and compute non-trivial |
| Privacy = no raw text leaves device | Same, plus inference privacy, plus model-extraction risk |

**The implication:** Federated LLMs require **parameter-efficient fine-tuning (PEFT)** — adapter modules and LoRA (Low-Rank Adaptation) that update only a small fraction of parameters (often <1%) while keeping the base model frozen. This makes on-device fine-tuning computationally feasible and keeps the shared updates small enough to aggregate.

### Step 2: Build the On-Device Personalization Stack

The privacy-first personalization architecture has three layers, each running on-device:

#### Layer 1: Frozen Base Model (Shared, Open-Source)

- The base LLM (e.g., a 1-7B parameter open-source model) is shipped frozen to every device.
- It provides general capability (reasoning, code, language) without any user data.
- Open-source is essential here: the base model is auditable, the user can verify what's running, and there's no hidden backdoor.

#### Layer 2: Personal Adapter / LoRA Module (Local, User-Specific)

- A small adapter (LoRA ranks typically 8-64, adding <1% of base parameters) is fine-tuned **on the user's device using the user's own data**.
- The adapter captures the user's vocabulary, coding style, domain knowledge, preferred response format.
- Only the adapter weights (small, MB-scale) are shared for federated aggregation — never the raw data.
- The user can inspect, export, or delete their adapter at any time (data sovereignty).

#### Layer 3: Local Retrieval + User-Specific Decoding (Local, Session-Specific)

- A local vector store (RAG) holds the user's documents, code history, and context — queried at inference time, never sent to a server.
- User-specific decoding parameters (temperature, repetition penalty, system prompt) adapt generation to the user's preferences.
- This layer is the most personal and the most privacy-sensitive — it stays entirely on-device.

### Step 3: Implement the Federated Aggregation Protocol

The federated round:

1. **Local training:** Each device fine-tunes its adapter on local data for a few steps.
2. **Update extraction:** Only the adapter weight deltas (not raw data, not the base model) are extracted.
3. **Secure aggregation:** Updates are encrypted/combined so the server sees only the aggregate, never individual updates. Techniques: secure multi-party computation, homomorphic encryption, or trusted execution environments.
4. **Differential privacy noise:** Calibrated noise is added to the aggregate to provide formal privacy guarantees (ε-differential privacy). The noise level is tuned to the privacy budget.
5. **Global adapter update:** The aggregated, noise-added delta updates the shared global adapter, which is redistributed to devices.
6. **Repeat** at a cadence that balances freshness, bandwidth, and privacy budget consumption.

**The key insight:** The shared global adapter improves for everyone (capturing common patterns across the user base) while each device's local adapter captures individual personalization. The base model never changes.

### Step 4: Solve the Data Heterogeneity (Non-IID) Challenge

User data is not independent and identically distributed (non-IID): different users have different vocabularies, domains, and usage patterns. This causes **client drift** — local updates diverge from the global optimum, degrading the aggregated model.

**Mitigation strategies:**

| Technique | How It Works | Tradeoff |
|---|---|---|
| **FedProx** | Adds a proximal term to the local loss, penalizing drift from the global model | Reduces personalization; under-fits diverse users |
| **Personalized FL (per-user adapters)** | Each user keeps a personal adapter; only a shared component is aggregated | Best privacy+personalization; more complex |
| **Clustering** | Group similar users; aggregate within clusters | Requires knowing cluster structure; privacy of cluster membership |
| **Local fine-tuning epochs control** | Few local epochs before aggregation reduces drift | More communication rounds; slower convergence |
| **Adapter-only federation** | Federate only the small adapter, keep base frozen | Updates small; personalization preserved in residual |

**For A-Tech:** The personalized-FL approach (per-user adapters + shared component) is the natural fit — it preserves the privacy-first personalization that is the whole point, while still letting the community benefit from shared improvements.

### Step 5: Manage the Privacy-Utility-Efficacy Tradeoff

There is no free lunch — every privacy mechanism costs something:

| Mechanism | Privacy Benefit | Utility Cost |
|---|---|---|
| Local-only training (no federation) | Maximum privacy — no updates leave device | No community benefit; each user's model is isolated |
| Federated with secure aggregation | Server cannot see individual updates | Compute/bandwidth overhead; doesn't protect against membership inference |
| Differential privacy noise | Formal privacy guarantee (ε-bound) | Noise degrades model quality; more noise = more privacy = less accuracy |
| Adapter-only federation | Base model never changes; small attack surface | Limited expressivity vs full fine-tuning |
| On-device inference only | No data leaves device at all | No cloud compute; limited to device capability |

**The design decision:** A-Tech's default should be **on-device inference + local adapter personalization + opt-in federated aggregation with differential privacy**. Users who want community benefit opt in; users who want maximum privacy keep their adapter local. The opt-in is the consent mechanism.

### Step 6: Evaluate Edge-vs-Cloud Compute Economics

| Dimension | On-Device | Cloud |
|---|---|---|
| Inference latency | Near-zero (no network) | Network round-trip |
| Per-query cost | Free (already paid for hardware) | Per-token inference cost |
| Privacy | Data never leaves device | Data sent to provider |
| Model size ceiling | Limited by device RAM/GPU | Effectively unlimited |
| Personalization | Native (local data, local adapter) | Requires shipping user data to cloud |
| Offline capability | Full | None |
| Update distribution | Push base + global adapter | Provider updates centrally |

**The 2026 economics shift:** On-device foundation models (Apple's on-device models, Phi, Gemma, Qwen small variants) have crossed the capability threshold for many personal tasks. Combined with local-first architecture, on-device is now the cost-effective AND privacy-effective choice for personal AI — not just the privacy-conscious choice.

### Step 7: Align with Regulation

- **GDPR data minimization:** On-device processing with no data leaving the device is the strongest data-minimization posture. Federated aggregation with differential privacy provides a formal guarantee.
- **EU AI Act risk classification:** On-device personal AI assistants for individuals are typically lower-risk than general-purpose cloud AI systems. Transparency obligations are lighter; the open-source model itself is auditable.
- **Consent-free on-device processing:** Processing that happens entirely on the user's device, on the user's own data, for the user's own benefit, generally does not require the same consent infrastructure as cloud processing of personal data. This is a regulatory advantage, not a loophole — the user's data never leaves their control.
- **Data subject rights:** The user can export their adapter (portability), delete it (erasure), or inspect it (transparency) — all natively, because it lives on their device.

### Step 8: A-Tech Application Matrix

| Product | On-Device Personalization Application |
|---|---|
| **A-Coder** | Per-developer LoRA adapter fine-tuned on the developer's codebase, coding style, and project conventions; local RAG over the developer's repo; federated aggregation of a shared "common code patterns" adapter (opt-in, DP-noised). The IDE becomes genuinely personalized to each developer without any code leaving their machine. Open-source base model + private adapter = the privacy-first coding assistant. |
| **Be Practical** | Per-learner adapter fine-tuned on the learner's progress, vocabulary, and learning style; local RAG over the learner's notes and completed modules; federated aggregation of a shared "common misconception" adapter (opt-in). The curriculum personalizes to each learner's pace and gaps without any learning data leaving the device. |
| **Builder's Club** | Per-member adapter fine-tuned on the member's contribution history, technical interests, and community context; local RAG over the member's contributions; federated aggregation of a shared "community knowledge" adapter (opt-in). The community benefits from shared learning while each member's contribution history stays private. |

### Step 9: Open-Source Reference Architecture

A-Tech should publish an open-source reference implementation of the federated LLM on-device personalization stack:

- **Open base model** (frozen, auditable)
- **Adapter module spec** (LoRA ranks, training protocol, export/import format)
- **Secure aggregation protocol** (open, auditable, no proprietary crypto)
- **Differential privacy budget** (configurable, documented)
- **Opt-in federation** (user chooses to participate; can withdraw)
- **Adapter sovereignty** (user can export, inspect, delete)

This is the open-source privacy-first alternative to cloud personalization. It is the architectural expression of A-Tech's values.

## Anti-Patterns

1. **Full fine-tuning on-device** — attempting to fine-tune the entire base model on-device. Use adapters/LoRA; freeze the base.
2. **Federating raw data** — the defining error. Federate updates (adapter deltas), never raw data.
3. **No differential privacy** — federated updates can leak individual data through gradient analysis. DP noise is the formal guarantee, not optional.
4. **Mandatory federation** — forcing all users to participate. Opt-in is the consent mechanism and the trust foundation.
5. **Ignoring non-IID** — assuming user data is homogeneous. Client drift degrades the model without heterogeneity mitigation.
6. **Cloud personalization as default** — defaulting to shipping user data to a cloud for personalization when on-device adaptation is feasible. This is the architectural choice A-Tech's values reject.
7. **Adapter lock-in** — storing the adapter in a proprietary format the user cannot export or inspect. Data sovereignty requires portability.
8. **Treating on-device as second-best** — positioning on-device personalization as the "privacy concession" rather than the superior architecture it is in 2026.

## Integration with Existing Skills

| Related Skill | Relationship |
|---|---|
| `federated-learning-for-privacy-preserving-ai` | General FL concepts and protocols. This skill extends to LLMs and on-device personalization specifically. |
| `federated-learning-as-a-service-2026` | FL as a commercial offering. This skill is the open-source, on-device, self-hosted complement. |
| `local-first-web-architecture-2026` | Local-first data sync (CRDTs, offline). This skill adds the LLM personalization layer on top of local-first data. |
| `bitnet-on-device-training-framework` | On-device training framework (1-bit models). This skill uses adapter fine-tuning on top of a frozen base, which can itself be a BitNet-style model. |
| `differential-privacy-synthetic-data` | DP for synthetic data generation. This skill uses DP for federated update noise — same family of technique, different application. |
| `privacy-preserving-local-ai` | Privacy-preserving local AI concept. This skill provides the LLM-specific personalization architecture. |
| `privacy-first-personalization-2026` | Privacy-first personalization concept. This skill provides the federated LLM implementation. |

## A-Tech Values Alignment

- **Open-source AI:** Open base model (auditable), open aggregation protocol, open adapter spec. The entire personalization stack is auditable — the opposite of black-box cloud personalization.
- **Data privacy:** The user's data never leaves their device. Federation is opt-in with differential privacy. Adapter sovereignty (export/inspect/delete) is native.
- **Financial freedom:** On-device inference is free after hardware; no per-token cloud cost. Personalization without a subscription. The economics of private AI favor the user, not the platform.
- **Practical implementation:** The three-layer stack (frozen base + personal adapter + local RAG), the aggregation protocol, the heterogeneity mitigations, and the A-Tech application matrix make this executable, not theoretical.

## References
- See [references/federated-llm-evidence-base.md](references/federated-llm-evidence-base.md) for the technical detail on LoRA/adapter methods, secure aggregation protocols, differential privacy budgets, the non-IID challenge and mitigations, the edge-vs-cloud compute comparison, regulatory alignment detail, and source bibliography.