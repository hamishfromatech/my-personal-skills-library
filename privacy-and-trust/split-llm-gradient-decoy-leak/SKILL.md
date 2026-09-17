---
name: split-llm-gradient-decoy-leak
description: Applies the first systems-security case study of split-LLM training privacy (Politis & Pappas, arXiv:2609.04382, Sept 3 2026) — the returned output gradient from an untrusted cloud node nullifies decoy rows' gradients to exactly zero, leaking which rows are real on every frame (4,096/4,096 across nine seeds) — while forward-channel privacy checks and quality checks BOTH pass. Use when auditing split-learning or split-inference privacy architectures, designing decoy-based defense systems, or briefing on why privacy evaluations must include the returned-gradient channel. NOT for federated-learning differential privacy (use the DP-FL stack) or for split inference at deployment (this is a training-time threat).
---

# Split-LLM Training: The Returned-Gradient Leak

## Overview
Split learning sends activations from a private local node (TLN) to an untrusted cloud node (UCN) which returns gradients. A standard defense is to MIX real rows with DECOY rows in the frame the UCN sees — the loss ignores decoys, so their gradients should be zero and harmless. The failure: **the returned gradient's decoy rows are exactly zero, and the pattern of zeros reveals which rows are real.** The leak reproduces on every frame (4,096/4,096 rows identified per run, nine seeds), survives forward-channel privacy checks AND quality checks, and a frame-reconstruction attack recovers ~+0.65 to +1.50 percentage points of extra tokens over a constant-guess baseline. A second configuration (within budget, deployable) reproduces the leak — this is not an exotic deployment artifact.

## When to Use
- Auditing a split-learning or split-inference privacy architecture for return-channel leaks
- Designing a decoy-based defense system (the decoy itself becomes the leak channel if gradients are not clipped/noised per-row)
- Writing threat models for privacy-preserving split training (the returned gradient is a distinct channel from the forward activation)
- NOT for differential-privacy federated learning (different mechanism — use adaptive-dp-fl-concept-drift-edge)
- NOT for split *inference* privacy (this is training-time, not inference-time)

## Core Process / Workflow
1. **Name the channel**: the leak lives in the RETURNED GRADIENT — the UCN's gradient output. Every prior evaluation checked only the FORWARD channel (activations sent TO the cloud) and the quality metric; neither catches this leak.
2. **Reproduce the instrument first**: the paper fixes a protocol in advance — inject a leak at known strength to prove the instrument can see one; run a shuffled-label control to prove it does not report absent leaks; set the threshold before the runs. This pre-registration discipline is the transferable skill.
3. **Run the leak test**: across nine seeds, decoy rows were identified 4,096/4,096 per run; shuffled-label controls recovered nothing. The pattern of exact zeros is the fingerprint.
4. **Fix minimally, then re-audit**: clipping and noising EACH ROW of the returned gradient closes the leak for ~0.01 nats of held-out cross-entropy. But the paper's own caveat stands — five attack classes (including those accumulating observations across training steps) were never measured; row-level clipping is a necessary, not sufficient, fix.
5. **Extend the audit pattern**: for any architecture where a party returns a computed artifact (gradient, output, hash, signature), test whether the artifact's structure encodes information the forward channel doesn't reveal — this generalizes beyond split LLMs.

## Key Evidence
- Decoy-row gradients are exactly zero (loss ignores decoys) — the zero pattern identifies real rows
- Frame-content attack recovers +0.65 to +1.50pp extra tokens over constant-guess baseline (nine seeds)
- Both forward-channel privacy check AND quality check PASS while the return channel LEAKS
- Fix: clip + noise each gradient row → leak closed for ~0.01 nats accuracy cost
- Five unmeasured attack classes remain, including cross-step accumulation

## Pairs with
`privacy-preserving-local-ai` (the local-first counter-architecture — don't split at all), `adaptive-dp-fl-concept-drift-edge` (DP-FL for federated, not split, learning), `differential-privacy-synthetic-data` (the DP mechanism this fix borrows), `agentic-supply-chain-exploit-defense` (the broader attack-surface discipline), `coder-agent-relay-regulated-deployment` (the enterprise pattern that routes around split-training exposure), `ai-coding-agents-sandbox-first` (the containment-first posture).

## A-Tech Alignment
- **Open source**: the pre-registration discipline (instrument + control + threshold-before-run) is a transferable audit protocol; row-level clipping/noising is implementable in standard PyTorch.
- **Privacy**: the lesson is architectural — a privacy check that doesn't audit the RETURN channel is incomplete; this extends the library's privacy-audit discipline beyond FL to split architectures.
- **Financial freedom**: a deployable fix at ~0.01 nat cost means privacy-preserving split training is affordable — no reason to sacrifice the architecture.
- **Practical**: the "audit the return channel" rule generalizes to any multi-party computation where one party returns a structured artifact.

*Source: Politis, G. & Pappas, E., "Privacy Failure in Split-LLM Training: The Returned Gradient Nullifies the Decoys," arXiv:2609.04382, Sept 3 2026 (cs.CR).*