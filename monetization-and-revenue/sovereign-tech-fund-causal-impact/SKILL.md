---
name: sovereign-tech-fund-causal-impact
description: Apply the first causal-evidence framework for the impact of public funding on open-source software sustainability, based on the Sovereign Tech Agency (STA) portfolio evaluation (Burin, 2026, arXiv:2607.05413, CC BY 4.0). Covers the Generalized Synthetic Control Method with propensity-score-matched donor pools, the finding that STF funding significantly increases project velocity (commits +143.8%, merged change requests +175.5%, new change requests +137.8%, new issues +249%) but has no significant effect on releases, contributors, or closed issues, the Goal-Question-Metric + CHAOSS measurement chain, and the policy implication that funding mobilizes existing development activity rather than expanding the contributor base or resolving backlog. Use when designing or evaluating public open-source funding programs, building an OSS sustainability evidence framework, defending open-source investment to policymakers, or selecting metrics for funding impact assessment. NOT for private/commercial funding models (use open-source-funding-platformization-2026) or for individual maintainer sponsorship (use open-source-sustainability-infrastructure).
---

# Sovereign Tech Fund Causal Impact

## Overview

For the first time, the causal effect of public funding on open-source software sustainability has been empirically estimated using a quasi-experimental framework. Laia Domenech Burin (Sovereign Tech Agency, 2026, arXiv:2607.05413) applied the Generalized Synthetic Control Method with propensity-score-matched donor pools to four flagship projects in the Sovereign Tech Fund (STF) portfolio — PyPI, curl, Fortran, and RubyGems — and found that funding produces statistically significant, large positive effects on project velocity (commits, change requests, merged requests, new issues) but **no significant effect on releases, contributors, or closed issues**. The finding is nuanced and actionable: public funding mobilizes existing development activity but does not, on its own, expand the contributor base or resolve the open backlog. This skill turns that evidence into a practical framework for designing and evaluating public open-source funding programs, and for defending open-source investment with causal evidence rather than descriptive claims.

## When to Use

- Designing or evaluating a public open-source funding program (sovereign tech, government, foundation)
- Building an OSS sustainability evidence framework or impact-measurement methodology
- Defending open-source investment to policymakers, treasuries, or budget committees
- Selecting metrics for funding impact assessment (which metrics match which program objectives)
- Diagnosing why an OSS funding program isn't producing expected outcomes (e.g., contributor growth)
- Designing a portfolio of complementary funding instruments (the STF + Fellowship + Resilience model)
- Building a causal-inference framework for any OSS intervention evaluation

NOT for:
- Private/commercial funding models (use `open-source-funding-platformization-2026`)
- Individual maintainer sponsorship or tip-jar models (use `open-source-sustainability-infrastructure`)
- Open-source business model design (use `open-source-profitability-evidence-framework` or `third-generation-open-source-models`)
- Descriptive project health dashboards (this skill is about *causal* impact, not monitoring)

## The Evidence: What STF Funding Actually Does

The study estimated the Average Treatment Effect on the Treated (ATT) for seven repository-level outcomes across four funded projects, using counterfactual trajectories constructed from a matched donor pool of 62 unfunded projects.

| Outcome | ATT (log) | p-value | Percentage effect | Significant? |
|---|---|---|---|---|
| Commits | -0.8912 | 0.0064 | **+143.8%** | Yes (** at 5%) |
| Merged change requests | -1.0133 | 0.0397 | **+175.5%** | Yes (** at 5%) |
| New change requests | -0.8661 | 0.0380 | **+137.8%** | Yes (** at 5%) |
| New issues | -1.2501 | 0.0001 | **+249.0%** | Yes (*** at 1%) |
| Releases | -0.6476 | 0.0585 | — | No (at 10%) |
| Contributors | -0.0283 | 0.9346 | — | No |
| Closed issues | -0.8178 | 0.1559 | — | No |

**Note on signs:** The ATT values are on a log scale relative to counterfactual; the percentage effects are derived via the inverse-log transformation (e^δ̂ − 1)×100 and represent multiplicative effects relative to the unfunded counterfactual trajectory. The positive percentage effects indicate funded projects had substantially higher activity than they would have without funding.

### The Core Interpretation

STF funding produces a **strong stimulus on new development activity and engagement** — more commits, more pull requests (new and merged), more new issues. But it does **not** significantly increase releases, contributors, or closed issues.

The interpretation: **funding mobilizes existing development activity rather than expanding the contributor base or accelerating backlog resolution.** The increased activity is driven by existing contributors working more, not by new contributors joining or by the backlog shrinking.

### The Two-Interpretation Caveat on New Issues
The +249% increase in new issues admits two readings:
1. **Community engagement** — new developments prompt users to report bugs and request features (a vitality signal).
2. **Expanding backlog** — new issues accumulate without a matching increase in closed issues, suggesting projects struggle to absorb the additional activity.

The data cannot distinguish these; the study flags both. The absence of an effect on *closed* issues leans toward interpretation 2 (backlog growth), but the engagement interpretation cannot be ruled out.

## The Methodology: Why This Is Causal, Not Descriptive

Most OSS impact reports are descriptive (before/after counts, self-reported maintainer satisfaction). This study is the first to apply a **quasi-experimental causal framework** to public OSS funding:

### The Challenge
- OSS projects are heterogeneous (size, age, community, tech stack) — naive treated-vs-untreated comparisons are biased.
- No randomized assignment — funders select projects by criticality, not by lottery.
- The "fundamental problem of causal inference": we can never observe both the funded and unfunded trajectory for the same project.

### The Solution
1. **Propensity score matching** — each funded repository matched to structurally comparable unfunded repositories (by dependents, stargazers, forks, average ranking, ownership). Caliper 0.0001. 62 control repos matched to 12 treated units. SMD reduced 74% (0.466 → 0.119).
2. **Generalized Synthetic Control Method (GSCM)** — relaxes the parallel-trends assumption of differences-in-differences; uses interactive fixed effects to model unobserved time-varying confounders; constructs counterfactual trajectories from the donor pool; produces frequentist uncertainty estimates (standard errors, confidence intervals via 1,000 repetitions).
3. **Goal-Question-Metric (GQM) + CHAOSS** — work packages from each funding contract mapped to goals → questions → CHAOSS community-health metrics, ensuring every measured metric is traceable to the work the project committed to deliver (not a one-size-fits-all metric set).

### Why GSCM Over Alternatives
- Differences-in-differences requires parallel trends (untestable, often violated in OSS).
- Standard synthetic control is single-treated-unit only (the STF has 12 treated repos).
- Augmented synthetic control (ASCM) was tested as robustness; GSCM selected for tighter pre-treatment fit and stronger theoretical alignment with unobserved confounders in OSS repository dynamics.

## The Policy Implication: Match Metrics to Program Objectives

The study's central policy contribution: **assessing interventions using mismatched indicators risks misrepresenting their impact.** Evaluating a fellowship program on commit velocity, or a fund contract on contributor growth, evaluates the wrong thing.

### The STF Portfolio Logic
The STF's *lack* of effect on contributors and closed issues may reflect **program design, not failure** — the STA's three complementary programs have different remits:

| Program | Remit | Expected effect |
|---|---|---|
| **Sovereign Tech Fund (STF)** | Targeted maintenance and development contracts | Commits, PRs, new issues (velocity) — *confirmed* |
| **Sovereign Tech Fellowship** | Invests directly in developers (the people) | Contributor growth, maintainer capacity — *the Fellowship's remit* |
| **Sovereign Tech Resilience** | Identifies and addresses critical vulnerabilities | Security outcomes, closed critical issues — *the Resilience program's remit* |

The absence of contributor growth under STF may be *correct*: that's the Fellowship's job. The absence of closed-issue acceleration may be *correct*: that's the Resilience program's job. This is a portfolio design insight — don't expect a single instrument to produce all outcomes.

### The Deeper Structural Finding
The asymmetry between activity-stimulating and work-resolving effects, alongside the absence of contributor growth, points to: **money alone does not resolve the binding constraint of maintainer capacity.** Funding stimulates work by existing contributors but doesn't, by itself, expand the pool of people who can sustain the project long-term. This validates the broader A-Tech thesis (from `open-source-sustainability-infrastructure` and `open-source-maintainer-ai-burden`) that structural maintainer support — not just project funding — is the missing layer.

## The Measurement Framework: GQM + CHAOSS

The study operationalizes a measurement chain that any funder can adopt:

1. **Goal** — derived from the funded work package (e.g., "strengthen the package ecosystem," "resolve technical backlog," "modernize the compiler").
2. **Question** — refined from the goal (e.g., "did backlog resolution accelerate?", "did new feature development increase?").
3. **Metric** — translated to a CHAOSS community-health metric (commits, new change requests, merged change requests, new issues, closed issues, contributors, releases).

The four metric dimensions:

| Dimension | Metrics | What it captures |
|---|---|---|
| Project activity & engagement | New issues, closed issues | Discussion, problem-solving, community involvement (and backlog) |
| Development & code contribution | Commits, new change requests, merged change requests | Codebase health, contribution frequency and acceptance |
| Community & dynamics | Contributors | Breadth/depth of contributor involvement |
| Project maturity & release activity | Releases | Ability to deliver updates, respond to user needs |

**Principle:** metrics are a compass — they indicate direction, not destination. They must be interpreted alongside qualitative understanding of what each project was funded to do.

## The Caliper on the AI-Noise Caveat
The study notes (in the new-issues metric description) that "the rise of AI-generated noise in issue trackers has added a further source of noise to this metric." This connects to the `open-source-maintainer-ai-burden` skill: AI-generated issues inflate the new-issues count, complicating interpretation of the +249% finding. Future funding-impact evaluations should filter AI-generated issues or treat them separately.

## A-Tech Application Matrix

### A-Coder
- **De-platforming pitch with causal evidence:** "Public funding causally increases open-source project velocity by 138-175% — your investment in open infrastructure is empirically effective, not just ideologically sound." Use the STF ATT numbers as evidence.
- **Open-source sustainability as a product feature:** A-Coder's open-source, local-first architecture aligns with sovereign-tech procurement criteria (digital sovereignty, supply-chain security). The causal evidence supports the pitch.
- **Metric selection for A-Tech's own community:** if A-Tech runs a funding/sponsorship program for A-Coder contributors, use the GQM + CHAOSS chain — don't measure contributor growth if the program funds development work; measure commits/PRs.

### Be Practical
- **Curriculum module:** "Causal Evidence for Open-Source Investment" — the STF study, the GSCM method, the portfolio-logic insight, and the metric-matching principle
- **Case study:** the four STF projects (PyPI, curl, Fortran, RubyGems) — what each was funded to do, what metrics matched, what the causal estimates showed
- **Exercise:** given a hypothetical OSS funding program, design the GQM + CHAOSS metric chain and predict which outcomes will/won't show effects

### Builder's Club
- **Community funding program design:** if Builder's Club operates a community fund for open-source AI developer tools, use the STF portfolio model (development contracts + fellowships + resilience) rather than a single instrument
- **Causal-impact evaluation toolkit:** open-source the GSCM + PSM pipeline (the study's code is at github.com/ldmnch/thesis_mds_2026) as a community resource for evaluating any OSS funding intervention
- **Metric-matching standard:** community-funded projects must declare which CHAOSS metrics match their funded work packages — preventing the mismatched-indicator problem

## Anti-Patterns

| Anti-Pattern | Why It Fails |
|---|---|
| Measuring all funded projects on the same metric set | Mismatched indicators misrepresent impact (the study's central policy warning) |
| Expecting a single funding instrument to produce all outcomes | Portfolio logic: contributor growth is the Fellowship's remit, not the Fund's |
| Treating new-issue growth as unambiguously positive | It may signal backlog growth, not engagement — and AI noise inflates it |
| Descriptive before/after reporting as "impact" | Without a counterfactual, you can't attribute the change to the funding |
| Donor pool of structurally dissimilar projects | Invalidates the synthetic control; produces spurious causal claims |
| Using pure economic valuation (COCOMO, ROI) as impact | Flattens socio-technical dynamics; misses the community dimension |

## Cross-Skill References

- `open-source-sustainability-infrastructure` — the structural maintainer-support thesis this study empirically supports (money alone doesn't resolve maintainer capacity)
- `open-source-maintainer-ai-burden` — the AI-generated issue noise caveat; the maintainer-capacity constraint
- `open-source-profitability-evidence-framework` — the investability/exit-plausibility evidence; this skill adds the causal-funding-impact evidence
- `open-source-funding-platformization-2026` — the 2026 funding surge and platformization era; this skill provides the evaluation methodology for that funding
- `open-source-risk-removal-monetization-2026` — selling risk removal; the STF's resilience program is institutional risk removal
- `open-source-license-economics-2026` — the licensing landscape; STF funds permissively licensed critical infrastructure

## Measurement Framework

| Metric | Target | Method |
|---|---|---|
| Velocity effect (commits, PRs) | Statistically significant positive ATT vs. counterfactual | GSCM with matched donor pool |
| Contributor effect | Measured but not expected to be significant under STF (Fellowship's remit) | GSCM; interpret against program objective |
| Metric-objective match rate | 100% of funded work packages mapped to matching CHAOSS metrics | GQM audit per contract |
| Donor pool validity | SMD < 0.10 on ≥ 50% of covariates post-PSM | Propensity score matching balance table |
| AI-noise-filtered issue counts | AI-generated issues separated from human issues | Issue-tracker AI detection + manual audit |
| Portfolio outcome coverage | Contributor growth covered by Fellowship; critical-issue closure by Resilience; velocity by Fund | Portfolio design audit |

## References

See `references/stf-causal-impact-evidence-base.md` for the full study extraction: design, methodology (PSM + GSCM + ASCM robustness), all ATT results with significance, the four project case details (PyPI, curl, Fortran, RubyGems), the GQM + CHAOSS metric chain, the discussion of limitations (control group construction, outcome metric selection, model fit), the policy implications, and the complete bibliography.