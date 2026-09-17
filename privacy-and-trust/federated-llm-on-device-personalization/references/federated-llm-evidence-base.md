# Federated LLMs & On-Device Personalization — Evidence Base

## Technical Foundations

### Parameter-Efficient Fine-Tuning (PEFT) for On-Device Feasibility

Full fine-tuning of an LLM (updating all billions of parameters) is infeasible on consumer hardware — the GPU memory and compute required exceed what phones, laptops, and edge devices can provide. PEFT methods make on-device fine-tuning feasible by updating only a small fraction of parameters while keeping the base model frozen.

**LoRA (Low-Rank Adaptation):**
- Inserts low-rank decomposition matrices (ranks typically 8-64) into the attention and feed-forward layers of the frozen base model.
- Adds <1% of the base model's parameters as trainable.
- On-device fine-tuning trains only the LoRA matrices; the base weights stay frozen.
- The trained LoRA adapter is small (MB-scale) and can be shared, exported, or aggregated without shipping the base model.
- Demonstrated to match or approach full fine-tuning quality on many tasks at a fraction of the parameter cost.

**Adapter modules:**
- Small bottleneck layers inserted between transformer layers.
- Similar parameter-efficiency to LoRA; slightly different insertion architecture.
- Trainable parameters are a small fraction of the base.

**The implication for federation:** Only the adapter/LoRA weights (small) need to be transmitted for aggregation — not the base model (large) and never the raw data. This makes federated LLM training bandwidth-feasible where full-model federation would not be.

### Secure Aggregation Protocols

The federated server must aggregate updates from many devices without learning any individual device's update (which could leak information about that device's data).

**Secure multi-party computation (SMPC):**
- Devices encrypt their updates so the server can compute the aggregate (sum) without seeing individual values.
- The server learns only the aggregated result, not the contributions.
- Overhead: cryptographic computation and communication rounds.

**Homomorphic encryption:**
- Updates are encrypted such that aggregation can be performed on ciphertext, producing an encrypted aggregate that only the holder of the decryption key can read.
- Heavier compute overhead than SMPC.

**Trusted execution environments (TEEs):**
- Aggregation runs inside a hardware-attested secure enclave (e.g., Intel SGX, AMD SEV, ARM TrustZone, Apple Secure Enclave).
- The server cannot inspect data inside the enclave.
- Requires trust in the hardware manufacturer and the attestation chain.

### Differential Privacy for Federated Updates

Even with secure aggregation, the aggregated update can leak information about individual participants (membership inference attacks, gradient analysis). Differential privacy (DP) provides a formal guarantee.

**The mechanism:**
1. Each device clips its update to bound the maximum contribution of any single participant (limiting individual influence).
2. Calibrated Gaussian or Laplacian noise is added to the aggregate.
3. The noise scale is determined by the privacy budget ε (lower ε = more noise = more privacy = less utility).
4. A privacy accountant tracks cumulative ε across rounds; the budget is consumed over the lifetime of the federation.

**The tradeoff:** More noise = stronger privacy guarantee but slower convergence and lower final model quality. The budget must be allocated across rounds (composition) — early rounds can spend more, later rounds conserve.

**Practical budget:** ε in the range of 1-10 is common for federated learning applications; lower ε (stronger privacy) typically requires more rounds or more users to maintain utility.

### The Non-IID (Data Heterogeneity) Challenge

In real federated deployments, user data is not independent and identically distributed:
- Different users have different vocabularies, domains, coding styles, learning patterns.
- Local updates trained on non-IID data diverge from the global optimum (client drift).
- Naive averaging of drifted updates produces a poor global model.

**Mitigation techniques:**

| Technique | Mechanism | Tradeoff |
|---|---|---|
| **FedProx** | Proximal term in local loss penalizes drift from global model | Reduces personalization; under-fits diverse users |
| **Personalized FL** | Each user keeps a personal component; only a shared component is aggregated | Best privacy + personalization; more architectural complexity |
| **Clustering** | Group similar users; aggregate within clusters | Requires cluster structure; cluster membership is itself sensitive |
| **Few local epochs** | Limit local training epochs before aggregation | More communication rounds; slower convergence |
| **Adapter-only federation** | Federate only the small adapter; keep base frozen | Small updates; personalization preserved in residual adapter |

**The personalized-FL approach** (per-user adapter + shared component) is the natural fit for privacy-first personalization: the user keeps their personal adapter (maximally personal, never shared), and only a shared component is aggregated (community benefit). This is the architecture that satisfies both privacy and personalization simultaneously.

## The On-Device Personalization Stack (Three Layers)

### Layer 1: Frozen Base Model
- Shipped to every device; never changes after deployment (except version updates).
- Provides general capability without any user data.
- Open-source essential: auditable, verifiable, no hidden behavior.
- 2026 options: Phi-3/4, Gemma-2, Qwen-2.5 small variants, Llama 3.x small, BitNet 1-bit models — all capable enough for many personal tasks at 1-7B parameters.

### Layer 2: Personal Adapter / LoRA Module
- Fine-tuned on-device using the user's own data (code, notes, interactions).
- Captures user-specific patterns: vocabulary, style, domain knowledge, preferred format.
- Small (MB-scale); the only component that is (optionally) shared for federation.
- User has full sovereignty: export, inspect, delete.

### Layer 3: Local Retrieval + User-Specific Decoding
- Local vector store (RAG) over the user's documents/code/context — queried at inference, never sent to a server.
- User-specific decoding parameters (temperature, system prompt, repetition penalty).
- Most personal and most privacy-sensitive layer; stays entirely on-device.

## Edge-vs-Cloud Compute Economics (2026)

| Dimension | On-Device | Cloud |
|---|---|---|
| Inference latency | Near-zero (no network) | Network round-trip |
| Per-query cost | Free (hardware already paid) | Per-token inference cost |
| Privacy | Data never leaves device | Data sent to provider |
| Model size ceiling | Limited by device RAM/GPU (1-7B practical) | Effectively unlimited |
| Personalization | Native (local data, local adapter) | Requires shipping user data to cloud |
| Offline capability | Full | None |
| Update distribution | Push base + global adapter | Provider updates centrally |
| Regulatory posture | Data minimization; on-device processing generally lower-risk | Data controller/processor obligations; cross-border transfer issues |

**The 2026 shift:** On-device foundation models have crossed the capability threshold for many personal tasks (coding assistance, writing, learning, summarization). Combined with local-first architecture, on-device is now the cost-effective AND privacy-effective choice — not merely the privacy-conscious compromise.

## Regulatory Alignment

**GDPR data minimization (Art. 5(1)(c)):** On-device processing with no data leaving the device is the strongest data-minimization posture. Federated aggregation with differential privacy provides a formal, auditable privacy guarantee.

**EU AI Act:** On-device personal AI assistants for individuals are typically lower-risk than general-purpose cloud AI systems. The open-source model is auditable; transparency obligations are lighter; the user's data never leaves their control.

**Consent-free on-device processing:** Processing that happens entirely on the user's device, on the user's own data, for the user's own benefit, generally does not require the same consent infrastructure as cloud processing of personal data. This is a regulatory advantage (the user's data never leaves their control), not a loophole.

**Data subject rights:** Export (portability), delete (erasure), inspect (transparency) are all native because the adapter and RAG live on the user's device.

## Privacy-Utility-Efficacy Tradeoff Summary

| Mechanism | Privacy Benefit | Utility Cost |
|---|---|---|
| Local-only training (no federation) | Maximum — no updates leave device | No community benefit; isolated models |
| Federated with secure aggregation | Server cannot see individual updates | Compute/bandwidth overhead; doesn't stop membership inference alone |
| Differential privacy noise | Formal ε-bound guarantee | Noise degrades quality; more noise = less accuracy |
| Adapter-only federation | Base never changes; small attack surface | Limited expressivity vs full fine-tuning |
| On-device inference only | No data leaves device at all | No cloud compute; limited to device capability |

**A-Tech default:** On-device inference + local adapter personalization + opt-in federated aggregation with differential privacy. Users who want community benefit opt in; users who want maximum privacy keep their adapter local.

## Source Bibliography

- LoRA: Hu E.J. et al. "LoRA: Low-Rank Adaptation of Large Language Models." ICLR 2022.
- PEFT survey: Liu X. et al. "Few-Shot Parameter-Efficient Fine-Tuning is Better and Cheaper than In-Context Learning." arXiv 2205.05638.
- Federated learning foundational: McMahan H.B. et al. "Communication-Efficient Learning of Deep Networks from Decentralized Data." AISTATS 2017 (FedAvg).
- FedProx: Li T. et al. "Federated Optimization in Heterogeneous Networks." MLSys 2020.
- Secure aggregation: Bonawitz K. et al. "Practical Secure Aggregation for Privacy-Preserving Machine Learning." CCS 2017.
- Differential privacy in FL: Geyer R.C., Klein T., Nabi M. "Differentially Private Federated Learning: A Client Level Perspective." arXiv 1712.07557.
- Federated LLMs survey: "Federated Large Language Models: Current Progress and Future Directions." arXiv 2409.15723 (updated June 2026).
- Personalized FL: Tan A.Z. et al. "Towards Personalized Federated Learning." IEEE TPDS 2022.
- Non-IID challenge: Zhao Y. et al. "Federated Learning with Non-IID Data." arXiv 1806.00582.
- On-device foundation models / edge AI: IBM "Foundation models at the edge" (2025-2026); Apple on-device AI architecture (2024-2026).
- Gboard federated learning: Google AI Blog, "Federated Learning: Collaborative Machine Learning without Centralized Training Data" (2017) and subsequent Gboard updates.

## A-Tech-Specific Extensions

These extensions apply the framework to A-Tech's open-source + privacy-first + financial-freedom values:

- **Open-source reference architecture:** A-Tech should publish an open-source reference implementation of the federated LLM on-device personalization stack (open base model, adapter spec, secure aggregation protocol, DP budget, opt-in federation, adapter sovereignty). This is the open-source alternative to cloud personalization.
- **Adapter sovereignty as data sovereignty:** The user can export, inspect, and delete their adapter — making the personalization layer fully under user control, the architectural expression of A-Tech's data-sovereignty value.
- **Opt-in federation as consent:** Federation is opt-in, not mandatory. The user chooses to contribute their (DP-noised) adapter updates to the community model. Withdrawing is always possible.
- **On-device as the superior architecture (2026):** On-device is not the privacy compromise; it is the superior architecture when on-device foundation models have crossed the capability threshold. Privacy and quality converge rather than trade off.