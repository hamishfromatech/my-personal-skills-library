---
name: lt-code-gradient-inversion-batch-recovery
description: Applies "Cascading Gradient Inversion via LT-Code Inspired Peeling in Federated Learning" (Saeed Shariati & Mohsen Alambardar Meybodi, arXiv:2609.09659, Sept 2026) as the LT-CODE-GRADIENT-INVERSION pattern — framing gradient inversion as erasure-correcting-code decoding lets passive attackers exceed prior analytic-attack upper bounds: exact batch recovery with labels from a single FedSGD round, certified without ground truth; a passive attacker observing an honestly trained network recovers 94–100% of ImageNet batches up to size 128, and >90% at batch sizes of several hundred in the active setting. Federated learning's privacy leakage has been materially underestimated. Use when assessing FL deployment risk, designing gradient-protection stacks, or evaluating batch-size privacy claims. NOT for [secure-aggregation topology attacks (use topology-betrays-privacy-sa-attack), DP noise mechanism design, or backdoor/poisoning defense (orthogonal threat class)].
---

# LT-Code Gradient Inversion: Batch Size Is Not Privacy

## Overview
Shariati & Meybodi (arXiv:2609.09659, Sept 2026) establish a formal connection between **gradient inversion and erasure-correcting codes**, and use it to build attacks that beat the known upper bounds on single-round analytic reconstruction. The reframe: a batch gradient is a set of sum-shares of per-sample gradients; decoding a set of noisy aggregate observations is structurally the **peeling decoder of an LT/fountain code** — identify and subtract recoverable components, propagate, repeat. The result is not incremental: on eight image and tabular benchmarks, the attacks recover **batches exactly, with every sample's label, from a single FedSGD round**, certifying each recovery without ground-truth data. Even a **passive attacker** who only observes an honestly trained network recovers **94–100% of ImageNet batches at sizes up to 128** — more than prior single-round attacks achieved even with active model manipulation — and **>90% at batch sizes of several hundred** in the active setting. The paper's verdict: "the privacy leakage of federated learning has been underestimated."

## The Evidence Base
- **Prior bounds:** analytic (closed-form) inversion degrades sharply with batch size; prior single-round attacks recovered only ~half a batch of size 100 even with full parameter control; known upper bounds capped what such methods could recover.
- **The LT-code construction:** treat the batch as source symbols and the gradient (plus controlled variants) as fountain-code shares; peeling recovers samples whose contributions can be isolated, subtracts them, and cascades — erasing the "grows-impossible-with-batch-size" prior.
- **Results:** exact batch + label recovery from one FedSGD round, certified recovery without ground truth; passive/honest-network setting 94–100% at batch ≤128 (ImageNet); active setting >90% at several hundred; outperforms prior single-round attacks "by a wide margin" on all eight benchmarks.

## Core Findings (the batch-recovery pattern)
1. **Batch size is not a privacy mechanism.** The operational comfort that "big batches make inversion impractical" is now formally false — decoding, not search, breaks the scaling wall.
2. **Passive beats prior active.** Observing an honestly trained network suffices for near-total recovery; no model poisoning, no parameter control, no insider access required. Threat models that assume cooperation are miscalibrated.
3. **Certified recovery changes the audit economics.** Self-verifying exactness (without ground truth) makes this a measurable leakage bound for any deployment, not a best-case demo.
4. **The defense stack must be compositional.** Gradient-level defenses (clipping, noise, secure aggregation) must be evaluated jointly — DP noise (per the DP-FL family), secure aggregation topology (per the SA-attack skill), and now batch-composition exposure are separate surfaces that compose.

## When to Use
- Threat-modeling any FL or FedSGD deployment before production: state the achievable batch-recovery bound under passive observation, not the marketing "raw data never leaves."
- Sizing gradient-protection stacks (clipping + noise + secure aggregation) — the attack defines what each layer must survive alone and composed.
- Evaluating vendor privacy claims that cite batch-size defenses: ask for passive-setting recovery numbers, not active-control best cases.

## NOT For
- Decentralized-FL secure-aggregation topology attacks (held by `topology-betrays-privacy-sa-attack` — different surface: neighborhood aggregate views).
- DP-SGD design and composition accounting (held by the DP-FL optimizer family; this is the attack that motivates them).
- Backdoor and poisoning defense — Ring and FDBA-class threats are orthogonal (integrity vs confidentiality).

## Core Process / Workflow
1. **Classify the exposure.** Single-round gradients? FedSGD or FedAvg with large local batches? Distributed settings change the share structure and the attack surface.
2. **Assume the passive adversary.** Model the attacker as an honest-network observer (defender-cooperative deployment) — that is the setting where recovery already hits 94–100% at batch ≤128.
3. **Layer the defenses and re-audit.** Compositional stack: DP clipping + calibrated noise (per-iteration), secure aggregation, topology hardening; verify each layer's marginal leakage reduction under the LT-code attack class.
4. **Certify with ground-truth-free checks.** Adopt the paper's certification discipline in red-team tests: recovery claims must be checkable without access to the true batches.
5. **Re-derive batch-size policy.** Treat batch size as a latency/utility knob only; document that it confers no privacy credit in the threat model.

## A-Tech Alignment
- **Privacy/sovereignty:** the honest accounting for Sovereign Data Center in a Box and any sovereign-FL narrative — raw data locality is real, but update leakage is total without a composed defense; the skill gives A-Tech's privacy content its strongest "underestimated" citation.
- **Open source:** the attack targets an open protocol (FedSGD) with public code; the defense conversation belongs in the same open forum, not vendor slideware.
- **Practical:** the code-theory framing is a reusable teaching device in A-Tech's FL-privacy content — the peeling decoder explains why prior "impractical at scale" intuition failed.

## Honesty Caveats
- Image and tabular benchmark results; NLP and large-model FL settings are not instantiated.
- The 94–100% figures are the paper's own benchmarks; independent replication not yet published.
- Defense evaluation is limited — the paper quantifies leakage, not a recommended minimum viable defense stack.
- FedSGD-specific; local-update (FedAvg) variants with multiple local steps change the observation model.

## Pairs-with
`topology-betrays-privacy-sa-attack` (the SA topology surface — complementary attack), `dp-sgd-family` skills (the noise defense), `federated-learning-for-privacy-preserving-ai`, `veridp-verifiable-dp-sgd`, `zk-proof-federated-learning-trust` (the cryptographic-enforcement complement), `slaclip-adaptive-clipping-dp-sgd`.

## References
- Saeed Shariati, Mohsen Alambardar Meybodi (arXiv:2609.09659, Sept 2026), "Cascading Gradient Inversion via LT-Code Inspired Peeling in Federated Learning."
- Prior bounds: single-round analytic inversion literature; Zhu et al. Deep Leakage from Gradients (2019) as the lineage anchor.

*Created: 2026-09-17 (Cycle 28, Run 3) — A-Tech Research Division*