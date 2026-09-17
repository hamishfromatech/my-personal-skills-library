---
name: flipd-majority-collusion-resistant-secure-aggregation
description: Optimized secure aggregation protocol for federated learning combining Multi-Party Computation (MPC) and Differential Privacy (DP) with distributed noise generation. Secure even when majority of clients collude with the server. Use when designing FL secure aggregation with majority-collusion threat model, needing both inference attack and backdoor attack defense simultaneously, or optimizing client-server communication cost to match unprotected FL.
---

# FLiPD: Majority-Collusion-Resistant Secure Aggregation

## Overview
FLiPD is an optimized secure aggregation (SA) protocol for federated learning that protects against both inference attacks and backdoor attacks via a combination of Multi-Party Computation (MPC) and Differential Privacy (DP). Its key innovation: distributed DP noise generation makes the protocol secure even when the majority of clients collude with the server — while maintaining client-server communication cost essentially identical to unprotected FL.

## When to Use
- Designing FL secure aggregation under majority-collusion threat models
- Needing simultaneous defense against inference AND backdoor attacks
- Optimizing communication cost for secure aggregation (matching unprotected FL)
- Building MPC+DP hybrid FL protocols with distributed noise generation
- Evaluating Prio+ vs FLiPD communication efficiency

## NOT For
- Centralized training privacy
- Pure DP-FL without secure aggregation (different threat model)
- Environments without at least 2 non-colluding servers

## Core Process / Workflow

### 1. Threat Model — Majority Client Collusion

Most SA protocols assume honest-but-curious server with no client collusion. FLiPD goes further:

| Threat | Standard SA | FLiPD |
|---|---|---|
| Inference attacks (gradient inversion) | Defended via SA | Defended via SA + DP |
| Backdoor/poisoning attacks | Requires separate filtering | Defended via DP noise |
| Server-only collusion | ✓ | ✓ |
| Client-server collusion (minority) | ✓ | ✓ |
| **Client-server collusion (majority)** | ✗ | **✓** (distributed DP) |

### 2. Combined Defenses

| Attack Type | Defense Mechanism |
|---|---|
| **Inference attacks** | Secure aggregation (MPC) — server cannot inspect individual updates |
| **Backdoor attacks** | DP noise (distributed generation) — perturbs gradients to prevent poison injection |

### 3. Distributed DP Noise Generation

Key innovation: DP noise is generated **distributedly** across clients, not centrally:
- Each client contributes noise shares
- Noise is added within the MPC computation
- Even if majority of clients + server collude, they cannot remove the DP noise
- This is what enables majority-collusion resistance

### 4. Communication Efficiency

| Metric | Unprotected FL | FLiPD | Prio+ (SOTA) |
|---|---|---|---|
| Client-server cost | Baseline | **Same as unprotected** | Higher |
| Server-server cost | N/A | **11% lower** than Prio+ | Baseline |

FLiPD achieves client-server communication cost essentially the same as plaintext FL — a significant practical advantage for bandwidth-constrained deployments.

### 5. Accuracy Results

| Model | Dataset | Accuracy |
|---|---|---|
| Linear Regression | HAR | 87% |
| CNN | MNIST | 90% |

### 6. Protocol Architecture

```
Client → [Encrypt + Add DP noise share] → MPC Servers → [Secure Aggregation] → Global Update
                     ↑                                                        ↓
                     └──────────── Distributed DP noise generation ────────────┘
```

Components:
- **MPC layer**: Secure aggregation preventing server from inspecting individual updates
- **DP layer**: Distributed noise generation providing formal privacy guarantees
- **Integration**: DP noise added within MPC computation so collusion cannot strip it

### 7. Comparison with Existing Solutions

| Feature | Standard FL | DP-FL | Standard SA | Prio+ | FLiPD |
|---|---|---|---|---|---|
| Inference defense | ✗ | Partial | ✓ | ✓ | ✓ |
| Backdoor defense | ✗ | ✓ | ✗ | ✗ | ✓ |
| Majority collusion | ✗ | ✗ | ✗ | ✗ | ✓ |
| Comm. = unprotected | N/A | ✗ | ✗ | ✗ | ✓ |

## References
- See [references/flipd-evidence.md](references/flipd-evidence.md) for cryptographic protocol details and comparison with Prio+.