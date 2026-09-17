---
name: dataguard-hardware-dp-guarantee
description: "Applies hardware-enforced differential privacy guarantees for ML training on accelerators. Use when designing privacy-preserving ML systems that cannot trust third-party applications, when building federated learning systems with untrusted aggregators, or when implementing hardware-level DP enforcement on TPUs/GPUs."
---

# DataGuard: Hardware-Enforced Differential Privacy

## Overview

DataGuard is the first framework that enforces differential privacy (DP) guarantees in hardware for ML accelerators (TPUs, GPUs). It eliminates the need to trust third-party training applications by guaranteeing that only correctly noised data can leave the device, regardless of what the application does. Based on Sanjaya et al. (AMD/University of Toronto, arXiv:2606.16809, June 2026).

## When to Use This Skill

- Building federated learning systems where the aggregator is untrusted
- Designing ML training on shared accelerators where data owners cannot verify the training application
- Implementing DP guarantees that must hold even against malicious applications
- Evaluating hardware-based privacy enforcement vs software-based approaches
- Designing privacy-preserving ML systems for healthcare, finance, or other sensitive domains

## The Core Problem

Existing DP-enabled federated learning assumes a third-party FL application can be trusted to correctly implement DP algorithms. In reality:

1. The aggregator (who controls the training application) is often an untrusted entity distinct from data owners
2. The training application operates directly on raw sensitive data
3. Applications may violate DP through subtle deviations — malicious design or programmer error
4. Software-based verification is error-prone, requires coordinating across parties, and limits programmability

## DataGuard Solution

Two hardware components enforce DP guarantees:

### 1. Tagging Mechanism
- Lightweight hardware that identifies and tracks correctly noised and clipped gradients
- Only data tagged as "safe" by DataGuard can leave the device
- All computation results automatically marked as sensitive (tag=0) unless produced by the noising module
- Memory tagging unit (MTU) moves tags between on-chip buffers and device memory

### 2. Noising Module
- Hardware module that ensures gradients are correctly noised and clipped
- Application is forced to use this module — only gradients noised by the module are tagged as "safe"
- Configured by privileged Privacy Management Application (PMA) with privacy budget (ε_max, δ_max)
- Accesses protected noise region in device memory

## Custom Instructions

DataGuard adds these instructions to the vector processor ISA:

| Instruction | Variant | Operation |
|-------------|---------|-----------|
| `add-noise` | Both | Add Gaussian noise to operands; compute ℓ2-norm |
| `audit` | Both | Verify clipping condition (ℓ2-norm ≤ Cth); increment epoch counter |
| `load-tagged` | DataGuard | Load data along with tags from memory |
| `vadd` | DataGuard | Vector add with tag propagation |
| `acc-grad` | DataGuardex | Accumulate per-example gradients |
| `load-record` | DataGuardex | Load subsampled training record |

## Two Variants

### DataGuard (Per-Batch Clipping)
- Supports per-batch clipping algorithms
- Simpler implementation, lower overhead
- **Performance**: <0.3% slowdown
- **Area**: <0.01% overhead
- **Power**: <0.07% overhead
- Tag size: 1 byte per 512 bytes of data

### DataGuardex (Per-Example Clipping + Subsampling)
- Supports per-example clipping with random subsampling
- Enables tighter DP bounds via privacy amplification by subsampling
- More complex, slightly higher overhead
- **Performance**: <0.55% slowdown
- **Area**: <0.05% overhead
- **Power**: <0.1% overhead
- Tag size: 4 bytes per 512 bytes of data (includes rid and depoch fields)

## Threat Model

**Adversary**: Untrusted aggregator who controls both:
1. The training application executed on client devices
2. The central server where gradients are aggregated

**Assumptions**:
- Local training infrastructure (hardware + system software) is trusted
- Aggregator has no physical access to client devices
- Communication channels are secured

**Focus**: Local Differential Privacy (LDP) — DP mechanism runs on client devices owned by data owners

## Privacy Guarantees

DataGuard enforces four conditions:
1. **Noising condition**: Data is noised with correct σ (calculated by PMA)
2. **Clipping condition**: ℓ2-norm of noised data ≤ Cth (verified by audit instruction)
3. **Budget condition**: Total privacy cost does not exceed (ε_max, δ_max)
4. **Privacy condition**: Only noised gradients can leave the device

### Formal Proof (Theorem VIII.1)
Any data shared out of the device satisfies (ε,δ)-DP regardless of application behavior. The proof uses:
- Gaussian mechanism with sensitivity bounded by 2Cth (triangle inequality)
- Noise calibrated: σ ≥ 2Cth × √(2log(1.25/δ))/ε
- Post-processing property: final orthonormalization after noise injection maintains DP guarantee

## Four Attack Defenses

### Attack 1: Sharing Unnoised Data
- All computation results marked sensitive (tag=0)
- PMA blocks any data with tag=0 from leaving device

### Attack 2: Bypassing Gradient Clipping
- Noising module calculates ℓ2-norm during `add-noise`
- `audit` checks ℓ2-norm ≤ Cth
- If check fails: CStatus register records the failure epoch
- Data with tag ≥ CStatus cannot leave device
- If `audit` not called: epoch not incremented, tag values not less than current epoch

### Attack 3: Exceeding Privacy Budget
- Epoch counter tracks `audit` invocations
- PMA calculates total privacy cost: (nε, nδ) after n audits
- Data cannot leave if cost exceeds (ε_max, δ_max)

### Attack 4: Bypassing Sampling (DataGuardex)
- depoch field in tags tracks when data was generated
- MTU checks depoch = current epoch when loading from scratchpad
- Data from previous iterations cannot be reused
- CStatus updated on mismatch

## Hardware Implementation

### Evaluated Accelerators
1. **TPU** (Google TPUv3): Weight-stationary dataflow
2. **DIVA**: Outer-product based dataflow
3. **DIVA-PPU**: DIVA with post-processing unit for ℓ2-norm
4. **OS**: Output-stationary dataflow (Shidiannao-based)

### Evaluated Models
VGG16, ResNet50, ResNet152, AlexNet, YOLOv3, GoogleNet, SqueezeNet, MobileNetV2, BERT-base, BERT-large

### Implementation Details
- RTL implementation in Bluespec System Verilog
- Area/power from openroad, CACTI for SRAM
- DataGuard: 32KB SRAM buffers per accelerator
- DataGuardex: 128KB SRAM buffers per accelerator
- Noising module: 0.003 mm² (7nm), 0.12W per accelerator

## Key Design Principles

1. **Preserve programmability**: Application can perform any computation; only data leaving the device must satisfy DP
2. **Minimal hardware changes**: New instructions mirror existing accelerator instructions (add-noise parallels vector-add)
3. **Configurable**: PMA sets privacy budget, clipping threshold, noise distribution
4. **Agnostic to noise generation**: Software sampling, hardware RNG, or verifiable approaches all work
5. **Supports all LDP FL algorithms**: Any algorithm that clips gradients by ℓ2-norm and adds Gaussian noise

## A-Tech Applications

### For A-Coder (Privacy-Preserving Development)
- On-device model fine-tuning with hardware-enforced DP
- Federated learning across development teams without trusting the aggregation service
- Privacy-preserving code analysis models

### For Be Practical (Educational Content)
- Curriculum on hardware-enforced privacy for AI systems
- Tutorials on implementing DP-FL with untrusted aggregators
- Case studies on healthcare/finance ML with DataGuard

### For Builder's Club (Community)
- Open-source DataGuard implementations for community accelerators
- Privacy-preserving federated learning demos
- Hardware verification tools for DP compliance

## Cross-References

- `federated-learning-for-privacy-preserving-ai` — FL fundamentals
- `privacy-first-ai-pipeline-defense` — Software-based DP enforcement
- `dp-lac-lightweight-adaptive-clipping` — Adaptive clipping for DP-FL
- `federated-consent-architecture-agent-systems` — Consent in FL systems
- `privacy-by-design-generative-ai` — Privacy-by-design principles

## Sources

- Sanjaya, Giannoula, Shreekumar, Colbert, Dewulf, Saeedi, Amer, Sines, Vijaykumar. "DataGuard: Guaranteeing Private Training in Systolic-array Based Accelerators." arXiv:2606.16809, June 2026.