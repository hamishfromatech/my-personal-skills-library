# Brand Perception Visual System Tagging Evidence Base

## Source
Marques dos Santos, J. P., & Marques dos Santos, J. D. (2024). "Explainable artificial intelligence (xAI) in neuromarketing/consumer neuroscience: an fMRI study on brand perception." *Frontiers in Human Neuroscience*, 18:1305164. doi: 10.3389/fnhum.2024.1305164

Published: 22 March 2024

## Study Design

### Participants
- 22 subjects (13 male, 9 female) with complete fMRI acquisitions
- Behavioral session for stimuli selection (SAM + PAD scales)
- fMRI session with passive impression formation paradigm

### Stimuli
- 4 categories: brands|preferred (BP), brands|indifferent (BI), objects (O), people (P)
- 40 examples per category (20 positive statements, 20 negative statements)
- Each slide presented for 4.0s followed by 3.5s fixation cross
- No motor response required (passive paradigm)

### fMRI Processing
- TR = 2500ms, 485 volumes per subject
- FSL preprocessing: motion correction, slice-timing, brain extraction, smoothing (5mm FWHM), high-pass filtering
- ICA (MELODIC) for dimensionality reduction: 125 ICs explaining 72.2% variance
- Feature extraction at hemodynamic peak (6.5s post-stimulus onset)

## Model Architecture

### ANN Configuration
- Shallow neural network (SNN): single hidden layer, 10 hidden nodes
- AMORE package in R (version 4.3.1)
- Backpropagation feedforward, adaptive gradient descent with momentum
- tansig activation (hidden), sigmoid activation (output)
- Hyperparameters: learning rate 1e-05, momentum 0.975, 500 epochs
- Best network from 50,000 random initializations

### Pruning and Retraining
- Path-weights analysis identified elbow points per output
- Pruned network: sparse architecture with retained connections
- Retrained with new hyperparameters (learning rate 9e-08, momentum 0.850)
- Retrained accuracy (55.9%) surpassed fully connected (54.6%)

### xAI Methods
- **Path-weights:** Product of input→hidden and hidden→output weights; identifies influential paths
- **Shapley values (SHAP):** DeepLIFT-based explainer; approximates conditional expectations of Shapley values
- **Hidden layer analysis:** SHAP values computed for hidden nodes to assess contribution

## Key Results

### Classification Performance
| Network | Accuracy | BP Precision | BI Precision |
|---|---|---|---|
| Best (fully connected) | 54.6% | 41.3% | 44.1% |
| Pruned | 50.4% | 39.7% | 40.1% |
| Retrained | 55.9% | 45.2% | 46.3% |

### Critical ICs for Brand Perception
- **IC2:** Temporal-occipital fusiform cortex, lateral occipital cortex (inferior division)
- **IC6:** Occipital pole, lateral occipital cortex (superior division), temporal areas
- **IC13:** Supracalcarine cortex, cuneal cortex, lingual gyrus, lateral occipital cortex (inferior)
- **IC15:** Occipital pole, lateral occipital cortex (inferior division)
- **IC47:** Inferior temporal gyrus (temporooccipital part)

### ICs Specific to Preferred Brands (BP)
- **IC4:** Cuneal cortex activation; bilateral deactivation in fusiform; temporal pole deactivation; superior frontal gyrus deactivation
- **IC37:** Bilateral precentral gyrus activation; supplementary motor cortex activation; lateral occipital cortex (superior)
- **IC40:** Ventral medial prefrontal cortex (paracingulate gyrus, frontal medial cortex) — extensive activation

### ICs Specific to Indifferent Brands (BI)
- **IC57:** Bilateral superior temporal gyri, planum temporale
- **IC78:** Bilateral frontal pole (dorsolateral prefrontal cortex)

## The "Tagging" Finding

### Core Insight
The most important finding is that a split between processing preferred vs. indifferent brands may occur during early processing stages, still in the visual system. There is no evidence of a "decision pipeline" that yields whether a brand is preferred or indifferent.

### Implications
1. Brand preference is "tagged" in visual areas before reaching the prefrontal cortex
2. The visual system contains sufficient information to classify brand preference
3. This is a parallel "tagging" process, not a sequential decision pipeline
4. Visual identity design directly impacts preference formation

### Limitations
- fMRI temporal resolution (TR = 2.5s) cannot sequence processes precisely
- EEG would be better suited for temporal sequencing
- Accuracy is modest (55.9%) — brand preference is not deterministic from visual cortex alone
- Machine learning methods are evolving; pipeline may improve

## A-Tech Value Alignment

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | xAI methods (SHAP, path-weights) are open-source; model architecture is transparent |
| **Data privacy** | fMRI data is anonymized; xAI analysis does not require personal data beyond neural responses |
| **Financial freedom** | Understanding brand perception enables better product positioning; reduces wasted marketing spend |
| **Practical implementation** | xAI pipeline is reproducible; applicable to brand identity design; deployable with Concept2Brain predictions |

## Adjacent Skills
- `concept2brain-predictive-neural-response-model` — predicts neural responses; validates visual tagging hypothesis
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad; visual tagging is the attention/memory interface
- `neurodesign-memory-embedding` — memory embedding; visual tagging is the encoding mechanism
- `narrative-transportation-developer-trust` — narrative transportation; visual tagging occurs before narrative processing
- `trust-calibration-ux-pattern` — trust calibration; visual tagging is the first stage of trust formation