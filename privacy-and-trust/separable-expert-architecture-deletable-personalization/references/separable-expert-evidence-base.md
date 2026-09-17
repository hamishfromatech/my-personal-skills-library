# Separable Expert Architecture — Evidence Base

## Primary source

Schneider, C., Schoenegger, P., Bariach, B. (2026). "Separable Expert Architecture: Toward Privacy-Preserving LLM Personalization via Composable Adapters and Deletable User Proxies." Microsoft AI. arXiv:2604.21571.

## Abstract (key claims)

- Current training approaches incorporate user information directly into shared weights, making individual data removal computationally infeasible without retraining.
- SEA is a three-layer architecture that decouples personal data from shared weights by combining a static base model, composable domain-expert LoRA adapters, and a per-user proxy artifact whose deletion constitutes deterministic unlearning.
- Evaluation on Phi-3.5-mini and Llama-3.1-8B confirms per-user differentiation in which personal data influences outputs while remaining isolated.
- Return to baseline after proxy removal: KL ≈ 0.21 nats, 82–89% verification pass rate.
- Near-zero cross-user contamination.
- Because user-specific information never enters shared weights, the architecture mitigates model inversion, membership inference, and training-data extraction against shared model components by construction.
- Converts machine unlearning from an intractable weight-editing problem into a deterministic deletion operation.
- Compatible with DP-SGD for privacy-preserving shared model improvement.

## The fundamental tension

As LLM personalization becomes widely used, user preferences are encoded into model weights θ via fine-tuning, producing models whose parameters entangle contributions from many users. When a user requests deletion:
- **Exact unlearning (SISA):** requires maintaining independently trained model shards — impractical at scale
- **Approximate unlearning:** offers no formal removal guarantees
- **LLM-specific gradient ascent:** can cause catastrophic collapse
- **Representation-level (RMU):** still modifies shared weights

Compounded by extraction attacks (model inversion, training data extraction, membership inference) that can recover private information from weight-encoded personalization — making it a privacy issue even absent deletion requests.

SEA's solution: prevent entanglement from occurring in the first place. If user-specific information never enters shared weights, "unlearning" is just deletion. Personalization must be *compositional* (assembled at inference from separable, deletable components), not *absorptive* (baked into shared parameters).

## Architecture specification

### Design Invariant (Separation)

> All user-specific information resides in an isolated, deletable proxy artifact. Shared model components (base model and expert adapters) contain no user-identifying information. Removing the proxy artifact is both necessary and sufficient for complete user data removal from the inference system.

Structural, not statistical. Approximate unlearning provides probabilistic guarantees; SEA guarantees architectural absence by construction.

### Three composition layers

**Base Layer:** Frozen, quantized LLM (Phi-3.5-mini-4bit, Llama-3.1-8B-4bit). Shared across all users. Never modified during user interactions. Periodic retraining on aggregated data with DP-SGD is a natural extension (out of scope).

**Expert Layer:** Bank of k domain LoRA adapters E = {E_1, ..., E_k}, each E_i = (B_i, A_i) trained on curated domain corpora (rank 32, α=64, all attention projections). Shared across all users. At inference:
```
W_expert = W_base + Σ_i w_i · B_i · A_i   (w ∈ Δ^k from per-query router)
```

**User Layer:** Per-user proxy P_u, isolated directory (~2–5 MB):
1. **Routing bias** b_u ∈ R^k: EMA from simulated interaction patterns (λ=0.5). Applied as scaled additive with clamp-and-normalize.
2. **Contrastive steering vectors** {s_u^ℓ} at layers ℒ = {12, 16, 20}: CAA from trait-aligned preference pairs (γ=1.0). Injected additively into residual stream.
3. **Personal LoRA** L_u = (B_u, A_u): rank-4 (deliberately small), trained via DPO on preference pairs (base model as reference). Only personal LoRA receives gradients during DPO — base + expert weights frozen.

### Inference pipeline (5 stages)

1. Route → 2. Bias → 3. Merge → 4. Steer → 5. Generate

### Deletion protocol (3 steps)

1. **Verify:** Omission-mode generation on held-out domain-generic prompts; KL divergence vs cached non-personalized baseline ≤ max(2·σ̂_KL, τ_min=0.15)
2. **Delete:** Secure filesystem removal of proxy directory (zero-overwrite)
3. **Audit:** Log deletion event, verification, timestamp

Architectural equivalence: omitting proxy at inference = deleting it. Verify step confirms deletion behavior before irreversible delete. KL verification is a sanity check on the architectural guarantee, not the guarantee itself.

## Experimental setup

- **Models:** Phi-3.5-mini-instruct (3.8B, 4-bit NF4), Llama-3.1-8B-Instruct (4-bit NF4) via QLoRA
- **Experts (k=4):** Security (~76K examples), Code (~50K), Data (synthetic text-to-SQL), General (Alpaca ~52K)
- **User profiles (4):** security_expert, casual_coder, data_analyst, general_user — each with domain affinity weights + positive/negative style traits
- **Router:** BART-MNLI zero-shot entailment + keyword fallback (softmax T=2.0)
- **Evaluation:** 70 runs per model (140 total), 20 prompts (5 per domain), 7 bypass observations per run. 95% CIs via t-distribution.

## Results

### Claim 1 — Personalization

| Metric | Phi-3.5-mini | Llama-3.1-8B |
|---|---|---|
| Weight shift | 0.052 ± 0.002 | 0.088 ± 0.003 |
| Jaccard similarity to baseline | 0.236 ± 0.005 | 0.316 ± 0.005 |
| Style trait match | 1.710 ± 0.101 | 0.629 ± 0.040 |

Moderate-to-strong personalization without touching shared weights. Security expert profile produces strongest signal (mean 3.01 on Phi, individual observations reaching 12). Deliberately moderate scope — consequence of rank-4 personal LoRA (price of deletability).

### Claim 2 — Separability

| Metric | Phi-3.5-mini | Llama-3.1-8B |
|---|---|---|
| Verified pass rate | 0.819 ± 0.035 | 0.892 ± 0.028 |
| KL divergence | 0.217 ± 0.012 | 0.212 ± 0.006 |

Bimodal KL distribution: verified in [0.00, 0.30], failures in [0.30, 0.94], no ambiguous intermediate population — consistent with structural guarantee. Deletion is deterministic and complete (proxy files removed, shared weights untouched). KL verification is separate stochastic measurement; 11–18% failures reflect generation variance, not residual user influence.

Threshold sensitivity (pass rate by σ multiplier):

| Multiplier | Phi-3.5-mini | Llama-3.1-8B |
|---|---|---|
| 1.0σ | 0.239 | 0.167 |
| 1.5σ | 0.513 | 0.600 |
| 2.0σ (chosen) | 0.819 | 0.892 |
| 2.5σ | 0.929 | 0.984 |
| 3.0σ | 0.971 | 0.994 |

Empirical noise floor σ̂_KL ≈ 0.15 stable across query-user pairs — consistent cross-query, cross-user, cross-model (not guaranteed by architecture; empirical finding).

### Claim 3 — Isolation

| Metric | Phi-3.5-mini | Llama-3.1-8B |
|---|---|---|
| Contamination | 0.009 ± 0.002 | 0.049 ± 0.005 |
| Cross-user similarity | 0.271 ± 0.010 | 0.351 ± 0.007 |

Cross-user similarity is structural (shared base + experts), not leakage. Proxies are isolated filesystem artifacts with no shared mutable state.

## Limitations and future work

1. **Synthetic user profiles:** placeholders for real preferences; four profiles aligned to four domains = easiest isolation test. Overlapping-domain profiles (two security users, different style) would be harder.
2. **Metrics:** Jaccard/keyword capture basic overlap, not subjective personalization quality.
3. **Scale:** 3.8–8B evaluation not intended to generalize to larger models (though invariant holds by construction regardless of scale).
4. **No ablation:** contribution of each proxy component (routing bias, steering, personal LoRA) not individually isolated.
5. **Proxy attack surface:** exfiltrating a single directory vs extracting user influence from distributed weights. For open-source base models, exfiltrated proxy loads directly against local copy — non-transferability is a hypothesis requiring validation, not a default assumption. Encryption at rest, access controls, retention policies are deployment requirements.
6. **Dual-use:** same deletion mechanism can remove other content or proprietary knowledge — compliance auditing implications.
7. **Expert convergence:** loss plateaus not reached; additional training could improve adapter quality.

### Immediate extension: DP-SGD

Applying DP-SGD to gradient aggregation when updating shared expert adapters from user interaction data. Architecture supports by construction. Open questions: per-sample gradient clipping overhead, privacy budget exhaustion under sequential composition, utility degradation in low-ε regimes, whether privacy amplification holds (Poisson subsampling, bounded sensitivity, composition theorems) — requires empirical attack validation.

## Cross-references to adjacent skills

| Adjacent skill | Relationship |
|---|---|
| `opal-private-memory-architecture` | Opal protects memory content and access patterns via ORAM/TEE. SEA is the personalization-weight layer — it could sit on top of Opal's memory infrastructure. |
| `privacy-preserving-ai-attribution-framework` | Attribution uses FL+DP+HE for a different problem (tracing output to training data). SEA is the personalization layer with deterministic deletion. Complementary. |
| `ai-agent-memory-architecture` | Infrastructure memory layer (benchmarks, multi-signal retrieval, OpenMemory MCP). SEA is the privacy-preserving personalization layer above it. |
| `federated-llm-on-device-personalization` | Federated personalization (PFL + LLMs). SEA is a non-federated alternative with deterministic deletion — simpler, stronger deletion guarantee, no communication overhead. |
| `privacy-first-competitive-differentiator` | Business case for privacy-first positioning. SEA operationalizes that positioning with a structural deletion guarantee. |
| `local-first-web-architecture-2026` | Local-first architecture. SEA's per-user proxy is a local-first artifact (filesystem directory, portable, deletable). |
| `post-quantum-privacy-architecture` | PQ privacy. SEA is orthogonal to PQ — it protects against a different threat model (user-data entanglement, not cryptographic interception). |

## Novelty confirmation

Grep across `/home/user/.skills` for "separable expert", "deletable user proxy", "user proxy", "architectural separation" returned no matches. The existing 40+ privacy-and-trust skills cover ORAM (Opal), federated learning, DP, HE, attribution, on-device personalization, and post-quantum — but none provides the architectural-separation approach to personalization-with-deletion. Opal is the closest (private memory) but solves a different problem (memory access patterns, not personalization weights). SEA is a new, structurally novel skill.