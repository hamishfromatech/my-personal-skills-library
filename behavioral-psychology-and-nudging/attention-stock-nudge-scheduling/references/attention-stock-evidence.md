# Evidence Base — Attention-Stock Nudge Scheduling

## The paper

Byrne, D. P., Goette, L., Martin, L. A., Miles, A., Jones, A., Schob, S., Staake, T., & Tiefenbeck, V. "How Nudges Create Habits: Theory and Evidence from a Field Experiment." SSRN 3974371 (Feb 2024, rev. Nov 2024; a 2023 working paper Oct 15 draft exists at Queen's econ. Also cited as habit27.pdf).

## Design

- **Setting**: 700 households, South East Water (Melbourne, Australia), April–October 2017; Amphiro B1 smart shower meters (water-flow powered, no clock, records per-shower data).
- **Instrument**: real-time feedback on litres used + temperature + melting-icecap visualization + energy-efficiency class at end of shower. Feedback-off mode shows only temperature (a working control display).
- **Seven conditions (T1–T7)** varying feedback-on/off cycling over 120 showers: T1 = always off (control); T2 = always on; T3 = 48/72; T4 = 24/48; T5 = 12/24; T6 = 6/12; T7 = 3/15 (on/off). Multi-person households get doubled cycles; results robust to single-person subsample (Appendix A.1/A.2).
- **Data**: 86,376 household-shower observations; 1,078 individuals; 555 households with returned-device data (93% device-return rate among installed; no differential attrition).

## Headline reduced-form findings

1. **Immediate, stable effect**: −7.31 L/shower on average (baseline 57 L, ~13% reduction). ON coefficient −7.39 with PostON slope 0.01 (ns) — effects do not build over exposure.
2. **Asymmetric dynamics**: OFF coefficient −4.85 (roughly half the treatment effect persists immediately post-off); PostOFF slope 0.08 (≈80 ml/shower recovery per shower), significant.
3. **Duration gradient (H2 confirmed)**: persistence rises with spell length — post-off increase in first 12 showers: 0 L (48-shower), 3.6–4.3 L (24/12/6-shower), 6.9 L (3-shower). Joint equality rejected (p=0.02).
4. **Cumulative savings**: T2 (always-on) yields 814 L over 120 showers; T4 (24/48) yields 580 L; T5 (12/24) yields 629 L — *intermittent* schedules beat comparable-constant schedules on totals despite equal on-time.

## Structural model

- Combines Chetty et al. (2009) salience/limited attention with Stigler–Becker (1977) habit formation; nests consumption-based vs attention-based state dependence.
- **Attention stock**: ωt builds toward 1 while feedback is on (αON) and decays toward a lower bound θ̄ while off (αOFF). θ=1 under feedback.
- **Preferred asymmetric attention estimates**: αON≈0.081, αOFF≈0.021, φ≈−7.42.
  - Build-up: half-life 9 showers [6,15], steady state reached at 18 showers.
  - Decay: half-life 33 showers [15,95]; effects persist ~59 showers (~2 months at one shower/day).
- **Out-of-sample validation**: attention model RMSE 2.405 vs 2.837 (asymmetric consumption) and 3.948 (symmetric consumption); also 7% better than a linear regression.
- **Mechanism ruling-out**: automatic-control (Camerer et al.) and experimentation/learning (Larcom et al.) mechanisms fail to explain persistence here.

## The (I,S,s) rule

- Motivation: when feedback is costly or rationed (budget K < horizon T), what allocation maximizes behavior change?
- Monte Carlo over 1 trillion random feedback sequences (T=120, K=48) finds the top structures share a shape: initial continuous build-up (I), then on/off cycling to maintain the attention stock within a band.
- Grid search over (I,S,s) with I∈{17-ish}, S∈[0.701,0.719], s∈[0.700,0.702]: optimal rule ≈ (I=17, S≈0.74, s≈0.70); average effect −5.97 L/shower [95% CI −4.73,−7.38].
- **Benchmark**: naive "spend the budget immediately" (symmetric consumption model's implication) yields −5.18 → the (I,S,s) rule is **~15% better**.
- Budget response: feedback effect vs K is convex — 20→40 showers of budget raises optimal effect 50% (−3.67→−5.50); 40→60 raises it 18%; ~80 and 120-period budgets produce similar average effects (diminishing returns).

## Design workflow (full)

1. **Measure or borrow a decay half-life**: for daily behaviors, ~2 months of post-nudge effect is the benchmark; re-estimate from your telemetry if available.
2. **Build-up phase**: sustain feedback (or a salient prompt) long enough to raise the attention stock — ~18 exposures is the shower estimate; scale to your behavior's frequency.
3. **Maintenance phase**: cycle feedback to keep attention in-band; the optimal band in-data was [0.700–0.719] of full attention — i.e., roughly "keep the user ~70% aware of costs while off".
4. **Intermittency beats constancy under rationing**: if you can't run feedback continuously, cycling (12/24-type schedules) beat front-loaded constant delivery for cumulative effect.
5. **Combine channels**: attention-stock management *during* the program (this skill) + artifact creation *after* (`nudge-persistence-meta-analysis-38-experiments`) = the full persistence stack.

## Attention-budget caveat (from the paper)

The authors explicitly do not model "attention budgets" across behaviors — attention drawn to one behavior may reduce attention for others (Shenhav et al. 2017; Bronchetti et al. 2022). Design for spillover risk when running multi-goal reminder systems.

## Caveats

- One domain, one utility, one device; single-shower behavior.
- Structural parameters are model-dependent; the attention mechanism won out in-sample and out-of-sample here, but the consumption-based mechanism was given favorable asymmetric treatment and still lost.
- The (I,S,s) optimality is a model-based counterfactual (1 trillion sequences, grid search), not a randomized test of the rule itself.
- 2017 data; app stores were closed during the trial to prevent cross-device feedback — a compliance posture, not a product reality.