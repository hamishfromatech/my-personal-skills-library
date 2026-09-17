---
name: subgroup-concentrated-dp-leakage-audit
description: Applies "Subgroup Membership Inference Audits of Differentially Private Synthetic Text" (Yidan Sun et al., arXiv:2609.09848, Sept 2026; 32 proxies × 4 datasets × 3 generators × 5 privacy budgets) as the SUBGROUP-CONCENTRATED-LEAKAGE pattern — under DP, leakage concentrates: a tenth of records carries roughly 40% of the residual leakage; random-record audits systematically underestimate leakage for high-risk subgroups; and which records leak is a property of the release mechanism, not the record, so record-level risk cannot be assessed independently of the release. Use when auditing synthetic-data releases, designing subgroup-aware privacy audits, or assessing DP guarantees beyond the average case. NOT for [average-case membership-inference benchmarking (the point is that average-case hides the risk), DP mechanism design, or FL optimizer selection (use the DP-FL optimizer family)].
---

# Subgroup-Concentrated Leakage: DP Protects the Average, Not the Vulnerable

## Overview
Yidan Sun, Viktor Schlegel, Srinivasan Nandakumar, Siew Kei Lam & Anil Anthony Bharath (arXiv:2609.09848, Sept 2026) formalize the audit game the library's DP-synthetic-data family has been missing: average-case membership-inference audits are structurally blind to the records that matter. Defining a **subgroup-targeted membership inference game** — the target pool is an explicit parameter — and instantiating it across 32 proxies, four datasets, three generators (DP-SGD fine-tuning, API-based prompting, activation steering), and five privacy budgets, the audit shows synthetic releases **leak subgroup membership, prior attacks systematically underestimate it, DP works at the aggregate level while leaving concentrated residual risk, and which records leak is a property of the release mechanism itself**. The headline numbers: **under DP, a tenth of the records carries roughly 40% of the leakage**, and DP removes more leakage from random records than from high-risk ones — protection is real but uneven, and the unevenness is the finding.

## The Evidence Base
- **Design:** subgroup-targeted MIA game; 32 proxies under three attacker-knowledge scenarios; DP-SGD fine-tuning vs API-based prompting vs activation steering; five (ε, δ) budgets; a merged-pool audit scoring both random and high-risk records against shared negatives to confirm the result at record level.
- **Results:** (1) synthetic releases leak subgroup membership; (2) average-case prior attacks underestimate it; (3) DP substantially reduces average leakage at every budget — effective in aggregate; (4) residual leakage is **concentrated**, not spread (~10% of records ≈ 40% of leakage); (5) DP's in-practice protection is **uneven** (more removal from random than high-risk records); (6) record-level leakiness is mechanism-dependent — it cannot be assessed independently of the release pipeline.

## Core Findings (the subgroup-audit pattern)
1. **Average-case audits are the wrong instrument.** Random-record MIA scores are the privacy-theater baseline; subgroup-targeted games expose what random sampling hides.
2. **Concentration is the DP boundary condition.** The worst-case guarantee holds; the realized distribution of leakage is heavy-tailed — the vulnerable decile carries most of the residual risk. Privacy budgets should be reported alongside a leakage-distribution summary, not just a mean.
3. **Mechanism-dependence is the design constraint.** The same record can be safe under one release pipeline and exposed under another; record-level risk assessment without pipeline context is meaningless — audit the release, not the record.
4. **Generator choice is a privacy decision.** DP-SGD fine-tuning vs API prompting vs activation steering produce different leakage distributions at the same budget — the generator is part of the privacy surface, not just the utility function.

## When to Use
- Auditing any synthetic-data release before publication: add a subgroup-targeted MIA pass and report the leakage distribution, not the average.
- Writing or reviewing DP claims: "DP reduces average leakage" is true and insufficient — the citable boundary is concentration.
- Designing privacy-preserving data-sharing products where vulnerable subgroups (medical, legal, small demographic groups) are the high-risk records.

## NOT For
- Average-case MIA benchmarking or leaderboard-style attack comparisons (the paper's point is that those instruments hide the risk).
- DP mechanism design or accountant selection — this is an audit method over released outputs, not a training-time mechanism.
- Federated-learning optimizer selection (held by the DP-FL optimizer family; this skill holds the release-side audit lens).

## Core Process / Workflow
1. **Parameterize the target pool.** Choose the subgroup definition that matters for the release (small cohorts, minority records, distinctive attributes) — not a random sample.
2. **Run the subgroup game.** Instantiate the attack under at least two attacker-knowledge scenarios; score the target pool and a random pool against shared negatives (merged-pool discipline).
3. **Report the distribution.** Publish leakage concentration (e.g., top-decile share) alongside the mean; a mean-only report is an incomplete audit.
4. **Bind the audit to the release mechanism.** Re-run whenever the generator, fine-tuning recipe, or budget changes; record-level clearance does not transfer across pipelines.
5. **Pair with canary-style audits.** For protocol-valid manipulation surfaces (shared banks, vote aggregation), combine with the canary-probe-audit pattern — realized-vs-promised gap reporting.

## A-Tech Alignment
- **Privacy/sovereignty:** the audit lens hardens Sovereign Data Center in a Box — synthetic releases from sovereign datasets should ship with a subgroup-leakage summary as part of the trust story.
- **Open source:** the audit method is a protocol any operator can run; publishing leakage distributions is an open-practice differentiator for open-source AI infrastructure.
- **Practical:** the six findings give A-Tech's privacy content its first quantitative "DP is necessary but not sufficient" evidence print for text generation.

## Honesty Caveats
- Text-domain instantiation; image and tabular subgroup dynamics may differ.
- Generator set covers three families; production stacks (RAG-augmented synthetic pipelines, multi-stage distillation) are outside the audit's coverage.
- The concentration fraction (~10%/~40%) is dataset- and generator-specific, not a universal constant.
- Subgroup definition choices shape results; the paper instantiates scenarios but the pool-selection discipline remains a design decision.

## Pairs-with
`canaries-in-the-bank-pe-audit` (the protocol-aware canary audit — complementary realized-vs-promised framing), `differential-privacy-synthetic-data`, `dp-sgd-audit-families` (if present; else the DP-SGD lineage skills), `federated-learning-for-privacy-preserving-ai`, `topology-betrays-privacy-sa-attack` (the attack-side complement), `google-parfait-open-privacy-ai-stack`.

## References
- Yidan Sun, Viktor Schlegel, Srinivasan Nandakumar, Siew Kei Lam, Anil Anthony Bharath (arXiv:2609.09848, Sept 2026), "Subgroup Membership Inference Audits of Differentially Private Synthetic Text."
- Carlini et al. 2022 (membership inference from first principles); Lokna et al. Delta-Siege (group-and-attack audits); Steinke et al. 2023 (one-run auditing).

*Created: 2026-09-17 (Cycle 28, Run 3) — A-Tech Research Division*