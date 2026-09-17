# Evidence Base: Multimodal EEG-CV Purchase Intent Prediction

## Source
Sathiya, A., & Jeyanthi, P. (2026). Neuromarketing Signals And Consumer Purchase Intent Prediction Using EEG And Computer Vision. IJSRET, May 2026.

## Study Design
- **Participants**: 120 participants viewing 500 e-commerce images
- **EEG Device**: 14-channel Emotiv EPOC+ (consumer-grade)
- **Eye Tracker**: Screen-based Tobii Pro Fusion
- **Task**: Binary purchase intent classification (buy vs. no-buy)

## Feature Extraction

### EEG Features (TCN Branch)
1. **Frontal asymmetry of alpha activity** (8-13 Hz): Approach-withdrawal motivation index
2. **Theta/beta ratio** (4-8 Hz / 13-30 Hz): Cognitive load and attention engagement
3. **Late positive potential (LPP)**: Emotional processing and motivational relevance

### Visual Attention Features (GAT Branch)
1. **Fixation density**: Spatial concentration of gaze points
2. **Saccade dynamics**: Eye movement patterns between fixation points
3. **Pupil size**: Arousal and cognitive effort indicators

## Model Architecture

### Temporal Convolutional Network (TCN)
- Processes EEG signals as temporal sequences
- Captures temporal dependencies in neural responses
- Dilated causal convolutions for receptive field expansion

### Graph Attention Network (GAT)
- Maps visual attention features as graph structure
- Nodes: fixation points; Edges: saccade connections
- Attention mechanism weights importance of visual regions
- Multi-head attention (4 heads) for robust feature extraction

### Hybrid Fusion
- Early fusion: concatenation of TCN and GAT feature vectors
- Joint representation captures temporal neural + spatial visual information

## Results

| Model | Accuracy | AUC |
|-------|----------|-----|
| Hybrid TCN+GAT | 88.3% | 0.94 |
| Unimodal EEG (TCN) | 74.2% | 0.81 |
| Unimodal Visual (GAT) | 72.8% | 0.78 |

## Key Findings
1. Hybrid model outperforms unimodal approaches by ~14-16 percentage points
2. EEG frontal asymmetry is strongest predictor of approach motivation
3. Fixation density is strongest visual attention predictor (β=0.38)
4. Consumer-grade EEG (Emotiv EPOC+) sufficient for research-grade prediction
5. Modality dropout in training solves fusion paradox (model robustness)

## A-Tech Alignment
- **Open-source AI**: PyTorch, consumer-grade EEG tools, open-weight models
- **Data privacy**: On-device EEG processing, consent-gated neural data
- **Financial freedom**: Lower CAC for SMEs via neuro-informed product design
- **Practical implementation**: Reproducible with consumer-grade hardware

## Cross-References
- `cognitive-targeting-ai-advertising` — cognitive state targeting framework
- `neuro-marketing-privacy-first-behavioral-analytics` — privacy-first neuromarketing
- `multimodal-eeg-eye-tracking-consumer-choice` — multimodal EEG+ET comparison
- `hybrid-eeg-gaze-decoding-scheme` — GFT + eye-tracking hybrid approach
- `forward-prediction-neuromarketing-framework` — predictive neuromarketing