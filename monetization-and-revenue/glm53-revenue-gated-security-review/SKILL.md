---
name: glm53-revenue-gated-security-review
description: Applies Z.ai's GLM-5.3 License (Aug 28 2026) — the first open-weight license to attach a security-review requirement to commercial hosting for companies above $10B aggregate revenue — and the structural reframe that open-weights-as-strategy now includes revenue-gated access control. Use when evaluating open-weight model licenses for commercial deployment, analyzing hyperscaler access-control requirements, or designing an open-weights distribution strategy that must anticipate tiered restrictions. NOT for the general open-weight license landscape (use open-source-license-economics-2026, license-axis-business-type) or for MiniCPM5-2B's quiet-drop pattern (see minicpm5-2b-quiet-weights-drop).
---

# The GLM-5.3 License: Security Review at $10B

## Overview
Z.ai published GLM-5.3's full 753B-parameter weights on Hugging Face (Aug 28 2026) under a new license that attaches one condition: **companies with more than $10B in aggregate revenue over any 12 consecutive months must pass a Z.ai security review before commercially hosting the model.** Individuals and companies below the threshold keep every prior right — download, run, fine-tune, build commercial products, no fees. The distinction between hosting (running the weights yourself at scale) and routing/embedding (using a marketplace or reseller) is the load-bearing line: the clause targets hyperscalers, not the ecosystem at large. This is the first open-weight license where "open" is conditioned on a security clearance at scale — the end of the era where the strongest Chinese models arrived with no strings attached.

## When to Use
- Evaluating an open-weight model's license for a commercial deployment (per-checkpoint, not per-vendor)
- Briefing on where open-weights governance is heading (revenue-gated access control)
- Advising a hyperscaler or large reseller on hosting an open-weight frontier model
- Designing an open-weights distribution strategy that must anticipate tiered restrictions
- NOT for per-checkpoint license lookup (use open-source-license-economics-2026)
- NOT for the stealth-launch / quiet-drop pattern (see minicpm5-2b-quiet-weights-drop)

## Core Process / Workflow
1. **Separate hosting from routing**: the $10B security review applies to companies who host GLM-5.3 (serve it at national scale) — not those who route through OpenRouter or embed in products. The distinction is the clause's scope, not its spirit.
2. **Read the strategic layer**: Z.ai is not the first lab to gate dangerous capability (OpenAI/Anthropic registration requirements exist) but the first to gate by REVENUE. OpenAI/Anthropic require registration for offensive-cyber models; Z.ai used temporary guardrails (two-week weight delay, security-partner evaluation, then release with a scale-triggered review).
3. **Map the three Chinese lab philosophies**: DeepSeek = MIT everywhere (unconditional); Moonshot Kimi K3 = attribution at scale ($20M/month trigger); Z.ai = security clearance at scale ($10B hosting trigger). The GLM-5.3 License is the strictest of the three.
4. **Read the consolidation context**: the clause arrived the same week as NVIDIA's reported ~$12.9B Hugging Face acquisition and Stripe's ~$7.5B OpenRouter deal — the infrastructure of openness (download page, routing layer, billing meter) consolidating into corporate hands on three continents. The license is Z.ai's hedge against a future where distribution channels answer to Washington.
5. **Extract the developer playbook**: (a) small companies and individual developers are untouched — no registration, no fee, full commercial use; (b) treat terms as generation-specific (GLM-5.2 shipped MIT; GLM-5.3 ships with a gate); (c) abstract the provider layer so models are swappable; (d) if you operate near hyperscaler scale, treat the review as a regulatory hurdle with a clock.

## Key Evidence
- GLM-5.3: 753B total / ~40B active MoE, 1M context, 128K output, BF16/FP8 on Hugging Face
- Security review trigger: >$10B aggregate revenue over any 12 consecutive months, applied to *hosting* (not routing/embedding)
- The two-week delay: weights released Aug 28 after API launch Aug 14; vetted security partners evaluated during the window
- GLM-5.3 CyberGym: 84.5% (best of any model tested, ahead of Claude Mythos 5 at 83.8 and GPT-5.6 Sol at 83.6); ExploitBench 54.4% vs GLM-5.2's 24.4%
- Brockman's "The Defenders Window" warning (Aug 17) named GLM-5.3 by name as the open-weights cyber-capability signal
- Counter-evidence: US/UK AI Safety Institutes jointly evaluated Kimi K3 — zero arbitrary code across 41 ExploitBench tasks (vs ~20 for capable proprietary models with safeguards disabled)
- Three Chinese lab license philosophies: DeepSeek MIT / Kimi attribution / Z.ai security review

## Pairs with
`open-weight-agentic-model-wave-august-2026` (the capability wave), `license-axis-business-type` (the per-model/per-size license mechanics this extends), `open-commons-acquisition-neutrality-2026` (the consolidation counterweight), `redmonk-open-weight-decision-lenses` (the eight-lens evaluation), `china-open-source-llm-arr-tracker` (the revenue context), `minicpm5-2b-quiet-weights-drop` (the contrast — a quiet drop vs a gated release), `abliteration-hosted-guardrail-removal` (the distribution-layer governance surface).

## A-Tech Alignment
- **Open source**: the clause is a conditional on the open commons, not an exit from it — small developers keep full rights; the precedent will spread if other labs follow.
- **Privacy**: the security review is a governance instrument for the largest deployers — not a consumer-facing privacy requirement; the privacy implication is that scale changes who must prove safety.
- **Financial freedom**: below $10B revenue, GLM-5.3 is free to run, fine-tune, and commercialize — the "golden age" persists for the 99%.
- **Practical**: the license is a decision-table input: per-checkpoint license read, hosting-vs-routing distinction, tiered-restriction checklist.

*Source: Z.ai GLM-5.3 License (Hugging Face, Aug 28 2026); The Batch / DeepLearning.AI analysis; The New Stack (Lardinois); worldngayon synthesis, Aug 30 2026.*