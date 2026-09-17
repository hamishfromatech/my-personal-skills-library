---
name: fedex-lora-exact-federated-aggregation
description: Applies Chorus (varmabudharaju, Apache-2.0, pip chorus-fl, v0.2.0) — an open-source federated LoRA fine-tuning framework implementing FedEx-LoRA exact aggregation with SVD residual folding, opt-in stateful DP accounting, and an explicit honest-tradeoffs doc — as the EXACT-AGGREGATION pattern for federated LLM fine-tuning plus the honest-disclosure repo pattern. Use when [advising on federated fine-tuning architecture for LoRA adapters, evaluating DP+FL stacks for privacy-first AI, or teaching the naive-FedAvg-math problem]. NOT for [topology-aware DP noise allocation — use fulcrum-topology-aware-dp — or general FL orchestration/compliance platforms — use the Aegis/Flower family].
---

# Chorus / FedEx-LoRA: Exact Federated LoRA Aggregation, With the Caveats Published

## Overview
Naive FedAvg is **mathematically broken for LoRA**: clients train adapters as W = B@A (two low-rank matrices), and avg(B@A) ≠ avg(B)@avg(A) — so averaging the A and B matrices separately across clients produces an inexact global update. Chorus (single-author, Apache-2.0) implements **FedEx-LoRA** (arXiv:2501.03075, ACL/ICLR 2025), which computes the exact weighted average of full-rank products B_i@A_i, takes the optimal rank-r SVD approximation (Eckart–Young), and **folds the residual into the base weights** — making the combined result exact when folding happens every round. Just as notable for the library: the repo ships a `docs/honest-tradeoffs.md` that states every limitation (opt-in DP accountant, sanity-check-only Byzantine defenses, global API keys, alpha software) in the README itself.

## When to Use
- Designing federated fine-tuning where clients hold private data and share only adapter deltas
- Explaining why naive FedAvg silently degrades LoRA federations (the avg-of-products identity)
- Evaluating whether a privacy-first AI stack needs exact aggregation, per-submission DP, and budget accounting
- Teaching the publish-your-caveats repo pattern as a trust signal in open-source security tooling
- NOT for network-topology DP noise allocation (use fulcrum-topology-aware-dp)
- NOT for production orchestration with RBAC/mTLS/compliance reporting (use the Aegis layer)

## Core Process / Workflow
1. **State the identity failure.** avg(B_i @ A_i) ≠ avg(B_i) @ avg(A_i). Any stack averaging LoRA A/B matrices independently is shipping silent approximation error into the global model.
2. **Apply the fix ladder.** (1) Exact weighted average of full-rank products B_i@A_i; (2) rank-r SVD via Eckart–Young for the approximation; (3) residual tracking; (4) fold residuals into base weights each round — exactness holds only under this default server path (the eval harness does not yet fold; issue #19).
3. **Wire the DP correctly.** Per-submission Gaussian noise with L2 clipping (`--dp-epsilon`) plus a stateful accountant (`--accountant-target-epsilon`, `--accountant-noise-multiplier`) that halts submissions when the budget is exhausted. **Without the accountant flags, privacy loss accumulates unbounded across rounds** — the single most important deployment flag in the repo.
4. **Audit the security claims honestly.** Norm-bound + z-score outlier defenses catch random-noise injection and trivial corruption; they will NOT stop adaptive adversaries (label-flipping, under-the-bound coordinated attacks, gradient inversion). Rate limiting is in-memory and single-process; API keys are global (one server per trust boundary); HTTP requires TLS-terminating reverse proxy.
5. **Adopt the disclosure pattern.** Mirror honest-tradeoffs.md in any privacy/FL tooling: name each claim, the exact condition under which it holds, and the known gap (with issue links). This is the repo-level equivalent of the skills library's honesty caveats.

## Key Evidence
- Repo: github.com/varmabudharaju/chorus (Apache-2.0, Python, pip chorus-fl; created Feb 2026, last push May 2026, 165 tests)
- FedEx-LoRA: arXiv:2501.03075 (ACL/ICLR 2025) — exact aggregation with SVD residual folding
- Mechanisms: per-submission Gaussian DP (server + client paths), RDP composition via Google dp-accounting (Opacus fallback), safetensors-only serialization (no pickle), per-IP rate limiting, WebSocket round notifications
- Honest tradeoffs (docs/honest-tradeoffs.md): exactness conditional on residual folding (#19); DP accountant opt-in (#20 eval-harness gap); Byzantine = sanity checks; global API keys v0.2.0; alpha, single-process FastAPI, filesystem storage, HTTP
- Example run: `chorus server --model meta-llama/Llama-3.2-3B --min-deltas 3` + client SDK submit_delta/pull_latest
- Comparison context: Aegis (compliance-first orchestration layer, Apache-2.0), Fulcrum (topology-aware DP), APPFL (DoE-backed PPFL framework) — the open FL/DP shelf is deepening across governance, topology, and exactness axes

## Pairs with
`fulcrum-topology-aware-dp` (spatial/topological DP axis — composes with Chorus's per-submission axis), `federated-learning-for-privacy-preserving-ai` (the base FL skill), `hybrid-compute-privacy-gate`, `hades-selective-feature-encryption-federated-learning` (selective-encryption counterpart), `privacy-preserving-local-ai`, `dptrainer-drop-in-differential-privacy` (integration-path counterpart), `adaptive-verifiable-federated-learning-2026`.

## A-Tech Alignment
- **Open source**: Apache-2.0, pip-installable, single-maintainer — the exactness fix is adoptable in an afternoon, and the honest-tradeoffs doc is a replicable trust artifact.
- **Privacy**: clients share adapter deltas, never raw data; DP is real but opt-in-accountant — the budget must be configured or it silently accumulates.
- **Financial freedom**: federated LoRA lets small teams pool model improvement without pooling data (and without frontier-lab API costs); exact aggregation removes a silent quality tax.
- **Practical**: four-step deployment checklist (exactness precondition, accountant flags, Byzantine limits, TLS proxy) usable in any FL stack review.

*Source: Chorus repository README + docs/honest-tradeoffs.md (github.com/varmabudharaju/chorus, v0.2.0); FedEx-LoRA (arXiv:2501.03075). Alpha software; author-published caveats; not independently audited.*