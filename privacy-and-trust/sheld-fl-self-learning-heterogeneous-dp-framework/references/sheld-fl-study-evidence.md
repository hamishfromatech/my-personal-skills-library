# SHELD-FL Study Evidence Base

This file holds the source abstract and the broader adaptive-DP-FL research context supporting the `sheld-fl-self-learning-heterogeneous-dp-framework` skill.

## Source 1 — SHELD-FL (Springer Nature, Complex & Intelligent Systems, 2026)

**Title:** "Privacy-adaptive end-to-end federated learning framework with self-learning differential privacy and personalized optimization for secure healthcare intelligence"
**DOI:** 10.1007/s40747-026-02266-8
**Journal:** Complex & Intelligent Systems (Springer Nature)
**Domain:** Electronic health records from simulated blood-bank distributed institutions

### Abstract (extracted from the published page)

The increasing need for secure and accurate analysis of sensitive healthcare data across distributed sources such as blood banks has amplified the adoption of Federated Learning (FL), which allows collaborative training without the distribution of raw data. While FL helps address data silos and ensures a degree of privacy, studies reveal that gradient updates still leak sensitive information, posing risks of data reconstruction. Although differential privacy techniques have been introduced to mitigate such threats, uniform noise injection often compromises learning performance. To overcome these limitations, this research proposes a novel privacy-adaptive framework titled Self-learning Heterogeneous-based privacy-Enhanced end-to-end Learning Design for Federated Learning (SHELD-FL). The framework integrates self-learning privacy budgeting to dynamically allocate privacy budgets based on gradient sensitivity, and heterogeneous differential privacy to vary noise levels per client according to data sensitivity, achieving a better privacy-utility balance. A gradient boosting classifier is used at the client side to enhance classification under non-IID conditions, and the Builder Optimization Algorithm (BOA) is employed at the server to optimize noise regulation during aggregation. Experimental results on electronic health records from simulated blood banks demonstrate that the proposed SHELD-FL framework achieves a high classification accuracy of 98.39% under a strong privacy setting (ε = 10), outperforming baseline approaches by 3–5%. Moreover, the framework reduces communication latency by approximately 40%, indicating its efficiency and scalability in real-world federated environments. These findings confirm that SHELD-FL offers a reliable, adaptive, and privacy-preserving solution for secure and collaborative healthcare data analysis across distributed institutions.

### Mechanism summary (from the abstract + paper metadata)

1. **Self-learning privacy budgeting** — dynamically allocates privacy budget (ε) per training round based on gradient sensitivity (gradients that leak more sensitive information receive more privacy budget / more noise).
2. **Heterogeneous differential privacy** — varies noise levels per client according to that client's data sensitivity, rather than applying a single global noise level.
3. **Client-side gradient boosting classifier** — enhances classification accuracy under non-IID (non-independent-and-identically-distributed) client data conditions.
4. **Server-side Builder Optimization Algorithm (BOA)** — optimizes noise regulation during aggregation.

### Reported results

- Classification accuracy: 98.39% at ε = 10 (strong privacy)
- Outperforms baseline (uniform DP-FL) by 3-5%
- Communication latency reduced ~40%
- Validated on EHRs from simulated blood-bank distributed institutions

### Source quality notes

- Published in a peer-reviewed Springer Nature journal (Complex & Intelligent Systems). The abstract is publicly available; full text is paywalled (could not extract the full methods section).
- The gradient boosting classifier + BOA details are summarized in the abstract; the precise algorithms for self-learning budgeting and BOA could not be fully extracted without the full text. The skill's mechanism descriptions are faithful to the abstract; implementation-level detail should be verified against the full paper before production deployment.
- Single-domain validation (healthcare EHRs, simulated blood banks). Generalization to finance/IoT/NLP is plausible but not yet empirically demonstrated.
- Publication year 2026; independent production replication is still emerging. Treat the 98.39% / -40% latency figures as the authors' reported result on their benchmark, not as a generalized guarantee.

## Broader context — the adaptive-DP-FL research landscape (2026)

SHELD-FL is part of a 2026 wave of adaptive-DP-FL research that moves beyond uniform noise. Related work already captured in the skill ecosystem:

- **SlaClip** (ICML 2026 Spotlight, University of Southampton / Guangzhou University) — adaptive gradient *clipping* for DP-SGD that extracts a free "Slack Indicator" (noise-perturbed binned CDF of gradient norms) from the clipping operation itself, requiring zero additional privacy budget. Solves the *clipping threshold* problem; SHELD-FL solves the *per-client noise and per-round budget* problem. The two are orthogonal and composable. (See `slaclip-adaptive-clipping-dp-sgd`.)
- **DMA — Delayed Momentum Aggregation** (ICML 2026, OIST) — Byzantine-robust FL with partial participation via a memory-cache aggregation mechanism. Solves the *adversarial-client and partial-availability* problem; SHELD-FL solves the *privacy-utility* problem. Composable: DMA for aggregation integrity, SHELD-FL for privacy budgeting. (See `federated-byzantine-robust-partial-participation`.)
- **Differentially Private Federated Learning: A Systematic Review** (ACM, 2026) — surveys the field; DP has become the de facto standard for privacy protection in FL due to its rigorous mathematical guarantees. The review notes the privacy-utility tradeoff as the central open problem that adaptive methods (like SHELD-FL) address.
- **Aggregation Techniques in Federated Learning Under Differential Privacy** (TU Wien thesis, 2026) — surveys aggregation techniques under DP, including the move toward multi-layered and adaptive privacy-preserving frameworks.
- **RDP-based privacy-adaptive FL** (multiple 2026 works) — the Rényi Differential Privacy (RDP) framework is often used to better manage privacy loss across multiple training rounds, a related but distinct approach to SHELD-FL's self-learning budgeting.

### The pattern across the 2026 wave

The convergent insight: **privacy budget should follow risk, not be spread evenly.** Whether the mechanism is adaptive clipping (SlaClip), per-client noise (SHELD-FL), memory-augmented aggregation (DMA), or RDP-based multi-round budgeting — the field is moving from static, uniform privacy parameters to dynamic, sensitivity-aware ones. This is the same principle that drives A-Tech's privacy-first design philosophy: privacy is not a fixed overhead, it is a budget spent where the risk is.

## Cross-references to existing skills

- **federated-byzantine-robust-partial-participation** — DMA; composable with SHELD-FL (robustness + privacy)
- **slaclip-adaptive-clipping-dp-sgd** — adaptive clipping; orthogonal and composable with SHELD-FL's per-client/per-round budgeting
- **federated-learning-for-privacy-preserving-ai** — general FL overview; SHELD-FL is a specific advanced algorithm
- **federated-learning-as-a-service-2026** — the business model; SHELD-FL is the algorithm that makes the service more accurate under strong privacy
- **federated-llm-on-device-personalization** — on-device LLM; SHELD-FL's heterogeneous-DP pattern applies if per-client data sensitivity varies
- **differential-privacy-synthetic-data** — DP for synthetic data; SHELD-FL's adaptive budgeting principle is transferable
- **bitnet-on-device-training-framework** — tiny on-device models; the client-side gradient boosting classifier in SHELD-FL could be a BitNet-scale model on constrained devices
- **privacy-first-competitive-differentiator** — SHELD-FL strengthens the privacy-first positioning with a concrete accuracy-under-strong-privacy result