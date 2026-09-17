---
name: meta-muse-secure-vm-sentinel
description: Applies Meta's Muse personal AI agent launch (Sept 8, 2026; iOS/Android/muse.ai, free + subscription tiers) — the first consumer agent shipped with a dedicated cloud Secure VM containing a separate, system-level-isolated Sentinel agent that gates every outbound request — as the AUDITABLE-GATE pattern for consumer agent trust design. Use when [designing or evaluating consumer personal-agent architectures, comparing isolation approaches (hardware vs VM vs OS sandbox), briefing on agent permission and approval flows, or teaching the Sentinel-sidecar pattern]. NOT for [device-isolation tier skills — use plugclaw-tee-consumer-agent-hardware and velloop-continuous-memory-agent-device — or local network compute routing — use home-ai-network-pair].
---

# Meta Muse: The Sentinel-Gated Personal Agent

## Overview
Meta's Muse is a proactive personal AI agent (tasks, bookings, multi-day projects, negotiation, checkout via Stripe Link) that runs on **Muse Secure VM** — a dedicated cloud virtual machine housing both the agent and the user's connected data/credentials. The load-bearing design element: a **separate Sentinel agent runs on that same machine, kept apart from Muse at the system level**, and **nothing Muse does reaches the internet unless the Sentinel approves it**, asking the user for permission when needed. This is the first mass-market consumer agent whose trust model is a second, isolated process reviewing the first's traffic — and it arrives the same week as consumer hardware isolation (PlugClaw) and local-network compute pooling (PAIR), completing the isolation spectrum from $149 sticks to sovereign cloud VMs.

## When to Use
- Designing approval gates for autonomous agents where a monitor must be independently isolated from the agent it polices
- Comparing consumer trust architectures: cloud-VM-gated (Muse), hardware-isolated (PlugClaw), device-adjacent (Violoop)
- Briefing on credential scoping — Muse cannot see stored passwords; Link issues one-time-use virtual cards
- Teaching why "approval" must be a separate trust domain, not a prompt or a permission checkbox
- NOT for local-first privacy posture (use hybrid-compute-privacy-gate, privacy-preserving-local-ai)
- NOT for hardware TEE isolation at the device layer (use plugclaw-tee-consumer-agent-hardware)

## Core Process / Workflow
1. **Read the three-party gate.** Every Muse action has: the agent (executor), the Sentinel (system-level separated gatekeeper on the same VM), and the user (notified when the gate needs a human). "No internet without Sentinel approval" converts the agent's blast radius from unbounded to gated.
2. **Map the credential-blindness layer.** Muse has no visibility into passwords or payment methods: credentials go to secure storage it can use without seeing; Link generates one-time-use virtual cards; purchase protections ride on Stripe Link. Trust is mediated, not granted.
3. **Apply the consent-graduation table.** People choose which apps connect and exactly what each can do (read vs send on email); sensitive actions (send email, purchase) require explicit check-in; a complete audit trail of everything done and planned is surfaced; users can disconnect any service anytime; training-data opt-out is preserved; "forget" is an explicit instruction.
4. **Position on the isolation ladder.** Muse = cloud-VM-gated (Sentinel in-software, same-host separation). PlugClaw = hardware isolation (structural, agent cannot touch host by construction). Violoop = memory-adjacent (device-held profile, physical approval button). PAIR/HomeAgent = compute pooling (no isolation claim). Each answers a different threat model; pick by threat, not by hype.
5. **Note the honest boundary.** Meta states the Secure VM is "contained so no one else's agent can reach it" and says the whole VM is encrypted so "not even Meta can access it" — vendor-claimed, not yet audited. The ad-separation commitment ("doesn't share conversations or VM data with Meta's ad systems") is the same architecture whose monetization pressure is now live elsewhere in the market.

## Key Evidence
- Launch: Meta newsroom, Sept 8, 2026 — "first personal AI agent built for everyone"; free for most uses + subscription plans
- Sentinel: "separate agent... kept apart from Muse at the system level; nothing Muse does reaches the internet unless the Sentinel approves it"
- Payment: Stripe Link with first-agent purchase protections; one-time-use virtual cards; 1Password support coming
- Model: Muse Spark, "Meta's most capable model to date" for agentic work
- Roadmap: Muse Confidential VM later in 2026 (VM encrypted with a key only the user holds)
- Same-week context: IFA 2026 privacy-first hub wave; JetBrains Q2 agent adoption; OpenAI/Adobe ad-policy dispute (Sept 10)

## Pairs with
`plugclaw-tee-consumer-agent-hardware` (hardware isolation tier), `velloop-continuous-memory-agent-device` (memory tier), `home-ai-network-pair` (local compute pooling), `mcp-dual-identity-problem` (credential scoping), `local-escalation-consent-control` (approval design), `agentic-commerce-trust-design`, `agent-runtime-permissions-audit-2026`, `prompt-data-barter-pricing-muse-spark` (the Spark model this launches on).

## A-Tech Alignment
- **Open source**: the Sentinel pattern is architecture, not product — an open-agent equivalent (system-level sandbox + approval proxy) is buildable today with sandboxing and egress rules.
- **Privacy**: consent-graduated access + audit trail + credential blindness is the consumer-facing version of least-privilege; treat vendor claims as unaudited.
- **Financial freedom**: one-time-use virtual cards and meditated credential storage reduce blast-radius economics of agent mistakes.
- **Practical**: three-party gate checklist (executor / gate / human) applicable to any agent product review.

*Source: Meta newsroom launch post (Sept 8, 2026); companion design posts "How We Built Safety Into Muse," "How We Designed Muse." Vendor claims not independently audited at writing.*