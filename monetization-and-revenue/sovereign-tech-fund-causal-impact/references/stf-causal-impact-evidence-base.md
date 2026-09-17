# Sovereign Tech Fund Causal Impact — Evidence Base

## Source

Domenech Burin, L. (2026). "Measuring the Invisible: Evaluating the Impact of Public Funding on Open Source Software." arXiv:2607.05413v1 [cs.CY], June 16, 2026. License: CC BY 4.0. Data Scientist, Sovereign Tech Agency, Berlin, Germany. Code repository: https://github.com/ldmnch/thesis_mds_2026

## Context: The Sovereign Tech Agency (STA)

- **Origin:** The Sovereign Tech Fund (STF) program launched in 2022 by the German Federal Ministry for Economic Affairs and Climate Action (BMWK), hosted by SPRIND GmbH. Aims to enhance resilience, security, and sustainability of critical open-source digital infrastructure.
- **STF contracts:** Maintenance and development contracts starting at €50,000, no upper limit, 6-24 month duration.
- **November 2024 transition:** STF became the Sovereign Tech Agency (STA), expanding to a portfolio of three complementary programs:
  1. **Sovereign Tech Fund (STF)** — targeted maintenance/development contracts (the program evaluated in this study).
  2. **Sovereign Tech Fellowship** — invests directly in developers (the people behind the code).
  3. **Sovereign Tech Resilience** — identifies and addresses critical vulnerabilities in digital infrastructure.
- **Portfolio size:** 187 projects in the STF portfolio.

## The Motivation: log4j, XZ Utils, and the Tragedy of the Commons

- 96% of codebases contain OSS; 70-90% of any software stack is OSS.
- The 2021 log4j vulnerability (Log4Shell) exposed societal dependence on unpaid OSS maintenance.
- The 2024 XZ Utils backdoor exposed the security consequences of under-maintenance.
- The "free rider" problem: resources offered free, everyone uses them, nobody is incentivized to contribute back.
- Maintainer burnout, security vulnerabilities, and commercial freeriding on volunteer labor are the structural threats the STF addresses.

## Three Funding Models (Literature)

1. **Micro-donations** — GitHub Sponsors etc.; ad hoc, modest, minimal interaction; long-term sustainability uncertain.
2. **Commercial funding** — sponsorship, consortia, employee contribution time, collaboration, hosting.
3. **Public funding** — States and public sector as funders, recognizing OSS funding as digital sovereignty and supply-chain security policy. The STA is the pioneer model.

## The Study Design

### Sample
- **Treated units:** 4 flagship STF projects (12 repositories): PyPI (5 repos), RubyGems (3 repos), Fortran (3 repos), curl (1 repo).
- **Selection rationale:** oldest projects (pilot round — multiple post-funding periods for evaluation); variety of maintenance work types (enables evaluating different metrics).
- **Donor pool:** 62 control repositories, matched via 1-to-1 nearest-neighbor propensity score matching (caliper 0.0001).
- **Donor pool source:** OpenSSF Scorecard dataset (1 million most critical OSS projects by direct dependencies), filtered by criticality and matched on package metadata from ecosyste.ms (287M+ repositories indexed).

### Matching Covariates
- Dependent repositories count
- Dependent packages count
- Stargazers count
- Forks count
- Average ranking (usage-based package ranking)
- Private ownership (open but owned by a private company)

### Covariate Balance (PSM via Pymatch)
- SMD reduced from 0.466 (before) to 0.119 (after) — a 74% reduction.
- 3 of 6 covariates achieved |SMD| < 0.10.
- Residual imbalance on popularity metrics (dependent_repos_count, stargazers, forks) addressed by GSCM's interactive fixed effects.

### Data Collection
- Via ecosyste.ms API: commits, pull requests, issues, releases.
- Filtered from 2015 onwards; grouped by quarters.
- 54 pre-treatment periods per repository.
- Treatment timing = quarter of STF contract signing (staggered adoption).
- All outcomes log-transformed (high sparsity); outliers trimmed (SD > 95th percentile).

## Methodology: Generalized Synthetic Control Method (GSCM)

### Why Not Differences-in-Differences (DiD)
- DiD requires parallel trends (untestable; often violated in OSS).
- DiD requires a single unexposed unit approximating the treated unit (hard in heterogeneous OSS).

### Why Not Standard Synthetic Control (SCM)
- SCM is single-treated-unit only; the STF has 12 treated repos with staggered adoption.

### Why GSCM (Xu, 2017)
- Relaxes parallel-trends assumption.
- Unifies SCM with interactive fixed effects (Bai, 2009) — models unobserved time-varying confounders.
- Three steps: (1) fit interactive fixed effects on control units → latent factors; (2) estimate treated-unit factor loadings from pre-treatment outcomes; (3) impute counterfactual using estimated factors and loadings.
- Produces frequentist uncertainty estimates (standard errors, confidence intervals via 1,000 repetitions).
- Draws on all control observations → improved estimation efficiency.

### Augmented Synthetic Control (ASCM) as Robustness
- ASCM (Ben-Michael, Feller, Rothstein, 2021) — doubly robust; ridge regression correction for imperfect pre-treatment fit.
- Tested as robustness; GSCM selected for tighter confidence intervals and stronger theoretical alignment.

### Factor Selection
- GSCM: optimal factors via internal cross-validation (range 3-7 across outcomes).
- ASCM: manual search 1-10 factors, minimizing Panel Criteria (Bai & Ng, 2002).
- Both converge on similar factor counts (divergence ≤ 3), suggesting comparable latent structure.

## Results: Average Treatment Effect on the Treated (ATT)

| Outcome | ATT (log) | p-value | Sig. | % effect |
|---|---|---|---|---|
| Commits | -0.8912 | 0.0064 | ** | +143.8% |
| Merged CRs | -1.0133 | 0.0397 | ** | +175.5% |
| New CRs | -0.8661 | 0.0380 | ** | +137.8% |
| New issues | -1.2501 | 0.0001 | *** | +249.0% |
| Releases | -0.6476 | 0.0585 | (n.s. at 10%) | — |
| Contributors | -0.0283 | 0.9346 | n.s. | — |
| Closed issues | -0.8178 | 0.1559 | n.s. | — |

Significance: *** p < 0.01, ** p < 0.05, * p < 0.10.

Percentage effects derived via (e^δ̂ − 1) × 100 (inverse log transformation); interpreted as multiplicative effects relative to the counterfactual trajectory.

**Significant positive effects:** commits (+143.8%), merged change requests (+175.5%), new change requests (+137.8%), new issues (+249.0%).
**No significant effects:** releases, contributors, closed issues.

### Interpretation
- **Velocity stimulus:** funded projects have substantially higher development activity than they would have without funding.
- **No contributor expansion:** the activity is driven by existing contributors, not new ones.
- **No backlog resolution:** closed issues did not increase; the new-issue surge may outpace closing capacity.
- **No release acceleration:** funding did not translate into more frequent releases.

### Confidence Interval Width
Wide CIs across significant metrics reflect heterogeneity among projects. The ATTs are "directional signals of the positive effect of funding rather than precise estimates."

## The Four Project Cases

### PyPI (5 repos)
- Python package index: 1,046,786 users, 794,245 projects.
- STF funded critical engineering, maintenance, support to PyPI and highly-used critical packages.
- Work focused on implementing new features to strengthen the ecosystem.

### curl (1 repo)
- Command-line tool and libcurl library; 20+ billion installations; used in cars, TVs, routers, printers, medical devices, phones.
- STF funded maintenance via technical backlog: resolved 122 known bugs through systematic classification and fixes; introduced new options for newer HTTP protocol versions.

### Fortran (3 repos)
- Programming language for computationally intensive science/engineering applications: numerical weather/ocean prediction, CFD, applied math, statistics, finance.
- STF funded modernization: compiler and package manager improvements, ensuring continued usability for contemporary scientific computing.

### RubyGems (3 repos)
- RubyGems.org and Bundler: library/package management for Ruby, included in every Ruby copy.
- STF funded community maintenance strengthening: reliability, security, scalability of RubyGems.org; maintainer quality-of-life improvements (automation, safer defaults, better workflows).

## The Goal-Question-Metric (GQM) + CHAOSS Chain

- **GQM** (Van Solingen, Basili, Caldiera, Rombach, 2002): start with qualitative assessment of funded work packages → scope to broad goals → refine to specific questions → translate to measurable metrics.
- **CHAOSS** (Community Health Analytics Open Source Software, Linux Foundation): the metric dictionary. Metrics used:
  - Issues New, Issues Closed (CHAOSS)
  - Change Requests, Change Requests Accepted (CHAOSS)
  - Code Changes Commits (CHAOSS)
  - Committers (CHAOSS)
  - Release Frequency (CHAOSS)
- **The chain ensures every metric is directly traceable to the work each project committed to deliver** — not a uniform metric set applied blindly.

## The AI-Noise Caveat (New Issues)
The study notes: "the rise of AI-generated noise in issue trackers has added a further source of noise to this metric." This connects to the `open-source-maintainer-ai-burden` skill. Future evaluations should filter AI-generated issues or treat them separately to avoid conflating AI noise with community engagement.

## Discussion and Limitations

### Control Group Construction
- PSM + GSCM mitigates observable confounding; unaddressed dimensions: project vulnerability, lifecycle stage, salary/cost heterogeneity across regions.
- Ideal donor pool: projects shortlisted by STA but excluded due to financial constraints (stronger causal basis; not feasible due to ethical/consent constraints).
- Future: additional proxies (core maintainer : lines-of-code ratio as structural fragility; repository creation date as lifecycle proxy).

### Outcome Metric Selection
- Funded work packages differ across projects (backlog resolution vs. new features vs. security hardening).
- Same metric may capture different processes depending on project — threat to internal validity.
- Future: stratify by contract type; construct outcome-specific indices tailored to each funding category.
- Challenge: disaggregating by funding objective complicates valid control group construction.

### Model Fit (GSCM)
- Pre-treatment fit acceptable for most outcomes (MSPE in Table 4).
- Commits and Contributors have higher MSPE (0.64, 0.62) — noisier, more sensitive to idiosyncratic events.
- Limited donor pool size is a constraint; larger, more diverse pool would improve fit and reduce ATT uncertainty.

## Policy Implications

1. **Match metrics to program objectives.** Evaluating a fellowship on commit velocity, or a fund on contributor growth, misrepresents impact.
2. **Portfolio logic.** STF's lack of effect on contributors/closed issues may be correct program design — those are the Fellowship's and Resilience program's remits.
3. **Money alone ≠ maintainer capacity.** Funding mobilizes existing contributors but doesn't expand the contributor base or resolve the structural maintainer-capacity constraint.
4. **GSCM + PSM is a viable quasi-experimental framework** for evaluating OSS funding interventions where causal identification has been limited by project heterogeneity and the absence of randomized assignment.
5. **OSS sustainability is multidimensional.** Financial resources interact with organizational, social, and technical factors. Evaluation requires nuanced, evidence-based approaches.

## Relationship to A-Tech Skill Library

- **`open-source-sustainability-infrastructure`** — the structural maintainer-support thesis this study empirically supports: money alone doesn't resolve maintainer capacity. The four sustainability architectures (Open Source Pledge, dual licensing, verified maintainer tipping, community governance) are complementary instruments to public funding.
- **`open-source-maintainer-ai-burden`** — the AI-generated issue noise caveat; the maintainer-capacity constraint that funding doesn't resolve.
- **`open-source-profitability-evidence-framework`** — the investability/exit-plausibility evidence (UNICEF Venture Fund). This skill adds the causal-funding-impact evidence.
- **`open-source-funding-platformization-2026`** — the 2026 funding surge ($5.8B, +42% YoY) and platformization. This skill provides the evaluation methodology for that funding.
- **`open-source-risk-removal-monetization-2026`** — selling risk removal; the STF's Resilience program is institutional risk removal.
- **`open-source-license-economics-2026`** — STF funds permissively licensed critical infrastructure; the licensing landscape context.

## Citation

Domenech Burin, L. (2026). Measuring the Invisible: Evaluating the Impact of Public Funding on Open Source Software. arXiv:2607.05413v1 [cs.CY]. CC BY 4.0. Code: https://github.com/ldmnch/thesis_mds_2026