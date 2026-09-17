# Promotional-Preventive Framing ERP — Evidence Base

## Study Details

**Citation:** Wang, X., Alves, R.R., Hu, Q. et al. Effects of promotional and preventive framing strategies in AI generated content on neural responses and trust. Sci Rep 16, 23547 (2026). https://doi.org/10.1038/s41598-026-63139-1

**Published:** July 24, 2026 (received Jan 27, 2025; accepted July 16, 2026)

**Funding:** STI2030 Major Projects [grant number 2021ZD0200409]

## Experimental Design

### Pre-Test
- 140 participants rated 20 products on 7-point Likert scale (1=utilitarian, 7=hedonic)
- Hedonic products: game console, perfume, mahjong set, wine, headphones, smartwatch, eBook reader, etc.
- Utilitarian products: USB flash drive, food storage, rice cooker, power bank, WiFi router, laptop, etc.
- Paired t-test: hedonic M=6.04 (SD 0.49) vs utilitarian M=1.39 (SD 0.38), t(139)=86.28, p<.001

### Main Experiment
- **N=28** (19 male, mean age 21.91, SD 2.63, range 18-34)
- Right-handed, native Chinese speakers, normal/corrected vision, no neurological history
- Compensation: 40 RMB
- IRB approved by Zhejiang University Neuromanagement Lab

### Stimuli
- 20 AIGC images (black-and-white, consistent design, 840×640 pixels, JPEG)
- 40 unique messages: 20 promotional (emphasizing benefits), 20 preventive (emphasizing risk avoidance)
- Examples:
  - Hedonic promotional: "Ignite every thrilling moment; bring the excitement home today!"
  - Hedonic preventive: "Don't risk missing out on unforgettable moments; ensure you don't miss the best thrilling experience!"
  - Utilitarian promotional: "Experience lightning-fast data transfer; enjoy the convenience of easy portability!"
  - Utilitarian preventive: "Don't let your precious data disappear; ensure secure storage with no risk!"

### Procedure
- 4 blocks of 40 trials each (160 total, randomized)
- Each trial: fixation (400-600ms) → blank → product image (1500ms) → blank → framed message (1500ms) → blank → trust rating (5-point Likert, unlimited time)
- 15 practice trials before experimental blocks
- 2-minute rest between blocks

### EEG Recording
- 64-electrode cap, 10-20 system, Neuroscan Synamp 2 Amplifier
- Band-pass: 0.05-100 Hz, sampling rate: 1000 Hz
- Left mastoid reference, cephalic grounding
- Electrode impedance below 5 kΩ
- EOG: vertical (above/below left eye), horizontal (outer canthus of each eye)

### ERP Analysis
- Epochs: -200ms to 800ms (1000ms total), fixation baseline
- Low-pass filtered below 30 Hz (24 dB/Octave)
- Epochs with >±100 µV deflections removed
- Components analyzed:
  - P300: 300-400ms, electrodes F3/FZ/F4/FC1/FCZ/FC2/C3/CZ/C4
  - N400: 400-500ms, electrodes F3/FZ/F4/FC3/FCZ/FC4/C3/CZ/C4
  - LPP: 600-800ms, electrodes F1/FZ/F2/FC1/FCZ/FC2/C3/CZ/C4
- Statistical: repeated-measures ANOVA, Greenhouse-Geisser correction, simple effect analysis

## Statistical Results

### Behavioral

**Reaction Time (ms):**
| Condition | Mean | SD |
|-----------|------|-----|
| Hedonic-Promotion | 1475.55 | 738.26 |
| Hedonic-Prevention | 1530.90 | 748.09 |
| Utilitarian-Promotion | 1328.84 | 724.45 |
| Utilitarian-Prevention | 1545.16 | 1013.72 |

Main effect of framing: F(1,27)=9.445, p=.005, η²p=0.259

**Trust Rating (1-5):**
| Condition | Mean | SD |
|-----------|------|-----|
| Hedonic-Promotion | 3.35 | 0.482 |
| Hedonic-Prevention | 3.02 | 0.570 |
| Utilitarian-Promotion | 3.50 | 0.462 |
| Utilitarian-Prevention | 3.06 | 0.598 |

Main effect of framing: F(1,27)=8.632, p=.007, η²p=0.242

### ERP Results

**P300 (300-400ms):**
- Significant interaction: F(1,27)=9.138, p=.005, η²p=0.253
- Hedonic promotion vs prevention: M_promo=0.653 vs M_prev=-0.768, F(1,27)=10.03, p=.004, η²p=0.271
- Utilitarian: no significant difference (p=.372)

**N400 (400-500ms):**
- Significant interaction: F(1,27)=5.823, p=.023, η²p=0.177
- Hedonic promotion vs prevention: M_promo=-0.727 vs M_prev=-2.142, F(1,27)=9.04, p=.006, η²p=0.251
- Utilitarian: no significant difference (p=.561)

**LPP (600-800ms):**
- Significant interaction: F(1,27)=7.736, p=.010, η²p=0.223
- Hedonic: trend toward larger LPP for promotion, but not significant (p=.089, η²p=0.103)
- Utilitarian: no significant difference (p=.217)

### Supplementary MVPA
- Whole-scalp time-resolved decoding: **no** reliable condition separability (all near chance)
- ROI-based decoding: **no** significant above-chance classification
- The univariate ERP effects are component-specific amplitude effects, NOT evidence of robust multivariate separability

## Theoretical Context

### Regulatory Focus Theory (Higgins, 1997)
- Promotion focus: driven by achievement, growth, accomplishing goals
- Prevention focus: motivated by safety, responsibility, avoiding negative outcomes
- Messages are more persuasive when regulatory focus matches recipient's motivational orientation

### Word-of-Machine Effect (Longoni & Cian, 2020)
- Consumers perceive AI recommenders as more competent for utilitarian than hedonic products
- AI is seen as better suited for calculation-based, function-oriented judgments
- This explains why framing matters less for utilitarian products in AIGC context

### Key Distinction from Prior Research
- Micu & Chowdhury (2010): promotional messages more effective for hedonic, preventive for utilitarian — in HUMAN recommendations
- This study: same pattern holds for AIGC, but the mechanism is different because of the word-of-machine effect
- For AIGC specifically: promotional framing helps AI overcome skepticism in hedonic domains

## Limitations
1. Sample limited to Chinese university students (cultural generalizability unclear)
2. Passive viewing paradigm (no interactive trust decisions)
3. ERP signals are indirect indicators, not direct neural markers of trust
4. MVPA did not provide reliable evidence of condition-level separability
5. Small sample (N=28) comparable to similar ERP studies but limits power
6. All content labeled as AI-generated (does not test undisclosed AIGC)

## A-Tech Alignment
- **Open-source AI:** Framework applies to AI-generated marketing for open-source AI products
- **Data privacy:** Behavioral proxies over biometric surveillance; on-device processing possible
- **Financial freedom:** SME access without lab equipment; actionable framing guidelines
- **Practical implementation:** Clear decision tree for message design; immediately actionable