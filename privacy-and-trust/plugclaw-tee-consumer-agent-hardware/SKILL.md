---
name: plugclaw-tee-consumer-agent-hardware
description: TrustKernel's PlugClaw — thumb-sized USB-C computer isolating the AI agent's entire execution environment from the host; TEE-backed confidential cloud inference. The structural tier of the privacy-isolation ladder. (Established in cycle 18 — see original skill for the full spec, demand signal, and three-tier ladder.)
---

# PlugClaw TEE Consumer Agent Hardware — Cycle-20 Refresh

*(Full framework established in cycle 18: $149 one-time, MediaTek Helio G80, PlugOS + Ubuntu, host-as-display-only, TEE confidential inference with remote attestation, the user-preference demand signal for confidential models, and the three-tier ladder: classification gate → local-first → hardware isolation.)*

## Cycle-20 addendum (2026-09-06): GA + free confidential inference + the ladder completes

PlugClaw moved from pre-order to **general availability (shipping worldwide)** on Sept 4, 2026, and TrustKernel opened **free confidential-model inference** on newly deployed servers — following the preview finding that users chose the confidential model "far more often" even when the non-confidential option was cheaper. The free tier is a demand-validation subsidy of unstated duration (unchanged caveat); the GA signal is that the *paid demand for structural privacy* was strong enough to fund dedicated confidential infrastructure. Spec sheet unchanged (Helio G80, 4/6GB, 64/128GB encrypted, PlugOS + Ubuntu, OpenClaw with GUI-agent/Android-Use layers, cross-host compatibility incl. HarmonyOS, BYOK, FCC/CE/RoHS/MFi).

The week's other two hardware announcements complete the isolation ladder this skill introduced, upgrading it from three tiers to five:
1. **Classification gate** — Perplexity PII-Tracer (probabilistic, false-negative risk; open benchmark now available for audit).
2. **Local-first compute** — NVIDIA PAIR + RTX Spark (pool idle home machines; routing, not isolation).
3. **Execution isolation** — **PlugClaw** (this skill): agent runs entirely off-host; blast radius = the stick; unplugging ends the session.
4. **Memory isolation + hardware approval** — **Violoop** (IFA 2026, $399 Kickstarter Sept 15): dedicated device holds the continuous memory graph on-device; physical button approval via STM32 security processor; screen never streamed.
5. **Enterprise-controlled relay** — Coder Agent Relay: the agent loop vendor-side, tool calls customer-side; the procurement-grade version of the same separation principle.

**The pattern to teach stays the same across all five:** *the agent's execution environment ≠ the user's device* — approximated in software by dedicated profiles/containers + credential scoping + memory that never syncs, and made structural by hardware.

## Standing pairs

`local-escalation-consent-control`, `hybrid-compute-privacy-gate` (now with the five-tier ladder cross-ref), `opal-private-memory-architecture`, `mcp-dual-identity-problem`, `velloop-continuous-memory-agent-device` (the memory tier), `coder-agent-relay-regulated-deployment` (the enterprise tier).

## A-Tech alignment (retained)

- **Privacy:** structural blast-radius containment at consumer price — now validated by GA and confidential-tier demand.
- **Open source:** OpenClaw-native, BYOK.
- **Financial freedom:** $149 one-time vs subscriptions.
- **Practical:** the five-tier ladder is the current decision table for any "should this agent touch my device/data?" review.