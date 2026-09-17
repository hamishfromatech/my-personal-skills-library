---
name: heterogeneous-multi-llm-federated-inference
description: Applies Boyapati et al.'s privacy-preserving heterogeneous multi-LLM federated inference framework (arXiv 2609.02947, Sept 1 2026; EMNLP 2026 Findings) — commercial LLM APIs collaborate without raw student data or model internals; epsilon-LDP via Laplace noise on prediction outputs before aggregation; residual-based aggregation mitigates heterogeneity under an honest-but-curious trust paradigm. Use when [designing privacy-preserving multi-model AI pipelines, evaluating federated inference for education or healthcare, advising on API-based AI without data sharing, or explaining LDP applied to outputs rather than training data]. NOT for [differential privacy in federated *training* — use dp-lac-lightweight-adaptive-clipping or the DP-FL stack — or single-provider API privacy policies].
---

# Privacy-Preserving Heterogeneous Multi-LLM Federated Inference

## Overview
Boyapati, Yu, Jiang & Zhan (arXiv 2609.02947, submitted Sept 1, 2026; accepted to **Findings of EMNLP 2026**) answer a question the library's FL stack never posed: **can commercial LLM APIs collaborate on a diagnosis task without seeing raw records or each other's internals?** Their framework says yes, by moving differential privacy *inference-side* — each federated entity (LLaMA-3.3-70B, GPT-4o-mini, Claude-3-Haiku) adds **Laplace noise locally to its own prediction output** under ε-local differential privacy *before* aggregation, with **residual-based aggregation** mitigating model heterogeneity.

The trust paradigm is explicit: **honest-but-curious** — API providers are presumed not to abuse submitted queries; the DP mechanism shields the *published diagnostic results* from external inference. Rigorous privacy-utility analysis shows strong guarantees with minimal accuracy loss; evaluations across three educational benchmarks confirm practical usability and cross-domain generalizability.

## Why it is a distinct pattern (not another DP-FL paper)
The entire in-library DP-FL stack (XCal-FL, FedGSA, DP-FedAdamW, DP-FedSOFIM, dptrainer, DDP-SA…) protects **training updates** in federated *learning*. This paper protects **inference outputs** in federated *inference* across heterogeneous commercial models — no fine-tuning happens at all. Three separations:

| Axis | DP-FL training stack | This framework |
|---|---|---|
| What is protected | Gradients/model updates | Prediction outputs |
| Where noise lands | During local training | At each entity's output, pre-aggregation |
| Participation | Clients train a shared model | Frozen commercial APIs contribute predictions |
| Trust assumption | Server semi-honest | Each API provider honest-but-curious |

## Key talking points
- **ε-LDP on outputs, not data.** Laplace noise lands on each entity's prediction before aggregation; raw student data and proprietary model internals never leave their owners.
- **Residual-based aggregation** absorbs model heterogeneity (70B open vs 4o-mini vs Haiku differ wildly in calibration) — the combiner learns from residuals, not raw scores.
- **Minimal accuracy loss** across three educational benchmarks; the framework generalizes cross-domain.
- **The honest-but-curious bargain** is the practical privacy posture for API-based multi-model systems: you cannot cryptographically stop a compliant-but-curious provider from logging queries, but you CAN stop the *published result* from enabling external inference — a different and more attainable guarantee than end-to-end cryptographic inference.
- **Design trigger:** when a pipeline mixes commercial APIs that each hold a slice of the problem (multi-model ensembles, agent swarms consulting specialist models) and the aggregate output must not leak any single record's contribution.

## Workflow
1. Frame the task as federated inference: N heterogeneous predictors, no training data pooling.
2. Choose ε; apply local Laplace noise to each entity's prediction output.
3. Aggregate with residual-based weighting; publish only the aggregated, noised result.
4. State the trust paradigm explicitly in the privacy policy — honest-but-curious providers, DP-shielded published outputs.
5. Report the privacy-utility curve (ε vs accuracy loss) as part of the release.

## Pairs with
`xcal-fl-explanation-privacy-calibration` (explanation-fidelity axis of the privacy budget), `dp-merging-geometry-aware-privacy`, `differential-privacy-synthetic-data`, `ai-privacy-interaction-taxonomy` (the interaction-layer privacy lens this extends from training to inference).

## A-Tech alignment
- **Privacy-first AI:** a deployable pattern for "use commercial AI without feeding it raw data" — the exact posture A-Tech's audience asks for.
- **Open source:** the architecture is provider-agnostic; open models (LLaMA) participate alongside commercial ones.
- **Practical implementation:** EMNLP-accepted, evaluated on three real benchmarks — buildable, not theoretical.

*Source: Boyapati, Yu, Jiang & Zhan, "Privacy-Preserving Heterogeneous Multi-LLM Federated Inference for Cognitive Diagnosis," arXiv 2609.02947 (Sept 1, 2026), Findings of EMNLP 2026. 16 pages, 3 figures.*