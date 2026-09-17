# Information Symmetry Evidence Base

## Source
D'Ambrogio, S., Grohn, J., Khalighinejad, N., Mattar, M. G., Hunt, L. T., & Rushworth, M. F. S. (2026). "Interpretable abstractions of artificial neural networks predict behavior and neural activity during human information gathering." *Nature Neuroscience*. https://doi.org/10.1038/s41593-026-02342-9

Published: 26 June 2026

## Study Design

### Task
- Information-sampling task: 20 participants (14 females), ages 19-32
- Two patches of 100 moving dots each; true color (red or black) hidden under covers
- Participants sample by hovering; revealed colors return to gray when switching away (memory-dependent)
- 80% of trials: one patch blocked (2-option); 20%: all three available (3-option)
- Four sessions per participant (~25 min each)

### Models Compared
1. **Linear:** VoI decreases at constant rate with each sample
2. **UCB:** Diminishing returns; steeper initial decline that flattens
3. **Hybrid ANN:** Deep network learns mapping from state to VoI (no fixed form)
4. **Symbolic:** 4-parameter expression recovered via symbolic regression from the trained ANN

### Validation
- Hybrid ANN outperformed linear and UCB for all participants (cross-validated)
- Symbolic model performed comparably to the full ANN
- Symbolic model outperformed UCB on an independent two-armed bandit dataset (n=89, Wilcoxon p=4.31×10⁻¹⁶)
- Even with offset parameter, symbolic model still outperformed UCB (p=2.21×10⁻¹²)

## Neural Correlates (7T fMRI)

### Regions of Interest
- VTA (ventral tegmental area)
- SN (substantia nigra)
- DRN (dorsal raphe nucleus)
- LC (locus coeruleus)
- VSN (ventral septal nucleus)
- ACC (anterior cingulate cortex)
- AI (anterior insula)

### Key Findings
- **VTA:** Positive coding of sum of VoS (β=11.3, p=0.020); negative coding of sum of VoI (β=−46.1, p=1.93×10⁻⁵). Pattern suited to arbitrating between sampling and choosing.
- **ACC and AI:** Track value-of-information computations (whole-brain GLM)
- **SN:** Negative association with both sum (β=−24.9, p=6.38×10⁻⁵) and difference (β=−22.7, p=0.008) of VoI
- **LC, DRN, VSN:** No significant association with VoI or VoS (within sensitivity limits)

### Model Comparison in Neural Data
- ANN-derived VoI provided better fit to neural data than linear (Cohen's d=−1.025, p=2.12×10⁻¹²) and UCB (d=−1.105, p=5.64×10⁻¹³)
- Symbolic function performed almost as well as ANN (d=−0.129, p=0.006) — negligible effect size, confidence interval overlapping zero
- Multivariate RSA confirmed AI and ACC (but not VTA or SN) covaried with trial-by-trial sampling duration

## The Symbolic Equations

### Value of Staying
$$V_{stay} = \beta_1 + \exp\left(-|\beta_2| \cdot \frac{N_{attended}}{N_{unattended}}\right)$$

### Value of Switching
$$V_{switch} = \beta_3 + \exp\left(-|\beta_4| \cdot \frac{\log(2 \cdot N_{unattended})}{N_{attended}}\right)$$

### Psychological Interpretation
- **β₁ (attentional inertia):** Baseline tendency to continue sampling from currently attended option
- **β₂ (information satiation):** Rate at which accumulated evidence reduces desire to stay (negative — value of staying decreases as Na/Nu increases)
- **β₃ (undirected exploration):** Baseline tendency to explore alternatives
- **β₄ (directed exploration):** Rate at which learning about current option increases interest in alternatives (negative — value of switching increases as Na grows relative to Nu)

### Key Insight
The ratio Na/Nu is scale-invariant: 100:10 and 10:1 both yield Na/Nu = 10. This means the brain seeks information *symmetry* — balancing knowledge across options — rather than minimizing per-option uncertainty.

## A-Tech Value Alignment

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | The symbolic regression approach makes the model transparent and reproducible; the 4-parameter expression is auditable |
| **Data privacy** | No biometric data; uses behavioral data only; applicable to UX design without privacy concerns |
| **Financial freedom** | Improves decision-making quality; applies to product design, pricing, and choice architecture |
| **Practical implementation** | The symbolic equations are computationally trivial; deployable in any decision-support interface |

## Limitations
- Two-option structure; extension to n-armed bandits requires adaptation (e.g., replacing N_unattended with mean across unattended options)
- Cannot dissociate decision uncertainty, evidence uncertainty, and reward uncertainty (highly correlated in design)
- Cross-task generalization tested on bandit tasks; broader validation needed
- The study did not permit dissociation of different uncertainty types

## Adjacent Skills
- `cognitive-load-reduction-for-ide` — cognitive load during evaluation; information symmetry reduces cognitive load by aligning with neural valuation
- `predictive-processing-interface-design` — predictive processing framework; information symmetry is the computational mechanism
- `curiosity-gap-progressive-disclosure` — curiosity-driven information seeking; symmetry principle explains when curiosity shifts between options
- `trust-calibration-ux-pattern` — trust calibration during evaluation; symmetry governs how evidence accumulates across trusted alternatives