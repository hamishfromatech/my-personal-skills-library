# Evidence Base — Nudge Persistence Meta-Analysis (38 Natural Field Experiments)

## The paper

Brandon, A., Ferraro, P. J., List, J. A., Metcalfe, R. D., Price, M. K., & Rundhammer, F. (2026). "Do The Effects of Nudges Persist? Theory and Evidence from 38 Natural Field Experiments." *Review of Economic Studies*, published Jan 31, 2026 (rdag012).

## Why it upgrades the library

The library already holds `nudge-persistence-technology-adoption` (cycle 11, Aug 2026 — same author team, same headline: half of energy reductions persist via technology adoption). This ReStud version is the **formalization**: it supplies the research design for *uncovering mechanisms* behind persistence, making it the citable methodological anchor rather than a working-paper result. The skill's function is the **persistence-decomposition diagnostic**, not a re-report of the 50% number.

## Study design

- **Question**: what mechanisms underlie long-term reductions in energy consumption from the widely implemented social-comparison (home energy report) nudge?
- **Two channels formalized**:
  1. **Technology adoption** — the reduction persists because the *home* holds durable artifacts (efficient appliances, thermostat settings/reprogramming)
  2. **Habit formation** — the reduction persists because the *person* changed (Becker–Murphy-style consumption stocks, or attention-based stocks per Byrne et al.)
- **The instrument**: resident move-out as a natural discontinuation. If the reduction lives in the person, it leaves with them; if it lives in the home, it remains. Comparing treatment vs control homes *after* move-out isolates the technology channel.
- **Finding**: fully **half of energy reductions persist in the home after treatment ends** — consonant with technology adoption as the operative channel.
- **Implication stated in-abstract**: the role of technology in creating persistent behaviour change has "important implications for designing behavioural interventions and evaluating their long-term social impacts."

## Relationship to prior skills

- `nudge-persistence-technology-adoption` (cycle 11): held the 50% finding + technology-adoption design playbook. This skill adds: (a) the peer-reviewed formalization, (b) the explicit two-channel formalization (habit vs technology), (c) the move-out instrument as a generalizable research design.
- Byrne et al. (2024) "How Nudges Create Habits" (SSRN, held in `attention-stock-nudge-scheduling`'s orbit): the attention-stock complement — within-treatment asymmetry (immediate onset, stable, gradual decay, duration-dependent persistence) and the (I,S,s) optimal feedback rule. The two papers bracket the question: Byrne et al. explain *during-treatment dynamics* via attention; Brandon et al. explain *post-treatment persistence* via artifacts. A complete design uses both.
- Illinois Workplace Wellness Study (Jones, Molitor & Reif, NBER w32745): the complementary one-shot finding — first screening raises future screenings 32.4–36.0pp annually (84–90%), habit formation via a one-shot (learning/taste-discovery) mechanism rather than reinforcement; second-dose incentives show no persistence. Useful contrast: infrequent-annual behaviors can habitize in one shot, while daily behaviors persist through artifacts.
- Byrne, Goette, Martin et al. (SSRN 3974371, rev. Nov 2024): same attention-stock evidence base, 700-household shower field experiment, seven cycling conditions (T1–T7), structural estimation favoring the attention mechanism (αON fast, αOFF slow: build-up half-life 9 showers, decay half-life 33, effects persist ~59 showers ≈ 2 months), and the (I,S,s) rule (initial ~15–17 shower build-up, then keep attention stock in [0.700–0.719]; 15% better average effect than the naive exhaust-budget rule).

## Design workflow (full version)

1. **Classify the channel before launch**: artifact-forming (tech/config) vs cue-based (habit/attention). Ambiguity is itself a finding to resolve with the move-out test.
2. **Estimate persistence ratios by channel**: ≈50% (technology), gradual decay ∝ duration (habit), near-zero post-attention-stock-decay (~2 months in shower data).
3. **Instrument discontinuation**: move-out, account closure, subscription lapse, device return — any natural treatment stop that splits person-effects from artifact-effects.
4. **Budget as artifact + nudge**: cost-effectiveness comparisons that ignore artifact cost overstate nudge ROI; those that ignore the artifact's persistence understate it.
5. **Report persistence with effect size**: intervention ranking changes when persistence-weighted.

## Caveats

- Domain: residential energy (the HER/Opower lineage); the 50% share is dataset-specific.
- Identification rests on non-selective move-out — addressed but not fully excluded.
- The technology channel's artifacts include appliance purchases that may reflect concurrent incentives, not the nudge alone.
- One-shot habit findings (health screenings) do not transfer to high-frequency daily behaviors; the wellness-study authors note screening-habit effects don't generalize mechanically.

## Cross-references in library

- `nudge-persistence-technology-adoption` — primary sibling skill
- `attention-stock-nudge-scheduling` — Byrne et al. treatment
- `nudge-effectiveness-reality-check` — the scaled-effect shortfall family
- `habit-mediation-systemchange-magic` — habit-as-mediator trial evidence
- `social-learning-ecology-diffusion` — diffusion-ecology complement