# DPTrainer Evidence Base

## Source: JetBrains Research Blog (August 27, 2026)

**Title:** "Differential Privacy for Hugging Face Trainers – Without Rewriting Your Training Loop"
**Authors:** Katie Fraser, Mihajlo Linic
**Published:** August 27, 2026
**URL:** https://blog.jetbrains.com/research/2026/08/dptrainer/

## The Gap DPTrainer Closes

### The DP Math Is Solved
Differential privacy (DP) provides a mathematical framework that protects individual data points used for training. The core guarantee: a model trained with DP behaves almost identically whether or not any single example was included in the training set. For LLMs, which memorize training data and can reproduce it in response to adversarial prompting, this is the strongest known defense against leakage.

### The Engineering Gap
- **Opacus** is the go-to library for DP-SGD in PyTorch: per-sample gradient computation, DPOptimizer, privacy accountants, Poisson-sampled data loaders
- **Hugging Face Trainer** and TRL's alignment trainers (SFTTrainer, POTrainer) are the top high-level training APIs for transformers: distributed training, checkpointing, evaluation, callbacks
- **Zero DP awareness** in HF Trainer — wiring Opacus requires touching model wrapping, optimizer creation, data loading, loss computation, checkpointing, and callback management
- These interact in subtle ways, and getting any one wrong can break the privacy guarantee **silently**

## DPTrainer Architecture

### Core Component: DPTrainer
- Extends `transformers.Trainer`
- Incorporates privacy budget via `PrivacyArguments` dataclass
- Every standard training argument, callback, checkpoint, and evaluation workflow works unchanged

### Core Component: privatize_trainer
- Patches any Trainer-based class at runtime
- Injects DPTrainer into the inheritance chain without touching the class's own logic
- Works with DPOTrainer (preference learning), SFTTrainer (instruction tuning), Seq2SeqTrainer (generation)
- The patched trainer keeps all original behavior while gaining DP-SGD

## What DPTrainer Automatically Manages

1. **Noise addition** — Adds calibrated Gaussian noise to aggregated gradients via DPOptimizer
2. **Gradient clipping** — Clips each sample's gradient individually before aggregation (not batch gradient); supports flat, adaptive (AdaClip), and per-layer strategies
3. **Gradient computation** — Wraps model in Opacus's GradSampleModule for per-sample gradients (required for DP-SGD correctness)
4. **Optimizer creation** — Intercepts create_optimizer to wrap HF-created optimizer with DPOptimizer
5. **Data loading** — Overrides get_train_dataloader to return DPDataLoader with Poisson sub-sampling (enables privacy amplification by sampling)
6. **Noise calibration** — Given target_epsilon and training configuration, computes correct noise_multiplier automatically (no manual binary search)
7. **Privacy accounting** — DPCallback hooks into optimizer step, tracks running privacy budget after every update
8. **Checkpointing** — Saves and restores accountant state alongside model weights (budget tracking remains correct after resume)
9. **Early stopping** — Privacy-budget-aware stopping halts training when entire budget is exhausted

## PrivacyArguments Configuration Options

| Parameter | Options | Description |
|-----------|---------|-------------|
| target_epsilon / noise_multiplier | Set one (mutually exclusive) | Privacy budget or fixed noise level |
| clipping | "flat" (standard), "adaptive" (AdaClip), "per_layer" | Clipping strategy |
| poisson_sampling | bool | Toggle Poisson sub-sampling for privacy amplification |
| grad_sample_mode | "hooks" (default) | Per-sample gradient computation mechanism |
| accountant | RDP (default) | Privacy accountant type |
| epsilon_log_mode | steps, eval, both, none | When to log budget expenditure |

## Code Examples

### Basic Usage
```python
from dptrainer import DPTrainer, PrivacyArguments

privacy_args = PrivacyArguments(
    target_epsilon=8.0,
    per_sample_max_grad_norm=1.0,
)

trainer = DPTrainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    privacy_args=privacy_args,
    data_collator=data_collator,
)
trainer.train()
```

### Privatizing TRL Trainers
```python
from trl import DPOTrainer
from dptrainer import PrivacyArguments, privatize_trainer

privatize_trainer(DPOTrainer)  # one line

trainer = DPOTrainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    processing_class=tokenizer,
    privacy_args=PrivacyArguments(target_epsilon=8.0, per_sample_max_grad_norm=1.0),
)
trainer.train()
```

## Key Insight: Engineering as the Adoption Barrier

The DP math is mature. Opacus provides production-grade DP-SGD. The barrier to adoption is that integrating DP into existing training infrastructure (especially Hugging Face's Trainer ecosystem, which most teams use) requires touching 6+ subsystems that interact subtly. Getting any wrong silently breaks the privacy guarantee.

DPTrainer eliminates this engineering barrier with a drop-in replacement. This validates a broader pattern in privacy-preserving AI: the cryptographic/DP algorithms are solved; the integration engineering is the bottleneck.

## Context Within JetBrains Research Privacy Program

JetBrains Research states they are "deeply concerned about user privacy and continually developing new methods and tools to improve privacy protection." DPTrainer is part of a broader research program that includes:
- Membership inference attack research and mitigations
- Privacy-preserving AI training methods
- Data collection policy improvements for AI-powered IDE features

The motivation: "By guaranteeing the privacy of our training method, we can exploit previously unavailable channels and use data generated every day through our IDEs." This enables training on real developer code (high quality, high quantity) while maintaining formal privacy guarantees.

## Relationship to Existing A-Tech Skills

- `privacy-preserving-ai-attribution-framework` — DP as Step 2 of the 5-step attribution stack
- `differential-privacy-synthetic-data` — DP for synthetic data generation pipelines
- `sheld-fl-self-learning-heterogeneous-dp-framework` — Adaptive DP budget allocation in federated learning
- `slaclip-adaptive-clipping-dp-sgd` — Adaptive clipping (DPTrainer supports AdaClip via the `clipping="adaptive"` option)
- `dp-lac-lightweight-adaptive-clipping` — Zero-hyperparameter adaptive clipping for DP-FL
- `google-gboard-private-fl-dp` — Production DP at Google scale (ε<1 achievable in production)
- `fedgsa-grassmann-manifold-dp-federated-lora` — Geometry-consistent DP aggregation

## Cross-References

7 cross-references to existing privacy and trust skills.