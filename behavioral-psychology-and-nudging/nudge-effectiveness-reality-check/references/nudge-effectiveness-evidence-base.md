# Nudge Effectiveness Reality Check — Evidence Base

## Source 1: Hu, Xia, Guo, Lu, Constantino, Ju (2025)
**Title:** "Assessing Nudge Impact: A Comprehensive Second-Order Meta-Analysis"
**Journal:** Journal of Behavioral Decision Making
**DOI:** 10.1002/bdm.70053

### Study Design
- Second-order meta-analysis (meta-analysis of meta-analyses)
- 13 articles (14 meta-analyses) included
- 1,638 primary studies synthesised
- ~30 million participants total
- Methodological quality assessed with AMSTAR 2

### Key Findings
- **Raw aggregated effect size: d = 0.27 (95% CI [0.16, 0.38])**
- **Bias-adjusted effect size: d = 0.004** (after adjusting for publication bias)
- Most meta-analyses rated "low" or "critically low" quality on AMSTAR 2
- Authors caution their findings inherit the limitations of the included meta-analyses
- Underscores "urgent need for higher quality, preregistered meta-analyses"

### Interpretation
The near-zero bias-adjusted effect does NOT mean nudges never work. It means the *average published effect* is almost entirely attributable to publication bias (the tendency of journals and authors to publish positive results and suppress null/negative results). The true effect distribution is wide: some nudges work, some do not, some backfire — and the average across all of them is close to zero once bias is removed.

### Related Critical Studies
- Szászi et al. (2022, PNAS): "No reason to expect large and consistent effects of nudge interventions" — methodological critique reaching a similar conclusion.
- Maier et al. (2022, PNAS): "No evidence for nudging after adjusting for publication bias" — a parallel finding using robust Bayesian meta-analysis.
- Beermann et al. (2024, ICIS): "How Effective Are Digital Green Nudges?" — 159 effect estimates, >1M observations, 67 studies; no significant average effect after publication-bias adjustment; substantial heterogeneity not explained by nudge category or personalization.

---

## Source 2: Mertens, Herberz, Hahnel, Brosch (2022)
**Title:** "The effectiveness of nudging: A meta-analysis of choice architecture interventions across behavioral domains"
**Journal:** Proceedings of the National Academy of Sciences, 119(1), e2107346118
**DOI:** 10.1073/pnas.2107346118

### Study Design
- Three-level meta-analytic model with random effects on treatment and publication level
- 455 effect sizes from 214 publications
- N = 2,149,683 participants
- Cluster-robust standard errors for dependent effect sizes
- Münscher et al. taxonomy: decision information, decision structure, decision assistance

### Overall Effect
- **Cohen's d = 0.45 (95% CI [0.39, 0.52])**, small-to-medium
- Robust to outlier removal (d = 0.42) and leave-one-out analyses (d = 0.43–0.46)
- Total heterogeneity τ² = 0.23 (considerable variability)

### Publication Bias
- Egger's test: b = 2.28 (p < 0.001) — significant publication bias
- One-tailed bias toward positive results in low-power studies
- Moderate bias assumption: effect attenuated 26.79% to d = 0.31
- Severe bias assumption: effect attenuated to d = 0.03
- Conclusion: true effect likely smaller than d = 0.45 but not zero

### Technique Category Effects (the hierarchy)
| Category | d | 95% CI |
|---|---|---|
| Decision structure | 0.55 | [0.45, 0.64] |
| Decision information | 0.38 | [0.29, 0.47] |
| Decision assistance | 0.31 | [0.23, 0.39] |
- Decision structure > decision information (b = 0.17, p = 0.02)
- Decision structure > decision assistance (b = 0.24, p < 0.001)
- No difference between information and assistance (p = 0.25)

### Technique-Level Effects
| Technique | d | 95% CI |
|---|---|---|
| Defaults | 0.62 | [0.50, 0.73] |
| Composition / range | 0.55 | [0.22, 0.88] |
| Effort / friction | 0.43 | [0.19, 0.67] |
| Consequence / micro-incentives | 0.43 | [0.29, 0.58] |
| Social reference / norms | 0.40 | [0.29, 0.51] |
| Visibility | 0.36 | [0.26, 0.45] |
| Translation / reframing | 0.31 | [0.18, 0.43] |
| Reminders | 0.30 | [0.20, 0.39] |
| Commitment devices | 0.30 | [0.13, 0.46] |

### Domain Effects
| Domain | d | 95% CI |
|---|---|---|
| Food | 0.72 | [0.49, 0.95] |
| Prosocial | 0.44 | [0.29, 0.59] |
| Environment | 0.43 | [0.32, 0.54] |
| Health | 0.34 | [0.22, 0.45] |
| Other | 0.29 | [0.05, 0.54] |
| Finance | 0.25 | [0.12, 0.37] |
- Food significantly larger than all other domains (all contrasts p < 0.05)
- Finance smallest; consistent with "high-stakes → less nudgeable" hypothesis

### Contextual Study Characteristics
- Location (US vs non-US): no significant difference (p = 0.599)
- Population (adults vs children): no significant difference (p = 0.258)
- Experiment type (lab, artifactual field, framed field, natural field): no difference (p = 0.846)
- Year of publication: more recent studies report smaller effects (p < 0.001) — consistent with declining-effect phenomenon as replication standards improve

### Backfire Prediction
- 95% prediction interval: [–0.48, 1.39]
- ~15% of interventions backfire (reduce or reverse desired behavior)
- Implication: a substantial minority of nudges produce negative effects; deploy with monitoring

### Theoretical Explanation for Technique Hierarchy
- Decision information and assistance rely on elaborate information processing (encoding, evaluating, integrating personal values/goals) — exceeds cognitive capacity more often under load.
- Decision structure provides a "general utility boost" — a cognitive shortcut that does not require deliberative evaluation.
- Decision structure is less susceptible to individual differences in values and goals, so it affects a larger share of the population.

---

## Source 3: de Ridder, Kroese, van Gestel (2022)
**Title:** "Nudgeability: Mapping Conditions of Susceptibility to Nudge Influence"
**Journal:** Perspectives on Psychological Science, 17(2), 346–359
**DOI:** 10.1177/1745691621995183

### Nudgeability Framework
Defines the conditions under which individuals are susceptible to nudge influence. Key conditions:
- **Decision stakes:** Low-stakes decisions are more nudgeable.
- **Attention at decision point:** Low-attention, habitual decisions are more nudgeable.
- **Domain knowledge:** Low-knowledge decisions (consumer cannot self-optimise) are more nudgeable.
- **Behavioral frequency:** Frequent, habitual behaviors are more nudgeable.
- **Goal conflict:** When the nudge aligns with an existing goal, it is more effective.
- **Cognitive load:** High cognitive load makes structural shortcuts more valuable.

### Implication
Nudgeability is not a property of the nudge alone; it is an interaction between the nudge, the behavior, the context, and the individual. Designers must assess nudgeability before committing to a nudge intervention.

---

## Source 4: Chater & Loewenstein (2022)
**Title:** "The i-frame and the s-frame: How focusing on individual-level solutions has led behavioral public policy astray"
**Journal:** Behavioral and Brain Sciences, 46, e147
**DOI:** 10.1017/S0140525X22002023

### i-frame vs s-frame Distinction
- **i-frame:** Interventions targeting individual behavior (nudges, defaults, framing).
- **s-frame:** Interventions targeting the system/structure that produces the behavior (pricing, regulation, availability, defaults at the policy level).
- Critique: The nudge field has over-invested in i-frame interventions because they are politically easier, even when s-frame interventions would be more effective.
- Implication: Before designing a nudge, ask whether the target behavior is primarily caused by an individual bias or a structural problem. If structural, a nudge may be the wrong tool.

---

## Source 5: Digital Nudge Cancer-Screening Meta-Analysis (Wang et al., 2025)
**Title:** "Evaluating digital nudge interventions for the promotion of cancer screening behavior: a systematic review and meta-analysis"
**Journal:** BMC Medicine, 23, 214
**DOI:** 10.1186/s12916-025-04028-8

### Findings
- 14 RCTs, 11 in meta-analysis, 4,477 individuals
- **OR = 1.81 (95% CI 1.35–2.44, p < 0.001)** for cancer screening uptake
- Multicomponent interventions (OR = 2.65) outperformed single-component (OR = 1.54)
- After excluding high-risk-of-bias studies: OR = 1.39, I² dropped from 75% to 42%
- Most common nudge: defaults (9/14 studies); aligned with System 1 thinking
- MINDSPACE framework applied: 8 of 9 nudge types used; "ego" never used

### Implication
Even in a domain where nudges show a statistically significant effect, the effect is modest (OR ≈ 1.4 after bias correction) and varies by technique. Multicomponent > single-component. This is consistent with the Mertens hierarchy: defaults (structure) dominate.

---

## Source 6: Cardiometabolic Nudge Meta-Analysis (Yu et al., 2025)
**Title:** "The effectiveness of nudge-based interventions on self-monitoring behaviours among patients with cardiometabolic diseases"
**Journal:** Health Psychology Review
**DOI:** 10.1080/17437199.2025.2532017

### Findings
- 35 RCTs
- Hedge's g = 0.56 (95% CI [0.44, 0.69]) for self-monitoring behaviors
- Reduced HbA1c (MD = –0.50), systolic BP (MD = –4.47), diastolic BP (MD = –2.02)
- Effect varied by delivery mode, components, and duration

### Implication
In a health-behavior domain with structured self-monitoring, nudges show a medium effect (g ≈ 0.56). This is higher than the Mertens health-domain average (d = 0.34), likely because the interventions were multicomponent and targeted a specific, frequent, measurable behavior (self-monitoring), which is highly nudgeable.

---

## Cross-Reference Network

This skill connects to and calibrates the following existing A-Tech skills:
- `optimal-nudging-resource-rational-framework` — provides the computational model; this skill provides the empirical effect-size baseline.
- `bottom-nudge-analysis-framework` — provides the mechanism typology; this skill provides the effect-size magnitudes per mechanism.
- `digital-nudging-ethical-persuasion` — provides the ethical guardrails; this skill provides the "do not over-promise" guardrail.
- `nudge-disclosure-transparency-effectiveness` — provides the transparency evidence; this skill confirms transparent nudges can be effective.
- `boosting-empowering-behavior-change` — the boost alternative when nudgeability is low.
- `boosts-vs-nudges-public-preference` — the public-preference evidence for choosing boosts over nudges.
- `hyper-nudging-ai-personalization-ethics` — the personalization layer; this skill warns that personalized nudges still face the publication-bias ceiling.
- `behavior-change-synthesis-2026` — the broader synthesis; this skill is the empirical-foundation sub-module.

## Grep-Confirmation of Novelty
- "second-order meta-analysis" — no matches in `/home/user/.skills`
- "nudgeability" — no matches
- "publication bias" — 3 matches in unrelated contexts (privacy taxonomy, BOTTOM evidence, nudge-disclosure references), none providing the Hu et al. second-order meta-analysis or the Mertens technique hierarchy as a standalone skill
- "i-frame" / "s-frame" — no matches
- The combination of (a) the d=0.27→0.004 second-order finding, (b) the Mertens technique/domain hierarchy, (c) the nudgeability framework, and (d) the backfire prediction interval as a single decision-support skill is novel.