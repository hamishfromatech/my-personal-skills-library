---
name: hybrid-compute-privacy-gate
description: Applies Perplexity Hybrid Compute for Mac (launched Sept 1, 2026; 9to5Mac/Pondero/byteiota/VentureBeat coverage) — the first shipped consumer architecture where an AI agent starts in the cloud and hands off sensitive steps to a local model mid-task: an on-device PII classifier (PII-Tracer, 0.6B-parameter bidirectional Qwen3 encoder, 37 token labels across nine PII categories + conversation-level sensitivity head, 0.965 recall on 10K+ char conversations, 79.4% on recurring identifiers vs GPT-5.6's 57.0%) applies one of four outcomes (keep local / mask with stand-in tokens / refuse / ask consent), and both the classifier and the PII-TRACE benchmark (13,148 synthetic conversations, 13 languages) are open-sourced. The inversion of local-first escalation and the 24GB RAM floor are the design constraints. Use when [designing privacy-gated agent architectures, evaluating PII-gating for regulated workflows, comparing local/cloud task-splitting approaches, or advising on on-device AI hardware requirements]. NOT for [hardware isolation — use plugclaw-tee-consumer-agent-hardware — or enterprise self-hosted execution — use coder-agent-relay-regulated-deployment].
---

# Hybrid Compute: The Privacy Gate Between Cloud and Local

## The shipped architecture (what actually changed)

Perplexity's Computer agent previously ran entirely in the cloud. Hybrid Compute (Mac app, Sept 1, 2026) changes the execution model: **each task starts in the cloud** — frontier models handle planning, web search, and long-horizon reasoning — and when a step involves private files or sensitive data, the agent **hands off to a local model on the Mac without restarting the task or losing context**. Results from both environments merge back into one output; the user sees only the result.

The linchpin is the **Privacy Gate**: an on-device PII classifier that reads every task *before* it's sent to the cloud. When it flags sensitive content, the gate applies one of four outcomes:
1. **Keep the step entirely local** (the step runs on-device, tokens never leave).
2. **Mask** sensitive spans with stand-in tokens before cloud transmission (restored on return).
3. **Refuse** the action outright.
4. **Prompt the user for explicit consent** before anything leaves.

## PII-Tracer (the auditable core)

- **Architecture:** 0.6B-parameter bidirectional Qwen3 encoder, two inference heads — token classification across **37 labels** (nine PII categories: private person names, account numbers, private addresses, email/phone, private URLs, private dates, government IDs, general secrets) plus a **conversation-level sensitivity score** for longer-context decisions.
- **Long-context handling:** sliding-window inference over 4,096-token windows to recover recall that would otherwise degrade — **0.965 recall on conversations over 10,000 characters**; **79.4% recall on recurring identifiers** (the hard case: the same person referenced multiple ways), versus GPT-5.6's 57.0% on the same benchmark.
- **Open-sourced:** both the model and the **PII-TRACE evaluation benchmark** (13,148 synthetic conversations, 13 languages) via the Secure Intelligence Institute — enterprise IT can audit exactly what gets classified and how the split is made. "That open audit trail is not a minor detail. It is the reason this can clear regulated-industry procurement where a vendor's word is not enough."
- **Local economics:** local processing consumes **no cloud credits** — "you're paying for the electricity… the only thing the credits are used for is the orchestration and the delegation."

## The architectural inversion (the transferable insight)

Most hybrid AI systems **escalate from local to cloud** when a task exceeds local capability. Perplexity flips it: **start with the most powerful model, then pull back to local when the data requires it.** The four-outcome gate (local/mask/refuse/consent) is the decision contract — and the design reason it works is that the classifier runs *before* transmission, with the user able to expand and review exactly what the gate flagged before anything is sent.

Enterprise controls complete the picture: admins set org-wide policies for what must stay local, what may be masked, and what requires consent; **every action that routes data off-device is logged**; the sensitivity threshold is tunable for compliance requirements. Demo workflows: a lawyer updating a privileged brief against local case files while the cloud agent pulls public case law (only anonymized legal questions leave); a PE associate's 40-minute background run on confidential projections + public comparables.

## The constraint that shapes adoption

**24GB unified memory floor** (32GB recommended; 8/16GB machines are excluded entirely — no workaround). The majority of Macs sold in the last two years shipped with 8 or 16GB — "the architecture earns its headline, but it will not reach most individual developers on the devices they actually carry." Local model: PPLX Qwen 3.8 27B, one-click download, no Ollama dependency, no API key. Apple silicon only (macOS 15+); Windows would require a different NPU implementation (not announced). Access: Pro, Max, and Enterprise subscribers.

## The design rules

1. **Gate before transmission, not after** — the classifier must run on-device before anything leaves; post-hoc filtering is a different (weaker) product.
2. **Offer the four-outcome contract** — local/mask/refuse/consent covers the compliance matrix; a binary local-vs-cloud switch is under-designed.
3. **Open-source the gate or expect distrust** — a proprietary PII classifier asking to read everything is a harder sell than an auditable one; publish the benchmark alongside the model.
4. **Start cloud, pull back on data sensitivity** — the inversion preserves capability for public steps and privacy for private ones without task restarts or context loss.
5. **Log every off-device route** — the enterprise audit log is what converts a privacy feature into a procurement asset.
6. **Name the memory floor honestly** — hardware requirements are adoption gatekeepers; publish them prominently and plan the smaller-model path for 16GB-class devices.

## The trust question worth asking

The Privacy Gate is itself an ML classifier, and **classifiers miss things — a false negative means sensitive data reaches the cloud anyway.** Perplexity's answer is transparency (reviewable flags, audit logs) plus the open benchmark. The deeper pitch asks professionals to trust one AI to decide what another AI is allowed to see — "for an industry that has spent three years telling lawyers, bankers and doctors to keep their most sensitive work away from the cloud, Perplexity's wager is that the fix was never to build a higher wall — it was to build a smarter gate." Whether that wager clears regulated-industry bar depends on the false-negative rate, which the open benchmark now lets outsiders measure.

**Cycle-20 refresh (2026-09-06):** the September wave confirms this architecture as a *ladder tier*, not a final form — three same-week datapoints now bracket it: (1) **PlugClaw GA** (Sept 4) ships the structural tier — hardware isolation of the entire agent execution environment at $149 — superseding classifier-gating where blast-radius-by-construction is required; (2) **Violoop** (IFA 2026 hands-on, Sept 5) adds the memory tier — a dedicated device holding a continuous memory graph on-device with a physical STM32-gated approval button, isolating *screen-awareness and memory* rather than execution or classification; (3) **Coder Agent Relay** (Sept 3–4) productizes the enterprise tier — agent loop vendor-side, tool calls customer-side, air-gapped option, five-function AI Operating Layer. The ladder is now: classification gate (this skill, probabilistic, false-negative risk) → local-first compute (PAIR/DGX, hardware floor) → execution isolation (PlugClaw) → memory isolation with hardware approval (Violoop) → enterprise-controlled relay (Coder). **Watch item:** the open PII-TRACE benchmark now allows third-party measurement of the false-negative rate — the number that decides whether "smarter gate" clears regulated procurement; publish-measured results before enterprise recommendations.

`plugclaw-tee-consumer-agent-hardware` (hardware isolation — the structural tier above classification), `home-ai-network-pair` (local compute pooling), `privacy-first-ai-pipeline-defense`, `genai-privacy-choice-ecosystems`, `local-escalation-consent-control` (the consent-ladder layer), `federated-local-first-ai`, `privacy-preserving-local-ai`, `data-sovereignty-jurisdiction-architecture-2026`.

## A-Tech alignment

- **Open source:** the classifier + benchmark open-source is the week's strongest open-privacy move — reusable in any agent architecture regardless of vendor; the 24GB floor is the honest counterweight.
- **Privacy:** the core subject — auditable gating with a four-outcome consent contract; the false-negative caveat keeps the recommendation honest rather than promotional.
- **Financial freedom:** local steps are credit-free (you pay electricity, not tokens) — a real cost incentive for privacy-preserving usage patterns on capable hardware.
- **Practical:** the six design rules + four-outcome contract are directly reusable in any agent privacy review; "smarter gate, not higher wall" is the framing for content, with the false-negative question as the critical follow-up.