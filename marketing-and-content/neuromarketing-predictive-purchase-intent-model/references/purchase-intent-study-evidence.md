# Neuromarketing Predictive Purchase Intent — Full Study Evidence

**Source:** Iyappan, A., Soundarya, M., Baby, R., Ravi, I.A., Leela, M.H., & Rajalakshmi, J. (November 6, 2025). "Neuromarketing Insights for Predicting Consumer Purchase Intent." Journal of Marketing & Social Research, 2(9), 42–49. DOI: 10.61336/jmsr/25-09-05. Open Access (Creative Commons).

---

## Study Design

### Sample
- **285 participants** (145 male, 140 female)
- Age 18–45, mean 28.7 years (SD = 6.2)
- Recruited via web registration and community advertisements; paid for participation
- Screening excluded: neurological disorders, severe vision problems, prior ad exposure (familiarity effects)

### Stimuli
- **20 video ads**, 30–60 seconds each
- **4 product categories:** electronics, fashion, FMCG, lifestyle
- **2 ad types:** emotional (narrative/music-heavy) vs. informational (feature-focused/text-based)
- Randomized presentation order
- Controlled lab environment

### Measurement Tools
- **EEG:** 32-channel Emotiv system, 256 Hz sampling rate
  - Frontal alpha band (8–13 Hz) — positive affect, approach motivation
  - Frontal beta band (13–30 Hz) — attentional focus, cognitive arousal
  - Frontal asymmetry — emotional engagement, approach-avoidance
  - Pre-processed for artifact/noise removal
- **Eye-tracking:** Tobii Pro Nano
  - Total fixation duration (ms)
  - Number of fixations
  - Gaze heatmaps
- **Purchase intent:** 5-point Likert scale (1 = very unlikely, 5 = very likely), self-reported after each ad

### Statistical Analysis
- SPSS 28.0 and R 4.3
- Descriptive statistics (mean, SD, min, max)
- Pearson correlation (r) — predictor vs. purchase intent
- Multiple regression — all predictors → purchase intent (controlling for age, gender, prior brand familiarity)
- ANOVA — ad type × product category on purchase intent
- Model fit: R², adjusted R², Cohen's f²

---

## Results

### Descriptive Statistics (Table 1)

| Metric | Mean | SD | Min | Max |
|---|---|---|---|---|
| Purchase Intent | 3.72 | 0.91 | 1 | 5 |
| Frontal Alpha | 12.5 µV | 2.3 | 7.2 | 18.6 |
| Frontal Beta | 15.3 µV | 3.1 | 9.8 | 22.7 |
| Fixation Duration | 2.85 s | 0.76 | 1.2 | 4.5 |
| Number of Fixations | 14.7 | 3.6 | 7 | 24 |

### Correlation Analysis (Table 2)

| Predictor | r with Purchase Intent | p-value |
|---|---|---|
| Frontal Alpha | 0.42 | <0.001 |
| Frontal Beta | 0.31 | <0.01 |
| Fixation Duration | **0.48** | <0.001 |
| Number of Fixations | 0.35 | <0.01 |

All predictors positively correlated. Fixation duration is the strongest single correlate.

### Multiple Regression (Table 3)

Model: Purchase Intent = α + β₁(Frontal Alpha) + β₂(Frontal Beta) + β₃(Fixation Duration) + β₄(Number of Fixations) + controls (age, gender, brand familiarity)

| Predictor | B | SE B | β | t | p |
|---|---|---|---|---|---|
| Frontal Alpha | 0.12 | 0.03 | 0.31 | 4.00 | <0.001 |
| Frontal Beta | 0.08 | 0.03 | 0.18 | 2.67 | 0.008 |
| Fixation Duration | 0.19 | 0.04 | **0.38** | 4.75 | <0.001 |
| Number of Fixations | 0.09 | 0.03 | 0.21 | 3.00 | 0.003 |

### Model Summary (Table 4)

| Metric | Value |
|---|---|
| R² | **0.53** |
| Adjusted R² | 0.51 |
| F-statistic (df) | 78.3 (4, 280) |
| p-value | <0.001 |

The four predictors jointly explain 53% of the variance in purchase intent.

### ANOVA — Ad Type

| Ad type | Mean purchase intent |
|---|---|
| Emotional | **4.01** |
| Informational | 3.43 |

F(1,283) = 16.8, p < 0.001. Emotional ads produce significantly higher purchase intent.

### ANOVA — Product Category

| Category | Mean purchase intent |
|---|---|
| Fashion | **3.95** (highest) |
| Lifestyle | ~3.75 |
| FMCG | ~3.60 |
| Electronics | **3.51** (lowest) |

F(3,281) = 4.27, p = 0.006. Product category significantly moderates purchase intent.

---

## Hypotheses Tested

| Hypothesis | Result |
|---|---|
| H1: Higher frontal alpha → higher purchase intent | **Supported** (β = 0.31, p < 0.001) |
| H2: Longer fixation duration → higher purchase intent | **Supported** (β = 0.38, p < 0.001, strongest predictor) |
| H3: Emotional ads > informational ads for purchase intent | **Supported** (4.01 vs 3.43, p < 0.001) |

---

## Key Findings Summary

1. **Fixation duration is the most powerful predictor** of purchase intent (β = 0.38, r = 0.48) — sustained visual attention on product-related elements drives purchase decisions more than any neural metric.
2. **Frontal alpha (positive emotion) is the second-strongest predictor** (β = 0.31, r = 0.42) — emotional engagement and approach motivation matter but slightly less than attention.
3. **The combined model explains 53% of purchase-intent variance** — both neural and attentional metrics contribute independently; they are complementary, not redundant.
4. **Emotional ads outperform informational ads** by 0.58 points on a 5-point scale (4.01 vs 3.43) — a 17% relative lift.
5. **Product category moderates the effect** — fashion benefits most from emotional advertising; electronics least.
6. **Physiological responses precede conscious preference** — neural and attentional changes occur before consumers become aware of their decision, validating neuromarketing over self-report.

---

## Limitations (acknowledged by authors)

- **Controlled lab environment** — real-world shopping behavior may differ due to environmental factors, time pressure, social cues.
- **Specialized equipment** — EEG and eye-tracking require hardware and expertise, limiting widespread adoption.
- **Self-reported purchase intent as dependent variable** — still subject to some self-report bias; actual purchase behavior was not measured (the intention-behavior gap).
- **Single cultural context** — Indian sample; generalizability across cultures not tested.

---

## Privacy-First Translation Framework

For A-Tech's privacy-first values, the lab metrics translate to behavioral signals measurable without biometric hardware:

| Lab metric | Neural/attention mechanism | Behavioral proxy | Measurement method |
|---|---|---|---|
| Fixation duration | Sustained visual attention | Dwell time on element; video replay rate | On-device interaction logging |
| Number of fixations | Repeated attention shifts | Scroll-back frequency; element revisit count | On-device scroll/interaction tracking |
| Frontal alpha (8–13 Hz) | Positive emotion, approach motivation | Share/forward rate; open-ended sentiment; community post tone | Consent-based sharing metrics + NLP sentiment |
| Frontal beta (13–30 Hz) | Cognitive arousal, focused attention | Interaction intensity; feature exploration depth; session length | On-device session analytics |

**On-device processing principle:** All behavioral signals computed locally. Only aggregated, consented metrics leave the device. No biometric data (EEG, eye-tracking, facial coding, GSR) collected.

**Validation thesis:** If the behavioral proxies correlate with the lab metrics at r ≥ 0.35 (the weakest lab predictor's correlation), the privacy-first model should retain meaningful predictive power. Community validation is an open research challenge.

---

## Related Research Context

- **Afshar & Azimi (2025)** — eye-tracking is a better predictor of purchase intent than EEG when used alone (consistent with this study's finding).
- **Vecchiato et al. (2014)** — frontal alpha asymmetry associated with approach motivation and purchasing behavior (consistent with H1).
- **Byrne et al. (2022)** — systematic review of EEG + machine learning in neuromarketing; classification accuracies >70% for high/low purchase intent.
- **Slanzi et al. (2017)** — combining eye-tracking, pupil dilation, and EEG predicts web users' click intention (Information Fusion).
- **Gupta, Kapoor & Verma (2025)** — Frontiers systematic review (109 studies) establishing the 3×3 neuromarketing consumer-journey typology (covered in `neuromarketing-consumer-journey-3x3-framework`).