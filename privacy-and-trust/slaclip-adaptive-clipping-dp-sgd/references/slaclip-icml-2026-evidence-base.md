# SlaClip: ICML 2026 Spotlight — Evidence Base

## Source

**Zou, S., Wang, S., Zhu, Z., Li, J., Changyu, D., Wu, H., & Sassone, V.** (2026). "SlaClip: Gradient Norm Slacks can be Indicator for Adaptive Clipping in DP-SGD." ICML 2026 (Spotlight).

- **Code**: https://github.com/ZsyRock/SlaClip
- **Keywords**: Differential Privacy, DP-SGD, Gradient Clipping, Adaptive Clipping
- **Institutions**: University of Southampton (UK) and Guangzhou University (China)
- **First author**: Shuyan Zou (PhD student, University of Southampton)
- **Corresponding authors**: Han Wu (Assistant Professor, Southampton) and Wang Shaowei (Associate Professor, Guangzhou University)
- **Core team**: Vladimiro Sassone (Professor, Southampton), Zhanxing Zhu (Associate Professor, Southampton), Dong Changyu (Professor, Guangzhou), Li Jin (Professor, Guangzhou)

## Background: DP-SGD and the Clipping Problem

### DP-SGD Fundamentals

DP-SGD (Differentially Private Stochastic Gradient Descent) is the classic method for deep learning with differential privacy, introduced by Abadi et al. (CCS 2016). It protects privacy through three steps per batch:

1. **Per-sample gradient computation**: Compute gradients for each individual sample
2. **Gradient clipping**: Clip each per-sample gradient to a maximum L2 norm C, limiting any single sample's influence
3. **Noise addition**: Add Gaussian noise calibrated to C to the clipped gradients
4. **Aggregation**: Average the noisy clipped gradients to produce the update

The clipping threshold C is critical:
- Noise is scaled to C (larger C = more noise)
- Clipping removes information (smaller C = more information loss)
- The optimal C balances these two forces

### The Fixed Clipping Problem

A fixed C is suboptimal because gradient norm distributions change during training:
- **Early training**: Large gradients dominate; high C is appropriate
- **Late training**: Small gradients dominate; lower C is appropriate
- A fixed C cannot adapt to this shift

### Adap-Clip: The Existing Adaptive Method

Andrew, Thakkar, McMahan & Ramaswamy (NeurIPS 2021) introduced Adap-Clip:
- Tracks the fraction of unclipped gradients in each batch
- Adjusts C toward a fixed target unclipped ratio (e.g., 50%)
- Adopted in mainstream DP tools: Meta's PyTorch Opacus and Google's TensorFlow Privacy

**Two problems with Adap-Clip:**

1. **Extra privacy cost**: Estimating the unclipped fraction typically requires an additional privacy query, consuming privacy budget or requiring stronger noise
2. **Fixed target ratio is not always appropriate**: In late training, small-norm gradients dominate and contribute little to the aggregate update. Mechanically maintaining 50% unclipped drives C down continuously, harming utility

## SlaClip's Innovation

### The Core Observation

The "slack" in the clipping operation — the difference between the gradient norm and the clipping threshold — is not waste information. It is a free signal about the gradient norm distribution.

### The Slack Indicator

After aggregation, Gaussian noise addition, and normalization, the Slack Indicator becomes:
- A **noise-perturbed, binned cumulative distribution function (CDF) estimate** of the gradient norms
- It provides more granular distribution information than a simple clipped/unclipped ratio:
  - Which gradients are near the current threshold
  - Which gradients are concentrated in the small-norm region
  - How the distribution is shifting over training

### Two Key Advantages

1. **Zero extra privacy consumption**: The Slack Indicator is computed from information already produced by the clipping operation. No additional privacy queries are needed. This is SlaClip's defining advantage.

2. **Dynamic target unclipped ratio**: SlaClip uses the CDF near-zero coordinates to estimate the small-gradient ratio. It dynamically adjusts the target unclipped ratio based on the current training stage, preventing the late-training threshold collapse that plagues Adap-Clip.

### How It Works

The Slack Indicator is computed throughout training, providing real-time feedback:

1. Per-sample gradients are clipped to threshold C
2. The clipping operation produces slack values (gradient_norm - C for unclipped, C - gradient_norm for clipped)
3. These slack values are aggregated, noise is added (as part of normal DP-SGD), and normalized
4. The resulting Slack Indicator is a binned CDF estimate
5. The near-threshold coordinates provide feedback similar to Adap-Clip's clipped/unclipped ratio
6. The near-zero coordinates estimate the small-gradient ratio for dynamic target adjustment
7. SlaClip updates C and the target unclipped ratio based on this information
8. Process repeats each batch throughout training

## Experimental Design

### Fair Comparison Protocol

The paper employs a rigorous fair comparison protocol:
- **Matched privacy budget**: All methods compared at the same epsilon
- **Same hyperparameter pool**: Each method, dataset, and privacy budget uses the same hyperparameter pool for grid search
- This ensures fair comparison rather than favoring methods with more tuning

### Results

SlaClip achieves competitive results across multiple datasets and privacy budget settings:
- Frequently achieves the best or second-best DP training accuracy
- Traditional adaptive clipping methods show higher sensitivity to learning rate and initial threshold combinations
- SlaClip's Slack Indicator partially mitigates the instability from initial clipping threshold selection

## SlaClip's Three Defining Properties

1. **No additional privacy queries** — Zero extra privacy consumption
2. **Plug-and-play** — Drop-in replacement for the clipping step; low additional computational overhead
3. **Richer information** — Binned CDF estimate provides more information than single clipped/unclipped statistics

## Relationship to Existing DP Training Ecosystem

| Component | SlaClip Relationship |
|---|---|
| PyTorch Opacus | SlaClip can replace the clipping module; adaptive clipping ideas already in Opacus |
| TensorFlow Privacy | Same — SlaClip can replace the clipping module |
| Adap-Clip | SlaClip supersedes Adap-Clip by providing zero-cost feedback and dynamic targets |
| Fixed clipping | SlaClip improves on fixed clipping by adapting to training stage |
| RDP/zCDP accounting | Compatible — no extra queries means accounting is unchanged |
| Federated learning | Enhances the local DP-SGD step; more budget remains for aggregation |

## A-Tech Privacy-First Stack Integration

SlaClip fits into the A-Tech privacy-first on-device training stack:

```
User Application (A-Coder per-developer adapter, Be Practical per-learner adapter)
  ↓
Federated Aggregation (opt-in, secure aggregation, DP)
  ↓
Local DP-SGD Training
  - SlaClip adaptive clipping ← ENHANCES THIS LAYER
  - LoRA adapter fine-tuning
  - BitNet 1-bit weights
  ↓
Frozen Base Model (on-device)
```

**Benefits:**
- Better adapter accuracy for the same privacy budget
- More privacy budget remaining for federated aggregation
- Improved handling of non-IID data (each client's gradient distribution is different; SlaClip adapts per-client)
- Compatible with the full A-Tech privacy stack (BitNet, FTTE, Gboard FL+DP, federated LLM)

## Related Research

- **Abadi, M. et al. (2016)** — "Deep learning with differential privacy." CCS 2016. Original DP-SGD.
- **Andrew, G. et al. (2021)** — "Differentially private learning with adaptive clipping." NeurIPS 2021. Adap-Clip.
- **McMahan, H.B. et al. (2017)** — FedAvg. Federated learning foundation.
- **Hu, E.J. et al. (2022)** — LoRA. Parameter-efficient fine-tuning for on-device adaptation.

## Open Source

- **Code repository**: https://github.com/ZsyRock/SlaClip
- **License**: Check repository (academic code release)
- **Integration opportunity**: Community contribution to Opacus and TensorFlow Privacy
- **A-Tech Builder's Club project**: Federated SlaClip integration with the privacy-first stack

## Citation

```
Zou, S., Wang, S., Zhu, Z., Li, J., Changyu, D., Wu, H., & Sassone, V. (2026).
"SlaClip: Gradient Norm Slacks can be Indicator for Adaptive Clipping in DP-SGD."
ICML 2026 (Spotlight).
```