---
name: local-escalation-consent-control
description: Applies the Perplexity Portable Computer launch (Aug 25 2026, with Nvidia DGX Spark) as the canonical case for the consent-vs-control gap in hybrid local-first AI — local-by-default with per-action cloud escalation governed by permission prompts is a strong privacy story but fails enterprise governance because consent is not control. Use when designing local-first products, hybrid local/cloud routing, escalation governance, enterprise privacy architecture, or evaluating "runs on your data, on your device" claims.
---

# Local Escalation: Consent vs. Control

## Overview

Perplexity's **Portable Computer** (Aug 25 2026) runs the entire agentic control plane — orchestrator, planner, tool router, scheduler, durable task queue, local search index, inference — on a local Nvidia DGX Spark or RTX-24GB+ Linux machine (Qwen 3.8 27B or post-trained PPLX 27B; Windows "coming soon"). Local work consumes **zero billing credits**; the system may escalate individual steps to a frontier cloud model **only after explicit per-action user approval**. This is the most consumer-legible embodiment yet of local-first AI — and the enterprise security community's reaction defines its real lesson: **the gate is a permission prompt, which is consent, not control.** Consent-based escalation depends on (a) a probabilistic model correctly classifying sensitive content, (b) correct scoping of the escalation payload, and (c) a user judging a request they cannot fully inspect — all three fail adversarially.

## When to Use

- Designing any local-first/hybrid product with a cloud-escalation path (the architecture of 2026: local model + local harness + optional frontier advisor)
- Writing enterprise security reviews of local-AI products ("is escalation governed by consent or by policy?")
- Building the escalation-governance layer that makes local-first *enterprise-eligible* rather than merely *privacy-branded*
- Evaluating vendor claims like "your data stays local" — separate where work runs from where data can *egress*
- NOT for: fully air-gapped deployments (no escalation path to govern), or pure cloud products

## Core Process / Workflow

### 1. The Local-First Stack Actually Shipped (What "Local" Now Means)

- Full agent harness on device: local models (Qwen 3.8 27B / PPLX 27B), vLLM underneath, agent harness, tools, app connectors (Drive/Gmail/Slack/GitHub), security sandbox, local search index.
- **Model-harness co-design**: small local models buckle under general-purpose harnesses; Perplexity built a minimal harness (succinct system prompt, few core tools, on-demand skill loading, connectors converted from token-hungry MCP servers to compact CLI tools, self-verification hooks, OS-level sandbox that *disables tooling if unavailable*). Reported internal-bench deltas (vendor-run): 82.6% local knowledge-work vs 77.6% (Pi harness) / 74.0% (Hermes) on the same 27B model.
- **Zero-metered local tokens**: always-on agents are economically absurd on frontier API pricing and ~free on owned silicon — the "credit counter parked at zero" demo is the product thesis.

### 2. The Hybrid Escalation Economics (The Number That Matters)

Terminal-Bench 2.1, per-task:

| Path | Score | Est. cost |
|---|---|---|
| Fully local (27B) | 59.6% | ≈ $0 |
| Local + cloud advisor escalation | 73.0% | ≈ $0.415 |
| Frontier model alone | 82.4% | ≈ $0.65 |

**Escalation recovered ~three-fifths of the gap to frontier at ~two-thirds of the cost** — and the user chooses per-task when that trade is worth it. Before any advisor call: a PII classifier runs over the outgoing context and the user sees exactly what would leave the device; the remote model returns *text guidance only* and never touches local files or tools. This local→advisory pattern (advice crosses, files don't) is the key architectural primitive.

### 3. The Consent-vs-Control Gap (Why Security Teams Object)

The documented failure modes of prompt-gated escalation:

1. **Consent fatigue / reflexive approval** — users approve pop-ups they don't read (see consent-fatigue-progressive-permissioning).
2. **Prompt-injected escalation** — attacker-controlled content in documents/emails can try to steer the escalation decision.
3. **Classifier-dependence** — a probabilistic model decides what is "sensitive"; deterministic guarantees are absent.
4. **Connector + egress combination** — Gmail/Drive/Slack/GitHub connectors on a device holding an authorized cloud path is the same shape as the Copilot exfiltration chains published in 2026.
5. **User-only authority** — the enterprise cannot say "you are not permitted to *ask*."

Perplexity's response mitigations (worth crediting): escalation requires the "allow advisor escalation" toggle *plus* per-action in-app review; a one-time-only escalation per task; content cannot self-authorize; sandbox restricts local data/outbound actions. These improve consent quality — they do not convert it into administrative control.

### 4. The Enterprise Escalation-Governance Checklist (What Makes Local-First Enterprise-Eligible)

Whoever ships this "owns the regulated-industry local AI market":

- [ ] **Central escalation policy** the local model cannot override (admin-defined allow/deny, deterministic rules blocking defined data categories regardless of model judgment)
- [ ] **Network-layer enforcement, not application-layer**: mandatory egress proxy / DLP inspection on every escalation payload
- [ ] **Immutable, tamper-evident audit log** of exactly what crossed the boundary
- [ ] **Deterministic classification** for regulated data classes; probabilistic models may assist but never gate alone
- [ ] **Bounded payloads**: advisory-text-only responses, no tool/file access for the remote model, per-task escalation quotas
- [ ] **Default-deny**: escalation off until explicitly enabled; local-only mode that disables the cloud path entirely

### 5. Position Local-First Honestly

Local-first ≠ local-only. Sell it as a *data-flow control surface*: most work stays on-device (privacy + zero marginal token cost), selected steps escalate under *policy* (not vibes). Consumer framing = privacy story; enterprise framing = compliance story; the difference is entirely in the governance layer.

## A-Tech Alignment

- **Data privacy**: the core case — on-device inference with owned harness + sandbox; PII classifier before egress; advisory-only remote calls.
- **Open-source AI**: built on open weights (Qwen 3.8 27B family); Perplexity plans to open-source its Local Knowledge Work Bench (53 tasks).
- **Financial freedom**: zero-metered local tokens; the $0 → $0.415 → $0.65 escalation ladder is a template for token-budget-conscious agent design.
- **Practical implementation**: harness-co-design + benchmark deltas + governance checklist are directly actionable.

## References

- Computerworld (Aug 25, 2026) "Perplexity's on-device AI offering promises data control and lower token costs"; VentureBeat (Aug 25, 2026) launch deep-dive with benchmark table and CISO commentary (Greis/Acceligence, Wilkes/Aikido, Mahapatra/Tribeca, Villanustre/LexisNexis).
- Related existing skills: `privacy-preserving-local-ai`, `local-first-web-architecture-2026`, `consent-fatigue-progressive-permissioning`, `federated-consent-architecture-agent-systems`, `privacy-first-personalization-2026`, `ai-sovereignty-hardware-stack`, `open-weight-agentic-model-wave-august-2026` (model supply).
