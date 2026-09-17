---
name: xcal-fl-explanation-privacy-calibration
description: Applies XCal-FL (Khavkin et al., arXiv 2609.03851, Sept 3 2026, Tel Aviv University + Yonsei) — the closed-loop explainability-driven DP noise calibration that makes explanation fidelity a FIRST-CLASS axis of the privacy trade-off, alongside accuracy and ε — improving predictive performance >10% and explanation fidelity up to 5× over static-noise FL on three medical imaging datasets. Use when [designing DP-FL deployments where explanations must remain trustworthy (clinical decision support, regulated imaging, audit-heavy sectors), allocating a privacy budget across accuracy/explainability/safety, or briefing on the XAI↔privacy interaction in federated systems]. NOT for [general FL accuracy methods with no explanation requirement — use the broader FL stack skills — or verifiable-FL integrity mechanisms — use veridp-verifiable-dp-sgd].
---

# XCal-FL: The Privacy Trade-Off Gains a Third Axis

## Overview
XCal-FL (Khavkin, Lee, Jin, Ko & Toch, arXiv 2609.03851, Sept 3 2026) is the first closed-loop, explainability-driven local-training algorithm for cross-silo DP-FL image classification: DP noise is calibrated *during training* from three complementary explainability signals, so the privacy budget buys trustworthy explanations as well as accuracy — with the discovery that explanation fidelity behaves non-linearly where accuracy scales roughly linearly.

## The three calibration signals
1. **Prediction logit variations** — measures causal influence on model confidence.
2. **Counterfactual margins** — captures decision-boundary sensitivity.
3. **Saliency concentration** — quantifies spatial coherence of model attention.

The loop: local training computes all three signals per layer → noise is allocated adaptively → formal DP guarantees are enforced via adaptive privacy accounting (Rényi-DP-style, closed-form).

## Why it matters now
- **The problem it solves is new and quantified:** DP noise distorts learned representations and degrades explanation fidelity — which limits DP-FL *exactly where trustworthy explanations are required* (assistive clinical diagnosis).
- **The headline numbers:** >10% predictive-performance improvement and up to **5× explanation fidelity** over static-noise FL across three medical imaging datasets; outperforms SOTA adaptive-DP baselines on fidelity.
- **The conceptual shift:** explanation fidelity is a **distinct dimension of the privacy trade-off that cannot be inferred from utility alone** — a DP-FL deployment can hit its accuracy target while silently destroying the explanations clinicians rely on.
- **Budget implications are asymmetric:** accuracy scales roughly linearly with privacy loss; **explanation fidelity does not** — non-linear dynamics mean a small ε change can swing explanation quality drastically. Privacy budgets must be allocated across three axes: accuracy, explainability, safety.

## The four-step design workflow
1. **Confirm the three signals** map to your modality: logit variation, counterfactual margin, saliency concentration — for imaging they are spatial; for text, adapt (e.g., attention concentration).
2. **Compute per-layer saliency during training** — the calibration loop is local, so clients adapt noise before sending updates; the server stays standard (FedAvg-compatible aggregation).
3. **Enforce formal DP accounting** — adaptive privacy accounting tracks cumulative (ε, δ) so guarantees are auditable, not vibes.
4. **Evaluate the three axes separately** — report accuracy, explanation fidelity, and ε-consumption as independent metrics. A model that wins on accuracy alone is not deployable in decision-critical settings.

## A-Tech alignment
- **Open source:** the algorithm operates on standard FL infrastructure (any FedAvg-compatible stack); the three signals are computable with open tooling (logit hooks, saliency maps, counterfactual margin estimation).
- **Privacy:** this is the core DP design pattern of A-Tech's privacy-first pipeline — now extended from "does the noise protect data" to "does the noise also protect the *decision quality* of the explanations."
- **Financial freedom:** the 5× fidelity improvement is a moat argument for privacy-first AI startups — DP-FL explanations that clinicians/auditors can trust are a paid differentiator over DP-FL competitors shipping degraded explanations.
- **Practical:** the three-signal calibration loop is teachable in one video segment; the "three-axis privacy budget" framing is a reusable slide.

## Honest caveats
- Evaluation is image classification on three medical imaging datasets — generalization to text/LLM workloads is a research question, not a finding.
- The 5× fidelity figure is relative to *static-noise FL*; absolute explanation-quality metrics are dataset-specific.
- Adaptive accounting adds compute; the paper reports closed-form guarantees, but per-layer calibration loops are heavier than static clipping.
- XCal-FL optimizes fidelity of the explanation method it calibrates for — it does not make every XAI method trustworthy under DP.

## Pairs with
`adaptive-dp-fl-concept-drift-edge` (edge DP-FL), `fed-sa`-style adaptive clipping skills, `privacy-preserving-ai-attribution-framework` (attribution under privacy), `safelm-unified-privacy-llm-framework` (LLM-side), `chain-of-thought-ux-reasoning-transparency` (the UX-side explanation obligation).

## References
- See [references/xcal-fl-evidence-base.md](references/xcal-fl-evidence-base.md) for the abstract, method summary, and the three medical-imaging benchmark context.