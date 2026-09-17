# Evidence Base: Adaptive DP-FL for Concept Drift in Edge Environments

## Primary Source

**Sudhakar, K., Jayasree, A., Sundaragiri, D., Talluri, U., Bhusarapu, H. N., & Sreenivas, T. S. (2026).** FedDriftGuard: adaptive federated learning with differential privacy for concept drift in edge environments. *Scientific Reports*, 16, 20326. https://doi.org/10.1038/s41598-026-51535-6

## Framework Architecture

### Three Integrated Components

1. **Client-side drift detection**
   - DDM (Drift Detection Method): monitors classification error rate
   - ADWIN (Adaptive Windowing): sliding window comparing distribution averages
   - Computes quantitative drift intensity d_t ∈ [0, d_max]

2. **Drift-aware aggregation**
   - Adaptive weight: γ_k = 1 + λ · d_k
   - Aggregation: w^(t+1) = Σ (γ_k · n_k / Σ γ_j · n_j) · w_k^t
   - Clients with higher drift contribute more strongly

3. **Drift-optimal privacy scheduler**
   - Noise variance: σ_t² = σ_max - (σ_max - σ_min) · (d_t / d_max)
   - Privacy budget constraint: Σ ε_t ≤ ε_total
   - Moments Accountant for cumulative privacy tracking
   - Higher drift → lower noise (prioritize adaptation)
   - Stable periods → higher noise (strengthen privacy)

### DP-DriftNet Model Architecture

- **Feature encoder**: MLP (tabular/sensor) or CNN (image)
- **Temporal modeling**: Bidirectional LSTM (Bi-LSTM)
  - Forward: h→t
  - Backward: h←t
  - Combined: H_t = [h→t, h←t]
- **Attention mechanism**: Additive attention
  - Scores: e_t = v^T tanh(W·H_t + b)
  - Weights: α_t = exp(e_t) / Σ exp(e_j)
  - Context: c = Σ α_t · H_t
- **Output**: softmax(W_c · c + b_c)
- **Differential privacy**: Gaussian noise N(0, σ_t²) added to gradients

### Communication Optimization

- **Update sparsification**: Top-k gradient components by magnitude
- **Compression**: Quantization of transmitted values
- **Periodic transmission**: E local epochs before communication

## Experimental Results

### Datasets
| Dataset | Domain | Drift Type |
|---------|--------|------------|
| Electricity Market | Energy | Natural (seasonal + abrupt) |
| Airline Delay | Transportation | Seasonal demand |
| TON_IoT | IoT Security | Attack intensity |
| FEMNIST | Image Recognition | Natural non-IID |
| Synthetic | Controlled | Sudden, gradual, recurring |

### Performance Comparison

| Metric | FedAvg | DP-FedAvg | Adaptive FL | FedDriftGuard |
|--------|--------|-----------|-------------|---------------|
| Accuracy | Baseline | - | Moderate gain | +9–14% |
| F1-score | Baseline | - | Moderate gain | +11–17% |
| Adaptation latency | 18-20 rounds | 18-20 rounds | 12 rounds | 7 rounds |
| Communication cost | High | High | Moderate | -20–35% |

### Drift Type Performance

| Drift Type | FedDriftGuard Recovery |
|------------|----------------------|
| Sudden | Fast (7 rounds), minimal accuracy drop |
| Gradual | Stable, smooth adaptation |
| Recurring | Re-acquires previous patterns |

### Dynamic vs. Fixed DP (Same Privacy Budget)

| Metric | Fixed DP | Dynamic DP (FedDriftGuard) |
|--------|----------|---------------------------|
| Accuracy | Lower | Higher |
| F1-score | Lower | Higher |
| Adaptation latency | Higher | Lower |
| Post-drift recovery | Slower | Faster |

### Privacy Budget Sensitivity

| ε Range | Performance |
|---------|-------------|
| ε = 1.0–3.0 (moderate) | Close to non-DP setting |
| ε ≤ 0.5 (extreme) | Gradual degradation, still competitive |
| σ² ≤ 0.5 | Moderate utility degradation |
| σ² > 1.2 | Significant utility loss |

### Communication Efficiency

| Method | Communication Rounds | Update Size |
|--------|---------------------|------------|
| FedAvg | >120 | >210 KB/round |
| DP-FedAvg | >120 | >210 KB/round |
| Adaptive FL | ~100 | ~150 KB/round |
| FedDriftGuard | ~80 | ~95 KB/round |

### Computational Overhead

| Method | Per-client overhead |
|--------|-------------------|
| FedAvg | Low |
| DP-FedAvg | Low |
| Adaptive FL | High |
| FedDriftGuard | Moderate (~65 ms) |

## Ablation Study Results

| Component Removed | Impact |
|-------------------|--------|
| Drift detection | Largest degradation (accuracy, F1, latency) |
| Drift-aware aggregation | Highest RMSE, largest latency increase |
| Attention layer | Significant accuracy/F1 drop |
| DP noise injection | Slight accuracy gain, but removes privacy guarantees |

### Component Contribution Ranking
1. Drift detection (most critical)
2. Drift-aware aggregation
3. Attention-based temporal encoding
4. Dynamic differential privacy

## Robustness Under Heterogeneous Conditions

| Condition | FedDriftGuard Performance |
|-----------|--------------------------|
| 100% client availability | Stable, high accuracy |
| 40% client availability | Graceful degradation |
| Severe non-IID (high skew) | Stable adaptation latency |
| Intermittent connectivity | Robust via drift-aware weighting |

## Statistical Significance

- Paired t-tests and Wilcoxon signed-rank tests
- All p-values < 0.05 (95% confidence)
- 95% confidence intervals do not span zero
- 20 repeated runs per configuration
- Effect sizes > 0.8 (large practical improvement)

## Comparison with Drift-Adaptive Baselines

| Method | Drift | Privacy | Edge Efficiency | Communication |
|--------|-------|---------|-----------------|---------------|
| CDA-FedAvg | Yes | No | No | Standard |
| FAC-fed | Yes (fairness) | No | No | Standard |
| DSDP | No | Yes (adaptive) | No | Standard |
| FedACM | Partial | No | Partial | Standard |
| FedDriftGuard | Yes | Yes (drift-adaptive) | Yes | Optimized |

## Limitations

1. Evaluation on controlled public/synthetic datasets; real-world edge deployments may have more complex drift
2. No adversarial threat models (gradient inversion, adaptive attackers)
3. Communication efficiency tested in simulation, not real hardware
4. Bi-LSTM computational overhead; lighter alternatives not yet evaluated
5. Formal privacy bounds under adaptive sensitivity remain open

## Future Work (from authors)

1. Real-world edge deployment on Raspberry Pi and NVIDIA Jetson
2. Stronger adversarial threat models
3. TCN and efficient Transformer alternatives to Bi-LSTM
4. Self-supervised drift forecasting
5. Multimodal federated drift learning
6. Cross-silo extensions

## A-Tech Alignment

- **Open-source AI**: Reproducible, public datasets, open evaluation
- **Data privacy**: Formal (ε,δ)-DP maintained; drift-adaptive noise preserves guarantees
- **Financial freedom**: 20-35% communication cost reduction; enables resource-constrained deployment
- **Practical implementation**: 5 datasets, statistical significance, ablation studies, robustness analysis