# A-Tech daily research — evidence base: Digital Addiction Economics

## Source

Allcott, Hunt (Stanford); Gentzkow, Matthew (Stanford); Song, Lena (UIUC). **"Digital addiction: Evidence and policy implications."** Hamilton Project / Brookings Institution, June 2026. Synthesis of Allcott, Gentzkow & Song, "Digital Addiction," *American Economic Review* 112(7), 2022 (the 2020 field experiment) plus the June 2026 policy frame.

## Baseline survey facts (N≈2,000 US adults 18–64, recruited via Facebook ads, Phone Dashboard Android app)

- Social media + smartphone use rank among top self-control-problem activities, alongside saving money, exercising, dieting.
- >70% often/always check phone immediately upon waking; >1/3 report using it longer than intended; ~40% want to reduce use by >20%; ~42% report no desire to change.
- 84% experience at least one moderate-addiction component (salience, tolerance, mood modification, relapse, withdrawal, conflict — Griffiths 2005 components model); 41% at least one severe component.
- 19% feel smartphone use made their life worse; most feel it made life better.

## Intervention 1: Pay to reduce (Bonus) — habit-formation signature

- $2.50 per hour of reduction vs. baseline, 3 weeks.
- Incentive period: **−39% (~56 min/day)**.
- Post-incentive decay, not reversion: 6 weeks after payments ended, **−12 min/day (~8%)** vs. control.
- Interpretation: reducing use today lowers future use — the definitional signature of habit formation (analogous to nicotine economics: Chaloupka/Levy/White 2019).

## Intervention 2: Self-set screen-time limits (Limit) — self-control signature

- App-specific daily limits, enforced by Phone Dashboard; offered to treatment arm, no requirement to use.
- **89% of the treatment group set binding limits** (with a guided setup process).
- Effect: **−22 min/day (~16%)** averaged over 12 weeks; stable across survey waves; persisted 6 weeks after final survey — evidence of genuine demand for commitment, not novelty.
- **Willingness to pay $4.20 for continued access over 3 weeks** — direct revealed-preference evidence of anticipated self-control failure.

## The economic model

- Consumption model calibrated from experiment: self-control problems, *exacerbated by habit formation*, account for **31% of social media use (~48 min/day)**.
- Heterogeneity: 22% of participants <10 min/day; 32% >60 min/day; 13% >100 min/day.
- Welfare arithmetic: self-control-problem overuse ≈ $4.62/person over 3 weeks ≈ **$20.3B/year** for 254M US users (Kemp 2026 user estimate).
- Important honesty note: users may use more than they'd like *and still* derive large value — platform consumer surplus estimates run $50B–$300B+/year (Allcott et al. 2020; Aral et al. 2025). The $20.3B is the *internal cost of the self-control gap*, not the value of the product.
- Self-control overuse also harms others via peer effects (Bursztyn et al. 2025, "collective traps"): individual excess pulls others into higher use — an externality channel the model flags but does not jointly estimate.

## Well-being effects (modest, honest magnitudes)

- Both interventions significantly reduced self-reported addictive behaviors (falling asleep with phone, losing sleep, "just a few more minutes," difficulty putting down).
- Subjective well-being: Bonus +0.09 SD (statistically significant; roughly 25–40% of the effect size of formal psychological interventions per Bolier et al. 2013); Limit +0.04 SD (insignificant).
- Mechanism: improved concentration/reduced distraction; effects on happiness, life satisfaction, depression, anxiety were insignificant in the ≤6-week window.
- Comparable anchors: 4-week Facebook deactivation +0.09 SD (Allcott et al. 2020); 5-week Facebook+Instagram deactivation +0.05 SD (2025 NBER).

## "Flexibility through Delay" (Allcott, Maxted & Meyer, forthcoming) — commitment design

- Variation on the limit feature: immediate override; 2-, 5-, 20-minute delays; unextendable.
- Rationale: when reaching a limit, the user decides about consumption minutes ahead — temptation dissipates quickly, so even short delays align the decision closer to long-run preference.
- Analogous products: one sec (breathing pause), ScreenZen, Unpluq, Opal — delay-based friction.
- Critique of incumbents: Apple Screen Time / Google Digital Wellbeing allow immediate override, i.e., zero calibrated friction.
- Naïveté finding: participants systematically underestimated their own future use.

## Policy & litigation landscape (as of the June 2026 brief)

- *K.G.M. v. Meta et al.*: March 2026 LA jury verdict — Meta and Google liable for deliberately designing addictive platforms for minors; $6M damages; first trial outcome among thousands of pending suits.
- European Commission (Feb 6, 2026): preliminary finding that TikTok's addictive design breaches the Digital Services Act.
- New York SAFE for Kids Act: prohibits algorithmically curated feeds for under-18s without parental consent (feature-level, mechanism-targeting approach preferred by the authors over access bans).
- Australia under-16 minimum-age ban: most sweeping access restriction; enforcement/verification concerns.
- 37 states + DC restrict student smartphone use in K–12 (31 laws in 2025–26); Allcott et al. 2026 NBER national lockable-pouch evidence.
- Policy toolkit the authors rank: (1) mandate/defaults for limit-setting tools with guided setup (89% adoption shows salience matters; tools exist but are buried); (2) design-feature restrictions (SAFE-style) over access bans for children; (3) liability exposure for engagement-maximizing design; (4) mandated researcher data access.
- Key unresolved research+litigation problem: distinguishing "engaging" design (preferred in advance) from "addictive" design (restricted in advance). Both raise time-on-platform; only the latter plausibly triggers liability.

## Limitations (from the authors)

- Field experiment ran in 2020 (pandemic onset); short-form video has since grown dramatically — TikTok-class engagement mechanics post-date the measurement.
- Sample skew (experiment volunteers); reweighting to national demographics *increases* the modeled self-control effect (baseline estimate conservative).
- Guided limit-setup used in the trial; unguided limits would likely produce smaller effects.
- Adults only (avg 34); results likely conservative for youth.

## A-Tech product-design checklist

1. Never build immediate-override limits; ship a calibrated delay (2–5 min default, user-adjustable).
2. Guided setup, not buried toggle: the 89% adoption number is a salience effect, not just a preference effect.
3. Measure behavior on-device without content capture (Phone Dashboard pattern).
4. Position reduction outcomes honestly: symptoms improve in weeks; well-being endpoints need months.
5. Track the engaging-vs-addictive litigable-feature taxonomy as a standing design review item.