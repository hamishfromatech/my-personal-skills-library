# Evidence Base: Consumer Mentalizing EEG Social Cognition

## Primary Sources

### Source 1: Mentalizing Framework for Consumer Behavior
**Authors:** Chiara Casiraghi, Margherita Zito, Vincenzo Russo
**Institution:** Behavior and Brain Lab IULM — Neuromarketing Research Center, Università IULM, Milan
**Publication:** Frontiers in Psychology, Volume 17, April 24, 2026
**DOI:** 10.3389/fpsyg.2026.1740012

**Key contributions:**
- First opinion paper to systematically introduce mentalizing as a core consumer behavior construct
- Proposes mentalizing as mediating process between marketing stimuli with social cues and consumer responses
- Distinguishes mentalizing from empathy, Theory of Mind, and perspective-taking following Quesque et al. (2024) consensus
- Argues EEG is the most promising method for measuring consumer mentalizing due to temporal resolution, portability, and cost-effectiveness
- Proposes research agenda: adapt mentalizing paradigms for EEG, validate in non-clinical populations, test with marketing outcomes
- Identifies ethical concerns: psychological privacy, manipulation risk, need for informed consent

**Conceptual framework:**
- Consumption stimuli with social cues → mentalizing processes → consumer responses
- Integrates with Elaboration Likelihood Model (cognitive resources + affective heuristics)
- Integrates with Narrative Transportation Theory (character identification requires mentalizing)
- Integrates with Heuristic-Systematic Model

### Source 2: Perspective-Taking EEG Study
**Authors:** Marco Bilucaglia, Chiara Casiraghi, Vincenzo Russo, Margherita Zito
**Institution:** Behavior and Brain Lab IULM, Università IULM, Milan
**Publication:** Frontiers in Behavioral Neuroscience, Volume 20, July 24, 2026
**DOI:** 10.3389/fnbeh.2026.1740368

**Study design:**
- N=20 participants (10 female, 10 male), ages 27-55 (M=41.32, SD=8.80)
- 4 food/beverage video ads (30s each) with testimonial-style product interactions
- Ads featured characters eating (food ads) or drinking (beverage ads) products
- Only interaction segments analyzed (mean duration 3.11 ± 1.18s)
- EEG: NVX-36 device, 22 Ag/AgCl scalp electrodes (10-10 system), 2KHz sampling
- Self-report: perceived ad effectiveness (EFF), emotional perspective-taking (EMO), cognitive perspective-taking (COG)

**EEG findings:**

| Stimulus | Scale | Band | Measure | Correlation | p-value |
|----------|-------|------|---------|-------------|---------|
| AD1 | COG | α | Clustering coefficient (C) | +0.709 | 0.050 |
| AD2 | COG | θ | Spectral power F7 | -0.708 | 0.086 |
| AD2 | COG | θ | Spectral power F4 | -0.705 | 0.086 |
| AD4 | EFF | α | Clustering coefficient (C) | +0.649 | 0.036 |
| AD4 | EFF | α | Kappa divergence (K, MST) | +0.641 | 0.036 |
| AD4 | EFF | α | Path length (L, MST) | +0.664 | 0.036 |
| AD4 | EFF | δ | Average degree (A) | +0.652 | 0.036 |
| AD4 | EFF | δ | Path length (L, MST) | +0.595 | 0.064 |

**Control analysis:** Matched neutral pre-stimulus windows showed no significant correlations (all p > 0.10), confirming effects are specific to product interaction segments.

**Interpretation:**
- Negative frontal θ correlation with cognitive perspective-taking: may reflect more fluent, automatic processing (reduced cognitive control/load)
- α clustering coefficient correlation with both COG and EFF: suggests shared neural substrate between perspective-taking and ad effectiveness
- Consistent with embodied simulation framework (Gallese, 2007): understanding others relies on automatic internal representations
- α connectivity linked to working memory, social cognition, information integration
- Kappa divergence linked to reward-related behaviors and network organization

**Limitations:**
- Small sample (N=20, one excluded = 19 analyzed)
- Exploratory design, α=0.10 significance level
- Only 4 ads, differing in product type, testimonial gender, interaction type
- Short interaction segments (~3s) may limit connectivity reliability
- Scalp-level analysis (needs 64+ electrodes for source-level)
- No direct measures of working memory, cognitive load, reward processing
- Effects not consistent across all stimuli (no significant correlations for AD3)

## EEG Methodology Details

### Equipment
- NVX-36 device (Medical Computer Systems)
- 22 Ag/AgCl scalp electrodes (10-10 system)
- 2 ear clips (A1, A2), 1 mastoid patch (M1)
- Monopolar montage, referenced to M1
- 2KHz sampling, 24-bit resolution
- iMotions software for stimulus delivery

### Preprocessing Pipeline
1. Re-reference to linked A1-A2
2. Resample to 512 Hz
3. Band-pass filter 0.1-40 Hz (zero-phase Butterworth)
4. CleanLine multi-taper regression (50, 100 Hz)
5. Artifact Subspace Reconstruction (κ=20)
6. ICA (SOBI algorithm) with ICLabel classifier (P≥0.9 "not brain" removed)
7. Current Source Density transformation
8. Epoch by experimental phases
9. Individual Alpha Frequency estimation (center of gravity, 7.5-12.5 Hz)
10. Subject-specific bands: δ=[0, IAF-6], θ=[IAF-6, IAF-2], α=[IAF-2, IAF+2], β=[IAF+2, IAF+16], γ=[IAF+16, IAF+25]

### Analysis Methods
- **Spectral:** Welch's method (1s Hamming windows, 50% overlap), normalized band powers per channel
- **Connectivity:** Corrected imaginary Phase Locking Value (ciPLV), top 10% edges thresholded
- **Network measures:** Average degree, clustering coefficient, characteristic path length (raw network)
- **MST measures:** Leaf fraction, diameter, tree hierarchy, kappa divergence (Kruskal's algorithm)
- **Statistics:** Spearman correlations, FDR correction, α=0.10 (exploratory)

## Related Constructs and Distinctions

| Construct | Definition | Relationship to Mentalizing |
|-----------|------------|----------------------------|
| Mentalizing | Inferential process of attributing mental states to self/others | Core process |
| Empathy | Emotional resonance and affective sharing | More complex; involves mentalizing + emotional component |
| Theory of Mind | Use of societal heuristics to interpret behavior | Related but uses heuristics rather than fundamental inference |
| Perspective-taking | Cognitively adopting another's viewpoint | Subset of mentalizing; visuo-spatial vs. cognitive vs. emotional |

## Ethical Framework

1. **Psychological privacy:** Mentalizing measurement captures how individuals infer others' intentions, beliefs, emotions
2. **Manipulation risk:** Insights could be used to design non-transparent persuasive strategies
3. **Informed consent:** Required for all mentalizing research
4. **Transparency:** Must explain how data are collected and used
5. **Data protection:** Strict adherence to neuroscience and consumer research ethical guidelines
6. **Non-exploitative use:** Responsible and non-manipulative application required