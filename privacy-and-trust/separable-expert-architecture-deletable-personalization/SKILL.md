---
name: separable-expert-architecture-deletable-personalization
description: A three-layer LLM personalization architecture that decouples user-specific data from shared model weights by combining a frozen base model, shared domain-expert LoRA adapters, and a per-user proxy artifact (routing bias, steering vectors, personal LoRA) whose filesystem deletion constitutes deterministic, verified unlearning — no retraining required. Use when designing personal AI assistants that must honor data-deletion rights (GDPR Right to Erasure), building multi-tenant LLM systems with per-user personalization, architecting privacy-preserving AI where user data must never enter shared weights, or converting machine unlearning from an intractable weight-editing problem into a tractable deletion operation. NOT for systems where personalization must be maximally expressive (absorptive fine-tuning), for centralized models where deletion is not a requirement, or for non-LLM personalization.
---

# Separable Expert Architecture: Privacy-Preserving LLM Personalization via Composable Adapters and Deletable User Proxies

## Overview

Separable Expert Architecture (SEA) solves the fundamental tension between LLM personalization and data deletion by ensuring user-specific information never enters shared weights in the first place. Instead of trying to surgically undo weight entanglement after fine-tuning (computationally intractable), SEA architecturally prevents entanglement: all user-specific state lives in an isolated, deletable proxy artifact (2–5 MB per user) that composes with shared components at inference time. Deleting the proxy directory is both necessary and sufficient for complete user data removal — verified empirically with 82–89% noise-calibrated verification pass rate and near-zero cross-user contamination.

## When to Use

- Building personal AI agents that must honor the Right to Erasure (GDPR Article 17, CCPA deletion requests)
- Designing multi-tenant LLM systems where each user gets personalized behavior but shared weights must remain clean
- Architecting privacy-preserving AI where user data must never contaminate a shared model
- Converting machine unlearning from an intractable algorithmic problem into a deterministic engineering operation
- Building systems where shared model components can be released/audited without risk of user-data exposure
- Designing A-Tech personal AI products (A-Coder personalization, Be Practical adaptive learning, Builder's Club individual assistants) with privacy as a structural guarantee
- NOT for systems requiring maximally expressive personalization (absorptive fine-tuning bakes preferences into weights but is intractable to delete)
- NOT for centralized models where per-user deletion is not a functional or regulatory requirement
- NOT for non-LLM personalization (the architecture is designed around LoRA adapters and inference-time composition)

## Core Process / Workflow

### 1. Understand the architectural invariant

SEA maintains a strict, structural (not statistical) invariant:

> **Invariant (Separation):** All user-specific information resides in an isolated, deletable proxy artifact. Shared model components (base model + expert adapters) contain no user-identifying information. Removing the proxy artifact is both necessary and sufficient for complete user data removal from the inference system.

This is structural, not probabilistic. Approximate unlearning provides probabilistic guarantees that user influence has been reduced below a threshold. SEA guarantees that user influence is *architecturally absent* from shared components — by construction, the system never permits user-specific gradients to flow into shared weights, so there is nothing to remove.

### 2. Assemble the three composition layers

SEA combines three layers at inference time:

**Layer 1 — Base Model (frozen, shared):**
- A quantized LLM (e.g., Phi-3.5-mini-4bit, Llama-3.1-8B-4bit) providing general language capabilities
- Shared across all users; base weights never modified during user interactions
- Contains no user-specific information by design

**Layer 2 — Expert Adapters (shared, dynamically weighted):**
- A bank of k domain-specific LoRA adapters E = {E_1, ..., E_k} (e.g., Security, Code, Data, General)
- Each E_i = (B_i, A_i) trained on curated domain corpora; shared across all users
- At inference, experts combine via weighted linear combination:
  ```
  W_expert = W_base + Σ_i w_i · B_i · A_i
  ```
  where w ∈ Δ^k (probability simplex) are per-query mixing coefficients from a lightweight router
- Encode domain knowledge only — no user information

**Layer 3 — Per-User Proxy (isolated, deletable):**
Each user u has an isolated proxy artifact P_u — a self-contained directory (~2–5 MB) with three complementary personalization mechanisms:

1. **Routing bias vector** b_u ∈ R^k: learned domain-affinity scores shifting expert selection toward user-preferred domains
   ```
   w̃_i = w_0,i + λ · b_u,i
   w_i = max(w̃_i, 0) / Σ_j max(w̃_j, 0)
   ```
2. **Contrastive steering vectors** {s_u^ℓ} at intermediate layers: computed via Contrastive Activation Addition from user preference pairs; injected additively into residual stream at inference:
   ```
   h^ℓ ← h^ℓ + γ · s_u^ℓ
   ```
   Encode stylistic preferences (verbosity, formality, technical depth) without modifying weights.
3. **Personal LoRA adapter** L_u = (B_u, A_u): rank-4 (deliberately small to bound proxy size) trained via DPO on user preference pairs. Captures user-specific knowledge that routing bias and steering alone cannot express. Base and expert weights frozen during DPO — only personal LoRA receives gradients.

### 3. Run the five-stage inference pipeline

Given query q from user u:
1. **Route:** Lightweight router classifies q into domain distribution w_0 over k experts.
2. **Bias:** Apply user routing bias b_u (shift expert selection toward user-preferred domains).
3. **Merge:** Combine weighted expert adapters + personal LoRA into single merged adapter on base model.
4. **Steer:** Inject user steering vectors γ·s_u^ℓ at layers ℓ via forward hooks (modify activations, not weights).
5. **Generate:** Standard autoregressive decoding with merged model produces personalized output.

### 4. Execute the deletion protocol

To delete user u:

1. **Verify (before irreversible delete):** Generate outputs in omission mode (proxy not loaded) on held-out domain-generic prompts. Compare token-frequency distributions against cached non-personalized baseline via KL divergence:
   ```
   D_KL(p_unpers || p_baseline) ≤ max(2·σ̂_KL, τ_min)
   ```
   where σ̂_KL is the empirical inter-sample KL noise floor (self-calibrating: high-variance queries get wider acceptance bands) and τ_min = 0.15 nats is a hard floor.

2. **Delete:** Secure filesystem removal of proxy directory P_u (zero-overwrite).

3. **Audit:** Log deletion event, verification result, and timestamp for compliance trail.

The architectural separation makes this a sanity check, not the guarantee itself. Without the proxy, the system's behavior is structurally equivalent to the non-personalized baseline — the same code paths execute with the same weights. The guarantee comes from the invariant: user information exists only in the proxy, and the proxy has been deleted.

### 5. Evaluate the three core claims

**Personalization (Claim 1):** The proxy measurably adapts outputs without modifying shared weights.
- Routing bias shifts expert selection (weight shift 0.052–0.088)
- Jaccard similarity to baseline low (0.236–0.316) — substantial output differentiation
- Style trait matching present (stronger for Phi-3.5-mini than Llama-3.1-8B)

**Separability (Claim 2):** Proxy removal restores baseline behavior.
- Mean KL divergence ~0.21 nats for both models
- 82–89% noise-calibrated verification pass rate
- Bimodal KL distribution: verified observations cluster in [0.00, 0.30], failures in [0.30, 0.94] — no ambiguous partial-leakage population

**Isolation (Claim 3):** No cross-user leakage between proxies.
- Contamination ≤ 0.05 in point estimates (0.009 for Phi-3.5-mini, 0.049 for Llama-3.1-8B)
- Cross-user similarity moderate (0.27–0.35) but structural (shared base + experts), not leakage

### 6. A-Tech applications

- **A-Coder (personalization):** Each developer gets a proxy encoding their coding style, domain preferences (frontend/backend/DevOps), and interaction patterns. Deleting a developer = deleting their proxy directory. Shared base + expert adapters remain clean and auditable.
- **Be Practical (adaptive learning):** Each learner's progress, style preferences, and knowledge gaps live in a deletable proxy. The learning model (base + pedagogy experts) is shared and never contaminated by individual learner data — a privacy-first differentiator for regulated education markets.
- **Builder's Club (individual assistants):** Community members get personalized assistants whose personalization is fully deletable. This is the "ownable architecture" from the Mozilla State of Open Source AI report: "A rented model can be deprecated; a memory held on the enterprise side of the firewall cannot."
- **Privacy-first competitive differentiator:** SEA converts the Right to Erasure from a compliance burden into a structural guarantee. The shared model can be open-sourced and audited without risk of user-data exposure — because no user data is in it.

## References

- See [references/separable-expert-evidence-base.md](references/separable-expert-evidence-base.md) for the full architecture specification, experimental results, mathematical detail, limitations, and cross-references to adjacent skills.
- Complements `opal-private-memory-architecture` (private memory via ORAM/TEE; SEA is the personalization-weight layer)
- Complements `privacy-preserving-ai-attribution-framework` (attribution with FL+DP+HE; SEA is the personalization layer)
- Complements `ai-agent-memory-architecture` (infrastructure memory layer; SEA is the privacy-preserving personalization layer above it)
- Complements `federated-llm-on-device-personalization` (federated personalization; SEA is a non-federated alternative with deterministic deletion)