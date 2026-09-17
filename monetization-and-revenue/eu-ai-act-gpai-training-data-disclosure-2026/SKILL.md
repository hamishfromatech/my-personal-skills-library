---
name: eu-ai-act-gpai-training-data-disclosure-2026
description: Applies the EU AI Act Article 53(1)(a) GPAI training-data disclosure obligation — effective for models placed on the market from Aug 2 2025, with full compliance obligations from Aug 2 2027 and an enforcement window opening NOW — as the strategic lens for how disclosure duties reshape open-weight vs closed-weight economics and procurement. Use when [evaluating open-weight release/licensing strategy against EU rules, writing about AI-regulation effects on open source, assessing a model's procurement readiness, or planning 2027 compliance timing]. NOT for [legal advice, EU AI Act high-risk system compliance for deployers, or non-EU regulatory regimes].
---

# EU AI Act GPAI Training-Data Disclosure: The Open-Weight Repricing Lens

## Overview
The EU AI Act's GPAI obligations require providers of general-purpose AI models to publish "sufficiently detailed information about the content used for training" (Art. 53(1)(a)) — via an AI Office template. For models placed on the market from **August 2, 2025**, high-level summaries were due immediately; full conformity obligations bite from **August 2, 2027**. The strategic consequence the library should track: **disclosure duties turn training-data opacity from a competitive moat into a compliance liability**, and they land asymmetrically on open-weight vs closed-weight business models — exactly in the window (now → mid-2027) when the industry is deciding what "open" includes.

## When to Use
- Evaluating whether a lab's release strategy (weights-only vs open-science) is EU-ready
- Content on how AI regulation reshapes open-source economics
- Procurement due-diligence: which models can clear EU paperwork in 2026–27
- Timing decisions: release calendars, license design, data-room preparation
- NOT for: deployer-side high-risk compliance; legal advice; US/UK/China regimes (pair with library skills for those)

## The Obligation, Precisely
- **Who:** providers of general-purpose AI models (Art. 53(1)(a) + Art. 55 for systemic-risk models)
- **What:** "sufficiently detailed information about the content used for training" — published summary per the AI Office template; not raw datasets
- **When:** placed on the market from **Aug 2, 2025** (summary duty) → **Aug 2, 2027** (full GPAI obligations + enforcement window opens)
- **Enforcement shape:** the AI Office + national authorities; penalties scale with model class; systemic-risk models carry heavier duties (evaluation, incident reporting, cybersecurity)

## The Strategic Read: Disclosure Reprices Data Strategies

**1. The moat inverts.** Pre-regulation, proprietary training data was a competitive moat. Post-disclosure, *undisclosed* training data is a compliance liability and market-access blocker in the EU. Labs that already operate in the open (publishing data or data-construction recipes — e.g., the K2 Horizon pattern of "data where licensed, recipes where not") made their disclosure duty architecturally trivial; labs built on scraped-and-secret corpora face a 2027 cliff.

**2. Open-weight labs hold a structural advantage on this specific duty.** The duty is *about the data*, not the weights. An open-weights lab with documented recipes can satisfy Art. 53(1)(a) with marginal marginal cost; a closed-API lab must either reveal strategically sensitive data provenance or risk non-compliance in its second-largest market. Expect the disclosure duty to accelerate the "open science" tier (see k2-horizon-open-science-fleet-2026) as a compliance strategy, not just a reputational one.

**3. Synthetic-data strategies get stress-tested.** Libraries' synthetic pipelines (diversity knobs, seeding, filtering) become auditable artifacts — recipes as disclosure. Vendors claiming "we only train on synthetic" must be able to evidence the pipeline; the K2 gzip-diversity-metric pattern is the kind of artifact that will be expected.

**4. The open/closed decision is now partly a regulatory decision.** License design (revenue triggers, category carve-outs — see license-axis-business-type) must be reconciled with a disclosure duty that applies regardless of license. A restrictive license does not waive disclosure; openness does not exempt either. What changes is *who can afford compliance*.

**5. Timing = 2026 is the decision window.** Models shipping now will be assessed under the 2027 regime. Release calendars, data-room construction, and recipe documentation are 2026 work items.

## Decision Table: Model-Release Postures vs EU GPAI Disclosure

| Posture | Disclosure cost | 2027 EU readiness risk | Strategic note |
|---|---|---|---|
| Open science (data/recipes/checkpoints published) | Near-zero — already public | Low | Compliance as a byproduct of openness; reputational upside |
| Open weights + recipes (K2 pattern) | Low — recipes satisfy "sufficiently detailed" | Low | The emerging sweet spot |
| Open weights only (weights-only OSS) | Medium — must construct a data summary from private records | Medium | Recipe documentation is the gap |
| Closed API, secret corpus | High — reveal provenance or resist | High | Moat becomes liability; watch 2026 posturing |
| Closed + licensed/cleaned corpus (Stability pattern) | Medium — rights-holder documentation exists | Low-Medium | Licensed-data equity models gain regulatory tailwind |

## Watch Items
- AI Office template finalization and what "sufficiently detailed" means in practice (2026–27 process)
- Whether enforcement treats *recipe-quality* summaries as sufficient for frontier-scale synthetic pipelines
- Cross-border interaction: US export-control frictions + EU disclosure duties compressing non-EU labs' European options simultaneously
- Whether open-weight Chinese labs (DeepSeek, Qwen, Kimi, GLM) find EU compliance *easier* than US closed labs — a plausible inversion of the policy-dynamic assumption (pair with the RedMonk geography lens)

## Honest Caveats
- This skill encodes the *strategic* read of a statutory duty; it is not legal advice — compliance decisions need counsel.
- Template specifics and enforcement practice are still forming; dates and duties above reflect the Act's published schedule, not tested interpretation.
- Effects on open-weight economics are projections; the library has no empirical dataset on disclosure-cost asymmetries yet.

## A-Tech Alignment
- **Open source:** the clearest regulatory tailwind for the open-science tier — disclosure duties make openness a compliance strategy.
- **Privacy:** disclosure of training-content provenance aligns with data-rights and provenance-preserving positions the library already holds (c2pa, licensed-data models).
- **Financial freedom:** small labs that document as they train avoid a seven-figure compliance retrofit; procurement-readiness becomes a solo-founder moat.
- **Practical:** the decision table + watch items are directly usable in advisory and content ("The EU just repriced your training data — and open labs hold the cheap seat").

## Related Skills
- `eu-ai-act-developer-compliance-2026` / `eu-ai-act-recalibration-hybrid-2026` — the Act's developer-side compliance family
- `licensed-data-equity-model` — the rights-holder documentation pattern that feeds disclosure
- `k2-horizon-open-science-fleet-2026` — the release posture that makes this duty cheap
- `open-source-license-economics-2026` / `license-axis-business-type` — the licensing layer this interacts with
- `data-sovereignty-jurisdiction-architecture-2026` — the jurisdictional overlay