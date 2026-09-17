# Concept2Brain Evidence Base

## Source
Santos-Mayo, A., Gilbert, F., Mirifar, A., Tebbe, A-L., Fang, R., Ding, M., & Keil, A. (2026). "Concept2Brain: an AI model for predicting neurophysiological responses to text and pictures." *Nature Communications*. https://doi.org/10.1038/s41467-026-75653-x

Published: 23 July 2026

## Key Findings

### Model Architecture
- Deep network designed to generate synthetic electrophysiological responses to semantic/emotional information conveyed through pictures or text
- Leverages CLIP (OpenAI) to generate a representation of a stimulus
- Maps the representation into an electrophysiological latent space
- Generates synthetic neural responses that closely resemble those observed empirically

### Validation
- The openly available resource generates synthetic neural responses that closely resemble empirically observed EEG data
- Provided as a web service tool for creating open and reproducible EEG datasets
- Can predict brain responses to any semantic concept or picture

### Applications
- AI-driven brain activity modeling
- New possibilities for studying how the brain represents the world
- Pre-testing stimuli before empirical studies
- Creating synthetic EEG datasets for research

### Funding
- National Institute of Health (R01MH125615, R01MH112558)
- National Science Foundation (2318984)

## Comparison to Traditional Neuromarketing

| Dimension | Traditional EEG Study | Concept2Brain Prediction |
|---|---|---|
| Cost | $50K-$200K per study | Near-zero (API cost) |
| Time | 6+ months | Seconds to minutes |
| Subjects | 20-30 humans required | None (synthetic) |
| Privacy | Collects neural data from humans | No human subjects |
| Reproducibility | Variable (subject variability) | Deterministic |
| Stimulus type | Constrained by lab setup | Any text or image |
| Generalizability | Limited to tested stimuli | Any semantic concept |

## A-Tech Value Alignment

| A-Tech Value | Concept2Brain Alignment |
|---|---|
| **Open-source AI** | Model is openly available; enables reproducible research |
| **Data privacy** | No human subjects required; eliminates privacy concerns of neural data collection |
| **Financial freedom** | Eliminates the $50K-$200K cost barrier of traditional neuromarketing studies |
| **Practical implementation** | Web service available; API-integratable; immediate deployment |

## Limitations
- Population-level predictions (not individual subject responses)
- Research-grade validation (not clinical-grade)
- Requires empirical validation for regulatory contexts
- Predicts EEG-like responses, not direct fMRI signals
- Quality depends on CLIP representation fidelity to stimulus semantics

## Adjacent Skills
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad; Concept2Brain provides the predictive layer
- `hybrid-eeg-gaze-decoding-scheme` — empirical validation of Concept2Brain predictions
- `predictive-neuromarketing-bayesian-framework` — computational theory; Concept2Brain is the implementation
- `neuro-marketing-privacy-first-behavioral-analytics` — privacy-first architecture; Concept2Brain is the extreme case
- `in-silico-neuromarketing-platform-pattern` — in-silico platform pattern; Concept2Brain is the foundational model