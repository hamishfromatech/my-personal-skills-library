---
name: fedmop-momentum-privacy-performance
description: Reconcile privacy and performance in federated learning through momentum-based trajectory hiding and gradient orthogonal projection. Use when designing FL systems that need both strong privacy guarantees against gradient leakage attacks AND superior model accuracy/convergence, without the traditional privacy-performance trade-off.
---

# FedMOP: Momentum-Based Privacy-Performance Reconciliation in FL

## Overview

FedMOP (Federated Learning with Momentum-Based Orthogonal Projection) simultaneously achieves strong privacy guarantees and superior model performance by leveraging initialization-based offset mechanisms on orthogonal dimensions. Gradient orthogonal projection counteracts local drift (performance), while momentum-based trajectory hiding makes the offset vector inherently unrecoverable (privacy). These mechanisms are synergistic rather than antagonistic. FedMOP provides 5-10x stronger privacy protection, 2-4% accuracy gains, and 1.5-2x faster convergence over existing defenses.

## When to Use

- Designing FL systems that need both privacy AND performance (not the traditional trade-off)
- Defending against gradient leakage attacks (csDLG, FGLA) without sacrificing accuracy
- Improving convergence speed on non-IID data while preserving privacy
- Building FL systems where privacy noise and Byzantine filtering must coexist without destructive interference
- NOT for: centralized learning, scenarios without gradient leakage risk, or when DP noise is acceptable regardless of accuracy cost

## Core Process / Workflow

### 1. Understand the Privacy-Performance Antagonism

Traditional FL defenses face a fundamental trade-off:
- **Differential privacy**: substantial noise significantly reduces accuracy on heterogeneous data
- **Gradient compression**: loses critical components needed for convergence
- **Secure aggregation**: prohibitive cryptographic costs
- **Existing acceleration methods**: focus on performance without addressing privacy

FedMOP bridges this gap by controlling each client's starting point for local training.

### 2. Implement Orthogonal Projection Offset (Performance)

**Consecutive participation:**
```
Ω_nat_t,i = (x_t − x_{t−1}) − Π_{t−1,i} · (x_{t−1,i} − x_{t−1})
```
where `Π_{t−1,i} = ⟨x_t − x_{t−1}, x_{t−1,i} − x_{t−1}⟩ / ‖x_{t−1,i} − x_{t−1}‖²`

- Projects global update onto orthogonal complement of local update direction
- Incorporates global statistical information without interfering with local gradient descent
- Orthogonality enables aggressive scaling for faster convergence

**Irregular participation:**
- Use averaged updates: `Ω_nat_t,i = (x_t − x_{ri})/(t−ri) − Π_{ri,i} · (x_{ri,i} − x_{ri})`

### 3. Implement Momentum-Based Trajectory Hiding (Privacy)

**The vulnerability:** Naive offsets are server-computable from observed models, enabling 1D brute-force search (~100 values).

**The solution — momentum evolution:**
```
Ω_t,i = γ_{t,i} · Ω_{t−1,i} + (1 − γ_{t,i}) · Ω_nat_t,i
```
where:
- `Ω_0,i ~ N(0, σ²_0 I_d)` — private initialization, never transmitted
- `γ_{t,i} ~ clip(N(γ̄, σ²_γ), 0.8, 0.95)` — sampled independently each round

**Recursive expansion:**
```
Ω_t,i = Π_{k=1}^t γ_{k,i} · Ω_0,i + Σ_{j=1}^t [Π_{k=j+1}^t γ_{k,i}] · (1−γ_{j,i}) · Ω_nat_{j,i}
```

**Why this works:**
- Reconstructing Ω_t,i requires inferring (d+t) unknowns: initial vector Ω_0,i ∈ R^d plus all historical coefficients {γ_1,i,...,γ_t,i}
- For modern networks d ≈ 10^7, creating O(N^{d+t}) search space
- Computationally infeasible inverse problem

### 4. Apply the Shift and Train Locally

```
β_{t,i} ~ clip(N(β̄, σ²_β), 0.2, 0.95)  # Random scaling
x̃_{t,i,0} = x_t + β_{t,i} · Ω_t,i     # Shifted initialization
# Then standard SGD for K steps
```

### 5. Verify Convergence Preservation

**Convergence rate:**
```
min_t E[‖∇f(x_t)‖²] = O(1/(K²T²)) + O(σ²_0 γ̄^{2T}) + O(σ²_γ T)
```
- O(1/(K²T²)) improves upon vanilla FedAvg's O(1/(KT))
- Momentum terms vanish asymptotically when σ_0, σ_γ are small
- Momentum provides exponentially weighted averaging (variance reduction)

## Performance Results

### Privacy Defense (MSE, lower = better for privacy)

| Scenario | Method | MSE (csDLG) | MSE (FGLA) |
|----------|--------|-------------|------------|
| Epochs=1, B=1 | DP | 0.48 | 0.41 |
| | Soteria | 0.54 | 0.38 |
| | OUTPOST | 0.19 | 0.11 |
| | **FedMOP** | **0.85** | **0.98** |

### Model Accuracy (CIFAR-100, full participation)

| Method | D1 (α=0.3) | D2 (α=0.05) |
|--------|------------|-------------|
| FedAvg | 31.61 | 30.35 |
| HierFed | 39.23 | 38.92 |
| **FedMOP** | **42.07** | **41.78** |

### Convergence Speed (rounds to 30% accuracy, CIFAR-100)

| Method | D1 Full | D2 Full | D2 40% |
|--------|---------|---------|--------|
| FedAvg | 312 | 124 | 102 |
| HierFed | 68 | 38 | 26 |
| **FedMOP** | **67** | **27** | **26** |

## References

- See [references/evidence-base.md](references/evidence-base.md) for full experimental results, ablation studies, and hyperparameter sensitivity.