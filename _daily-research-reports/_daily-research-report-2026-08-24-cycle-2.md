# Daily Research Report — 2026-08-24 (Cycle 2)

**Date:** August 24, 2026 (Auckland time)
**Research Focus:** Agent-ready API monetization, microservice privacy-preserving FL, LoRA fine-tuning under differential privacy, flow-matching generative FL, majority-collusion-resistant secure aggregation

---

## Executive Summary

Today's research cycle (cycle 2) identified **five high-impact novel developments** across monetization and privacy-preserving federated learning domains, resulting in **five new skills** created. The most significant findings include: (1) Nevermined's comprehensive agent-readiness framework defining the four requirements (discovery, access, commerce, delivery) and five pricing models for making APIs monetizable by autonomous AI agents, with real-world benchmarks from kone's 46,000-advertiser network showing 10-60× CTR improvement over display advertising; (2) MOSAIC-FL's microservice architecture with threshold CKKS homomorphic encryption — the only FL framework with native threshold HE, polyglot microservices, and t-of-N fault-tolerant decryption, validated on genomic cancer subtyping; (3) LA-LoRA's local alternating update scheme solving three previously underexplored challenges in LoRA-DP-FL, achieving +16.83% accuracy improvement over prior SOTA at strict privacy budgets (ε=1); (4) FedFG's novel use of flow-matching generators as a unified backbone for both privacy and robustness in FL — the first framework to do so — outperforming GAN-based approaches with smoother distributions and stronger inversion resistance; (5) FLiPD's majority-collusion-resistant secure aggregation combining MPC and distributed DP noise generation, secure even when the majority of clients collude with the server while matching unprotected FL communication costs.

---

## Skills Created (5 new skills)

### 1. Agent-Ready API Monetization Pattern
**Category:** monetization-and-revenue
**Source:** Nevermined Team (nevermined.ai, August 13, 2026)

**Key Findings:**
- Agent-ready products must satisfy four connected requirements: Discovery (OpenAPI/MCP/A2A), Access (scoped credentials), Commerce (explicit pricing/payment rules), Delivery (completion verification)
- Five monetization models: usage-based, task/workflow, outcome-based, credit-based, hybrid
- Protocol stack: MCP (tool connection) + A2A (agent discovery) + AP2 (delegated authorization) + x402 (HTTP payment) + MPP (machine payment)
- Identity, access, and payment must be separated — they answer different questions
- Autonomous payments require delegated authority with spend caps, time limits, merchant allowlists, immediate revocation
- Meter every chargeable event before scaling: customer/agent ID, resource, timestamp, pricing plan, units, price, payment reference, completion status
- kone network benchmarks: agent CTR 3-6% vs 0.1% display; revenue $30-95 per 1K sessions by vertical; median ARPU $1.25/month, top decile $4+
- Amazon Bedrock AgentCore payments GA: Coinbase + Stripe Privy wallets, x402 + MPP support, "upto" scheme for spending ceilings, per-session spend caps + expiry
- LemonCake: open-core x402 rail, card-funded, no crypto, 3% fee, per-agent identity with kill switch
- Locus: 600+ API billing layer, wholesale pool model with markups, 5-3% platform fee

**Novelty:** While the library contains `agent-native-advertising-economics` (kone network) and various agentic payment skills, no existing skill provides the comprehensive agent-readiness framework with the four requirements, five pricing models, protocol stack selection guide, and implementation patterns for making any API agent-ready.

**A-Tech Alignment:** Open-source (MCP standard, LemonCake MIT SDK, open frameworks), data privacy (scoped credentials, no raw card exposure), financial freedom (sustainable agent monetization), practical implementation (concrete protocol selection, metering templates, pitfalls)

### 2. MOSAIC-FL Microservice Privacy-Preserving FL
**Category:** privacy-and-trust
**Source:** Largillier, Paygambar, Gouy-Pailler, Meyer, Mziou & Stan (CEA Paris-Saclay/CNRGH, arXiv:2607.25107, July 2026)

**Key Findings:**
- Microservice architecture: each federated node = cluster of Orchestrator + ML Engine + Crypto Provider containers with strict separation of concerns
- gRPC + Protocol Buffers enable polyglot implementation (Rust for crypto, Python for ML) — no SDK required
- Threshold CKKS homomorphic encryption (ThHE): t-of-N fault-tolerant decryption, key renewed every round to counter key-recovery attacks
- Finite State Machine synchronizes components and detects/prevents failures
- Lossless accuracy: CKKS at Δ=2^45 matches baseline exactly
- Genomic application: BRCA breast cancer subtyping on TCGA with Transformer model (679k params)
- Performance: 5% overhead for complex training (Transformer), 64% for small models (CNN)
- Only FL framework with native threshold HE + microservices + polyglot support (vs Flower/FATE/PySyft)

**Novelty:** While the library contains `hades-selective-feature-encryption-federated-learning` and `safelm-unified-privacy-llm-framework`, no existing skill covers microservice-based FL architecture with threshold homomorphic encryption, FSM orchestration, and genomic applications.

**A-Tech Alignment:** Open-source (framework architecture, standard libraries), data privacy (threshold HE, no single point of decryption), practical implementation (modular, extensible, real genomic deployment)

### 3. LA-LoRA Privacy-Preserving Federated Large Models
**Category:** privacy-and-trust
**Source:** Liu, Miao, Xi & Liu (ICLR 2026)

**Key Findings:**
- Three previously underexplored challenges in LoRA-DP-FL: gradient coupling (simultaneous A/B updates), compounded noise amplification (DP noise on both matrices), global model sharpness (aggregated parameters create sharp loss landscape)
- LA-LoRA solution: Local Alternating — update A while freezing B, then B while freezing A
- +16.83% accuracy improvement over prior SOTA (RoLoRA) on Swin-B/Tiny-ImageNet at ε=1
- SOTA on both large vision models (Swin Transformer) and large language models (RoBERTa)
- Strengthens convergence guarantees in noisy federated environments
- Minimal implementation overhead — alternating requires_grad flags
- Especially impactful for vision models where LoRA-DP degradation was previously most severe

**Novelty:** No existing skill covers the local alternating update scheme for LoRA under DP-FL, or the three challenges (gradient coupling, noise amplification, model sharpness) it addresses.

**A-Tech Alignment:** Open-source (ICLR publication, code provided), data privacy (differential privacy with strict budgets), practical implementation (minimal code change, compatible with existing DP-FL frameworks)

### 4. FedFG Flow-Matching Generative Federated Learning
**Category:** privacy-and-trust
**Source:** Wang, Pan & Yao (Sun Yat-sen University, arXiv:2603.27986, March 2026)

**Key Findings:**
- First framework to use flow-matching generators as unified backbone for both FL privacy AND robustness
- Architecture: private feature extractor (local) + flow-matching generator (shared, replaces extractor in communication) + public classifier (shared)
- Flow-matching trained via conditional ODE: h(t) transported from noise → real features via vector field v_θ(h,t,y)
- Server-side robustness: synthetic feature probes from global generator → outlier detection (Hellinger distance + Hampel rule) + accuracy-aware reweighting
- >94% accuracy at 30% malicious clients across sign flipping, inner product manipulation, and MPAF attacks
- Lower PSNR/SSIM than GAN-based FedCG on gradient inversion attacks (DLG, IG) — smoother distributions harder to invert
- Maintains ~90% accuracy under non-IID (Dirichlet β=0.5) where baselines collapse to 10%
- Convergence guarantee: O(1/√(QR)) under nonconvex objectives
- Open-source code available

**Novelty:** No existing skill covers flow-matching as a generative backbone for FL, or the unified privacy+robustness approach with synthetic probe verification. GAN-based approaches (FedCG, GAN-Filter) exist but flow-matching provides fundamentally different properties.

**A-Tech Alignment:** Open-source (arXiv, GitHub code), data privacy (split learning with private extractors), practical implementation (works across MNIST/FMNIST/CIFAR10, handles non-IID)

### 5. FLiPD Majority-Collusion-Resistant Secure Aggregation
**Category:** privacy-and-trust
**Source:** Chandran, Önen & Schneider (Simula, EURECOM, TU Darmstadt, IACR ePrint 2026/324, February 2026)

**Key Findings:**
- Combined MPC + DP protocol for FL secure aggregation
- Defends against both inference attacks (via MPC secure aggregation) AND backdoor attacks (via DP noise) simultaneously
- Key innovation: distributed DP noise generation — secure even when majority of clients collude with server
- Client-server communication cost: essentially identical to unprotected FL
- Server-server communication: 11% lower than Prio+ (prior SOTA, Addanki et al. SCN'22)
- Accuracy: 87% (Linear Regression, HAR), 90% (CNN, MNIST) — acceptable privacy-utility trade-off
- Requires at least 2 non-colluding MPC servers

**Novelty:** While `zk-proof-federated-learning-trust` mentions FLiPD in passing, no dedicated skill exists for the majority-collusion-resistant protocol design pattern with distributed DP noise generation.

**A-Tech Alignment:** Open-source (IACR ePrint, CC BY license), data privacy (formal DP guarantees + MPC, majority collusion resistance), practical implementation (communication cost matching unprotected FL removes adoption barrier)

---

## Skills Evaluated but Already Covered (no new skill needed)

The following findings were evaluated but found to be already covered by existing skills:

1. **Vella & Blincoe longitudinal AI coding study** (May 2026) — covered by `productivity-experience-paradox-supervisory-engineering`, `supervisory-engineering-work`, `productivity-experience-paradox-ai-coding`
2. **Catalan et al. "I'm Not Reading All of That"** (CHI 2026) — covered by `agentic-cognitive-engagement-decline`, `developer-ai-cognitive-engagement-decline`
3. **Balepour et al. "(Im)Paired Programming"** (2026) — covered by `coding-agent-comprehension-harm`, `agentic-code-comprehension-decline-empirical`
4. **SafeLM unified privacy LLM framework** — covered by `safelm-unified-privacy-llm-framework`
5. **HADES selective feature encryption FL** — covered by `hades-selective-feature-encryption-federated-learning`
6. **Fed-ADS adaptive privacy-utility trade-off** — covered by `fed-ads-adaptive-privacy-utility-tradeoff`
7. **Cognitive privacy neuromarketing paradox** (Agarwal 2026) — covered by `cognitive-privacy-neuromarketing-paradox`, `neuromarketing-paradox-cognitive-privacy`

---

## Cross-References

- **agent-ready-api-monetization-pattern** ↔ `agent-native-advertising-economics`, `agentic-payment-protocol-convergence-2026`, `mcp-payment-support-specification`, `outcome-based-pricing`, `token-based-ai-pricing-2026`, `real-time-metering-ai-agent-revenue`
- **mosaic-fl-microservice-privacy-preserving-fl** ↔ `hades-selective-feature-encryption-federated-learning`, `safelm-unified-privacy-llm-framework`, `adaptive-verifiable-federated-learning-2026`, `federated-learning-as-a-service-2026`
- **la-lora-privacy-preserving-federated-large-models** ↔ `safelm-unified-privacy-llm-framework`, `dp-fedadamw-dpfl-large-model-optimizer`, `slm-first-monetization-playbook`
- **fedfg-flow-matching-generative-federated-learning** ↔ `federated-byzantine-robust-partial-participation`, `federated-unlearning-cybersecurity-risk`, `adaptive-verifiable-federated-learning-2026`
- **flipd-majority-collusion-resistant-secure-aggregation** ↔ `zk-proof-federated-learning-trust`, `ddp-sa-distributed-dp-secure-aggregation`, `federated-byzantine-robust-partial-participation`

---

## Research Methodology

1. **Scan**: Searched across four primary domains — agentic AI monetization, neuromarketing AI personalization, developer experience with agentic coding, and privacy-preserving federated learning
2. **Synthesize**: Cross-referenced all findings against the existing 565-skill library using filename search and content grep to identify true novelty gaps
3. **Create**: Built 5 new skills with SKILL.md + references/ subdirectories, each under 500 lines in the main file
4. **Update**: Updated README.md index with new skill entries
5. **Report**: Saved this report to `_daily-research-reports/`

Total skills in library after this cycle: **570** (565 existing + 5 new)