# DataGuard Evidence Base: Technical Details, RTL Implementation, Security Proofs, Performance Benchmarks

## Introduction

This reference document provides the full technical details of DataGuard and DataGuardex, drawn from the arXiv paper (2606.16809, published June 15, 2026) by Sanjaya, Giannoula, Shreekumar, Colbert, Dewulf, Saeedi, Amer, Sines, and Vijaykumar (University of Toronto and AMD). It covers the RTL implementation results, formal security proofs, and detailed performance benchmarks that support the claims in the SKILL.md.

## Part 1: Differential Privacy Background

### Formal Definition

A function M: X → Y is (ε, δ)-differentially private if for any two adjacent datasets D and D' (differing by one record) and for all K ∈ Y:

```
P(M(D) = K) ≤ e^ε · P(M(D') = K) + δ
```

Where:
- ε controls the privacy loss (smaller = stronger privacy)
- δ is the probability of catastrophic leakage (typically δ ≪ 1/n for n records)

### Gaussian Mechanism

For a function F with ℓ2-sensitivity S (max change in output when one record changes), the Gaussian mechanism adds noise:

```
M(x) = F(x) + N(0, σ²),  where σ = √(2·S²·ln(1.25/δ))/ε
```

### DP in Federated Learning

DP-SGD modifies SGD by:
1. Computing gradients for a batch
2. Clipping gradients to bound ℓ2-norm ≤ Cth
3. Adding Gaussian noise with σ = Cth·√(2·ln(1.25/δ))/ε
4. Updating model weights with noised gradients

**Per-batch clipping:** Gradients for the entire batch are summed, then clipped to ‖g‖₂ ≤ Cth. Sensitivity = 2·Cth.

**Per-example clipping:** Each per-example gradient is clipped to ‖gi‖₂ ≤ Cth, then summed. Noise added to the sum. Sensitivity = Cth.

**Privacy amplification by subsampling:** With sampling probability q < 1, T rounds cost (q√T·ε, δ) instead of (T·ε, T·δ). This is tighter and allows more training for the same budget.

### Composition

If round i satisfies (εi, δi)-DP, then T rounds satisfy (Σεi, Σδi)-DP by composition. The privacy budget (εmax, δmax) is the upper bound; training stops when the cumulative cost reaches it.

## Part 2: Detailed Hardware Design

### Noising Module

The noising module is implemented in the vector processor (VPU) execute stage. It contains three sub-units:

#### Noise Addition Unit
- Adds a floating-point operand and a noise term
- Noise values are stored in a protected memory region on device memory, populated by the PMA via software noise sampling on the host CPU
- Protected region configured via `noise_br` configuration register
- On-chip buffers partitioned: 1/4 used for sampled noise values (6 MB of 24 MB total)
- First invocation triggers a 6 MB batched data transfer from device memory to on-chip buffers
- Uses pipelined HardFloat-based implementation to avoid impacting critical path
- Requires twice as many FMA operations as a plain FP addition

#### ℓ2-Norm Calculation Unit
- Maintains cumulative sum of squares (S) of operands to `add-noise`
- 128 registers (Pi) store partial sums, one per VPU lane
- 128 fused multiply-add units, one per lane
- Two special registers:
  - `CStatus`: Status register, written with epoch value if clipping check fails (can only be cleared by PMA)
  - `Cth`: Configuration register for clipping threshold (set by PMA before execution)
- On `audit`: partial sums are aggregated to Pagg, checked Pagg ≤ Cth, partial sums cleared
- If check fails and CStatus = 0: CStatus is set to current epoch value

#### Budgeting Unit
- 8-bit counter (`epoch_b`), incremented on each `audit` execution
- Reset to 1 by PMA before application execution
- Used by PMA to compute total privacy cost

### Tagging Module

#### Tag Storage
- Data divided into 512-byte blocks (128 elements × 4 bytes for single-precision FP)
- Each block has 1 byte of metadata (DataGuard) or 2 bytes (DataGuardex)
- Tags stored in protected tag region of device memory
- Additional SRAM tag buffers added to on-chip buffers (no port contention with application data — implemented as additional bank with independent port)

#### DataGuard Tag Values
- Tag = 0: Sensitive (default for all computation results except `add-noise`)
- Tag = epoch value: Noised (set by `add-noise` instruction)
- PMA checks: tag value must be < CStatus (if CStatus ≠ 0) for data to be shared

#### DataGuardex Tag Fields (2 bytes)
```
| s (1 bit) | rid (1 byte) | depoch (7 bits) |
```
- `s`: Whether data is noised
- `rid`: Record identifier (0 = multiple records used; -1 = acc-grad result)
- `depoch`: Round in which data was generated

#### Tagging Module Operation
1. Systolic array results: always marked sensitive (tag = 0)
2. VPU results: handled by tagging module based on opcode
   - `add-noise`: tag = epoch (noised)
   - Any other instruction: tag = 0 (sensitive)
3. Data fetched from device memory: tagged 0 in on-chip buffers
4. Data written back to memory: on-chip tags update device memory tags
5. Control flow instructions: tags of operands checked; if tag = 0, CStatus updated (control flow dependent on sensitive data not allowed)
6. `vadd` on noised operands: if all source operands have non-zero tags, result tagged with higher tag value (aggregation of noised data allowed)
7. `load-tagged`: fetches tags along with data for aggregation operations

#### DataGuardex Tagging Modifications
1. `s`: Set to 0 if any operand has s=0 (except `add-noise` which always sets s=1)
2. `rid`: Set to -1 for `acc-grad`; otherwise, if all s=0 operands have same rid x, set to x; else 0
3. `depoch`: Set to current epoch value
4. Additional checks:
   - `add-noise` operands must have rid = -1 (ensures noising accumulated gradients, not raw data)
   - `acc-grad` operands must have rid ≠ 0 (ensures operating on single-record gradients)
   - If checks fail and CStatus = 0: CStatus set to epoch

### Memory Tagging Unit (MTU)

#### DataGuard MTU
- Moves tags between on-chip buffers and device memory alongside data
- Configuration register `tag_br` for tag region base address
- Calculates addresses for tag storage/retrieval

#### DataGuardex MTU (additional)
- Checks `depoch` field of any data fetched from scratchpad region
- If depoch ≠ current epoch and CStatus = 0: CStatus updated to epoch
- Prevents reuse of intermediate results from previous iterations

### DataGuardex Device Memory Layout

Device memory divided into 5 segments:
1. **Model region:** Model weights and noised gradients (initialized with s=1 by PMA)
2. **Records region:** Sensitive training data (accessed only via `load-record`)
3. **Scratchpad region:** Intermediate results (depoch checked by MTU on fetch)
4. **Secure scratch region:** Noise samples and record-to-index mappings (protected, not accessible by application)
5. **Tags region:** Tag metadata (protected)

### DataGuardex `load-record` Instruction

Operands: index (idx), offset (off), target (tgt), size (bsize)
1. Maps idx to a record index tid via the secure scratch mapping (generated by PMA)
2. Computes base address: off + (tid × rec_sz)
3. Loads bsize bytes from main memory to on-chip buffers at tgt
4. Results tagged with the record's rid

### DataGuardex Noising Unit Modifications

- `acc-grad`: Calculates ℓ2-norm for gradients per record (based on tagged rid)
- Partial sums per record stored in secure scratch region
- On `audit`: partial sums fetched, per-record ℓ2-norms computed, each checked ≤ Cth
- If any check fails and CStatus = 0: CStatus set to epoch

## Part 3: RTL Implementation Results

### Implementation Details
- **Language:** Bluespec System Verilog
- **Synthesis tools:** openroad (area and power)
- **SRAM modeling:** CACTI
- **Technology nodes:** 7nm (ASAP7) for noising unit logic, 22nm for SRAM modeling
- **Target frequency:** 1 GHz

### Area Overhead (TPUv3 baseline, ~650mm² per chip, two accelerators)

| Component | DataGuard | DataGuardex |
|-----------|-----------|-------------|
| Noising unit (per accelerator) | 0.003 mm² | 0.003 mm² |
| Noising unit chip-wide | 0.0008% | 0.0008% |
| Tag SRAM (per accelerator) | 32 KB | 128 KB |
| Tag SRAM chip-wide area | 0.01% | 0.05% |
| **Total chip-wide area** | **0.01%** | **0.05%** |

### Power Overhead (TPUv3, 450W TDP, 1 GHz)

| Component | DataGuard | DataGuardex |
|-----------|-----------|-------------|
| Noising module (per accelerator) | 0.12 W | 0.12 W |
| Static power (SRAM, per accelerator) | 0.02 W | 0.04 W |
| Dynamic power (SRAM, per accelerator) | 0.0105 W | 0.1024 W |
| **Total power overhead (chip-wide)** | **0.07%** | **0.10%** |

### Memory Overhead

| Component | DataGuard | DataGuardex |
|-----------|-----------|-------------|
| Tag storage (TPUv3, 32GB) | 64 MB | 256 MB |
| Percentage of total memory | 0.2% | 0.8% |

### Energy Overhead (estimated from memory traffic)

| Accelerator | DataGuard | DataGuardex |
|-------------|-----------|-------------|
| OS (output-stationary) | 0.08% | 0.61% |
| TPU, DIVA, DIVA-PPU | 0.12% | 0.80% |

(Estimated using HBMv2 energy access of 3.6 pJ/bit)

## Part 4: Security Proofs

### Theorem VIII.1 (DataGuard)

**Statement:** Given that data shared out of the device for DataGuard is a result of M: D → ℝⁿ, then it satisfies (ε, δ)-DP when M is a standard Gaussian mechanism such that:

```
M(x) = f(x) + N(0, 4·Cth²·z²·In),  where z = √(2·ln(1.25/δ))/ε
```

**Proof:**

By the Gaussian mechanism (Dwork et al., 2006), M is differentially private if max‖f(D) − f(D')‖₂ = 2·Cth for two adjacent datasets (D, D') ∈ D.

The `noise_l2` operation in DataGuard ensures ‖f(D)‖₂ ≤ Cth.

By the triangle inequality:

```
‖f(D) − f(D')‖₂ ≤ ‖f(D)‖₂ + ‖f(D')‖₂ ≤ Cth + Cth ≤ 2·Cth
```

Thus, M satisfies (ε, δ)-DP. ∎

**Extension notes:** Proofs for DataGuardex and for privacy budgeting can be constructed analogously. DataGuardex uses Cth sensitivity (per-example clipping bounds each example to Cth), and the subsampling amplification bound (q√T·ε) follows from standard privacy amplification by subsampling theorems.

### Security Against Four Attack Scenarios

#### Attack 1: Sharing Unnoised Data
- **Threat:** Application transmits raw training data or unnoised gradients
- **Defense:** Tagging module marks all computation results as sensitive (tag = 0). PMA will not allow tag = 0 data to leave device.
- **Guarantee:** Only data processed through `add-noise` can be tagged non-zero.

#### Attack 2: Bypassing Gradient Clipping
- **Threat:** Application noises via `add-noise` but skips clipping
- **Defense:** Noising module automatically computes ℓ2-norm during `add-noise`. `audit` checks norm against Cth. If unclipped, CStatus updated.
- **Variant:** Application skips `audit` entirely. Epoch not incremented. Tag value = current epoch, not < current epoch. PMA detects violation.
- **Guarantee:** Clipping condition enforced regardless of application behavior.

#### Attack 3: Exceeding Privacy Budget
- **Threat:** Application runs more iterations than allowed
- **Defense:** Epoch counter tracks `audit` invocations. PMA computes total cost (n·ε for n audits). If cost > budget, PMA terminates application.
- **Guarantee:** Budget cannot be exceeded.

#### Attack 4 (DataGuardex): Bypassing Subsampling
- **Threat:** Application reuses intermediate results from previous iterations
- **Defense:** MTU checks `depoch` field on fetch. If depoch ≠ current epoch, CStatus updated. PMA terminates.
- **Guarantee:** Subsampling enforcement maintained.

### Additional Security Properties

**Noising non-gradient data:** If application applies DataGuard instructions to raw training data (not gradients), the resulting noised data still satisfies DP. Sensitivity is bounded by Cth, noise is sampled per Gaussian mechanism. No privacy violation.

**Control flow on sensitive data:** Tagging module checks tags of control flow instruction operands. If tag = 0 (sensitive), CStatus is updated. This prevents the application from making control flow decisions based on raw sensitive data that could leak information.

**Budget allocation:** PMA can be integrated with privacy management systems (Cohere, Privacy Budget Scheduling) for budgeting across multiple applications/mechanisms, including potential collusion. Composition theorems apply when combining different DP techniques.

## Part 5: Performance Benchmarks

### Evaluation Setup

- **Simulator:** Custom cycle-accurate, based on SCALE-sim + Ramulator v2.0
- **Validation:** Against Google Cloud TPUv3 (Pearson correlation > 0.92 for GEMM, vector addition, noising)
- **Accelerators:**
  - TPU: 128×128 weight-stationary systolic array
  - DIVA: 128×128 outer-product dataflow
  - DIVA-PPU: DIVA + post-processing unit with hardware adder trees for ℓ2-norm
  - OS: 128×128 output-stationary (Shidiannao-based), same SRAM params as TPU
- **Common config:** 1 GHz, 24 MB on-chip SRAM, two accelerators per chip, HBMv2 (450 GB/s per accelerator)
- **Batch size:** 8
- **DataGuard:** 1 training round = 10 iterations
- **DataGuardex:** 1 training round = 1 iteration

### Slowdown Results (DataGuard)

| Model | TPU | DIVA | DIVA-PPU | OS |
|-------|-----|------|----------|-----|
| AlexNet | 0.06% | 0.17% | 0.18% | 0.11% |
| GoogleNet | 0.07% | 0.18% | 0.18% | 0.11% |
| MobileNetV2 | 0.05% | 0.20% | 0.18% | 0.13% |
| ResNet152 | 0.09% | 0.20% | 0.18% | 0.14% |
| ResNet50 | 0.09% | 0.21% | 0.18% | 0.13% |
| SqueezeNet | 0.10% | 0.20% | 0.18% | 0.13% |
| VGG16 | 0.09% | 0.20% | 0.18% | 0.13% |
| YOLOv3 | 0.09% | 0.21% | 0.19% | 0.13% |
| BERT-base | 0.09% | 0.20% | 0.18% | 0.13% |
| BERT-large | 0.09% | 0.20% | 0.18% | 0.13% |
| **Average** | **0.09%** | **0.20%** | **0.18%** | **0.13%** |

### Slowdown Results (DataGuardex)

| Model | TPU | DIVA | DIVA-PPU | OS |
|-------|-----|------|----------|-----|
| AlexNet | 0.31% | 0.50% | 0.55% | 0.37% |
| GoogleNet | 0.31% | 0.50% | 0.55% | 0.37% |
| MobileNetV2 | 0.31% | 0.50% | 0.55% | 0.37% |
| ResNet152 | 0.31% | 0.50% | 0.55% | 0.37% |
| ResNet50 | 0.31% | 0.50% | 0.55% | 0.37% |
| SqueezeNet | 0.31% | 0.50% | 0.55% | 0.37% |
| VGG16 | 0.31% | 0.50% | 0.55% | 0.37% |
| YOLOv3 | 0.31% | 0.50% | 0.55% | 0.37% |
| BERT-base | 0.31% | 0.50% | 0.55% | 0.37% |
| BERT-large | 0.31% | 0.50% | 0.55% | 0.37% |
| **Average** | **0.31%** | **0.50%** | **0.55%** | **0.37%** |

### Additional Memory Traffic

| Accelerator | DataGuard | DataGuardex |
|-------------|-----------|-------------|
| OS | ~0.08% | ~0.61% |
| TPU, DIVA, DIVA-PPU | ~0.12% | ~0.77-0.89% |

### Sensitivity Studies

#### Tag Size Impact (DataGuardex on DIVA-PPU)

| Tag Size (bytes per 512 bytes data) | Slowdown |
|-------------------------------------|----------|
| 1 | 0.31% |
| 2 | 0.42% |
| 4 | 0.55% |
| 8 | 0.79% |
| 16 | 1.18% |
| 32 | 1.91% |
| 64 | 3.06% |
| 128 | 63.23% |

(Slowdown increases with tag size due to additional memory accesses. 4 bytes is the maximum needed for DataGuardex.)

#### Batch Size Impact (DIVA-PPU)

| Batch Size | DataGuard Slowdown | DataGuardex Slowdown |
|------------|--------------------|-----------------------|
| 8 | 0.18% | 0.55% |
| 16 | 0.15% | 0.55% |
| 32 | 0.13% | 0.55% |

(DataGuard slowdown decreases with batch size because compute-bound kernel ratio increases. DataGuardex remains constant because per-example operations scale with batch size.)

### Performance Breakdown Analysis

Five key observations from the evaluation:

1. **TPU has lowest slowdown** because additional memory requests overlap with compute-bound (ComB) kernel execution. TPU spends more time on ComB kernels (95% for MobileNetV2) vs DIVA (80%).

2. **OS has higher slowdown than TPU** despite lower additional memory traffic (0.08% vs 0.12%) because OS has fewer ComB stalls — less opportunity to hide memory latency.

3. **DIVA-PPU vs DIVA (DataGuard):** DIVA-PPU has slightly lower slowdown (0.18% vs 0.20%) because the PPU accelerates ℓ2-norm computation, reducing MemB kernel time and allowing better overlap.

4. **DIVA-PPU vs DIVA (DataGuardex):** DIVA-PPU has slightly higher slowdown (0.55% vs 0.50%) because additional memory traffic is higher (0.89% vs 0.77%).

5. **MobileNet and SqueezeNet on DIVA** show highest DataGuardex slowdowns (2.08% and 2.31%) because small matrix sizes are efficiently executed on the systolic array, making additional memory requests more impactful.

## Part 6: ISA Additions Summary

| Instruction | Applies To | Operation |
|------------|-----------|-----------|
| `add-noise` | DataGuard, DataGuardex | Add Gaussian noise to operands |
| `audit` | DataGuard, DataGuardex | Verify clipping condition |
| `load-tagged` | DataGuard | Load data from memory along with tags |
| `vadd` | DataGuard | Vector add (with tag propagation) |
| `load-record` | DataGuardex | Load a subsampled training record |
| `acc-grad` | DataGuardex | Add two per-example gradients |

All new opcodes extend the vector-processor ISA and require only modest datapath modifications (dedicated noising and tagging modules). Other changes are confined to metadata handling: small SRAMs for tags and an MTU that moves tag bytes along with normal data transfers.