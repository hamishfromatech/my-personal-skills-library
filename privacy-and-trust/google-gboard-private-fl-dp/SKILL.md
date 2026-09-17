---
name: google-gboard-private-fl-dp
description: Apply Google's production federated learning + differential privacy framework (30+ Gboard language models, ε ≤ 1 strong DP guarantee, MF-DP-FTRL algorithm, SecAgg) to design privacy-preserving on-device model training systems. Use when implementing federated learning at scale, calculating differential privacy guarantees, configuring secure aggregation, or building production privacy-first AI training pipelines for A-Coder, Be Practical, and Builder's Club.
version: 1.0.0
---

# Google Gboard Private Federated Learning + Differential Privacy — Production Blueprint

## Overview

Google's Gboard team has achieved what was previously theoretical: production-grade federated learning with formal differential privacy guarantees for on-device language models at massive scale. As of February 2024 (and continued through 2026), 30+ Gboard next-word-prediction neural network language models have launched across 7+ languages and 15+ countries, all trained with federated learning (FL) and differential privacy (DP). Several models achieve ε ≤ 1, satisfying Google's own Tier 1 "strong privacy guarantee" threshold — the first time this has been accomplished for models trained directly on user data in production.

For A-Tech, Google's Gboard deployment is the gold-standard reference architecture for privacy-preserving AI training. It proves that federated learning with differential privacy is not a research curiosity but a production system serving hundreds of millions of users. Every principle, algorithm, and trade-off documented here has been battle-tested at planetary scale.

---

## The Privacy Stack: Four Layers

### Layer 1: Federated Learning (FL)
**What it does:** Multiple devices collaboratively train a shared model while keeping all training data on-device. Only model updates (gradients/weights) are transmitted to a central server for aggregation.

**Why it matters for privacy:** Raw user data (keystrokes, code, documents) never leaves the device. The server sees only aggregated model updates, not individual data points.

**Gboard scale:** Thousands of devices participate in each training round; 12,000+ devices per round for the strongest DP models.

### Layer 2: Differential Privacy (DP)
**What it does:** Provides a mathematical guarantee that the trained model cannot be used to infer whether any specific individual's data was in the training set. Formally characterized by (ε, δ) where smaller ε = stronger privacy.

**Gboard guarantees:**
- δ = 10⁻¹⁰ (negligibly small probability of privacy failure)
- ε ranges from 0.994 to 13.69 across deployed models
- Models with ε ≤ 1 (Portuguese-Brazil, Spanish-LatinAmerica) achieve Tier 1 "strong" privacy
- Models with ε ~ 10 achieve Tier 2 "reasonable" privacy
- All future Gboard LMs trained on user data require DP guarantees

### Layer 3: Secure Aggregation (SecAgg)
**What it does:** Cryptographic protocol ensuring the server can only access the aggregated sum of device updates, never any individual device's contribution. Even if the server is compromised, individual updates remain hidden.

**Gboard deployment:** SecAgg additionally applied to the Spanish (Spain) and English (US) models, layered on top of DP.

### Layer 4: Transparency and Auditability
**What it does:** Google open-sourced the key algorithmic approaches and privacy accounting:
- TFF (TensorFlow Federated) aggregator
- TFP (TensorFlow Privacy) DPQuery
- DP accounting code
- FL system infrastructure

**Why it matters:** External researchers can verify the privacy claims. This is the difference between "trust us" and "verify us."

---

## The DP-FTRL Algorithm Family

### Why Not DP-SGD?

Standard differentially private stochastic gradient descent (DP-SGD) adds noise to each gradient update. However, for federated learning, DP-SGD suffers because:
- Noise accumulates across many rounds, degrading utility
- The privacy-utility trade-off worsens as the number of rounds increases

### DP-FTRL (Follow-The-Regularized-Leader)

DP-FTRL is the algorithm Google chose for production Gboard training. Key advantages:
- **Better privacy-utility trade-off** than DP-SGD for multi-round federated training
- **Tree-based aggregation:** Noise is added in a tree structure that allows the privacy budget to be amortized across rounds more efficiently
- **Streaming-compatible:** Designed for the sequential nature of federated rounds

### MF-DP-FTRL (Matrix Factorization DP-FTRL)

The breakthrough that enabled ε ≤ 1. MF-DP-FTRL improves on DP-FTRL by:
- **Optimizing the noise injection matrix** using matrix factorization techniques
- **Reducing the privacy cost per round** without increasing the noise magnitude
- **Enabling strong DP (ε ≤ 1)** with sufficient device participation (12,000+ per round)

**Result:** The Portuguese-Brazil model achieved (ε=0.994, δ=10⁻¹⁰)-DP — the strongest privacy guarantee ever announced for a production model trained directly on user data.

---

## The Privacy Tiers

Google defines three tiers of DP guarantee (from "How to DP-fy ML" guide):

| Tier | ε Range | Description | Privacy Strength |
|------|---------|-------------|------------------|
| Tier 1 | ε ≤ 1 | Strong | Individual contributions are statistically undetectable |
| Tier 2 | ε ~ 10 | Reasonable | Strong protection against membership inference |
| Tier 3 | Finite ε | Basic | Bounds the privacy loss; better than no DP |

**A-Tech target:** Tier 1 (ε ≤ 1) for any model trained on user code/data. If population size is insufficient for Tier 1, accept Tier 2 as the minimum.

---

## Best Practices for Private FL (from Google's Production Experience)

### 1. Pre-train on Public Data
Always pre-train the base model on a large public dataset (e.g., multilingual C4) before federated fine-tuning on user data. This:
- Gives the model strong baseline utility
- Reduces the amount of user data needed for fine-tuning
- Improves the privacy-utility trade-off (less user data exposure = stronger DP for same utility)

### 2. Maximize Client Participation
The number of devices participating per round directly determines the DP guarantee achievable:
- More devices per round → same noise level → stronger DP (lower ε)
- Target: 6,500+ devices per round for reasonable DP; 12,000+ for strong DP (ε ≤ 1)
- Configure client participation frequency (e.g., once every few days) based on computation budget and population size

### 3. Adaptive Clipping
Clip per-device updates to bound their contribution, then add noise proportional to the clip norm:
- **Fixed clipping:** Simpler; clip norm chosen based on experience
- **Adaptive clipping:** Automatically adjusts clip norm during training; better utility in practice

### 4. Secure Aggregation (SecAgg)
Layer SecAgg on top of DP for defense-in-depth:
- DP protects against the server inferring individual data from the model
- SecAgg protects against the server seeing individual updates even before DP noise is applied
- Together: even a compromised server cannot reconstruct individual user data

### 5. Restrict Client Contributions
Limit how often each client can participate in federated rounds:
- Example: at most 2 participations across 2,000 rounds over 14 days
- Reduces the cumulative privacy exposure of any single user
- Enables tighter DP accounting

---

## A-Tech Application: A-Coder Federated Code Intelligence

### The Vision

A-Coder users generate valuable training signal every time they accept, reject, or modify an AI suggestion. This signal, aggregated across thousands of developers, could produce dramatically better code completion models. But this data is proprietary code — it cannot be uploaded to a central server.

**Solution:** Federated learning of code completion models using the Gboard blueprint.

### Architecture

```
┌────────────────────────────────────────────────────────┐
│  FEDERATED TRAINING ROUND                                │
│                                                         │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐        ┌──────┐│
│  │ Device 1│  │ Device 2│  │ Device N│  ...   │Server││
│  │ (local  │  │ (local  │  │ (local  │        │      ││
│  │  code)  │  │  code)  │  │  code)  │        │      ││
│  └────┬────┘  └────┬────┘  └────┬────┘        └──┬───┘│
│       │            │            │                  │    │
│       ▼            ▼            ▼                  │    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐            │    │
│  │ Local   │  │ Local   │  │ Local   │            │    │
│  │ LoRA    │  │ LoRA    │  │ LoRA    │            │    │
│  │ update  │  │ update  │  │ update  │            │    │
│  └────┬────┘  └────┬────┘  └────┬────┘            │    │
│       │            │            │                  │    │
│       ▼            ▼            ▼                  │    │
│  ┌─────────────────────────────────────┐            │    │
│  │     SecAgg (cryptographic)          │───────────▶│    │
│  │     Server sees ONLY aggregate      │            │    │
│  └─────────────────────────────────────┘            │    │
│                                                     │    │
│  ┌─────────────────────────────────────┐            │    │
│  │  DP noise added to aggregate        │◀───────────│    │
│  │  (MF-DP-FTRL, ε ≤ 1 target)        │            │    │
│  └─────────────────────────────────────┘            │    │
│                                                     │    │
│  ┌─────────────────────────────────────┐            │    │
│  │  Global LoRA adapter distributed    │───────────▶│    │
│  │  to all devices for next round      │            │    │
│  └─────────────────────────────────────┘            │    │
└────────────────────────────────────────────────────────┘
```

### Key Design Decisions

**Model:** BitNet 1-bit base (from the BitNet skill) + LoRA adapters (5–20 MB). The tiny adapter size makes federated rounds bandwidth-feasible.

**Privacy target:** ε ≤ 1, δ = 10⁻¹⁰ (Tier 1). Achievable with 6,500+ participating developers per round.

**Pre-training:** Base model pre-trained on public code corpora (The Stack, GitHub public repos with permissive licenses).

**Client participation:** Each developer participates at most twice per 2-week training cycle.

**SecAgg:** Enabled by default. Even though DP provides the formal guarantee, SecAgg adds defense-in-depth.

**Output:** A continuously improving community code completion adapter, distributed to all participants. No individual's code is ever exposed.

---

## A-Tech Application: Be Practical — Teaching FL+DP

### The Curriculum Opportunity

Google's Gboard deployment is the most accessible, well-documented case study for teaching privacy-preserving AI. Be Practical should include a module covering:

**Module: "Privacy-Preserving AI in Production"**
1. **Why privacy matters in AI training** — the data exposure problem, why cloud training isn't always safe
2. **Federated learning fundamentals** — data stays local, updates are aggregated
3. **Differential privacy intuition** — the "noise" that protects individuals, ε as a privacy budget
4. **The Gboard case study** — 30+ models, ε ≤ 1, production at scale
5. **Hands-on exercise** — run a simulated federated learning round with DP (using TensorFlow Federated or Flower)
6. **SecAgg** — cryptographic defense-in-depth
7. **The privacy-utility-computation triangle** — you can't maximize all three; choose your trade-offs deliberately
8. **Ethical considerations** — transparency, auditability, user consent, the right to opt out

### Why This Module Matters
Most AI courses teach model training as if privacy doesn't exist. Be Practical's audience — developers building real products — needs to understand that privacy-preserving training is not theoretical. It's running on their phones right now (if they use Gboard).

---

## A-Tech Application: Builder's Club — Federated Community Models

### The Community Model Flywheel

Builder's Club can operate a federated learning cooperative where members contribute to improving shared models without exposing their proprietary data:

**Round 1: Code Intelligence Adapter**
- Members fine-tune BitNet LoRA adapters on their local codebases
- Adapters aggregated with DP + SecAgg
- Community adapter distributed to all participants
- Every member's A-Coder gets better; no one's code is exposed

**Round 2: Domain-Specific Adapters**
- Subgroups form around domains (web dev, data science, embedded, ML)
- Each subgroup runs its own federated rounds
- Domain adapters improve faster than a monolithic model

**Round 3: Federation of Federations**
- Domain adapters can be composed or merged
- Community votes on which adapters to promote to the "core" distribution
- The marketplace supports both free community adapters and paid premium adapters

### Governance Principles
1. **Opt-in participation:** Members explicitly consent to each federated round
2. **Transparency:** Privacy guarantees (ε, δ) published for every round
3. **Auditability:** Aggregation code open-source; any member can verify the math
4. **Exit rights:** Members can leave and take their adapter contributions with them (adapters are portable)
5. **No data centralization:** The cooperative never stores raw training data; only aggregated updates

---

## Measuring and Reporting Privacy Guarantees

### The Privacy Accounting Protocol

Every A-Tech federated training run should report:

```
═══════════════════════════════════════════
  A-TECH FEDERATED TRAINING REPORT
═══════════════════════════════════════════

Model:              A-Coder Code Completion v2.1
Base:               BitNet-1B (pre-trained on public C4-code)
Training dates:     2026-07-01 to 2026-07-14
Federated rounds:  500
Devices per round:  8,247 (avg)
Total participants: 12,391 unique devices

DP Guarantee:       (ε = 0.87, δ = 10⁻¹⁰)
Privacy Tier:       Tier 1 — Strong
Algorithm:           MF-DP-FTRL
SecAgg:             Enabled
Clip norm:          Adaptive (median 0.42)

Pre-training:        Public code corpus (The Stack v2)
User data exposure:  Code completion accept/reject/modify signals only
                        (no raw code transmitted)

Verification:        Aggregation code: github.com/a-tech/fl-aggregator
                     DP accounting:    github.com/a-tech/dp-accounting
                     Audit log:         Available to participants on request

═══════════════════════════════════════════
```

### Why Formal Reporting Matters
1. **Trust:** Users can verify the privacy claim themselves
2. **Regulatory compliance:** Provides auditable evidence for GDPR, EU AI Act, and other frameworks
3. **Community standard:** Sets the expectation that all federated systems report their guarantees
4. **Continuous improvement:** Tracking ε over time shows whether privacy is improving or degrading

---

## Key References

- **Google Research Blog** (Zheng Xu, Yanxiang Zhang, February 21, 2024) — "Advances in private training for production on-device language models" — the primary source for all technical details in this skill
- **Google** — "Federated Learning of Gboard Language Models with Differential Privacy" — formal publication of DP guarantees
- **Google** — "How to DP-fy ML" — the DP tier guide (Tier 1: ε ≤ 1, Tier 2: ε ~ 10, Tier 3: finite ε)
- **DP-FTRL** — "Federated Learning of Gboard Language Models with Differential Privacy" (algorithm details)
- **MF-DP-FTRL** — Matrix Factorization DP-FTRL algorithm (enabling ε ≤ 1)
- **TensorFlow Federated (TFF)** — Open-source federated learning framework
- **TensorFlow Privacy (TFP)** — Open-source DP accounting library

---

## A-Tech Values Alignment Summary

| Value | How This Skill Advances It |
|-------|--------------------------|
| Open-Source AI | Aggregation and DP accounting code open-sourced; community can verify privacy claims |
| Data Privacy | Core principle — user data never leaves the device; formal mathematical privacy guarantees |
| Financial Freedom | Federated learning enables community model improvement without cloud compute costs; no vendor lock-in |
| Practical Implementation | Based on Google's production deployment (30+ models, 7+ languages); concrete architecture, privacy accounting protocol, phased roadmap |