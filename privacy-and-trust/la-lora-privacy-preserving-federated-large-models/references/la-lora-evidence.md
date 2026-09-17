# LA-LoRA Evidence Base

## Source
Liu, Miao, Xi, Liu. "Rethinking LoRA for Privacy-Preserving Federated Learning in Large Models." ICLR 2026.

## Three Challenges Identified

1. **Gradient coupling**: LoRA introduces two trainable low-rank matrices A and B (W = W_0 + BA). Simultaneous updates create gradient interactions between asymmetric matrices. Under DP noise, these coupled gradients become unstable.

2. **Compounded noise amplification**: DP-SGD adds noise to both A and B gradients. When both are perturbed simultaneously, noise compounds multiplicatively through the BA product, not additively.

3. **Global model sharpness**: When server aggregates LoRA parameters from heterogeneous clients, the combined parameter space creates sharp loss landscapes. Sharp minima generalize poorly and are sensitive to the noise introduced by DP.

## LA-LoRA Solution

**Local Alternating**: Instead of updating both A and B at each step, alternate:
- Even steps: update A, freeze B
- Odd steps: update B, freeze A

This:
- Eliminates gradient coupling (only one matrix's gradient is active per step)
- Halves per-step noise (only one matrix perturbed)
- Aligns update directions across clients (each client alternates in sync)

## Theoretical Contributions

- Convergence guarantee strengthened under noisy federated environment
- Update direction alignment reduces global model sharpness
- Formal analysis shows the alternating scheme maintains the expressivity of LoRA while reducing the effective noise variance

## Experimental Results

### Swin-B on Tiny-ImageNet (ε = 1)
- LA-LoRA: SOTA accuracy
- RoLoRA (best baseline): 16.83% lower accuracy
- Improvement: +16.83 percentage points

### RoBERTa on text tasks
- LA-LoRA achieves SOTA under strict DP budgets
- Demonstrates broad applicability across both LVMs and LLMs

### Key Insight
The alternating mechanism is especially impactful for **vision models** (LVMs) where LoRA-DP degradation was previously most severe. Language models showed smaller but still significant gains.

## Comparison with RoLoRA

RoLoRA (the prior SOTA) addresses some LoRA-DP issues but doesn't decouple gradients. LA-LoRA's alternating mechanism is a fundamentally different approach:
- RoLoRA: modifies the aggregation or noise application
- LA-LoRA: modifies the local training schedule (when each matrix is updated)

## Practical Implications

1. For ε ≤ 1: LA-LoRA provides large gains (16.83% on Swin-B)
2. The approach is architecture-agnostic (works for Transformers, Swin, RoBERTa)
3. Minimal implementation overhead — just alternating requires_grad flags
4. Compatible with existing DP-FL frameworks (DP-SGD, FedAvg)