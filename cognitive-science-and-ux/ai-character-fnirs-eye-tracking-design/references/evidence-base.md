# Evidence Base: AI-Generated Character Design fNIRS + Eye-Tracking

## Source
Cha, J., Lee, N. & Kim, S. (2026). Impact of AI-generated characters on visual attention and prefrontal cognitive responses across age and image type conditions: using fNIRS and eye-tracking. *Frontiers in Human Neuroscience*, 20:1723608. doi:10.3389/fnhum.2026.1723608

## Experimental Design

### Participants
- N=24 (11 male, 45.8%; 13 female, 54.2%)
- Age: 21-26 years (M=21.91, SD=2.63)
- All right-handed, native Chinese speakers
- Normal/corrected-to-normal vision, no neurological/mental health history
- IRB approved: Hongik University IRB no. 7002340-202409-HR-023

### Stimuli
- 18 static facial images: 6 age conditions × 3 image types
- Age: infant (0-6y), child (7-12y), adolescent (13-19y), adult (20-39y), middle-aged (40-64y), elderly (65+y)
- Image type: real (licensed stock), 2D (MidJourney illustration), 3D (MidJourney 3D character)
- Standardized: uniform white background, front-facing, subtle non-exaggerated smile, 4:5 aspect ratio
- ROI-verified: mean luminance and contrast comparable across image types
- Expert panel (5 experts) validated realism and type-specific characteristics

### Procedure
- Each stimulus: 10s viewing + 20s questionnaire + 25s rest = 55s per set
- 18 sets total, ~16.5 min session
- E-Prime 3.0 for stimulus presentation
- Questionnaire (5 items, 1-5 Likert): image interest, image liking, attractiveness, character persuasiveness, character trustworthiness

### Eye-Tracking Setup
- Tobii Pro Spectrum (Tobii Technology, Stockholm)
- Near-infrared illumination, pupil-center/corneal-reflection method
- Sampling rate: 1200 Hz
- Metrics: total duration of fixations, average duration of fixations, number of fixations, average pupil diameter

### fNIRS Setup
- NIRSIT (OBELAB Inc., Seoul), 48 channels
- Wavelengths: 780 and 850 nm
- Prefrontal cortex coverage via default montage
- Channel-to-region mapping:
  - Right DLPFC: Ch 1,2,3,5,6,11,17,18
  - Left DLPFC: Ch 19,20,33,34,35,38,39,43
  - Right FPFC: Ch 7,8,12,13,21,22,25,26
  - Left FPFC: Ch 23,24,27,28,36,37,41,42
  - Right OFC: Ch 14,15,16,29,30
  - Left OFC: Ch 31,32,46,47,48
  - Right VLPFC: Ch 4,9,10
  - Left VLPFC: Ch 40,44,45

### fNIRS Processing Pipeline
1. Invalid value interpolation (≤5 consecutive samples: nearest-neighbor)
2. Channel QC (reject: median intensity <30, CV >15%, saturation)
3. Optical density conversion
4. Motion correction: TDDR (Temporal Derivative Distribution Repair)
5. Modified Beer-Lambert law → HbO concentration
6. DCT bandpass filter: 0.005-0.1 Hz
7. Short-separation regression (channels ≤15 mm)
8. Block-averaged HbO from 10s stimulus period vs preceding rest baseline
9. FDR correction for 48 channel-wise tests

## Results

### Questionnaire (Descriptive Only)
- Adult condition tended higher across ratings
- Infant condition under real image type showed notably higher mean
- 3D condition generally lower
- No inferential tests applied

### Eye-Tracking Results

#### Average Pupil Diameter (Key Finding)

**Within 3D condition (age effect):**
| Comparison | Mean (mm) | F | p | η² |
|---|---|---|---|---|
| Middle-aged vs Infant | 3.70 vs 3.19 | 3.88 | .003 | .230 |
| Middle-aged vs Child | 3.70 vs 3.35 | | | |
| Middle-aged vs Adolescent | 3.70 vs 3.30 | | | |
| Middle-aged vs Adult | 3.70 vs 3.23 | | | |
| Middle-aged vs Elderly | 3.70 vs 2.96 | | | |

**Within middle-aged condition (type effect):**
| Comparison | Mean (mm) | F | p | η² |
|---|---|---|---|---|
| 3D vs Real | 3.70 vs 3.26 | 4.17 | .022 | .275 |
| 3D vs 2D | 3.70 vs 3.25 | | | |

#### Number of Fixations
- Within adult condition: Real < 2D (p<.05) and Real < 3D (p<.05), F=3.76, p=.030, η²=.190
- Other conditions: no significant differences

#### Total/Average Fixation Duration
- No significant main effects or interactions

### fNIRS Results

#### Age Condition Effects

| Channel | Region | F | p | η² | Post-hoc |
|---|---|---|---|---|---|
| Ch 5 | Right DLPFC | 3.936 | .003 | .146 | Child > Infant, Adolescent, Adult, Middle-aged |
| Ch 19 | Left DLPFC | 2.568 | .038 | .100 | Child > other age groups (not significant after Bonferroni) |
| Ch 27 | Left FPFC | 3.143 | .015 | .120 | Child > Infant, Middle-aged; Adult > Infant, Middle-aged, Elderly |

#### Image Type Effects

| Channel | Region | F | p | η² | Post-hoc |
|---|---|---|---|---|---|
| Ch 35 | Left DLPFC | 5.388 | .006 | .190 | 2D and 3D > Real (3D vs Real significant after Bonferroni) |
| Ch 34 | Left DLPFC | 2.773 | .065 | .108 | Trend: 3D > Real |
| Ch 39 | Left DLPFC | 2.849 | .062 | .110 | Trend: 3D > Real |

## Theoretical Interpretation

### Pupil Diameter → Emotional Arousal
- Pupil dilation correlates with emotional arousal (Bradley et al., 2008)
- Middle-aged + 3D combination maximizes arousal → highest initial engagement
- Consistent with empathic engagement findings (Zhang et al., 2022)

### DLPFC → Cognitive Control and Evaluation
- DLPFC associated with attentional control, decision-making, working memory (Kroger et al., 2002; Owen, 1997)
- Child condition activates right DLPFC → may engage prefrontal circuitry related to regulatory control
- AI-generated characters activate left DLPFC more than real images → greater evaluative demand

### Complementary Pupil-HbO Patterns
Two distinct processing modes emerge from the integrated analysis:

**Mode 1: High pupil + Low DLPFC**
- Strong bottom-up attentional capture by affective stimuli
- Limited deliberate cognitive control during passive viewing
- Heightened arousal via sympathetic activation; DLPFC often reduced (Bradley et al., 2008; Buhle et al., 2014)
- Under acute stress: norepinephrine → rapid pupil dilation; DLPFC function weakened (Arnsten, 2009)

**Mode 2: Low pupil + High DLPFC**
- Strong top-down cognitive control with stable autonomic arousal
- Well-learned/predictable stimuli → reduced processing load → prefrontal goal maintenance without additional arousal (Granholm & Steinhauer, 2004)
- Cognitive reappraisal: prefrontal activity increases while subjective emotional arousal decreases (Ochsner & Gross, 2005)

### Applied Interpretation
- **3D middle-aged condition**: May be more compatible with rapid attention capture and initial interest
- **Child and AI-generated character conditions**: May be compatible with greater evaluative demands → simpler, more digestible messaging recommended
- These applied interpretations remain tentative given prefrontal HbO non-specificity

## Limitations
1. Small sample (n=24), demographic homogeneity (all 20s, university students)
2. Only female AI-generated characters (ecological validity limited)
3. No direct behavioral outcome measures (e.g., engagement, comprehension, decision-making)
4. fNIRS non-specificity: HbO changes not process-specific; reverse inference risk (Poldrack, 2011)
5. Questionnaire metrics treated as secondary (descriptive only, no inferential tests)
6. Future work: larger diverse samples, both character genders, additional psychophysiological measures (EDA, EEG), applied settings testing

## Cross-References to Existing Skills
- `consumer-mentalizing-eeg-social-cognition`: Both measure neural responses to marketing stimuli; this skill uses fNIRS + eye-tracking for character design, while the other uses EEG for social cognition in consumption
- `ai-model-label-framing-self-expression`: Both examine AI-generated character/model effects on consumer response; this skill focuses on physiological measurement, while the other focuses on labeling and self-expression
- `hybrid-eeg-gaze-graph-signal-neuromarketing`: Both use multimodal neuroimaging; this skill uses fNIRS + eye-tracking, while the other uses EEG + eye-tracking with GFT
- `multimodal-eeg-cv-purchase-intent-prediction`: Both use multimodal physiological measurement for consumer response prediction
- `concept2brain-predictive-neural-response-model`: Both relate to neural response prediction from visual stimuli