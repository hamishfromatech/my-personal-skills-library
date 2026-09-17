# Evidence Base: HADES Selective Feature Encryption FL

## Source
Kaynak, Bayramoglu, Sav (Bilkent University, Ankara, Turkey). "HADES: Privacy-Preserving Federated Learning via Selective Feature Encryption and Hybrid Model Fusion." arXiv:2606.22928v1, 2026.

## System and Threat Model

### System Model
- N parties collaboratively train a neural network without sharing local datasets
- Final model hosted by one party or external entity (cloud provider)
- All parties safeguard data privacy at every stage: communication, training, prediction
- Potential attackers: individual party, server, or collusion of multiple parties

### Threat Model
- Honest-but-curious setting with K parties
- Possible collusions involving up to K-1 parties
- Parties adhere to protocol but attempt to infer information from others' data
- Primary goal: prevent reconstruction and inference attacks during FL training
- Prediction-output attacks out of scope (focus on training phase)

## PCA Feature Selection for Encryption

### Method
- Input: feature set F = {f_1, ..., f_d}
- PCA finds principal components describing most variance
- Top-i principal components selected as F_HE (encrypted subset)
- Remaining features = F_P (plaintext subset)
- Selection of i determined by: variance explained, ciphertext capacity, desired utility

### Privacy Rationale
- Many raw features exhibit high correlations exploitable by attackers
- Encrypting principal directions preserves most privacy-sensitive signals
- Sharply reduces plaintext surface available to attacker
- Even if decryption possible (prevented by MHE), adversary needs PCA transformation parameters
- PCA method is illustrative; HADES is agnostic to feature-selection technique

### Ciphertext Capacity Constraint
- Ring size N limits data per ciphertext: |F_HE| ≤ 2^{N-1} / B
- For batch size B on single-layer network with single output neuron
- Layer sizes and operations must ensure tensors remain representable

### Federated PCA Options
1. Each party applies PCA locally → Private Set Intersection for consensus
2. Privacy-preserving multi-party PCA (PPPCA, SF-PCA)

## Multiparty Homomorphic Encryption (MHE)

### CKKS Scheme
- Cyclotomic ring R = Z[X]/(X^N + 1), N = power of 2
- Each ciphertext encodes vector of complex numbers z ∈ C^{N/2}
- N/2 slots for encoding values
- Used: N = 2^13, 2^12 slots, 64-bit precision, L = 6 levels

### Key Operations
- **Key Generation**: Parties collaboratively generate individual secret keys → public encryption/evaluation keys
- **Collective Bootstrapping**: Refreshes ciphertext to enable further computations when level depleted
- **Encryption**: Using public key
- **Evaluation**: Computations over ciphertexts using evaluation key
- **Decryption**: Requires cooperation of all/threshold parties

## Fusion Mechanism

### Score-Level Fusion
Fused encrypted logit:
```
z̄_HE = α·z_HE + (1-α)·HE(z_P)
```

Plaintext logit:
```
z̄_P = z_P
```

### Loss Functions
Residual objective: ℓ(ẑ, y) := ẑ - y

```
L_HE = ℓ(z̄_HE, y_i) = z̄_HE(x_i) - HE(y_i)
L_P  = ℓ(z̄_P, y_i)  = z̄_P(x_i) - y_i
```

- Both losses retained locally, never shared with server
- Encrypted parameters never decrypted
- Only unencrypted components are intentionally unencrypted model parameters

### Fusion Weight
- α ∈ [0,1] controls relative contribution (default: 0.5)
- Intermediate fusion possible: concatenate/add hidden representations
- α tunable based on domain (e.g., if private features more important for performance)

## Training Algorithm (Algorithm 2)

```
1. Initialization:
   - Parties agree on secure PCA transformation
   - Each client applies PCA: split data into X_HE and X_P
   - Initialize local models W_HE^k and W_P^k

2. Training Loop (for each iteration t):
   a. Local Training (each client k):
      - Forward: z_P through W_P^k → plaintext logits
      - Forward: z_HE through W_HE^k → encrypted logits
      - Fused: z̄_HE = α·z_HE + (1-α)·HE(z_P)
      - Compute L_HE and L_P
      - Calculate gradients ∇W_HE and ∇W_P
      - Send [∇W_HE, ∇W_P] to server
   
   b. Aggregation (server):
      - ΔW_HE = Σ ∇W_HE^k
      - ΔW_P = Σ ∇W_P^k
      - Send back to clients
   
   c. Model Update (each client):
      - W_HE^{t+1} = W_HE^t - η·ΔW_HE
      - W_P^{t+1} = W_P^t - η·ΔW_P
```

## Experimental Setup

### Datasets
| Dataset | Samples | Features | Classes |
|---------|---------|----------|---------|
| Breast Cancer Wisconsin (Diagnostic) | 569 | 30 | 2 |
| MNIST | 70,000 | 784 (28×28) | 10 |
| SVHN | 99,289 | 3,072 (32×32×3) | 10 |

### Configuration
- 70%-30% train-test split (SVHN: predefined 73.78%-26.21%)
- 10 epochs, SGD, local batch size B=1 (maximizes iDLG attack success for rigorous evaluation)
- K=10 clients
- Fusion weight α=0.5
- CKKS: N=2^13, 2^12 slots, 64-bit precision, L=6 levels
- OpenFHE-python library

### Attack Evaluation
- Improved Deep Leakage from Gradients (iDLG)
- Implemented from scratch using NumPy and L-BFGS from SciPy
- Metrics: RMSE, PSNR, SSIM, LPIPS
- B=1 chosen to maximize attack success (most challenging defense condition)

## Q1: Reconstruction Attack Mitigation

### Variance Explained by PCA

| Feature Count | BCD Var% | MNIST Var% | SVHN Var% |
|--------------|----------|------------|-----------|
| 1 | 98.14 | 9.76 | 57.91 |
| 2 | 99.79 | 16.92 | 63.62 |
| 4 | 99.99 | 28.48 | 72.96 |
| 8 | 100.00 | 43.87 | 79.95 |
| 16 | 100.00 | 59.58 | 86.41 |
| 32 | - | 74.49 | 91.60 |
| 64 | - | 86.28 | 96.00 |
| 128 | - | 93.68 | 98.45 |
| 256 | - | 97.94 | 99.47 |
| 512 | - | 99.94 | 99.86 |

### Reconstruction Quality Under iDLG Attack

| Dataset | Metric | |F_HE|=0 | |F_HE|=16 | |F_HE|=64 | |F_HE|=256 | |F_HE|=1024 | All-1 |
|---------|--------|---------|----------|----------|-----------|------------|--------|
| BCD | RMSE | 0.00 | 92.99 | – | – | – | 98.86 |
| BCD | PSNR | ∞ | 13.51 | – | – | – | 8.60 |
| BCD | SSIM | 1.00 | 0.86 | – | – | – | 0.65 |
| MNIST | RMSE | 0.00 | 0.22 | 0.29 | 0.42 | – | 0.63 |
| MNIST | PSNR | ∞ | 13.48 | 11.09 | 8.92 | – | 5.97 |
| MNIST | SSIM | 1.00 | 0.48 | 0.26 | 0.11 | – | 0.04 |
| MNIST | LPIPS | 0.00 | 0.17 | 0.29 | 0.49 | – | 0.64 |
| SVHN | RMSE | 0.00 | 0.17 | 0.20 | 0.26 | 0.39 | 0.62 |
| SVHN | PSNR | ∞ | 16.21 | 14.29 | 11.96 | 8.97 | 7.31 |
| SVHN | SSIM | 1.00 | 0.53 | 0.22 | 0.04 | 0.01 | 0.01 |
| SVHN | LPIPS | 0.00 | 0.16 | 0.26 | 0.53 | 0.83 | 0.87 |

### Key Observations
- |F_HE|=0 (no encryption): perfect reconstruction (RMSE=0, SSIM=1)
- Increasing |F_HE| steadily degrades reconstruction
- MNIST: 16→256 encrypted features: SSIM 0.48→0.11, LPIPS 0.17→0.49
- SVHN: 16→256: SSIM 0.53→0.04, LPIPS 0.16→0.53
- Moderate encryption budget approaches worst case without encrypting all features
- SVHN at 256: SSIM 0.04 vs 0.01 at all-1 — diminishing returns

## Q2: Model Utility Preservation

| Dataset | CE-F_HE=0 | CE-F_HE | FL-F_HE | FL-F_HE approx (HADES) |
|---------|-----------|---------|---------|------------------------|
| BCD | 94.71 | 95.88 | 96.49 | **97.08** |
| MNIST | 95.00 | 94.80 | 94.82 | **94.99** |
| SVHN | 63.1 | 66.2 | 67.3 | **70.4** |

- |F_HE| values: BCD=16, MNIST=256, SVHN=256
- Reductions in encrypted parameters: 2x (BCD), 4x (MNIST), 16x (SVHN)
- Sigmoid approximation closely matches exact activations
- Fusion network introduces no meaningful degradation
- SVHN: 63.1% → 70.4% — hybrid design recovers signal in high-dimensional space

## Q3: HADES vs PCA-Only Baseline

- Training solely on PCA-selected subspace leads to clear accuracy drop (MNIST, SVHN)
- Selecting only "most important" components insufficient; discards task-relevant info
- HADES recovers utility through score-level fusion
- Plaintext branch preserves fine-grained patterns; encrypted branch protects sensitive components
- Gains stem from fused dual-network design, not PCA alone

## Q4: Scalability

### Training Time vs |F_HE| (synthesized dataset, 32 features)
- Direct correlation: as |F_HE| decreases, training time decreases
- Up to 28% improvement by reducing encrypted feature dimension
- Even deep networks maintain ~7% decrease in worst case
- Training time scales approximately linearly with |F_HE|
- Scales consistently with increasing number of clients (improvements from local training steps)

### Hidden Layer Impact
- Wider hidden layers: higher absolute cost
- Incremental penalty of doubling F_HE remains comparable
- "No Hidden Layer" baseline substantially faster but same upward trajectory
- Ciphertext packing, not hidden-layer computation, dominates at large F_HE

### Operation Counts (per n-layer network, per client)
| | Rotation | MultPT | MultCT |
|---|----------|--------|--------|
| Forward | 2n-1 | 2n | n |
| Backward | 2n-1 | 2n+1 | 2n+1 |
| Total | 4n-2 | 4n+1 | 3n+1 |

## Q5: Runtime Performance

### Timing Analysis (selected configurations)

| Dataset | Batch | |F_HE| | GI-Load | One-GI (s) | Total (s) |
|---------|-------|-------|---------|----------|-----------|
| BCD | 1 | 30 | 16X | 0.519 | 202.5 |
| BCD | 16 | 2 | X | 0.511 | 12.5 |
| MNIST | 1 | 256 | 16X | 0.313 | 15,337.5 |
| MNIST | 4 | 64 | 4X | 0.294 | 3,602.8 |
| SVHN | 1 | 256 | 16X | 0.305 | 22,332.2 |
| SVHN | 4 | 64 | 4X | 0.277 | 5,071.9 |

- Increasing B: slight One-GI cost (additional rotation) but amortized by reduced GI-Load
- For fixed GI-Load, One-GI primarily influenced by |F_HE|
- Larger batch sizes within single ciphertext: reduces memory, CPU requirements

## Packing Strategy Details

### Alternating Packing
- Odd-numbered layers: column-major packing
- Even-numbered layers: row-major packing
- Enables perfect alignment for matrix multiplication

### Padding
- Matrices padded to power-of-2 dimensions
- Multiplication area μ = m × k × ñ (padded dimensions)
- Padding prevents data overwriting during ciphertext rotations
- Aligns rows/columns for efficient processing

### Dynamic Packing for Deep Networks
- Dimension slots vector d = [d_0, d_1, ..., d_L]
- Multiplication area vector μ[i] = (N/2) / (d[i] × d[i+1])
- Alternates packing based on layer index
- Forward pass: Algorithm 3
- Backward pass: Algorithm 4

### Single-Ciphertext Mini-Batching
- Entire mini-batch encoded in single ciphertext
- Contrasts with POSEIDON/Hercules (parallelize over examples or process sequentially)
- Reduces memory usage and CPU core requirements
- Two additional rotation calls needed but enables larger effective batch sizes

## Comparison to Prior Work

### Secure Aggregation
- Bonawitz et al.: server computes sum without learning individual updates
- BatchCrypt: Paillier encryption on projected, quantized gradients
- Park & Lim: distributed HE with partial decryption
- BlindFL: segmented FHE selectively
- **Limitation**: protects only from server; client-side model remains vulnerable

### Fully Encrypted FL
- POSEIDON: first MHE-based FL for neural networks; hours to days training time
- Hercules: improves on POSEIDON complexity
- **Limitation**: encrypts ALL parameters; high computational overhead

### HADES Difference
- First to support selective encryption within end-to-end encrypted FL
- Encrypts only privacy-sensitive data, leaves non-sensitive in plaintext
- Maintains strong privacy guarantees against client-side attacks
- Significantly reduces computational overhead
- Complementary to existing encrypted FL methods (not replacement)

## Connection to Existing Privacy Skills

- **MOSAIC-FL** (existing): Microservice FL with threshold CKKS — HADES offers selective encryption as lighter alternative
- **AdaDP-FedSec** (existing): Adaptive DP + secure aggregation — HADES offers selective HE without DP noise
- **HADES Selective Feature Encryption** (this skill): Hybrid encrypted-plaintext FL — novel middle ground
- **FLiPD** (existing): MPC + DP for majority-collusion resistance — HADES focuses on reconstruction attack prevention
- **FedFG** (existing): Flow-matching generative FL — different privacy mechanism (generative vs selective encryption)

## A-Tech Alignment

- **Open-source AI**: Built on OpenFHE (open-source); reproducible with standard libraries; supports open-weight models in FL
- **Data privacy**: Selective encryption provides strong privacy with minimal overhead; PCA-based feature selection protects most sensitive data; MHE prevents any single party from decrypting
- **Financial freedom**: 2-16x reduction in encrypted parameters reduces compute cost; makes privacy-preserving FL accessible to smaller organizations; runtime improvements enable practical deployment
- **Practical implementation**: Concrete algorithm with pseudocode; experimental validation across 3 datasets; packing strategy detailed; operation counts provided; configurable privacy-utility-tradeoff via |F_HE| and α