---
name: dp-fedsofim-server-side-fisher-preconditioning
description: Server-side second-order optimization for DP-FL using rank-one Fisher proxy with O(d) complexity and zero additional privacy cost. Use when accelerating DP-FL convergence without increasing privacy budget or client memory, or deploying second-order methods in communication-constrained settings. NOT for non-DP federated learning or when client-side second-order computation is acceptable.
---

# DP-FedSOFIM Server-Side Fisher Preconditioning

## Overview
DP-FedSOFIM moves all curvature estimation to the server, constructing a rank-one Fisher Information Matrix proxy from privatized aggregated gradients via Sherman-Morrison formula. Achieves O(d) client complexity, zero additional privacy cost, and 4-5x convergence speedup over first-order DP-FL baselines.

## When to Use
- Accelerating differentially private federated learning convergence
- When client-side memory must remain O(d) (no O(d²) Hessian computation)
- Needing second-order optimization without additional privacy budget consumption
- Deploying DP-FL in communication-constrained settings (fewer rounds needed)
- NOT for non-DP federated learning (use standard SOFIM instead)
- NOT when client-side second-order computation is acceptable (DP-FedNew for low-dim)

## Core Process / Workflow

### 1. Client-Side (Identical to DP-FedGD)
- Per-example gradient clipping to C_g
- Sum clipped gradients + Gaussian noise: E_i,t ~ N(0, (C_g σ_g)²/n I_d)
- Normalize and send g_i,t = (S_i,t + E_i,t) / |D_i|
- NO additional client computation — O(d) memory

### 2. Server-Side Curvature Proxy
1. **Aggregate**: G_t = (1/n) Σ g_i,t
2. **Momentum buffer**: M_t = β M_{t-1} + (1-β) G_t (EMA of gradients)
3. **Fisher proxy**: Î_t = M_t M_t^T + ρI_d (rank-one + regularization)
4. **Preconditioner** (Sherman-Morrison): H_t = (1/ρ)I_d - M_t M_t^T / (ρ² + ρ||M_t||²)
5. **Update**: θ_{t+1} = θ_t - η_t H_t G_t

### 3. Leverage Key Properties
- **Zero additional privacy cost**: H_t is deterministic function of privatized G_t → post-processing preserves DP
- **O(d) complexity**: Sherman-Morrison avoids O(d³) inversion; only O(d) per round
- **Noise suppression**: EMA reduces DP noise variance by factor (1-β)/(1+β)
  - β=0.9: ~19x variance reduction
  - β=0.95: ~39x variance reduction
- **Anisotropic rescaling**: Stronger contraction along dominant gradient direction

### 4. Handle Tight Privacy Budgets (Warm-Start)
For ε ≤ 0.5 where early gradients are noise-dominated:
- Disable preconditioning for first 20 rounds (EMA-only updates)
- Gradually activate: q_t = (1-λ_t)(1/ρ)G_t + λ_t H_t G_t where λ_t ↑ 1
- Allows momentum buffer to accumulate stable signal before curvature estimation

### 5. Tune Hyperparameters
- **ρ (regularization)**: Higher for tight privacy (more isotropic), lower for relaxed
- **β (EMA momentum)**: Higher for tight privacy (more noise suppression)
- **η (learning rate)**: Grid search per privacy regime
- Warmup rounds for ε ≤ 0.5: ~20 rounds

### 6. Verify Convergence Applicable to Your Setting
- **Strong convexity**: Linear convergence to neighborhood (Theorem 4.21)
- **PL condition**: Extended guarantee (Theorem 4.29)
- **Non-convex smooth**: O(1/T) stationarity (Theorem 4.31)
- Error floor = DP noise + clipping bias + same-step coupling penalty

## References
- See [references/nair-2026-dp-fedsofim.md](references/nair-2026-dp-fedsofim.md) for full paper details.