---
name: ietf-federated-learning-agent-privacy
description: Applies the IETF draft-kale-agntcy-federated-privacy-01 architecture for privacy-preserving federated learning in multi-tenant AI agent systems. Use when building cross-tenant FL systems, designing privacy-preserving agent learning, or implementing differential privacy for agent training.
---

# IETF Federated Learning Agent Privacy

## Overview

This skill applies the IETF Internet-Draft `draft-kale-agntcy-federated-privacy-01` (July 2026, Cisco Systems), which specifies an architecture for privacy-preserving federated learning (FL) in multi-tenant AI agent deployments. The draft addresses a critical gap: as AI agents are deployed across organizational tenants (enterprises, departments, cloud customers), training them collaboratively without exposing raw behavioral data requires formal privacy guarantees, standardized protocol metadata, and clear threat models.

The architecture targets the AGNTCY framework for AI agent interoperability and defines how multiple tenants can jointly train agent models while maintaining strict data isolation, differential privacy bounds, and regulatory compliance (GDPR, HIPAA, CCPA). This is an IETF standards-track contribution, meaning it is open, vendor-neutral, and intended for broad industry adoption.

## Key Framework & Principles

### Tenant Data Isolation

The foundational principle: **raw behavioral data never leaves tenant boundaries**. Each tenant (e.g., an enterprise customer of an agent platform) retains full control of its data — agent interaction logs, tool invocations, retrieved documents, and user conversations. Only differentially-private model updates (or adapter parameters) are transmitted to a central aggregation server.

This isolation is enforced by architecture, not policy:
- Local data stores remain within tenant infrastructure.
- Local model training occurs on tenant premises or tenant-controlled cloud instances.
- DP noise injection happens locally before any parameter transmission.
- The central server receives only noisy aggregates, never raw data.

### Formal Privacy Guarantees

The architecture mandates **differential privacy (DP)** with formal bounds:

- **Epsilon (ε)**: Range 1.0–10.0, controlling the privacy-utility tradeoff. Lower ε = stronger privacy, more noise.
- **Delta (δ)**: Set to 1/n (where n is the number of records), representing the probability of a catastrophic privacy failure.
- **Gaussian mechanism**: Applied to FedAvg updates. The Gaussian mechanism adds calibrated noise to parameter updates before transmission.

The privacy budget is allocated across training rounds. Once the budget is exhausted, training must stop or the privacy guarantee degrades. This enforces a finite, auditable privacy cost.

### Regulatory Compliance

The architecture is designed to satisfy:
- **GDPR**: Data minimization (raw data never transmitted), purpose limitation (training only), and data subject rights (tenant retains all data for deletion/access requests).
- **HIPAA**: Protected health information stays within tenant boundaries; only DP-protected model updates cross boundaries.
- **CCPA**: Tenant-side data control aligns with consumer right-to-delete and right-to-know requirements.

### System Components

1. **Local Data Stores**: Per-tenant storage for agent interaction data.
2. **Local Model Training**: Per-tenant training pipeline that computes model (or adapter) updates.
3. **DP Noise Injection**: Local module that applies Gaussian mechanism noise and clipping to updates before transmission.
4. **Central Aggregation Server**: Receives noisy updates from multiple tenants and performs FedAvg aggregation. The server is assumed honest-but-curious (see threat model).

### FedAvg with Gaussian Mechanism DP

The core training algorithm:

1. Each tenant trains locally on its data for E epochs.
2. Local updates are clipped to bound sensitivity (L2 norm clipping, threshold C).
3. Gaussian noise (σ²C²) is added to clipped updates.
4. Noisy updates are transmitted to the central server.
5. The server averages noisy updates (FedAvg) and returns the new global model.
6. Repeat for T rounds, tracking cumulative privacy budget via the moments accountant or Rényi DP.

### Privacy Budget Allocation

- **ε range**: 1.0–10.0 (configurable per deployment).
- **δ**: 1/n, where n is the tenant's dataset size.
- Budget is consumed across rounds. The system must track and report remaining budget.
- Higher-stakes deployments (healthcare, finance) should target ε ≤ 3.0.

### Protocol Metadata

Each communication round includes structured metadata:

- **Task metadata**: Identifies the training task (agent type, capability being trained).
- **Round metadata**: Current round number, total rounds, remaining privacy budget.
- **Update metadata**: Tenant ID (pseudonymized), update size, noise parameters applied.
- **Release metadata**: Version, privacy budget consumed, model card information for the released model.

### PEFT/LoRA Adapter Handling

For parameter-efficient fine-tuning (critical for large agent models):

- Clipping and noise apply to **transmitted adapter parameters only** (not the full model).
- **Rank-aware aggregation**: LoRA adapters of different ranks must be handled carefully — the aggregation server must account for rank differences when averaging.
- This makes FL practical for large models (e.g., 70B+ parameter agents) where full-model FL is infeasible.

### Threat Model

- **Server**: Honest-but-curious. The server follows the protocol but may attempt to infer tenant data from received updates. DP noise protects against this.
- **Tenants**: Honest. They follow the protocol. (Byzantine tenants are addressed by extensions.)
- **Communication**: Secure channels (TLS) between tenants and server.
- **Secure aggregation (recommended)**: Using Bonawitz et al. (2017) protocol, the server sees only the aggregate, never individual tenant updates. This strengthens the honest-but-curious server model.

### Extensions

- **Secure Aggregation** (Bonawitz17): Server learns only the sum of updates, not individual contributions.
- **Byzantine Fault Tolerance**: Robust aggregation rules (e.g., Krum, trimmed mean) to handle malicious tenants submitting poisoned updates.

### Agent-Specific Risks

The draft identifies risks unique to AI agents (vs. traditional FL):

- **Tool credentials**: Agent logs may contain API keys or OAuth tokens inadvertently captured in training data.
- **Retrieved documents**: RAG-fetched documents may contain sensitive information that leaks through model updates.
- **Prompt injection**: Malicious inputs in tenant data could manipulate agent behavior in ways that propagate through FL.
- **Tool poisoning**: If a tenant's agent interacts with a compromised tool, the resulting behavioral data may be corrupted.

### The Update Privacy vs. Inspection Tension

A fundamental tension: **DP protects update privacy** (preventing the server from inferring tenant data), but **update inspection is needed for poisoning defense** (detecting malicious updates). You cannot fully inspect an update that is differentially private. The draft recommends:
- Secure aggregation for privacy.
- Byzantine-robust aggregation at the aggregate level (not individual inspection) for defense.
- This is an open research area — the draft acknowledges the tension rather than fully resolving it.

## Practical Application Guidance

### Example Configuration

```json
{
  "task": "agent-customer-support-v2",
  "federated_learning": {
    "algorithm": "fedavg",
    "rounds": 50,
    "local_epochs": 3,
    "privacy": {
      "mechanism": "gaussian",
      "epsilon_target": 5.0,
      "delta": "1/n",
      "clipping_norm": 1.0,
      "noise_multiplier": 0.8
    },
    "adapter": {
      "type": "lora",
      "rank": 16,
      "rank_aware_aggregation": true
    },
    "secure_aggregation": {
      "enabled": true,
      "protocol": "bonawitz17"
    }
  },
  "tenants": [
    {"id": "tenant-alpha-pseudonym", "dataset_size": 50000},
    {"id": "tenant-beta-pseudonym", "dataset_size": 120000}
  ]
}
```

### When to Apply This Skill

- **Cross-tenant agent training**: Multiple organizations collaboratively training a shared agent model.
- **Regulated industries**: Healthcare or finance deployments where data cannot leave tenant boundaries.
- **Multi-department enterprises**: Different departments (with different data access policies) training a shared internal agent.
- **Agent platforms**: SaaS agent platforms where customer data must remain isolated.

### When NOT to Apply

- **Single-tenant training**: Standard centralized training is simpler if data isolation isn't required.
- **Non-agent FL**: The agent-specific risks (tool credentials, prompt injection) don't apply; use general FL frameworks instead.

## A-Tech Alignment

- **Open Source**: This is an IETF Internet-Draft — an open, vendor-neutral standards contribution from Cisco Systems. IETF drafts are publicly available and designed for broad implementation.
- **Data Privacy**: The entire architecture centers on formal differential privacy guarantees, tenant data isolation, and regulatory compliance. This is a core data-privacy skill.
- **Practical Implementation**: The draft includes example JSON configurations, structured protocol metadata, and specific algorithm recommendations (FedAvg + Gaussian mechanism, Bonawitz17 secure aggregation). It is implementable, not theoretical.

## Cross-References

- **genai-privacy-choice-ecosystems**: Addresses user-facing privacy choice design for GenAI products, complementing this skill's infrastructure-level privacy approach. Together they cover privacy from user interface to training pipeline.
- **guardchain-fl-aigc-trust-framework**: Extends federated learning trust with blockchain-based verification at three stages (data preparation, adapter update, parameter aggregation). Use GuardChain when you need verifiable trust beyond DP guarantees.
- **open-source-funding-channels-2026**: Relevant for sustaining open-source FL infrastructure projects that implement this architecture.