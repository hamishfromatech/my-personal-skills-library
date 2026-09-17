# Hybrid EEG-Gaze Decoding Scheme — Evidence Base

## Primary Source

**Kalaganis, F.P., Georgiadis, K., Oikonomou, V.P., Laskaris, N.A., Nikolopoulos, S., Kompatsiaris, I.** (2025). "A hybrid neuromarketing approach exploiting EEG graph signal processing and gaze dynamic patterning." *Brain Informatics*, 12, 23. Open access. DOI: 10.1186/s40708-025-00272-z. Published September 23, 2025.

Funding: European Regional Development Fund / Greek National Funds, project NeuroMkt (T2EDK-03661).

## Dataset: NeuMa

- 42 participants (23 male, 19 female, age 31.5 ± 8.84)
- 6 brochure pages, each with 24 unique supermarket products
- Products grouped by category (dairy, frozen, etc.) to mimic typical supermarket leaflet
- Task: browse freely (keyboard arrows), select products to buy (left mouse click)
- No restriction on cost or number of selected products
- Voluntary participation, written informed consent (CERTH Ethics Committee, ETH.COM-68)

### Recording
- **EEG:** Wearable Sensing DSI 24, 19 electrodes (10-20 system: Fp1, Fp2, Fz, F3, F4, F7, F8, Cz, C3, C4, T7, T8, Pz, P3, P4, P7, P8, O1, O2), A1/A2 mastoid references, 300 Hz
- **Eye-tracking:** Tobii Pro Fusion (screen-based), 120 Hz

### Preprocessing
- EEG: 3rd-order zero-phase Butterworth filter [0.5-45 Hz]; Artifact Subspace Reconstruction (ASR) + FORCe for artifact removal; bandpass to 8-30 Hz for GFT
- Eye-tracking: linear interpolation for missing values (eye blinks)
- Epoching: gaze-defined (time intervals where gaze within product boundaries); discard epochs < 0.5s

## Mathematical Formulation

### Graph Fourier Transform (GFT)

Graph G = {V, E, W} where:
- V = {1, 2, ..., |V|} nodes (EEG electrodes)
- E = edges (pairwise connections)
- W ∈ R₊^{|V|×|V|} weighted adjacency matrix

**Envelope Correlation for W:**
Given concurrent filtered signals x_m(t), x_n(t) for electrodes m,n:
ρ^{m,n} = |ρ(A_m(t), A_n(t))|
where A_m(t), A_n(t) are signal envelopes via Hilbert transform, ρ(.) is Pearson correlation, |.| absolute value.

**Graph Laplacian:**
L = D − W (D = degree matrix)

**Eigenanalysis:**
U = [u_0, u_1, ..., u_{|V|-1}] eigenvectors, λ_k eigenvalues ordered ascending
- Large eigenvalues → swift variations across graph domain
- Small eigenvalues → slow variations

**GFT:**
X̃ = U^T X (project multichannel EEG X to graph Fourier domain)

**Feature extraction (power per eigenmode):**
p(m) = (1/T) Σ_{t=1}^{T} |X̃(m,t)|², m = 1, 2, ..., |V|

### Hjorth Parameters (eye-tracking behavioral features)

For accumulated eye distance signal x(t):
- **Activity** = var(x(t)) — total power, linked to trajectory length
- **Mobility** = sqrt(var(dx/dt) / var(x(t))) — mean frequency, mirrors speed of eye motion
- **Complexity** = mobility(dx/dt) / mobility(x(t)) — bandwidth, captures motion irregularity

### Marketing Eye-Tracking Features (per product)
1. Number of fixations
2. Duration of fixations
3. Time to first fixation
4. Dwell time (total fixation duration on area of interest)
5. Blink rate (stress indicator)

### Classification Pipeline
1. Per epoch (product): extract GFT features (EEG) + Hjorth + marketing features (eye-tracking)
2. Normalize, average across segments (handle varying epoch duration)
3. Concatenate (early fusion)
4. Wilcoxon statistic-based feature selection (train set only)
5. Linear SVM per subject
6. Leave-One-epoch-Out Cross Validation (LOOCV)

## Performance Metrics

### Cohen's Kappa (κ)
κ = (2(TP·TN − FN·FP)) / ((TP+FP)(FP+TN) + (TP+FN)(FN+TN))

Interpretation (Landis & Koch 1977):
- [0, 0.20] — slight
- [0.21, 0.40] — fair
- [0.41, 0.60] — moderate
- [0.61, 0.80] — substantial
- [0.81, 1] — almost perfect

### Results (averaged across subjects)

| Method | Cohen's κ | F1-score | Notes |
|---|---|---|---|
| GFT_hybrid (proposed) | ~0.35 | ~0.54 | Upper fair; statistically significant improvement (p < 0.05, Wilcoxon signed-rank) |
| GFT_EEG (EEG only) | ~0.18 | ~0.31 | Slight; GFT alone insufficient |
| ET_indices (eye-tracking only) | ~0.27 | ~0.48 | Fair; eye-tracking more reliable than EEG_indices |
| Hybrid_indices (naive fusion) | ~0.25 | ~0.43 | Fair; naive fusion of traditional indices |
| EEG_indices (traditional AW/workload/mem) | ~0.05–0.10 | ~0.31 | Near chance; some subjects negative κ |

**Improvement margins:**
- GFT_hybrid vs GFT_EEG: κ +0.17, F1 +0.23
- GFT_hybrid vs ET_indices: κ +0.08, F1 +0.06
- GFT_hybrid vs Hybrid_indices: κ +0.10, F1 +0.11
- GFT_hybrid vs EEG_indices: κ +0.30, F1 +0.23

## Physiological Findings

### Brain Connectivity (Graph Analysis)
- Trial-averaged weighted adjacency matrices (envelope correlation) per subject
- **Consistent coupling:** ⟨Fp1, Fp2⟩ — prefrontal asymmetry, well-documented in decision-making
- **Beyond prefrontal:** Numerous connections from prefrontal area to other regions (occipital, parietal)
- **Subject-specific:** Most prominent connections varied across subjects; only small portion common
- **Coefficient of variation analysis:** Low values (high mean, low variance) indicate systematically strong couplings during "Buy" condition
- **Implication:** Decision-making engages brain areas beyond typical neuromarketing schemes (which focus only on prefrontal asymmetry)

### Eye-Tracking Behavior (Hjorth Descriptors)
- **"Buy" condition:** ~80% of behavioral features in lower-value octant (reduced activity, mobility, complexity)
  → Gaze stabilization on intended product
- **"NoBuy" condition:** Features span full range (no stabilization pattern)
- **Limitation:** Some "NoBuy" samples overlap with "Buy" low-value cluster → limited discriminative power alone

## Why Traditional Indices Fail

Traditional neuromarketing EEG indices (approach-withdrawal, mental workload, memorization):
1. Examine only very specific phenomena and brain patterns
2. May not capture complex neural activity during realistic marketing stimuli
3. Decision-making is multi-factorial — engages brain areas beyond those measured
4. High subject variability means group-level indices lose individual signal
5. Some subjects produced negative κ values (worse than chance) with traditional indices

## Why GFT Works

1. **Functional connectivity:** Captures inter-electrode coupling patterns, not just power at individual sites
2. **Graph structure:** EEG electrode array as graph skeleton, tailored per subject based on functional connectivity
3. **Eigenmode decomposition:** Projects signals to graph Fourier domain, revealing variability across connectivity modes
4. **Subject-specific:** Graph weights are personalized → handles individual variability
5. **Vector output:** Produces feature vectors compatible with early fusion (unlike Riemannian geometry)

## Why Hybrid Fusion Works

1. **Complementary information:** EEG captures internal decision processes; eye-tracking captures external attention behavior
2. **Different failure modes:** EEG has high subject variability; eye-tracking is more stable but has lower discriminative power
3. **Compensation:** Eye-tracking features stabilize where EEG is noisy; GFT features add signal where eye-tracking is ambiguous
4. **Early fusion advantage:** SVM's inherent feature weighting exploits the complementary structure

## Limitations

- 42 participants — larger validation needed
- Supermarket brochure browsing is more realistic than lab ads but still not real-world purchasing
- Online implementation not yet tested (feasible but requires calibration session + data buffer)
- Multi-layer graph (multiple frequency bands) not yet implemented — could enable subject-independent decoding
- Deep learning architectures with GFT concepts not yet explored
- EEG cannot detect deep brain structures (hippocampus, limbic system) that support decision-making — only cortical surface activity

## Future Directions

- **Online implementation:** Low computational complexity; dynamic graph weight updates; data buffer for eye-tracking feature completion
- **Multi-layer graphs:** Multiple frequency bands for wider brain phenomena → potentially subject-independent decoding
- **Deep learning:** If subject consistency achieved, DL with GFT concepts could improve performance
- **Real-time marketing:** Dynamic tailoring of content based on immediate neural + gaze responses

## Cross-References

- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad + AI model layer
- `predictive-neuromarketing-bayesian-framework` — Bayesian Brain / predictive coding framework
- `neuromarketing-predictive-purchase-intent-model` — single-variable predictor hierarchy (fixation duration > frontal alpha > fixation count > frontal beta)
- `neuromarketing-consumer-journey-3x3-framework` — stage-specific framework
- `neuro-marketing-privacy-first-behavioral-analytics` — privacy architecture
- `cognitive-load-reduction-ai-scaffolding` — attention/cognitive load
- `closed-loop-cognition-marketing` — measurement → analysis → optimization → deployment loop