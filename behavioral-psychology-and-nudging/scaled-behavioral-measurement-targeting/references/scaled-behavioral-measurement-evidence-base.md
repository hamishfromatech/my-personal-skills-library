# Scaled Behavioral Measurement Targeting — Evidence Base

## Source
Bauer, K., Grunewald, A., Hett, F., Jagow, J., & Speicher, M. (June 18, 2026). "Treatment Targeting by Scaled Behavioral Measurement." DFG Research Unit FOR 5392 Discussion Paper #2026/04 (DICE, University of Düsseldorf).

Preregistered under AEARCTR-0006291.

---

## 1. Study Design

### Nested Two-Experiment Architecture
The study combines two nested experiments:

**RCT (Field Experiment):** Large-scale randomized controlled trial at a major European online fashion retailer. ~500,000 website visits (242,228 control, 242,108 treatment). Treatment: loss-framed vs gain-framed discount message.

- Control: "Secure your discount of up to 70% on items on sale."
- Treatment: "Don't lose your discount of up to 70% on items on sale."

**Behavioral Measurement Experiment:** Nested within RCT. N=582 participants recruited via pop-up banner. Single incentivized choice task measuring loss aversion. Participants endowed with 15% voucher, then chose to keep or stake it in a lottery.

Three gamble versions (between-subjects):
- Version 1 (N=211): 5% vs 20% voucher
- Version 2 (N=181): 5% vs 25% voucher
- Version 3 (N=190): 5% vs 30% voucher

Loss held constant (15%→5%); gain varied across versions.

### Digital Footprints
33 categorical features available to any website host by default:
- Referral source (newsletter, Facebook, Instagram, Google, other)
- Entry page (children, female, male categories)
- IP region (Baden-Württemberg, Bavaria, Berlin, Hesse, NRW, other)
- IP provider (Telekom, Vodafone, Telefonica, 1&1, other)
- Device type (mobile/desktop)
- Operating system (Android, Mac, iOS, Windows, other)
- Browser (Chrome, Safari, Firefox, ChromiumEdge, other)
- Time of week (weekday/weekend)

---

## 2. Loss Aversion Classification

### Cumulative Prospect Theory Framework
Binary gamble L = (p, x1, x2) evaluated relative to reference point r (15% voucher consumption value). Piecewise linear utility:

u(x) = m(x-r) if x ≥ r; -λm(x-r) if x < r

where m = voucher value sensitivity, λ = loss aversion.

### Partial Ordering (Measurement 1)
Six types: VJ where V = gamble version (1,2,3), J = accept/decline (A,D).

(i) λ̄(1D) ≤ λ̄(2D) ≤ λ̄(3D) — declining more favorable gambles = more loss averse
(ii) λ̄(1A) ≤ λ̄(1D), λ̄(2A) ≤ λ̄(2D), λ̄(3A) ≤ λ̄(3D) — accepting < declining within version

43.1% classified as loss averse (types 1D, 2D, 3D). Consistent with Chapman et al. (2022) ~50% in representative US sample.

---

## 3. Results

### Result 1: Non-Targeted Loss Framing Fails
Average treatment effect (ATE) on purchase volume: +0.63% (95% CI: [-2.7%, +2.8%], p=0.964)
Average treatment effect on conversion rate: -0.64% (95% CI: [-2.8%, +1.6%], p=0.57)

Loss framing is ineffective when applied indiscriminately. Consistent with Apostolova-Mihaylova et al. (2015), Krawczyk (2011).

### Result 2: Behavioral Measurement Predicts Heterogeneity
In measurement sample: Above-median loss aversion (categories 4-6) shows positive treatment effects; below-median shows negative.

Field sample with ML-predicted loss aversion:
- Above-median predicted LA (categories 4-6): +1.5% to +6.5% purchase volume
- Below-median predicted LA: -2.2% to -6.3% purchase volume
- Interaction term: +6.9pp larger effect for above-median (p < 0.02)

Distributions aligned: measurement sample median=3, field median=3; μ_exp=3.32 vs μ_pred=3.38; σ²_exp=1.96 vs σ²_pred=1.62; Mann-Whitney p=0.375.

### Result 3: Targeted Policy Yields Significant Revenue
Policy: treat only predicted loss-averse visitors (categories 4-6, 43.1% of visitors).

Off-policy evaluation (100 bootstrap iterations):
- Predicted LA policy: **+1.608% revenue** (95% CI: [1.4%, 1.8%], p < 0.01)
- Random targeting benchmark: -0.053% (95% CI: [-0.23%, 0.12%], p=0.34)

### Result 4: Outperforms Causal Forest
Causal Forest (Wager & Athey 2018) trained on 70% of RCT data, evaluated on 30%:
- Causal Forest policy: +0.16% revenue (95% CI: [-0.01%, 0.4%]) — NOT statistically significant
- Behavioral measurement policy: +1.94% revenue (95% CI: [1.66%, 2.21%]) — statistically significant

Behavioral measurement targeting treats 43.1% vs Causal Forest treats 44.4% — similar intensity, different selection quality.

Individual-level correlation: predicted loss aversion explains only 0.7% of variance in Causal Forest ITE predictions (R²=0.007). Most heterogeneity Causal Forest detects is NOT aligned with theory-motivated loss aversion.

---

## 4. Machine Learning Pipeline

### Model
XGBoost gradient-boosted decision trees. Multi-class classification (6 loss aversion categories).

### Training
- 582 measurement observations (duplicates removed)
- Hyperparameter search: 50 random draws, 5-fold cross-validation
- Optimization metric: macro-averaged F1-score (equal weight to all LA classes)
- One-hot encoding for 33 categorical features

### Prediction
Trained model predicts 6-class loss aversion for ~500K field sample visitors. Binary loss-averse indicator: categories 4-6 = loss averse.

---

## 5. Theoretical Foundations

### Why Theory-Guided Targeting Wins
Fernández-Loría & Provost (2022), Fernandez-Loria & Jorge (2026): proxy-based treatment targeting can outperform direct CATE estimation when the proxy captures an important moderator.

Direct CATE estimation (Causal Forest) infers treatment responsiveness from realized revenue differences — noisy, between-subject, low signal-to-noise.

Behavioral measurement predicts the causal moderator (loss aversion) at the individual level — higher signal-to-noise because:
1. Loss aversion is measured per individual, not inferred from outcomes
2. Theory identifies it as the mechanism driving response to loss framing
3. ML prediction transfers this individual-level signal to the field

### Economic Theory as Regularization
Theory restricts the learning problem to a theoretically identified causal moderator. This restriction improves signal-to-noise ratio vs unconstrained CATE estimation from noisy digital footprints.

Analogy: Chen et al. (2023) — structural economic models as informative priors for ML. Economic theory and ML complement each other.

---

## 6. Interpretability and Ethics

### Interpretability Advantage
- Treatment assignment linked to latent economic preferences (loss aversion)
- Easy to explain WHY a visitor is targeted
- Contrast: Causal Forest = "black box" with obscure assignment reasons

### Regulatory Alignment
- EU GDPR and US law guarantee right to explanation for ML-driven decisions
- Theory-guided targeting inherently interpretable
- Risk of discrimination, adverse impacts, exploitation reduced

### Transferability
Theory-guided rules may be more robust across populations, platforms, settings than data-driven "black box" rules that may be sample-specific.

---

## 7. Digital Footprint Predictors of Loss Aversion

Lasso OLS coefficients (selected):
- Mobile device users: significantly LESS loss averse
- Baden-Württemberg region: significantly MORE loss averse
- Non-linear interactions captured by XGBoost beyond single-feature analysis

---

## 8. Limitations

1. Single product category (fashion), single country (Germany) — generalizability uncertain
2. Coarse loss aversion measurement (single choice task, partial ordering only)
3. Cross-sectional — no temporal dynamics
4. Measurement sample N=582 — ML training limited
5. Off-policy evaluation assumes treatment orthogonality (satisfied by RCT design)
6. No causal identification of WHY digital footprints predict loss aversion

---

## 9. A-Tech Application Framework

### When to Use This Approach
- Interventions with theory-identified moderators (loss aversion, time preferences, risk attitudes, social preferences)
- Environments where large-scale piloting is infeasible, risky, or costly
- Settings with noisy outcome data and small treatment effects
- Regulatory requirements for interpretable targeting

### Step-by-Step Implementation
1. **Identify theoretical moderator** — What latent preference does economic theory say drives response to your intervention?
2. **Design behavioral measurement** — Incentivized choice task eliciting the moderator in a small sample
3. **Collect digital footprints** — Features observed in both measurement and field samples
4. **Train ML model** — Predict moderator class from footprints (XGBoost, cross-validated)
5. **Deploy targeting rule** — Treat only high-moderator individuals in field
6. **Evaluate** — Off-policy evaluation using RCT data if available; A/B test if not

### A-Tech Alignment
- Open-source: XGBoost, scikit-learn, EconML all open-source
- Data privacy: uses only default-available digital footprints, no personal data, no surveillance
- Financial freedom: makes ineffective interventions profitable; reduces targeting cost
- Practical implementation: validated at 500K-visitor scale, reproducible pipeline

---

## Cross-References

- `behavioral-design-practical-playbooks` — general behavioral intervention design
- `llm-iterative-personalized-nudging` — LLM-based personalization of behavioral interventions
- `ai-pricing-monetization` — pricing strategy as behavioral intervention
- `friction-based-pricing-discovery` — pricing discovery through behavioral measurement
- `community-monetization-ladder` — community-based targeting approaches
- `hyperbolic-discounting-reversal` — time preference measurement and intervention

---

*Evidence base compiled: 2026-08-17 | A-Tech Research Division*