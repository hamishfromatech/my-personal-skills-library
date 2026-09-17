---
name: open-source-licensing-landscape-2026
description: Navigate the 2026 open-source licensing landscape using the RedMonk data-driven analysis — the rise of permissive licensing (73% of licensed projects), the Apache vs MIT dynamics, the package-ecosystem filter (npm/ISC overrepresentation), the irrelevance of source-available licenses (BSL/SSPL) at scale, and the potential AGPLv3 resurgence. Use when selecting an open-source license based on current ecosystem data, evaluating the permissive-vs-copyleft trend, understanding package-repository license distributions, or assessing the strategic relevance of source-available licenses. NOT for BSL decision frameworks (use open-source-license-economics-2026) or for AI-era license circumvention defense (use ai-license-circumvention-defense).
---

# Open-Source Licensing Landscape 2026 — The Data

## Overview

In March 2026, RedMonk (Stephen O'Grady) published the first comprehensive data-driven analysis of the open-source licensing landscape in nearly a decade, comparing historical Black Duck data, the GitHub Archive, and current deps.dev data (a superset of package repositories). This skill distills the quantitative findings into actionable intelligence for A-Tech's license selection, competitive positioning, and community strategy.

The headline finding: **permissive licenses dominate at 73% of licensed projects (down from 82% in 2022), copyleft continues its long decline, and source-available licenses (BSL, SSPL) are statistically irrelevant at scale — though strategically relevant for specific high-profile projects.** A possible AGPLv3 resurgence is the trend to watch.

## When to Use

- Selecting an open-source license based on current ecosystem data rather than ideology
- Evaluating the permissive-vs-copyleft trend for strategic positioning
- Understanding package-repository license distributions (npm, PyPI, Maven, NuGet, etc.)
- Assessing whether source-available licenses (BSL, SSPL) are gaining or losing traction
- Determining the competitive implications of license choices in the 2026 landscape
- Evaluuating the Apache vs MIT choice with current data
- Assessing the AGPLv3 resurgence signal

NOT for:
- BSL decision frameworks and fork economics (use open-source-license-economics-2026)
- AI-era license circumvention defense (use ai-license-circumvention-defense)
- Open-source funding and platformization (use open-source-funding-platformization-2026)
- License strategy for AI model weights (use open-source-license-strategy-ai-era)

## The Data Sources

| Source | Coverage | Status |
|---|---|---|
| **Black Duck** | Historical (pre-2017) | No longer extant |
| **GitHub Archive** | Hosted source code | Available but underwent disruptive dataset changes (small post-2022 samples) |
| **deps.dev** | Packaged/deployed code (superset of package repositories) | Current, active |

**Critical caveat:** In the largest single source (GitHub), 80%+ of projects do not carry licenses and are excluded from the analysis. As code production approaches zero cost (AI code assist), this delta will likely increase. The analysis evaluates available data, not full licensing reality.

## Key Findings

### 1. The Rise (and Slight Retreat) of Permissive Licensing

The industry crossed from copyleft majority to permissive majority sometime between 2014 and 2017. Since then, permissive licenses have dominated.

| Year | Permissive Share | Trend |
|---|---|---|
| 2022 | 82% | Peak |
| 2025 | 73% | Slight retreat (possible sampling issue OR early AGPLv3 resurgence) |

The 2022 → 2025 decline (82% → 73%) could be:
- A sampling artifact (GitHub Archive post-2022 sample sizes are unusually small; RedMonk is still investigating)
- An early signal of copyleft resurgence (several major projects returned to AGPLv3 — Elastic, Redis)
- The deps.dev packaging data shows no similar shift (more stable signal)

**For A-Tech:** The permissive dominance (73%) means the default expectation in the ecosystem is permissive. Choosing copyleft or source-available requires explicit justification and may face community resistance.

### 2. Apache vs MIT — The Permissive Internal Dynamic

The two primary beneficiaries of the permissive shift: Apache Software License and MIT.

| License | Strength | Best For |
|---|---|---|
| **Apache 2.0** | Patent termination provisions (minimize patent infringement litigation risk) | Commercial open source, enterprise usage, AI models (TensorFlow, PyTorch, Gemma, Qwen, Mistral) |
| **MIT** | Simplicity, no patent provisions | Libraries, small projects, npm ecosystem |

**The 2015 inflection:** The CNCF (favored Apache licenses) and popular Apache-licensed projects (TensorFlow 2015, PyTorch 2016) drove Apache's share up to ~30% by 2022, at the expense of MIT.

**The 2023 reversal:** Apache usage dropped dramatically while MIT spiked — likely an artifact of small post-2022 GitHub Archive samples and npm's outsized weight in the deps.dev data (npm defaults to ISC/MIT). The deps.dev data (more stable) does not show the same dramatic swing.

**For A-Tech:** Apache 2.0 remains the enterprise standard for AI projects. MIT is safe for libraries. The "Apache for models, MIT for libraries" heuristic holds.

### 3. Package-Ecosystem License Distribution

License distribution varies significantly across package repositories:

| Repository | Notable Patterns |
|---|---|
| **npm** | Heavily skews toward ISC (historical `npm init` default); MIT also common; massive weight in deps.dev (~3x all other repos combined) |
| **Maven** | Solidly in Apache's orbit (Java ecosystem) |
| **NuGet** (.NET) | >50% of packages have licenses that don't map to SPDX identifiers — unclassifiable |
| **PyPI, Go, Cargo, RubyGems** | Generally skew permissive |

**The packaging filter:** Packaged code (deps.dev) shows higher rates of permissive license usage than hosted source code (GitHub). GPL licenses are 34x more common on GitHub than in deps.dev — predictable given deployed code's preference for permissive licenses.

**For A-Tech:** When analyzing competitor or ecosystem license distributions, check whether the data reflects hosted source (GitHub) or packaged deployments (deps.dev). The packaging filter matters.

### 4. Source-Available Licenses (BSL, SSPL) — Statistically Irrelevant

| Finding | Detail |
|---|---|
| Statistical significance | Source-available licenses (BSL, SSPL, Elastic License, RSAL) are **not measurable in a statistically significant way** |
| Trend | They remain extremely uncommon and are not trending upward |
| Strategic relevance | Still relevant because of the high-profile projects that carry them (MongoDB, Terraform, Redis) |
| Notable reversals | Elastic and Redis returned to AGPL (OSI-approved) — SSPL/RSAL didn't prevent forks and didn't sustain community trust |

**For A-Tech:** Source-available licenses are a high-risk, low-adoption path. The data confirms that the vast majority of the ecosystem rejects them. The projects that adopted them (MongoDB, HashiCorp) faced community forks. The projects that returned to open source (Elastic → AGPL, Redis → AGPL) suggest that the community pressure toward OSI-approved licenses is strong.

### 5. The AGPLv3 Resurgence Signal

Several major projects returned to AGPLv3 from source-available licenses:
- **Elastic** (2024): SSPL → AGPLv3. SSPL didn't prevent the OpenSearch fork; AGPL's stronger copyleft proved a better deterrent.
- **Redis** (2025): RSAL/SSPL → added AGPL as option. Valkey fork already fragmented the ecosystem.

**Why AGPL:** Strong copyleft forces anyone offering the software as a service to open-source their modifications — most effective against hyperscalers, but discourages some enterprise adoption.

**The possible trend:** If the permissive decline (82% → 73%) is real (not just sampling noise), it may reflect early adoption of AGPLv3 by projects seeking hyperscaler protection without going source-available. This is the trend to watch.

**For A-Tech:** AGPLv3 is a viable defensive option for A-Coder's SaaS-facing components, but it's a niche choice — 73% of the ecosystem is still permissive. Default to Apache 2.0; consider AGPL only for specific SaaS-protected components where hyperscaler appropriation is a realistic threat.

## The 2026 Licensing Landscape Summary

| Category | Share | Trend | A-Tech Implication |
|---|---|---|---|
| Permissive (MIT, Apache, BSD, ISC) | ~73% | Dominant, slight retreat | Default choice; maximum adoption |
| Copyleft (GPL, AGPL, LGPL) | ~27% | Long decline, possible AGPL resurgence | Consider AGPL for SaaS-protected components |
| Source-available (BSL, SSPL, RSAL) | <1% (not measurable) | Not trending | High-risk, low-adoption; avoid |
| Unlicensed (no license) | 80%+ of all projects | Growing (AI code generation) | Never ship without a license; always specify |

## Practical Frameworks for A-Tech

### Framework 1: The Data-Driven License Selector

Based on the 2026 data, the license selection hierarchy is:

1. **Default to Apache 2.0** for AI models, enterprise software, and any project where patent protection matters. This is the 73% majority choice and the enterprise standard.
2. **Use MIT** for libraries, small utilities, and npm packages. Simpler than Apache, no patent provisions, maximizes adoption.
3. **Consider AGPLv3** only for SaaS-facing components where hyperscaler appropriation is a realistic threat AND you're willing to accept some enterprise adoption friction. This is the emerging defensive choice.
4. **Avoid BSL, SSPL, RSAL** — statistically irrelevant at scale, high community resistance, fork-prone. If you need SaaS protection, AGPL is the evidence-backed alternative.
5. **Never ship without a license.** 80%+ of projects are unlicensed; always specify your license explicitly.

### Framework 2: The Competitor License Intelligence Pattern

When evaluating a competitor's license:
1. Check if it's OSI-approved (permissive or copyleft) or source-available (BSL, SSPL, RSAL)
2. If source-available: assess fork risk and community sentiment (check for forks, GitHub issues about licensing)
3. If OSI-approved: assess permissive vs copyleft positioning for competitive differentiation
4. Check the package repository (deps.dev) for the deployed distribution, not just the GitHub repo

### A-Tech Application Matrix

#### A-Coder
- **Core IDE: Apache 2.0** — consistent with the 73% permissive majority; enterprise-friendly; patent protection
- **Enterprise plugins: Proprietary** — clear value differentiation
- **SaaS-protected components (if any): Consider AGPLv3** — only for components where hyperscaler re-hosting is a realistic threat
- **Never use BSL/SSPL** — the data shows it's a high-risk, low-adoption path with fork-prone outcomes

#### Be Practical
- **Curriculum module:** "The 2026 open-source licensing landscape." The data: 73% permissive, 27% copyleft, source-available is statistically irrelevant. The AGPLv3 resurgence signal. The Apache vs MIT choice. The packaging filter.
- **The key insight for learners:** License choice is not ideology — it's data. The ecosystem has voted with its code: permissive wins. But know when copyleft (specifically AGPL) is the defensive choice.

#### Builder's Club
- **License decision template:** A community template for the data-driven license selector, with the 2026 landscape data as reference.
- **Ecosystem license audit:** Community members audit their own projects' license choices against the 2026 data. Are they following the majority (permissive) or making a deliberate copyleft choice?
- **AGPLv3 adoption tracker:** Track which community projects adopt AGPLv3 and whether the resurgence signal strengthens. Community data complements the RedMonk analysis.

## Cross-References

- **`open-source-license-economics-2026`** — BSL decision framework, fork economics, commoditize-your-complement. This skill provides the quantitative landscape data that informs those decisions.
- **`open-source-license-strategy-ai-era`** — License strategy for AI model weights. This skill provides the broader landscape context.
- **`ai-license-circumvention-defense`** — Defending against AI-enabled copyleft circumvention. This skill's data on the AGPLv3 resurgence informs the defense strategy.
- **`open-source-funding-platformization-2026`** — Funding trends and license strategy. This skill's data on permissive dominance complements the investor preference for permissive + dual-license mixes.
- **`dual-license-monetization`** — Dual-licensing business model. This skill's data supports the Apache 2.0 core + proprietary enterprise pattern.

## Limitations and Caveats

- **80%+ of projects are unlicensed** and excluded from the analysis. The data represents licensed projects only, not the full licensing reality.
- **No single source of truth.** Datasets have changed over time (GitHub Archive), some are no longer available (Black Duck), and none have insight into software behind enterprise firewalls.
- **Post-2022 GitHub Archive samples are unusually small.** The permissive decline (82% → 73%) may be a sampling artifact. The deps.dev data (more stable) doesn't show the same shift.
- **npm is overrepresented in deps.dev** (~3x all other repos combined), which biases toward ISC/MIT permissive licenses.
- **NuGet has >50% unclassifiable licenses** — the .NET ecosystem's license distribution is poorly understood.
- **Source-available license data is limited.** BSL/SSPL projects may not consistently appear in GitHub or package repository datasets because they're not open source.

## References

- **O'Grady, S. (March 25, 2026).** "The State of Open Source Licensing in 2026." RedMonk / tecosystems. https://redmonk.com/sogrady/2026/03/25/open-source-licensing-2026/
- **Data sources:** Black Duck (historical, no longer extant), GitHub Archive (disrupted dataset), deps.dev (current, active)
- **Historical context:** O'Grady's 2012 "post-open source world" thesis; 2017 Black Duck comparison