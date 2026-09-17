# Evidence Extraction: Abdollahi, Bonyadi Naeini & Hosseini (2026)
## "Decision-Making in E-Commerce Platforms: Evidence from Sustained Attention, Cognitive Inhibition, and EEG Measures"

**Status**: Just Accepted (uncorrected proof), Basic and Clinical Neuroscience — accepted 2026/05/19, published online July 10, 2026. DOI: 10.32598/bcn.2026.8591.1

**Why this study matters for A-Tech**: It is one of the few 2026 consumer-neuroscience papers that reports a *negative* attention→purchase relationship with a plausible neural mechanism, directly challenging the "attention = money" assumption baked into most marketing analytics.

---

## Study Design

| Element | Detail |
|---|---|
| Sample | N = 30 healthy right-handed adults, 18–30 yrs (12 F / 18 M), Tehran; G*Power: effect size 0.6, R² = 0.4 |
| Exclusions | Neurological/psychiatric history, psych meds, substances, head injury, fatigue |
| Cognitive tasks | Continuous Performance Test (CPT, sustained attention: omission/commission errors, RT); Stroop (cognitive inhibition: interference score) |
| EEG | 64-channel, 10–20 system, conductive gel; 2-min eyes-open + 2-min eyes-closed baseline |
| Simulated platform | MATLAB multi-sided platform (SnappFood-like online food ordering); 180 food stimuli (pizza, hamburger, steak, pasta) with price, ratings, FDA badge overlaid |
| Trial structure | Fixation 500–1000 ms → food image alone 3000 ms (observation) → fixation 600–800 ms → same image + Buy/Not Buy choice 6000 ms (decision) |
| Preprocessing | FIR 1–40 Hz; bad-channel interpolation; ICA (ICLabel) for blink/muscle artifacts; Welch spectral power |
| Bands | delta (1–3), theta (4–7), alpha (8–13), beta (13–30), gamma (31–60 Hz), averaged over frontal/central/parietal-occipital regions |
| Analysis | Pearson correlation + simultaneous multiple regression (SPSS) |

## Key Behavioral Findings

1. **Sustained attention ↔ buying: r = −0.326, p = .039 (significant negative bivariate correlation)** — higher sustained attention associated with FEWER purchases. Explains 10.7% of variance in bivariate terms (R² = .107).
2. **Cognitive inhibition ↔ buying: r = 0.012, ns** — inhibition (Stroop) unrelated to purchase tendency.
3. **Multiple regression**: R = .327, R² = .107, Adj. R² = .041. Attention β = −0.328 (p = .084, *below* conventional significance after controlling inhibition); inhibition β = −0.023 (p = .901).
4. Zero-order vs partial correlation for attention identical (−.326/−.326); attention–inhibition correlation −0.107 (ns) — the attenuated regression p-value is not a suppression artifact but a sample-size/power issue.
5. CPT performance (M RT 435 ms, 148/150 correct) and Stroop interference (994 vs 939 ms congruent/incongruent) confirmed instruments worked; sample was cognitively healthy and within typical GHQ-28 range (19.42).

## Key EEG Findings

**Observation stage (image alone)** — widespread significant channels across ALL bands:
- Delta: Fz, C4, T8, O1, Tp8, C1
- Theta: AF4, P3, AF7, Po7
- Alpha: C6, C2
- Beta: Fp2
- Interpretation: broad perceptual, attentional, emotional-evaluation, and reward-processing engagement during initial exposure.

**Decision stage (Buy/Not Buy)** — localized to theta + alpha only:
- Theta: P10 channel region (p = .00541)
- Alpha: F7 (.01329), Cp6 (.02450), O2 (.04465)
- Interpretation: selective engagement of attentional-control and evaluative fronto-parietal networks; shift from diffuse perceptual processing to focused evaluation.

## The Mechanism (Why Higher Attention → Fewer Purchases)

- Attentional capture vs deliberative evaluation are distinct processes; deep sustained processing recruits evaluation networks that weigh costs, compare alternatives, and detect flaws.
- Food stimuli are emotionally salient; dual-process models (Kahneman; Shiv & Fedorikhin) predict stimulus-driven intuitive choices dominate under high emotional salience — *unless* attentional control engages.
- Supporting literature cited in paper: Gidlöf et al. 2017 (visual attention predicts shelf choice); Milosavljevic et al. 2012 (visual saliency biases evidence accumulation); Saffari et al. 2023 (EEG+VR shopping attention).
- Note carefully: the paper does NOT claim "attentive people never buy." It shows that, in this sample/task, stronger sustained-attention capacity correlated with fewer buys, and that deliberation recruits specific theta/alpha fronto-parietal activity. Generalization beyond food e-commerce, young adults, and lab settings is untested.

## Practical Implications Reported by Authors

- Digital marketing should focus on attention-*capturing* design (motion, contrast, salient framing) rather than cognitive-load reduction alone — BUT see our skill's counter-reading: for conversion (not awareness), deep attention is a friction signal.
- Manipulating visual saliency can bias rapid choice — flagging ethical line: saliency biasing is a manipulation lever; use for clarity, not exploitation.
- Sustained attention is a stronger lever than inhibition in typical (non-clinical) consumers.

## Limitations (from authors + our reading)

- Small N (30) → wide CIs; regression p = .084 for the headline variable. Treat direction as signal, magnitude as provisional.
- Lab-simulated platform; no real monetary stakes; food-only category.
- EEG spatial resolution limits source claims; deep structures (limbic) invisible.
- Single country/culture sample (Tehran); WEIRD-adjacent generalization risk.
- Behavioral "buying" is a forced binary per trial, not real purchase history.

## A-Tech Operationalization Notes

- Use dwell-time and interaction patterns as privacy-first proxies; do NOT infer neural states from individuals — the correlation is population-level.
- When running CRO experiments inspired by this: pre-register, control for confounds (the inhibition lesson), report effect sizes with CIs, and prefer directional hypotheses over point estimates.
- Ethical inverse (friction-as-protection) maps to boost-not-nudge design; see zero-party-consent-loop and boosting skills for consent-safe implementation.
