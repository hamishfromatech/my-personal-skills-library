# Multimodal EEG + Eye-Tracking Consumer Choice Evidence Base

## Primary Sources

### 1. Usman et al. (2025) — Frontiers in Computational Neuroscience
**"Multimodal consumer choice prediction using EEG signals and eye tracking."**
- Dataset: NeuMa (42 participants, 144 products, EEG 300 Hz + ET 120 Hz).
- Preprocessing: Butterworth bandpass (0.5-45 Hz), ASR + FORCe artifact removal, SMOTE.
- EEG features: statistical + Welch PSD + DWT (handcrafted) + CNN-LSTM (deep).
- ET features: fixation duration, saccade amplitude, dwell time, Hjorth parameters (handcrafted) + LeNet-5 on gaze plots (deep).
- Classifier: stacking ensemble — base = Random Forest, Gradient Boosting, XGBoost; meta = Random Forest.
- Result: **84.01% accuracy**, 0.83 precision, 0.84 recall, 0.83 F1 (AUC 0.89).
- Unimodal baselines: EEG-only 62%, ET-only 65% → multimodal gain ~20pp.

### 2. Mallick et al. (2026) — Research Square preprint
**"Novel Framework for Multimodal Classification of Consumer Preference using EEG signals and eye-tracking data."**
- Dataset: NeuMa (preprocessed version).
- Architecture: Wide & Deep hybrid.
  - Deep stream: InceptionTime (parallel 1D convolutions, kernels 10/20/40) → 128-dim embedding.
  - Wide stream: 44 time-domain statistics (19 EEG means + 19 EEG stds + 6 ET stds).
  - Fusion: CatBoost gradient boosting on 172-dim concatenated features.
- Key innovations:
  - **Modality dropout**: 30% probability of zeroing eye-tracking channels during training → forces robust EEG learning (without it, accuracy drops to 72.1%).
  - **Grokking-oriented training**: 100 epochs, weight decay 0.02, label smoothing 0.1, cosine annealing with warm restarts.
  - **Strict checkpointing**: save only on validation-loss improvement.
- Result: **77.51% accuracy (±0.49%)**, weighted F1 0.7460 (±0.21%); +5.51pp over prior best on NeuMa (Georgiadis 2023: 72%).
- Ablation: EEG-only 75.23%, ET-only 68.41% → multimodal gain, modality dropout essential.

### 3. Koduru et al. (2026) — Zenodo
**"Neuromarketing Analytics For Predicting Consumer Purchase Intent In Digital Marketplaces."**
- Dataset: 120 participants, 500 e-commerce images; EEG + ET + GSR.
- Architecture: Temporal Convolutional Network (TCN) + multi-head attention.
- Features: spectral EEG (theta, alpha, beta, gamma), ET (fixation duration, saccade amplitude, pupil dilation), GSR phasic responses.
- Result: **89.2% accuracy** (AUC not reported); outperforms unimodal (EEG 76.4%, ET 78.1%, GSR 71.2%).
- Strongest predictors: gamma-band power (30-45 Hz) during product exposure + pupil dilation change.

### 4. Sathiya & Jeyanthi (2026) — IJSRET
**"Neuromarketing Signals And Consumer Purchase Intent Prediction Using EEG And Computer Vision."**
- Dataset: 120 participants, 500 e-commerce images; EEG + computer-vision eye-tracking.
- Architecture: TCN (EEG branch) + Graph Attention Network (visual attention branch).
- Result: **88.3% accuracy**, AUC 0.94; unimodal EEG 74.2%, visual 72.8%.

### 5. Kalaganis et al. (2025) — Brain Informatics
**"A hybrid neuromarketing approach exploiting EEG graph signal processing and gaze dynamic patterning."**
- Dataset: NeuMa.
- EEG branch: Graph Fourier Transform (GFT) on envelope-correlation functional connectivity → eigenmode power features.
- ET branch: Hjorth behavioural descriptors + marketing metrics (fixations, dwell time, time-to-first-fixation, blink rate).
- Classifier: Linear SVM with Wilcoxon feature selection; LOOCV per subject.
- Result: κ=0.35 (fair-to-moderate), F1=0.54; outperforms typical neuromarketing indices (κ=0.05) and hybrid indices (κ=0.25).
- Physiological finding: strong couplings beyond frontal asymmetry (prefrontal ↔ occipital); subject-specific connectivity patterns.

### 6. Iyappan et al. (2025) — Journal of Marketing & Social Research
**"Neuromarketing Insights for Predicting Consumer Purchase Intent."**
- Dataset: 285 participants, 20 video ads (emotional vs informational), 4 product categories.
- Tools: 32-channel Emotiv EEG (256 Hz) + Tobii Pro Nano eye-tracker.
- Analysis: correlation, multiple regression, ANOVA.
- Result: R²=0.53; fixation duration strongest predictor (β=0.38, r=0.48), frontal alpha (β=0.31, r=0.42), number of fixations (β=0.21), frontal beta (β=0.18).
- Emotional ads → higher purchase intent (M=4.01) than informational (M=3.43), F(1,283)=16.8, p<0.001.

## Comparative Accuracy Table

| Study | Year | Modalities | Model | Accuracy | Dataset |
|---|---|---|---|---|---|
| Usman et al. | 2025 | EEG + ET | Stacking ensemble | 84.01% | NeuMa (42 subj) |
| Mallick et al. | 2026 | EEG + ET | InceptionTime + CatBoost | 77.51% | NeuMa (42 subj) |
| Koduru et al. | 2026 | EEG + ET + GSR | TCN + attention | 89.2% | 120 subj, 500 images |
| Sathiya & Jeyanthi | 2026 | EEG + CV-ET | TCN + GAT | 88.3% | 120 subj, 500 images |
| Kalaganis et al. | 2025 | EEG (GFT) + ET | Linear SVM | κ=0.35 | NeuMa (42 subj) |
| Iyappan et al. | 2025 | EEG + ET | Regression | R²=0.53 | 285 subj, 20 ads |
| Georgiadis et al. | 2023 | EEG only | SPDNet + Riemannian | 72% | NeuMa (42 subj) |
| Afshar & Azimi | 2025 | EEG only | GNN (ResidualGCN) | 69.1% | NeuMa (42 subj) |

## The Fusion Paradox and Modality Dropout

**Problem**: In multimodal learning, one modality may dominate, causing the model to "lazily" rely on the easier signal and fail to learn complementary representations from the harder modality.

**Solution (Mallick et al. 2026)**: Randomly zero eye-tracking channels with probability 0.3 during training.
- Without modality dropout: accuracy 72.1% (collapses to ET-only performance).
- With modality dropout: accuracy 77.51% (genuine multimodal learning).
- The network is forced to learn robust EEG patterns rather than lazily relying on stronger eye-tracking signals.

## Cross-References

- `graph-neural-network-neuromarketing` — GNN EEG-only companion; the topology-aware alternative.
- `hybrid-eeg-gaze-decoding-scheme` — GFT + gaze (Kalaganis 2025); the graph-signal-processing variant.
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad; multimodal implements the measurement layer.
- `neuromarketing-predictive-purchase-intent-model` — purchase-intent prediction framework; multimodal provides the implementation.
- `concept2brain-predictive-neural-response-model` — synthetic EEG; multimodal validates against real EEG.
- `forward-prediction-neuromarketing-framework` — direction-of-inference and vendor evaluation.

## A-Tech Value Alignment

- **Open-source AI**: NeuMa is publicly available; InceptionTime and CatBoost are open-source; modality dropout is a training strategy not a proprietary technique.
- **Data privacy**: EEG + ET can run on local devices; federated multimodal training is feasible.
- **Financial freedom**: Pre-launch ad testing with 84-89% accuracy prevents wasted marketing spend; modality dropout reduces compute waste.
- **Practical implementation**: All architectures have open-source implementations; 5-fold cross-validation is reproducible; modality dropout is a one-line code change.