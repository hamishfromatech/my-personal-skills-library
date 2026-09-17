---
name: license-axis-business-type
description: Extracts the 2026 pattern where open-weight licenses now regulate business type and scale, not just commercial use — MiniMax M2.7→M3, Kimi K3's MaaS carve-out, Qwen3.8-Max's "AI Work Assistant" category, Qwen3.8-Flash-Next's no-threshold Qwen Community License 1.0, and MiniMax H3's geographic exclusions. Use when drafting model licenses, choosing between open-weight release terms, analyzing a vendor's licensing strategy, or assessing license risk before building on an open-weight model.
---

# License Axis: Business Type and Scale

## Overview

Through April–August 2026, open-weight licensing crossed a line: from regulating *whether* a model could be used commercially to regulating *which kind of business* and *at what scale*. The Kimi K3 license targets "Model-as-a-Service" operators above $20M group revenue (internal enterprise use explicitly exempt); Qwen3.8-Max adds a distinct "AI Work Assistant" category — programming and office productivity products, with Alibaba's own Qoder and QwenWork as the license's own named examples — above $50M, while its sibling Qwen3.8-27B stays Apache 2.0; MiniMax moved from a hard commercial-use ban (M2.7, April) to a marked-and-notify regime under $20M relevant-product revenue (M3, June); Qwen3.8-Flash-Next (Aug 26, an experimental Qwen4-architecture preview) introduced the *no-threshold* Qwen Community License 1.0, requiring separate authorization for MaaS and AI Work Assistant uses regardless of size; and MiniMax H3 (video) excludes the US/EU/UK/South Korea by license terms. Meanwhile DeepSeek V4-Pro and Zhipu GLM-5.3-Flash remain unmodified MIT. The result: a license now participates in defining competitive boundaries — the game-engine precedent (Unreal Engine free until a revenue threshold) applied to model weights.

## When to Use

- Drafting or reviewing an open-weight model license (or equivalents for datasets/tools)
- Advising a lab on whether a business-type carve-out is strategically sound vs. self-defeating
- Analyzing a vendor's licensing move for what it signals about competitive intent
- Choosing which open-weight model to build on: license-risk assessment against MIT/Apache alternatives
- NOT for: the metered revenue-share mechanics themselves (use `metered-open-license-revenue-share-2026`); permissive-license selection in non-AI software; open-core boundary design (use `open-core-business-model-strategic-framework`); fork-cycle trap analysis (use `oss-license-trap-fork-cycle`)

## Core Process / Workflow

### 1. Locate the License on the New Axis

The old question was "is it open?" The 2026 question is "who does it charge, at what scale, for what business type?"

| License pattern | Example (2026) | Trigger | Target |
|---|---|---|---|
| Unmodified MIT/Apache | DeepSeek V4-Pro, GLM-5.3-Flash | None | Everyone — free distribution as moat |
| Conditional-free (notify) | MiniMax M3 | >$20M relevant-product revenue | Scaled commercial products |
| Business-type carve-out | Kimi K3 | MaaS + >$20M group revenue | Inference resellers, clouds |
| Business-type + category carve-out | Qwen3.8-Max | MaaS + "AI Work Assistant" + >$50M group revenue | Direct competitive products (coding/office agents) |
| No-threshold category license | Qwen3.8-Flash-Next (Qwen Community License 1.0) | MaaS or AI Work Assistant, any size | Same categories, no escape by scale |
| Geographic exclusion | MiniMax H3 (video) | US/EU/UK/KR excluded | Litigation/regulatory risk offloading |

Diagnostic: a license that names a *product category* (like "AI Work Assistant" naming coding agents) is protecting the vendor's own adjacent products, not merely monetizing scale.

### 2. Run the Substitution Test Before Accepting a Carve-Out

A business-type carve-out only holds when no close MIT/Apache substitute exists at similar capability for the targeted use case:

1. **Capability substitutability:** can a developer swap in an unmodified-MIT model at comparable quality for the targeted job? (DeepSeek V4 is the standing control; the entire metered-license experiment lives or dies here.)
2. **Ecosystem gravity:** do fine-tuning ecosystems, tooling, or context length create switching costs that survive the license?

If both fail, the carve-out chills the ecosystem that feeds the vendor's cloud/API business — the exact dynamic that made earlier Qwen models Apache in the first place.

### 3. Use the Flagship-Restriction Pattern Deliberately

Alibaba's structure is the replicable pattern: restrict the flagship (Qwen3.8-Max), keep sibling models Apache (Qwen3.8-27B), and let the flagship's carve-out protect the vendor's application-layer products (Qoder, QwenWork) from being built on its own weights for free. For a lab releasing a family:

- **Flagship:** business-type carve-out or revenue-share meter (protects product adjacency + captures scale value)
- **Small/mid models:** permissive (funnel; ecosystem gravity)
- **Preview/experimental releases:** watch the direction of travel — Flash-Next's no-threshold license shows tightening can arrive through *previews*, not just flagships

### 4. Read Geographic Exclusion as Litigation Shield

MiniMax H3 excludes the US (ongoing Hollywood copyright litigation over generated video content, per the official Hugging Face discussion) and EU/UK/South Korea (evolving regulatory environments), with separate authorization available to institutions committing to compliance measures. This is a new use of license terms: not monetization but litigation/regulatory risk offloading. Expect geographic carve-outs to proliferate as generative video attracts suits.

### 5. Track What Each Move Signals

| Move | Signal |
|---|---|
| Flagship restriction + sibling permissive | Protecting product adjacency; ecosystem funnel intact |
| Threshold tied to *group* revenue (not model revenue) | Leverage over scaled deployments regardless of attributable revenue |
| Revenue-share negotiations with US clouds (Moonshot ↔ Azure/AWS/GCP) | Open weights as distribution; the meter as the price of distribution |
| Unmodified MIT retained (DeepSeek, GLM-5.3-Flash) | Distribution advantage + monetize serving/API; "free" as structural weapon |
| License tightened across successive releases (M2.7→M3; Qwen Community License 1.0) | Direction of travel matters more than any single release — expect tightening until a MIT substitute forces re-think |

## Cross-Domain Linkages (A-Tech)

- **Open-source values:** this axis determines whether "open weights" remains a meaningful term; A-Tech's position is that business-type carve-outs are legitimate but should be legible (explicit thresholds, explicit categories, no retroactivity).
- **Monetization:** direct extension of `metered-open-license-revenue-share-2026` — that skill covers the revenue-share meter and its economics; this skill covers the *business-type and scale* axis that arrived alongside it.
- **Privacy/trust:** geographic exclusions interact with data-sovereignty planning (`data-sovereignty-jurisdiction-architecture-2026`).
- **Financial freedom:** for builders, license substitution risk is now a first-class input to model choice — build on models whose license terms you would accept at scale, and keep a MIT-licensed fallback mapped.

## References

- Reuters — "Alibaba plans to charge big users of its next open-source AI model" (Aug 7, 2026); "China's Moonshot in talks with Microsoft, Amazon, Google over K3 revenue sharing" (Aug 26, 2026).
- 36Kr / Siliconist Pro — "Open-source models no longer intend to allow everyone to use them for free" (Aug 27, 2026): MiniMax M2.7→M3 timeline, Qwen3.8-Max "AI Work Assistant" category with Qoder/QwenWork examples, Qwen3.8-Flash-Next Qwen Community License 1.0 (no threshold), DeepSeek V4-Pro MIT, Zhipu GLM-5.3-Flash MIT, MiniMax H3 geographic exclusions, MongoDB/Elastic/HashiCorp precedent, Unreal Engine parallel.
- GIGAZINE — Kimi K3 license clause reproduction (MaaS trigger, $20M consecutive-12-month group revenue, internal enterprise use exempted) (Aug 27, 2026).
- Related existing skills: `metered-open-license-revenue-share-2026`, `kimi-cloud-revshare-watch`, `open-source-license-physics-monetization`, `open-source-license-strategy-ai-era`, `open-source-licensing-landscape-2026`, `ai-license-circumvention-defense`, `omla-open-weight-royalty-license`, `deepseek-costco-strategy-ten-month-rule`, `third-generation-open-source-models`.
