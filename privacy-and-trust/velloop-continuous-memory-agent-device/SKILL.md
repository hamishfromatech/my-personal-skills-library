---
name: velloop-continuous-memory-agent-device
description: Applies Violoop's IFA 2026 hands-on (Tom's Guide, Sept 5, 2026; Kickstarter Sept 15, 2026, $399 intro/$799 retail) — a desktop hardware AI agent that keeps screen-aware assistance off the host computer: 8B-parameter local model at 26 TOPS processes screen data on-device with a local-first "continuous memory graph" (workflows/preferences learned over time, never transmitted), an STM32H563 security processor gating sensitive approvals, a physical hardware approval button for multi-step/sensitive actions, and cloud escalation that sends "only the information necessary" (screen never streamed). Positions as the upgrade-free path for aging laptops (runs Windows AND macOS). Use when [reviewing local-first agent hardware, designing approval-gated agent UX, comparing the 2026 privacy-isolation hardware wave, or advising owners of older hardware on agent access]. NOT for [software-only sandboxing — use ai-coding-agents-sandbox-first — or USB-stick agent isolation — use plugclaw-tee-consumer-agent-hardware].
---

# Violoop: The Continuous-Memory Agent That Lives Beside Your Laptop

## The product thesis

Violoop (IFA 2026, Tom's Guide hands-on) answers a specific 2026 problem: agentic tools like OpenClaw want to run *on* your computer, consuming its resources and battery — and older machines can't carry them. Instead of an upgrade or a software install, Violoop is a **separate desktop device that connects to your computer and handles AI processing itself**, screen-aware like software agents but with the compute (and the memory) living on its own hardware. Launch: Kickstarter September 15, 2026, at **$399 introductory / $799 after**, shipping mid-October.

## The four design elements worth extracting

1. **Continuous memory graph (on-device).** The agent learns your workflows, preferences, and patterns over time — more context per request — with screen data processed directly on the device and, per the company, **never transmitted to the cloud**. The memory architecture, not the model, is the differentiating asset: an 8B local model at 26 TOPS is modest by 2026 standards; a persistent, personalized memory graph is not.
2. **Minimal-disclosure cloud escalation.** When cloud power is needed, the device sends "only the information necessary" to complete the task; **the screen itself is never streamed**. This inverts the screen-recording agent pattern (constant capture) into request-scoped disclosure.
3. **Hardware-gated approvals.** A dedicated **STM32H563 security processor** handles sensitive-task approvals, and a **physical button** on top acts as a hardware-level approval key — multi-step or sensitive actions require a physical press rather than a software yes. Approval moves from a click in the agent's own UI to a separate physical channel the agent cannot simulate.
4. **Host-agnostic attachment.** Works with **both Windows and macOS**, largely plug-and-play — the "don't upgrade your perfectly good laptop" positioning (the reviewer's own M3 MacBook Air use case). Note the power detail: it is *not* powered by the laptop's USB-C — rear ports are USB-C, HDMI, and a dedicated power input.

## Context: the third rung of the 2026 isolation ladder

Violoop completes the week's privacy-hardware trio, each isolating a different surface:
- **PlugClaw** ($149 USB-C stick): the *agent's entire execution environment* (apps, credentials, memory) runs off-host; host is display/input only; unplugging ends the session.
- **NVIDIA PAIR** (free, open source): pools *compute* across idle home machines for local inference — routing, not isolation.
- **Violoop** ($399): isolates *screen-awareness and memory* from the host — the agent watches a dedicated device's representation of your context, with hardware-gated escalation, and accumulates memory that never syncs to someone else's cloud.

The ladder for buyers: **compute pooling** (PAIR) → **execution isolation** (PlugClaw) → **memory isolation with hardware approval** (Violoop). For builders, the pattern to copy is the separation: agent memory ≠ host memory; approval channel ≠ agent channel; disclosure = minimum necessary per request.

## Honest caveats

- Hands-on coverage, not a review unit test protocol: "setup is supposed to be largely plug-and-play. At least, that's what Violoop tells me." Claims are company-published.
- Crowdfunded (Kickstarter) — delivery risk, and the $399→$799 gap makes early pricing a discount on unproven hardware.
- 8B/26 TOPS bounds capability: heavy agentic workloads will lean on the cloud path more than the marketing implies; "only the information necessary" is unauditable by the user.
- Privacy posture is local-first by design, but the continuous memory graph means a very complete profile of you living on the device — physical security of the device itself becomes the threat model (theft, physical extraction).
- No independent security audit of the STM32 approval channel at publication.

## Pairs with

`plugclaw-tee-consumer-agent-hardware` (execution isolation), `home-ai-network-pair` (compute pooling), `local-escalation-consent-control` (the consent-escalation pattern this hardwareizes), `personal-sovereignty-seat-ceiling`, `local-first-web-architecture-2026`, `privacy-preserving-local-ai`.

## A-Tech alignment

- **Open source:** no open hardware/firmware announced — flag as the gap; the *pattern* (separate approval channel, on-device memory graph, minimum-disclosure escalation) is implementable with open tooling for DIY builders.
- **Privacy:** the strongest consumer design of the week — screen data on-device, memory on-device, physical approval out-of-band; the residual risk (device-held profile) deserves equal billing in any review.
- **Financial freedom:** $399 one-time as the alternative to both a hardware upgrade and a subscription agent — the owned-silicon asset framing; Kickstarter pricing is the arbitrage window, with delivery risk attached.
- **Practical:** the four-element pattern (memory graph / minimal disclosure / hardware approval / host-agnostic) is a ready-made evaluation rubric for every "private AI companion device" claim; "the agent can't press the button" is the memorable security line.