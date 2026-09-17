# GNN Neuromarketing Evidence Base

## Primary Source

**Afshar, M.P. & Azimi, A. (2025). "EEG-Based Consumer Behaviour Prediction: An Exploration from Classical Machine Learning to Graph Neural Networks." arXiv:2509.21567v2.**

### Dataset
- NeuMa dataset (Georgiadis et al., 2023, Sci Data).
- 42 participants (23 M, 19 F; age 31.5 ± 8.84).
- 144 supermarket products across 6 brochure pages.
- 21 dry EEG sensors (DSI-24), 300 Hz, 19 analysis channels (10-20 system).
- Tobii Pro Fusion eye-tracker, 120 Hz.
- Task: browse digital brochure, left-click products to "buy".
- Binary labels: Buy (minority, ~20%) vs NoBuy (majority, ~80%).

### Classical Models (three pipelines)
- Pipeline A: high-correlation removal (|r|>0.9) → StandardScaler → PCA (90% variance).
- Pipeline B: high-correlation removal → StandardScaler → UMAP (50 components).
- Pipeline C: top-100 features by independent t-test (p<0.05) → StandardScaler → PCA (95% variance).
- Models: LR, KNN, RF, XGBoost, LightGBM, SVM-RBF, Naïve Bayes, Gaussian Process.
- Stacking ensemble: base = NB, LR, KNN, LightGBM; meta = XGBoost.

### Classical Results (Pipeline A, representative)
| Model | Accuracy | Class 1 Recall |
|---|---|---|
| Random Forest | 0.786 | 0.01 |
| SVM-RBF | 0.780 | 0.18 |
| LightGBM | 0.743 | 0.24 |
| Logistic Regression | 0.593 | 0.44 |

Key finding: classical models achieve high overall accuracy by predicting the majority class; minority (buy) recall collapses.

### GNN Architecture Construction
- Node features: spectral features (FFT + Welch PSD) per electrode across delta/theta/alpha/beta/gamma bands → 98-dim per electrode.
- Graph: fully-connected, 19 nodes; edge weights = |Pearson correlation| between electrode spectral-feature vectors.
- GNN models evaluated: BaselineGCN, BaselineGAT (4+1 heads), BaselineSAGE, ResidualGCN, HybridModel (MLP+GCN+GAT), RegularizedGNN (LayerNorm), LightweightGCN, BalancedGAT, MultiGNN (3 parallel branches), ResidualAttentionGNN, DeepGNN (3 hierarchical blocks).
- Training: AdamW (lr=0.001, wd=0.01), batch=32, ReduceLROnPlateau (factor=0.5, patience=5), max 100 epochs, early stop (patience=15), class-weighted cross-entropy.
- Validation: K-fold (5-fold) stratified cross-validation.

### GNN Results
| Model | Accuracy | Class 1 Recall | Class 1 F1 |
|---|---|---|---|
| BaselineGCN | 0.637 | 0.42 | 0.33 |
| BaselineGAT | 0.631 | 0.45 | 0.34 |
| BaselineSAGE | 0.628 | 0.46 | 0.34 |
| ResidualGCN | 0.691 | 0.25 | 0.25 |
| BalancedGAT | 0.619 | 0.54 | 0.37 |
| DeepGNN | 0.677 | 0.35 | 0.31 |
| HybridModel | 0.627 | 0.48 | 0.35 |

Key finding: GNNs achieve lower overall accuracy than classical models but substantially higher minority-class recall (BalancedGAT: Class 1 Recall 0.54 vs Random Forest: 0.01). GNNs model brain connectivity structure classical approaches flatten.

### Theoretical Implications
- Graph-based modelling introduces a novel method in neuromarketing: instead of time/frequency domain features, brain connectivity captures inter-regional interactions.
- Decision-making involves distributed brain networks; GNNs can model these interactions directly.
- Future direction: hybrid classical + GNN architectures combining accuracy and minority detection.

### Practical Implications
- Companies can use GNN-based EEG classification to segment consumers by brain response patterns.
- Pre-launch ad testing: show multiple ad versions, use GNN to select the version with strongest "buy" connectivity signature.
- Personalised marketing on a deeper level than surveys.

### Limitations
- Small dataset (42 participants); results may vary with larger populations.
- Binary classification only; multi-class preference gradations not explored.
- Future: combine with eye-tracking, physiological data, fMRI/fNIRS.

## Related Prior Work (comparative baselines)

| Study | Year | Model | Dataset | Accuracy |
|---|---|---|---|---|
| Özbeyaz | 2021 | ANN, SVM | 11 subjects, smartphones | 72% |
| Zeng et al. | 2022 | KNN, SVM | 15 subjects, sport shoes | 94.22% |
| Mashrur et al. | 2022 | SVM, KNN, NB, DT | 20 subjects, 3 ads | 87% (affective attitude) |
| Hakim et al. | 2023 | Deep learning | 213 subjects, 72 products | 85% (WTP) |
| Azadravesh et al. | 2024 | CNN-LSTM | 25 subjects, 42 products | 97.12% |
| Georgiadis et al. | 2023 | SPDNet + Riemannian | NeuMa | 72% |
| Usman et al. | 2025 | Stacking ensemble (RF+GB+XGB) | NeuMa | 84.01% |
| Mallick et al. | 2026 | InceptionTime + CatBoost | NeuMa | 77.51% |

## Cross-References

- `hybrid-eeg-gaze-decoding-scheme` — GFT-based EEG + eye-tracking hybrid (Kalaganis et al., 2025); the graph-theoretic companion to this skill.
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory-AI triad; GNN extends the AI model layer.
- `in-silico-neuromarketing-platform-pattern` — TRIBE v2 open-source neural engagement; GNN complements with connectivity-level features.
- `concept2brain-predictive-neural-response-model` — CLIP-to-EEG synthesis; GNN can classify the synthesised responses.
- `neuromarketing-predictive-purchase-intent-model` — purchase-intent prediction; GNN adds the minority-class detection layer.

## A-Tech Value Alignment

- **Open-source AI**: NeuMa dataset is publicly available (Sci Data); GNN architectures are implemented in PyTorch Geometric (open-source).
- **Data privacy**: EEG-based classification can run on local devices; no cloud data transmission required.
- **Financial freedom**: GNN-based ad pre-testing reduces wasted marketing spend by detecting the rare "buy" signal classical models miss.
- **Practical implementation**: Code available; 98-dim spectral features are computationally tractable; 5-fold cross-validation is reproducible.