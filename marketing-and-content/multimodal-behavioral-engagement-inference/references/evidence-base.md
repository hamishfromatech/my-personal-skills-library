# Evidence Base: Multimodal Behavioral Engagement Inference

## Primary Source

**Authors:** Charlotte De Sainte Maresville, Christine Petr (IAE Bretagne Sud); Olfa Haggui, Felipe Restrepo (Odaptos)
**Publication:** Journal of Marketing Analytics, July 8, 2026
**DOI:** 10.1057/s41270-026-00505-y
**Funding:** CIFRE doctoral fellowship (ANRT) in partnership with Odaptos

## Study Design

### Two-Study Structure

**Study 1 (Measurement Development):**
- N=34 video-mediated interaction sessions
- Webcam-recorded at 20 fps
- Semi-structured human-to-human video interviews
- Unit of analysis: temporally localized micro-gesture sequence
- 37 categories of upper-body micro-gestures detected via automated recognition (83.7% Top-1, 95.6% Top-5 accuracy)
- 5 engagement categories specified a priori: Focused Attention, Reflective Processing, Distracted, Interactive, Tired
- 4,177 classified gesture-phrase instances
- Classification purity >90% for 29 of 37 gestures; 100% for 13 gestures

**Study 2 (Explanatory Modeling):**
- N=40 sessions with complete multimodal information
- 82.5% exact overlap with Study 1 (33 of 40 participants)
- Session duration: 1.5-26.5 min (M=11.7, SD=5.6)
- Participants: 20 female, 20 male; ages 19-60 (M=36.9, SD=11.2)
- 4,391 total segments analyzed

### Multimodal Tag Construction

Tags required convergence across ≥2 modalities within same segment:
- **ATTENTION_HIGH**: gaze (fixation duration, gaze entropy, scanpath transitions) + posture (movement variability) — ≥3 of 4 indicators
- **STRESS**: voice (arousal, vocal intensity) + face (negative valence, facial tension, blink-rate variability) + gaze (entropy, instability) + text (hedging, self-correction, negative affect) — ≥2 of 4
- **FIXATION**: prolonged gaze concentration + elevated affective activation
- **INTENTION_POS**: positive sentiment/approach language + positive vocal tone + positive facial valence + target-oriented gaze — ≥2 modalities, ≥1 semantic/affective
- **INTENTION_NEG**: negative sentiment/avoidance language + negative vocal tone + negative facial valence + gaze avoidance — ≥2 modalities
- **REGULATION_ACTIVE**: reduced arousal vs. preceding segment + gaze restabilization + verbal self-monitoring + emotional stabilization

## Key Results

### Descriptive Statistics (N=40 sessions)

| Tag | Mean | SD | Notes |
|-----|------|----|-------|
| ATTENTION_HIGH | 0.230 | 0.217 | Highest prevalence |
| STRESS | 0.062 | 0.087 | Episodic |
| INTENTION_POS | — | — | (not reported in main text) |
| INTENTION_NEG | — | — | Highly skewed, sparse |
| REGULATION_ACTIVE | 0.009 | 0.019 | Very low; only 13 of 40 sessions |

### Correlation Structure

| | STRESS | FIXATION | ATTENTION_HIGH | INTENTION_POS |
|---|--------|----------|----------------|---------------|
| STRESS | — | +0.708 | -0.257 | +0.593 |
| FIXATION | | — | — | — |
| ATTENTION_HIGH | | | — | — |
| INTENTION_POS | | | | — |

### H2a: Stress → Attentional Stabilization (SUPPORTED, ROBUST)

**Baseline:** STRESS negatively but non-significantly associated with ATTENTION_HIGH (β=-0.645, p>0.05)
**With controls:** STRESS strengthens to significant (β=-1.472, p=0.011); FIXATION positive (β=9.672, p=0.046)

**Robustness battery (all passed):**
- VIF: all below 2.1
- Condition index: ≤2.47
- Bootstrap 95% CI: excludes zero
- Leave-one-out: no sign reversal across all 40 sessions
- Fractional logit: preserves sign and significance
- Gender covariate: no significant interaction
- Duration covariate: unchanged
- **Feature-subset reconstruction:** Negative in 4 of 5 subsets, significant in 2 (gaze: r=-0.419, p=0.007; video/face: r=-0.430, p=0.006)

### H2b: Stress → Intentional Engagement (PARTIALLY SUPPORTED, FRAGILE)

**Session-level:** STRESS positively associated with INTENTION_POS (β=0.191, p<0.001, R²=0.352)
**Full model:** Remains significant (β=0.173, p=0.016)

**Feature-subset reconstruction (FAILED):**
| Reconstruction | r with INTENTION_POS | p-value |
|----------------|---------------------|---------|
| All fused features | 0.593 | <0.001 |
| Gaze only | 0.179 | ns |
| Voice only | 0.015 | ns |
| Text only | 0.079 | ns |
| Video/face only | 0.115 | ns |

The association **collapses** in every non-overlapping subset. It is substantially a measurement-construction artifact.

### Temporal Ordering (NOT SUPPORTED)

Segment-level clustered logistic regression:
- STRESS at t → ATTENTION_HIGH at t+1: b=-0.318, p=0.392 (ns)
- STRESS at t → INTENTION_POS at t+1: b=0.542, p=0.169 (ns)
- Permutation tests confirm null results (one-sided p=0.997 and 0.884)

**Conclusion:** All associations are contemporaneous, not sequential.

### Convergent Validity (Partial)

Blind human coding of 144 stratified segments (text only):
| Tag | Algorithm-Human Correlation | Cohen's κ |
|-----|---------------------------|-----------|
| STRESS | r=0.367, p<0.001 | 0.242 (fair) |
| INTENTION_POS | r=0.307, p<0.001 | 0.225 (fair) |
| ATTENTION_HIGH | r=-0.069, p=0.411 | — (not significant) |

ATTENTION_HIGH shows no text-based correspondence because it's constructed from gaze/postural features not accessible via transcript.

## Theoretical Context

### Dual-Process Account
- **Overload perspective:** Stress disrupts attentional continuity (Yap et al., 2021)
- **Effort-mobilization perspective:** Moderate strain mobilizes goal-directed processing (Baumeister et al., 1998)
- **Finding:** Disruption side is well-supported; mobilization side is observed but not established as independent of measurement construction

### Embodied Cognition
- Bodily/behavioral traces as inputs to behavioral inference (Barsalou, 2008; Krishna, 2011)
- Inference is more secure for some constructs (STRESS→ATTENTION_HIGH) than others (STRESS→INTENTION_POS)

## Incremental Validity (H5)

| Model | R² | ΔR² |
|-------|----|----|
| ATTENTION_HIGH alone → INTENTION_POS | 0.014 (ns) | — |
| ATTENTION_HIGH + STRESS → INTENTION_POS | 0.353 | +0.339 |

STRESS explains substantial variance beyond ATTENTION_HIGH, but this is **internal incremental contribution** within the framework, not validity against external criterion.