# Evidence Base: Hybrid EEG Gaze Graph Signal Neuromarketing

## Primary Source

**Kalaganis, F. P., Georgiadis, K., Oikonomou, V. P., Laskaris, N. A., Nikolopoulos, S., & Kompatsiaris, I. (2025).** A hybrid neuromarketing approach exploiting EEG graph signal processing and gaze dynamic patterning. *Brain Informatics*, 12, 23. https://doi.org/10.1186/s40708-025-00272-z

- Open access (CC BY-NC-ND 4.0)
- Published: 23 September 2025
- Project NeuroMkt (EU ERDF + Greek National Funds)

## Dataset: NeuMa

| Property | Value |
|----------|-------|
| Participants | 42 healthy adults (23M, 19F) |
| Age | 31.5 ± 8.84 years |
| Task | Digital supermarket brochure browsing |
| Pages | 6 pages, 24 products/page (144 total) |
| Selection | Click products intended to purchase |
| EEG device | Wearable Sensing DSI 24 (19 dry electrodes, 10-20 system) |
| EEG sampling | 300 Hz |
| Eye tracker | Tobii Pro Fusion (120 Hz) |
| Electrodes | Fp1, Fp2, Fz, F3, F4, F7, F8, Cz, C3, C4, T7, T8, Pz, P3, P4, P7, P8, O1, O2 |
| References | A1, A2 (mastoid) |
| Preprocessing | Butterworth bandpass [0.5-45 Hz], ASR + FORCe artifact removal |
| Epochs | Gaze-defined (per-product), >0.5s minimum |
| Labels | "Buy" (clicked) vs "NoBuy" (not clicked) |

## Methodology

### Graph Fourier Transform (GFT) on EEG

#### Graph Construction
1. Filter EEG to 8-30 Hz (alpha + beta bands)
2. Compute envelope correlation between electrode pairs:
   ```
   ρ^{m,n} = |ρ(A_m(t), A_n(t))|
   ```
   where A_m(t) is the Hilbert transform envelope of signal m
3. Weighted adjacency matrix W (19×19), values in [0,1]
4. Graph Laplacian: L = D - W (D = degree matrix)
5. Eigenanalysis: U = [u_0, u_1, ..., u_{18}], eigenvalues λ_k

#### GFT Application
```
X̃ = U^T X
```
where X is the multichannel EEG signal (19 × T)

#### Feature Extraction
```
p(m) = (1/T) Σ_{t=1}^{T} |X̃(m,t)|²
```
Power per eigenmode as feature vector

### Eye-Tracking Descriptors

#### Behavioral Features (Hjorth Parameters)
Computed on accumulated gaze distance trajectory:
```
Activity(x(t)) = var(x(t))
Mobility(x(t)) = sqrt(var(dx/dt) / var(x(t)))
Complexity(x(t)) = mobility(dx/dt) / mobility(x(t))
```
- Activity: total trajectory length (overall power)
- Mobility: mean frequency (speed of eye motion changes)
- Complexity: bandwidth (irregularity in motion)

#### Marketing Features
- Number of fixations
- Duration of fixations
- Time to first fixation
- Dwell time
- Blink rate

### Fusion and Classification
1. Early fusion: concatenate GFT-EEG features + ET features
2. Per-subject LOOCV (Leave-One-epoch-Out Cross Validation)
3. Wilcoxon statistic-based feature selection (train set only)
4. Linear SVM classification

## Results

### Classification Performance (Averaged Across Subjects)

| Method | Cohen's κ | F1-score | Interpretation |
|--------|-----------|----------|----------------|
| EEG_indices | ~0.05 | ~0.31 | Slight agreement |
| ET_indices | ~0.25 | ~0.48 | Fair agreement |
| Hybrid_indices | ~0.25 | ~0.48 | Fair agreement |
| GFT_EEG | ~0.18 | ~0.44 | Fair agreement |
| **GFT_hybrid** | **~0.35** | **~0.54** | Fair-to-moderate agreement |

- GFT_hybrid outperforms all competing approaches with statistical significance (Wilcoxon signed-rank test, p < 0.05)
- κ improvement: +0.08 to +0.30 over baselines
- F1 improvement: +0.06 to +0.23 over baselines

### Per-Subject Performance
- Most subjects: κ in fair range (~0.35)
- Only 2 subjects (S12, S32): non-positive κ values
- Some subjects (S04, S07): substantial κ values
- High subject variability in EEG-based approaches

### Key Findings

#### 1. GFT Outperforms Traditional EEG Indices
- GFT_EEG (κ≈0.18) significantly outperforms EEG_indices (κ≈0.05)
- Traditional indices (AW, mental workload, memorization) insufficient for complex decision-making
- GFT captures inter-channel connectivity that power-based indices miss

#### 2. Eye-Tracking More Reliable Than EEG Alone
- ET_indices (κ≈0.25) > EEG_indices (κ≈0.05)
- Eye movements capture phenomena directly related to stimulus
- Brain processes are more complex and interrelated

#### 3. Hybrid Fusion Outperforms Either Modality
- GFT_hybrid (κ≈0.35) > GFT_EEG (κ≈0.18) + ET_indices (κ≈0.25)
- Complementary information from both modalities
- Early fusion allows classifier to learn optimal feature weighting

#### 4. Only Hybrid Achieves Meaningful Performance
- Neither EEG nor ET alone reaches moderate agreement
- Only their combination achieves fair-to-moderate performance

## Physiological Findings

### Brain Connectivity Patterns

#### Consistent Couplings (Across Subjects)
- **Fp1-Fp2**: Prefrontal asymmetry (well-documented, decision-making)
- Prefrontal area connections (highly associated with everyday decisions)

#### Subject-Specific Connections
- Most salient functional connections varied across individuals
- Only a limited subset shared among subjects
- Connections extend beyond conventional frontal dipoles
- Prefrontal-occipital couplings observed (distinct brain regions)

#### Implication
Decision-making is multi-factorial, utilizing brain areas not studied in typical neuromarketing schemes. The process differs for each individual, affected by preference, emotional judgment, and various cognitive sub-processes.

### Eye-Tracking Behavioral Patterns

#### "Buy" Condition
- ~80% of behavioral features fall in lower-value octant
- Reduced mobility, activity, and complexity
- Indicates gaze stabilization on intended purchase product
- Increased focus and attention

#### "NoBuy" Condition
- Behavioral features cover full range of values
- More varied gaze patterns
- Less focused attention

#### Limitation
"Buy" condition gaze stabilization has limited predictive power alone because "NoBuy" samples also populate the same area. Specificity for "Buy" class, not full discrimination.

## Technical Limitations

1. **EEG spatial resolution**: Cannot detect deep brain structures (hippocampus, limbic system)
2. **Subject variability**: High inter-subject variability in EEG requires personalized models
3. **Dataset size**: 42 participants limits generalization
4. **Task specificity**: Supermarket brochure browsing may not generalize to all purchase scenarios
5. **Online implementation**: Requires calibration session for graph construction
6. **Riemannian geometry limitation**: Cannot combine directly with ET features due to Riemannian structure particularities

## Comparison with Related Methods

| Method | Approach | Modality | κ | F1 |
|--------|---------|---------|---|-----|
| Traditional indices | AW, MW, memorization | EEG only | ~0.05 | ~0.31 |
| RNeuMark | Riemannian geometry | EEG only | Similar to GFT_EEG | Similar |
| GFT_hybrid (this) | Graph Fourier Transform | EEG + ET | ~0.35 | ~0.54 |
| GNN (Afshar & Azimi) | Graph Neural Networks | EEG only | Lower accuracy, better minority recall | — |
| Attention+Riemannian (Kalaganis 2025) | Self-attention + SCMs | EEG only | 77.80% balanced accuracy | — |

## Future Directions

1. **Multi-layer graph**: Cover functional connectivity for additional frequency bands
2. **Subject-independent decoding**: Achieve consistency for generalized solutions
3. **Deep learning + GFT**: Explore DL architectures with GFT concepts
4. **Real-time implementation**: Online system with dynamic graph updates
5. **Multimodal expansion**: Add GSR, facial coding, pupillometry

## A-Tech Alignment

- **Open-source AI**: NeuMa public dataset, standard libraries (scikit-learn, PyTorch)
- **Data privacy**: Behavioral proxy adaptation eliminates biometric surveillance
- **Financial freedom**: Open dataset enables SME access without lab equipment
- **Practical implementation**: Reproducible protocol on public benchmark