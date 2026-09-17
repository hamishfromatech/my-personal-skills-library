# Habit Degradation Strategies — Evidence Base

## Source

**Edgren, R., Baretta, D., & Inauen, J. (2026).** "Habit degradation strategies promote faster early reductions in unhealthy snacking habit strength in intensive longitudinal randomised controlled trial." *Communications Psychology*, 4, 67. DOI: 10.1038/s44271-026-00432-9. University of Bern. Published March 4, 2026.

Preregistered on OSF (Open Science Framework). Open data and R analysis code available.

---

## Study Design

**3 × 2 factorial + control design**, single-blind randomized controlled trial.

- **Factor 1 (Strategy)**: 3 levels — substitution, inhibition, reduced accessibility
- **Factor 2 (Reward)**: 2 levels — reward present, reward absent
- **Control group**: no strategy instruction, no reward (self-monitoring only)
- **Blinding**: single-blind (participants unaware of other conditions / full hypotheses)
- **Duration**: 13 weeks
- **Total observations**: 13,922 SRBAI (Self-Report Habit Index / habit strength) reports across all participants and time points
- **Preregistration**: OSF

### The Three Degradation Strategies

1. **Substitution** — replace the unhealthy snack with a healthy alternative. Implementation intention: "If I feel the urge to snack unhealthily, then I will eat a healthy alternative instead."
2. **Inhibition** — suppress the urge / inhibit the behavior directly. Implementation intention: "If I feel the urge to snack unhealthily, then I will inhibit/suppress the urge."
3. **Reduced accessibility** — alter the environment so the habit cue or target is less accessible. Implementation intention: "If I feel the urge to snack unhealthily, then I will make sure unhealthy snacks are not available / remove them from my environment."

### Reward Factor
A 2-level reward factor (reward vs no reward) was crossed with strategy. The reward was **externally induced** (provided by the study), contrasting with intrinsic/self-generated reward studied in other habit literature.

---

## Participants

| Characteristic | Value |
|---|---|
| N | 313 |
| Mean age | 32 years |
| Female | 84% |
| Bachelor's degree or higher | 57% |
| Recruitment | Social media |
| Language | German-speaking |

**Limitation note**: The sample is predominantly female (84%) and relatively highly educated (57% bachelor's+). Generalizability to broader populations is constrained.

---

## The Four Outcome Metrics (Mathematical Descriptions)

The study defined four complementary outcome metrics for habit degradation, each capturing a different facet of change dynamics:

### 1. Magnitude of Change
The total reduction in habit strength from baseline to the lower asymptote (or end of observation if no asymptote reached).

- **Computation**: difference between baseline habit strength (intercept of the fitted asymptotic curve, or first observation) and the predicted lower asymptote of the fitted curve.
- **Interpretation**: "How much did the habit weaken in total?"

### 2. Likelihood of Reaching 95% Asymptote
A binary per-person outcome: did the individual's habit strength trajectory reach a stable lower plateau within the observation window?

- **Computation**: fit an asymptotic function to each individual's within-person time series; determine whether the curve reached 95% of its total projected decline within the 13-week window.
- **Interpretation**: "Did the habit actually stabilize at a lower level, or was it still declining at the end?"
- **Result**: of 79 participants with valid asymptotic fits, 66 reached the 95% asymptote.

### 3. Rate of Change (GAM First Derivatives, Weeks 1–2)
The speed of habit strength reduction in the earliest period, extracted from generalized additive models.

- **Computation**: fit a GAM to each individual's within-person time series (habit strength ~ time, with smooth term for time). Extract the **first derivative** of the fitted smooth function at each time point. Aggregate the rate of change over weeks 1–2 (the earliest window).
- **Dynamic knot selection**: the GAM smooth term uses data-driven knot placement to adapt to each individual's trajectory shape.
- **Interpretation**: "How fast was the habit weakening at the very beginning?"
- **This is the metric where the intervention vs control difference was significant (H2.1, p=0.042).**

### 4. Time to Reach 95% Asymptote
The number of days from baseline to the point at which the habit strength entered the 95% asymptote band.

- **Computation**: from the fitted asymptotic curve, solve for the time point at which the curve reaches 95% of its total decline.
- **Interpretation**: "How long did it take for the habit to stabilize?"
- **Result**: mean 22 days, range 1–79 days (highly idiosyncratic).

---

## Results: All 12 Hypotheses Tested

The study tested 12 hypotheses spanning the four outcome metrics across strategy comparisons and the intervention-vs-control contrast. Only **H2.1** was confirmed after multiplicity (Bonferroni) adjustment.

| Hypothesis | Metric | Comparison | Result |
|---|---|---|---|
| **H2.1** | **Rate of change (week 1)** | **Intervention vs control** | **CONFIRMED — p=0.042 (Bonferroni-adjusted). Intervention groups had significantly faster week-1 rate of habit strength reduction than control.** |
| H1.x (magnitude) | Magnitude of change | Strategy vs strategy; intervention vs control | Not significant after adjustment |
| H2.x (rate, other periods) | Rate of change (later weeks) | Intervention vs control; strategy vs strategy | Not significant after adjustment (the rate advantage is concentrated in week 1) |
| H3.x (asymptote likelihood) | Likelihood of reaching 95% asymptote | Strategy vs strategy; intervention vs control | Not significant after adjustment |
| H4.x (time to asymptote) | Time to reach 95% asymptote | Strategy vs strategy; intervention vs control | Not significant after adjustment |
| Reward main effect | Across metrics | Reward vs no reward | Not significant (externally induced reward showed no effect) |
| Strategy × reward interaction | Across metrics | — | Not significant |

**Summary**: Of 12 hypotheses, only H2.1 (week 1 rate of change, intervention vs control) survived Bonferroni correction. No between-strategy differences were significant on any metric.

---

## Asymptotic Model Details

- **Valid for**: 79 participants (those whose time series could be fit with an asymptotic function with acceptable fit)
- **Reached 95% asymptote**: 66 of 79 valid participants
- **Mean time to 95% asymptote**: 22 days
- **Range**: 1–79 days

The wide range (1–79 days) underscores that **habit degradation timelines are highly idiosyncratic**. There is no population-typical stabilization window; individual differences dominate. Any product claiming "habits break in X days" is overgeneralizing.

---

## GAM Analysis Details

- **Valid for**: 250 participants (those with sufficient data density for GAM fitting)
- **Method**: Generalized Additive Model with smooth term for time
- **Dynamic knot selection**: data-driven placement of knots in the smooth term, adapting to each individual's trajectory shape
- **Extraction**: first derivative of the fitted smooth function at each time point → continuous rate-of-change profile per participant
- **Aggregation for hypothesis testing**: rate of change averaged over the week-1 (and weeks 1–2) window

**Methodological contribution**: GAM-based rate-of-change extraction is a **novel habit measurement approach**. Prior habit research largely relied on pre/post comparisons or single-slope estimates. The GAM first-derivative method yields a time-varying rate profile, enabling detection of *when* change is fastest — which is exactly where the intervention effect lived (week 1).

---

## Intervention Fidelity

**35% of implementation intentions did not match the assigned strategy.**

Participants wrote their own implementation intentions based on strategy-specific instructions, but coding of the written intentions revealed that 35% did not cleanly match the assigned strategy. This reflects **blended strategy use in real life** — people naturally combine substitution, inhibition, and environmental modification rather than adhering to a single pure strategy.

**Implications**:
- Intervention fidelity is a real-world constraint, not just a study artifact
- The lack of between-strategy differences may partly reflect blended use (strategies converging in practice)
- Design interventions that are robust to blending rather than assuming pure-strategy adherence
- The intervention-vs-control effect (H2.1) survived despite 35% mismatch, suggesting the benefit of *having any strategy* is robust to imperfect fidelity

---

## Reward Manipulation

**Externally induced reward showed no effect** on any of the four outcome metrics.

This contrasts with the intrinsic reward literature, where self-generated reward / intrinsic motivation is associated with better habit change outcomes. The study's reward was externally provided (by the study protocol), not intrinsic.

**Interpretation**: The source of reward matters. External reward (imposed by an interventionist) does not boost habit degradation; intrinsic reward (self-generated, internally meaningful) may. This aligns with self-determination theory and the broader intrinsic motivation literature.

**Design implication**: Build interventions around intrinsic motivation, not external rewards. Gamification points/badges imposed by a system may replicate the null effect seen here; personally meaningful rewards may not.

---

## Self-Monitoring Effect

**The control group also showed habit decline.**

The control condition received no strategy instruction and no reward — only the daily self-report habit strength measurement (SRBAI). Despite this, the control group's habit strength declined over the 13 weeks.

**Interpretation**: Repeated self-monitoring (daily reporting of one's own habit strength) is itself an active intervention. The act of observing and recording one's habit may increase awareness, disrupt automaticity, or serve as a soft form of inhibition.

**Design implications**:
- Any habit measurement that involves repeated self-report may itself change the habit. This is a confound for evaluation but a feature for intervention design.
- The intervention-vs-control gap (H2.1) is measured *above and beyond* the self-monitoring effect — making it a conservative test.
- In product design, a simple daily check-in ("how strong was your snack habit today?") may be a low-cost active ingredient, not just a measurement tool.

---

## Methodological Contributions

1. **GAM-based rate-of-change as a novel habit measurement** — extracting first derivatives from within-person GAM fits yields a continuous, time-varying rate of change profile. This is more informative than pre/post or single-slope methods and enabled the detection of the week-1-specific intervention effect.
2. **Within-person idiographic analysis approach** — the study models each individual's trajectory separately (asymptotic fit + GAM per person) before aggregating. This respects the high idiosyncrasy of habit change (1–79 day stabilization range) that group-level averages would obscure.
3. **Four-metric outcome framework** — magnitude, asymptote likelihood, rate of change, and time to asymptote jointly capture *whether*, *how fast*, and *when* habit change occurs. Reporting only magnitude (as most studies do) misses the rate-of-change dynamics where interventions have their effect.
4. **Intensive longitudinal design in a real-world RCT** — 13,922 observations over 13 weeks in a non-clinical, community-recruited sample. Demonstrates feasibility of intensive longitudinal methodology in ecologically valid settings.

---

## Limitations

| Limitation | Detail |
|---|---|
| **Self-report measurement** | Habit strength measured via SRBAI (self-report), not behavioral observation or implicit measures. Self-report is subject to social desirability and demand characteristics. |
| **Suboptimal adherence** | Not all participants completed all time points; GAM analysis valid for 250 of 313, asymptotic fit valid for 79 of 313. Attrition and missing data constrain generalizability. |
| **Low event-contingent engagement** | Event-contingent reporting (reporting at the moment of the habit episode) had low engagement; most data came from time-based (daily) reports. |
| **Predominantly female sample** | 84% female. Findings may not generalize to male or gender-diverse populations. |
| **Single habit domain** | Unhealthy snacking only. Generalization to other habit domains (e.g., digital habits, exercise, substance use) is not tested. |
| **Externally induced reward only** | The null reward effect is specific to externally provided reward; intrinsic reward was not manipulated. |
| **Intervention fidelity** | 35% of implementation intentions didn't match assigned strategy; between-strategy null results should be interpreted with this caveat. |

---

## Cross-References to Existing Skills

| Skill | Relationship |
|---|---|
| `rapid-habit-transition-switch` | Covers the rapid transition dynamics of habit change (the "switch" / tipping point). This skill provides the degradation-strategy and rate-of-change measurement complement. |
| `habit-formation-neuroscience-2026` | The neural-circuit mechanism of habit formation (Asaoka et al., ACC→RSC and LOFC→CS pathways). This skill is the behavioral-intervention / degradation counterpart. |
| `dual-pathway-habit-regulation-model` | The two-dissociable-circuits model (strategy vs execution). This skill's behavioral strategies (substitution, inhibition, reduced accessibility) are candidate interventions targeting those circuits, though the link is inferential. |
| `nudge-persistence-technology-adoption` | Nudge persistence over time. The asymptotic + GAM rate-of-change methodology from this study could be directly applied to measure nudge persistence dynamics. |
| `behavior-change-synthesis-2026` | The umbrella 2026 behavior change evidence synthesis. This study is one input. |

---

## Citation

Edgren, R., Baretta, D., & Inauen, J. (2026). Habit degradation strategies promote faster early reductions in unhealthy snacking habit strength in intensive longitudinal randomised controlled trial. *Communications Psychology*, 4, 67. https://doi.org/10.1038/s44271-026-00432-9

**Affiliation**: University of Bern.
**Published**: March 4, 2026.
**Preregistration**: OSF (Open Science Framework).
**Open data**: OSF. **Open code**: R analysis scripts available.