# Engagement-Gated Nudging — Full Evidence Base

## Study 1: Recife Digital Nudges RCT

**Citation**: Elacqua, G., Kutscher, M., Nascimento, D., Dias, I., Margitic, J.F. "The Promise and Limits of Digital Nudges: Personalized School Recommendations in Recife's Centralized Admission Platform." IDB (published Nov 25, 2025; doi:10.18235/0013854).

### Design
- Context: Matrícula Online — Recife's centralized admission platform (2022 transition from first-come-first-served to deferred-acceptance allocation). 2023 enrollment process.
- Sample: 14,772 students raw → 8,631 analyzed (12,034 program-school applications). Randomization: seeded, backend, sticky per user; three arms — Control, T1 Quality (top 40% IDEPE by proximity, ≤3km; thresholds 4.98 primary / 4.2 upper elementary), T2 Distance (proximity only). Carousel: five schools at a time, activated by first school click ("trigger school").
- Recommendation quality: T1 — 76.1% of recommendations higher IDEPE than trigger school; 75.9% closer. T2 — 58% closer.
- Compliers ("recommendation takers"): added ≥1 recommended school to application list. T1: 402 (14%); T2: 708 (24%). Total 1,110.

### Engagement Reality (the constraint)
- 99% of users logged in once; ~3 minutes average platform time; 75.6% had already decided on a school before logging in (survey); ~73.5% applied to only one school; carousel appeared after first selection.
- Prior research echo: 2022's first-come-first-served history trained urgency-driven, single-option behavior — a system-adoption lag.

### Results
- **ITT (Panel A)**: null or modest across school-quality (top-1 IDEPE, mean IDEPE, top-40% share), proximity (top-1 and mean distance), and application-count outcomes. Quality treatment: −2.6pp seat-securing (significant; interpret via general-equilibrium allocation). First-grade subsample: only average-distance reduction (−178m, quality arm).
- **LATE (Panel B)**: slightly larger, still insignificant (low compliance keeps LATE ≈ ITT average).
- **Complier analysis (the mechanism revealed)**:
  - T1 compliers: significant positive effects on ALL IDEPE outcomes (top-1 +0.406, mean +0.327, top-40% +0.198 — all p<0.001); accepted ~340m farther schools (first-year compliers); more applications (+1.22); placement rate 65% vs 47.3% non-compliers.
  - T2 compliers: also applied to higher-quality and more schools — engagement itself (expanding the choice set) drove gains regardless of ranking criterion.
  - 99.7% of complier recommendations outperformed non-recommended options on both quality and proximity.
- **Survey heterogeneity**: undecided parents (B/C categories) responded in the Distance arm, not the Quality arm; distance-tolerant parents: no effect (willingness ≠ action); maternal education: proximity effects only.
- **MDEs**: 0.035 (top-40) to 0.174 (mean distance) — powered for moderate effects; nulls are not trivial under-powering, they're engagement-limited.

### Policy/Design Implications (from the authors, systematized)
1. Timing: intervene before preferences crystallize (75.6% pre-decided) — entry-point placement, pre-application information campaigns.
2. Salience and placement of recommendations inside the flow — the carousel's post-decision position limited interaction.
3. System literacy: multi-option selection, truthful ranking, algorithm transparency — nudge ROI depends on understanding the allocation system.
4. Supply: nudges cannot conjure nearby quality; align interventions with local context (quality effects concentrated where good schools were reachable).
5. Compliers prove the mechanism works for engaged users — the design problem is engagement, not recommendation quality.

## Study 2: Tailoring Through Choice

**Citation**: Lipman, S.A., Alvarez Colic, N., Sukurica, L., Tholen, L. "Tailoring through choice: comparing the effect of randomly assigned and self-selected behavioural interventions in promoting healthier snack choice." Behavioural Public Policy (published March 27, 2026; doi:10.1017/bpp.2026.10036).

### Design
- Doubly randomized control trial (Delevry & Le 2019 design), field setting, Dutch university campuses (Rotterdam, Leiden, The Hague; Oct–Dec 2023, paused Jan 2024, resumed Feb 2024). n=839.
- Interventions: (i) small financial incentive (€0.10 attached to healthier snack — friction-reduced after pilot), (ii) calorie information, (iii) social-norm nudge ("60% chose healthily in a similar experiment").
- Arms: random condition (assigned to one intervention or no-intervention control) vs choice condition (self-select). Real snack choices (mandarin vs Mars; grapes vs chips).

### Results
- **Chosen interventions: marginally significantly healthier vs no-intervention control. Randomly assigned: not significant** (incentives approached significance — the strongest assigned intervention).
- Raw proportions: control 59.4% → random-excl-control 66.6% → choice 69.7%.
- Selection: 51% incentives / 41% calorie labelling / 8% social norms.
- Logistic regressions: chosen calorie labelling (and social norms after demographic controls) significantly increased healthy choices; chosen incentives did not — self-interested selection dilutes incentive effectiveness ("economic rationality would predict all respondents prefer this intervention").
- Predicted-selection matching (Appendix E): receiving the intervention you were predicted to choose (without explicitly choosing) is associated with healthier choices — **alignment, not the act of choosing alone, partly explains the effect**. Reweighting the random arm to the choice arm's intervention distribution modestly improved effectiveness but didn't eliminate the gap.
- Univariate self-selection correlates: age, diet quality, demand for commitment, attitude to healthy food, need for autonomy, susceptibility beliefs (calorie-label choosers agree they read labels; price-importance choosers pick incentives).

### Interpretation Boundaries
- No significant chosen-vs-assigned difference within intervention types (underpowered for the ~3pp gap — MDE exceeds it). The result is: choice beats control; assignment doesn't; direction favors choice within type.
- One-shot design = lower bound; longer horizons favor choice-based interventions for adherence (Carlisle et al. 2022 meta-analysis).
- Student sample limits external validity; equity effects of choice unexplored (Coles et al.: women benefit from choice, men from assignment — heterogeneity caveat).

## Unified Framework: The Engagement Gate (four multiplicative gates)

1. **Timing gate**: is the user at a moment when preferences are still open? (Recife: 75.6% had pre-decided → gate closed for 3/4 of users before any nudge could matter.)
2. **Engagement gate**: does the user actually see and interact with the intervention? (Recife: 1 login, 3 minutes, carousel after first selection → 86–76% never engaged.)
3. **Receptivity gate**: is the prior belief updatable? (Recife: undecided parents responded; strongly-predetermined didn't. Motivated reasoning — already captured in the choice-architecture occupational-decisions skill — moderates this.)
4. **Autonomy amplifier**: does the user choose/opt in? (Lipman: choice conditions produced effects assignment didn't; alignment is the active mechanism.)

**Reporting implication**: always report ITT AND complier effects. ITT answers "does deployment work at average engagement"; complier analysis answers "does the mechanism work when it fires." Product decisions need both — deployment design fixes the gates; mechanism quality fixes the complier effect.

## Cross-Reference: The Related Literature Anchor

- **Mertens et al. 2021 (PNAS meta-analysis)**: choice architecture d=0.43 overall; decision STRUCTURE interventions (defaults, effort, composition) outperform decision information and decision assistance; food domain most responsive (d=0.65), finance least (d=0.24); ~15% of interventions backfire; moderate publication bias.
- **DellaVigna & Linos**: nudge-unit effects much smaller than academic RCTs (publication + scale effects) — consistent with the engagement-gate reading: at scale, average engagement drops.
- **Choice Architecture in Occupational Choices** (Dell et al., UZH, April 2026; in library): rank-order effects and cognitive-load-reducing redesign on 246,869 users — the platform-design mechanism layer that complements these deployment-context findings.
- **Colombia ICFES pop-up experiments** (Bettinger, Kremer, Lizarazo, Neilson, Posso, Saavedra): 519,000 students; digital nudges raised information consumption from <5% to 15–25%; simple messages beat elaborate frames (20.9% vs 13.1%); but enrollment effects limited — the same "engagement moves, outcomes lag" pattern at national scale.

## A-Tech Applications

- Product design checklist for any recommendation/nudge feature: place at pre-decision moments; measure engagement explicitly; report complier effects; offer intervention choice where consent-first.
- Evaluation template for vendor or consultant "nudge" claims: ask for ITT *and* complier results, engagement rates, and placement timing before believing average effects.
- Open-source alignment: choice-based deployment (users pick their intervention) is the consent-first pattern; assignment-based deployment inherits paternalism critiques already captured in the library (affective paternalism, boosts-vs-nudges preference skills).
