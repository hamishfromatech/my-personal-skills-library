---
name: knexa-fl-peer-llm-federation
description: Implements orchestrated-decentralized federated LLM fine-tuning where a non-aggregating matchmaker learns optimal peer pairing via contextual bandits, and knowledge transfer occurs through secure text-based distillation. Use when designing P2P federated learning systems, avoiding centralized aggregation bottlenecks, or matching heterogeneous LLM agents for collaborative improvement.
---

# KNEXA-FL: Orchestrated-Decentralized Peer-to-Peer LLM Federation

## When to Use

- Designing federated learning systems that avoid centralized aggregation
- Matching heterogeneous LLM agents (different sizes, architectures) for collaboration
- Privacy-preserving LLM improvement where a central server must never see model weights
- Multi-organization LLM co-training with trust boundaries
- P2P knowledge distillation workflows

## Core Problem Solved

Centralized federated learning has two failure modes:
1. **Security vulnerability**: Central aggregator sees all updates (single point of attack)
2. **Statistical inefficiency**: Random P2P pairing wastes collaboration on mismatched peers

KNEXA-FL resolves both by introducing a **Central Profiler/Matchmaker (CPM)** that never aggregates model updates — it only learns *which peers should collaborate*.

## Architecture: The Three-Party Separation

```
┌─────────────────────────────────────────────────┐
│         Central Profiler/Matchmaker (CPM)        │
│  • Learns optimal pairing policy (LinUCB)       │
│  • Sees ONLY abstract agent profiles             │
│  • NEVER accesses models, weights, or data       │
└───────────────────┬─────────────────────────────┘
                    │ match decision
    ┌───────────────┴───────────────┐
    │                               │
┌───▼───┐                     ┌───▼───┐
│ Peer A │ ←── text-based ──→ │ Peer B │
│ (LLM)  │    distillation     │ (LLM)  │
└───────┘                     └───────┘
```

**Key insight**: The CPM is non-aggregating. It does not combine model updates. It only decides *who should talk to whom*. Actual knowledge transfer happens directly between matched peers via text-based distillation.

## The LinUCB Matchmaker

The CPM formulates P2P collaboration as a **contextual bandit problem**:

- **Context**: Abstract agent profiles (task type, model architecture family, parameter count range, data distribution characteristics — sanitized for privacy)
- **Arms**: Possible peer pairings
- **Reward**: Performance improvement after collaboration (measured on shared validation set)
- **Algorithm**: LinUCB with sub-linear regret relative to oracle pairing

**Why this works**: Random pairing wastes rounds on mismatched collaborations. The bandit learns which profile combinations produce the best mutual improvement and exploits that knowledge while maintaining exploration.

## Secure Text-Based Distillation

Knowledge transfer between matched peers uses **text-based distillation** (not gradient sharing):

1. Peer A generates outputs on a public transfer set
2. Peer B fine-tunes on Peer A's outputs (student role)
3. Bidirectional: both peers improve from each other
4. No raw data or model weights ever cross the trust boundary

**Privacy properties**:
- CPM never sees models or data
- Peers exchange only text outputs on public transfer sets
- Privacy-preserving profile sanitization before CPM sees agent metadata
- Guardrail filters prevent toxic/harmful knowledge transfer

## Key Empirical Results

On heterogeneous code generation (6 agents, 410M-620M parameters, HumanEval + MBPP):

| Metric | KNEXA-FL | Random P2P | Central KD |
|--------|----------|-----------|------------|
| Pass@1 improvement | **+50% relative** | baseline | initial gain then collapse |
| Convergence | **Stable** | Slow, noisy | Catastrophic collapse |
| Regret | **Sub-linear** | Linear | N/A |
| Model access by CPM | **None** | N/A | Full |

**Critical finding**: Centralized distillation baselines show initial improvement but then suffer *catastrophic performance collapse*. KNEXA-FL maintains stable convergence — the orchestrated P2P approach is more robust than centralized alternatives.

## Federation Configuration (Reference)

The paper validates with heterogeneous backbones:

| Client | Backbone | Parameters | Architecture |
|--------|----------|-----------|-------------|
| C0 | Qwen-0.5B | 620M | Qwen |
| C1 | Cerebras-GPT-590M | 590M | Cerebras |
| C2 | BLOOM-560M | 560M | BLOOM |
| C3 | Pythia-410M | 410M | Pythia |
| C4 | Qwen-0.5B | 620M | Qwen (dup) |
| C5 | Cerebras-GPT-590M | 590M | Cerebras (dup) |

Duplicate backbones are independent agents with distinct LoRA adapters, seeds, and Dirichlet non-IID data partitions (α=0.1).

## Implementation Guide

### Designing a KNEXA-FL-style System

1. **Define agent profiles**: Extract privacy-preserving metadata (task type, model family, parameter range, data distribution fingerprint). Sanitize before sending to CPM.

2. **Implement the matchmaker**: Start with LinUCB. Context = profile pair. Reward = post-collaboration performance delta. Maintain exploration/exploitation balance.

3. **Set up text-based distillation**: Use a public transfer set (not private data). Peer generates outputs; partner fine-tunes on outputs. Filter for quality and safety.

4. **Privacy guardrails**: Profile sanitization (no raw data in profiles), guardrail filter (block harmful transfer), SIER (safety-informed embedding representation).

5. **Heterogeneous LoRA**: Adaptive LoRA configuration per architecture (rank 8, alpha 32, learning rate 3e-5 local / 5e-5 KD).

### For Open-Source Implementation

The reference implementation (BSD-3-Clause, Python) includes:
- `knexa_fl/cpm/` — CPM orchestration + LinUCB + privacy profiles
- `knexa_fl/agents/` — Agent with PEFT training + guardrails
- `knexa_fl/p2p/` — Adaptive knowledge distillation
- `src/grpc_p2p/` — gRPC-based P2P communication layer
- `baselines/` — Random-P2P, FedID-CentralKD, FedAvg, FedSKD

## A-Tech Applications

**A-Coder**: Federated code intelligence across organizations without sharing proprietary codebases. Match organizations with complementary code domains.

**Be Practical**: Curriculum on decentralized AI collaboration — teaching P2P federated concepts, contextual bandits for matchmaking, text-based distillation.

**Builder's Club**: Open-source KNEXA-FL fork for community LLM improvement. Contributors match by domain, distill knowledge through shared outputs.

## Distinction from Existing Skills

| Skill | Focus | Overlap | Distinction |
|-------|-------|---------|-------------|
| `federated-learning-for-privacy-preserving-ai` | General FL concepts | Privacy-preserving FL | KNEXA-FL is specifically P2P orchestrated, not centralized |
| `chain-federated-fine-tuning` | Sequential FL | LLM fine-tuning | KNEXA-FL uses bandit-matched P2P, not chained sequential |
| `zk-proof-federated-learning-trust` | ZKP verification | Trust in FL | KNEXA-FL uses non-aggregating CPM, not ZKP |
| `federated-llm-on-device-personalization` | On-device LLM FL | LLM federation | KNEXA-FL targets heterogeneous multi-org, not single-device |

## A-Tech Alignment

- **Open-source AI**: BSD-3-Clause reference implementation, open-weight model federation
- **Data privacy**: CPM never sees models/data; text-based distillation preserves data locality
- **Financial freedom**: No central infrastructure investment; peers collaborate directly
- **Practical implementation**: AAAI-2026 validated, 6-agent heterogeneous federation, sub-linear regret proven

## References

- Singh, Vissol-Gaudin, Otung & Sekiya. "Learning to Collaborate: An Orchestrated-Decentralized Framework for Peer-to-Peer LLM Federation." AAAI 2026.
- Code: github.com/FujitsuResearch/knexa-fl (BSD-3-Clause)
- Li et al. "Federated Optimization in Heterogeneous Networks." MLSys 2020. (FedProx)
- McMahan et al. "Communication-Efficient Learning of Deep Networks from Decentralized Data." AISTATS 2017. (FedAvg)