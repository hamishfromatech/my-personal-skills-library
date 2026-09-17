# SSTQ — Full Evidence Base

## Citation & Status

Javanmard, A. (USC), Mirrokni, V. (Google Research), Woodruff, D.P. (CMU). "SSTQ: Privacy-Preserving Vector Quantization via Subsampled Stochastic Quantization." arXiv:2608.05127 (posted Aug 5, 2026).

**Status caveats**:
- Preprint, not peer-reviewed; "optimal" MSE-scaling claim rests on authors' own proofs.
- Empirical evaluation: CIFAR-10 and Fashion-MNIST only (clean, balanced, 10-class image sets). No text, tabular, or non-IID unbalanced tests.
- **No public source code** (related repo Privacy-PORCUPINE covers a different method; last updated Oct 2024). Watch authors' GitHub for a code drop — the adoption signal.
- Credibility anchor: TurboQuant (the base method SSTQ builds on) was formally verified in an April 2025 preprint (machine-checked proofs closed gaps in the original) — the family has verified mathematical foundations even though SSTQ itself doesn't yet.

## Technical Mechanism

1. **Overcomplete equal-norm tight frame**: gradient projected onto N coordinates, N > d. A frame generalizes a basis — spread signal across redundant coordinates for robustness to noise and quantization.
2. **Subsampling**: each client sends a subset of the N coordinates — the direct bandwidth lever.
3. **Privacy-aware codebook**: quantization levels designed so the quantization step itself injects the privacy noise; no separate noise-adding step on top.
4. Variants:
   - **Flat Randomized Response**: probability spread uniformly across codebook levels.
   - **Metric-Aware Laplace**: noise shaped to codebook geometry; better suited to higher bit-width regimes.

## Quantitative Results

- Codebook-dependent MSE: **O(4^b) → O(2^b)**:
  - b=4: proportional to 256 → 16 (16× reduction)
  - b=8: 65,536 → 256 (256× reduction)
- Communication: **⌈log₂ N⌉ + b bits per client**, N = Θ(d). For d = 1,000,000 parameters: ~20 + b bits per client per round — the full compressed, privacy-protected gradient in a few bytes.
- Why it matters in practice: quantization/privacy error compounds across thousands of FL rounds; a scaling improvement multiplies through the entire training run.

## The Problem Context

- FL gradient updates leak raw data (gradient-inversion / Deep-Leakage-from-Gradients line of work).
- Local DP noise requirement grows with per-update information; compression error stacks on top in conventional pipelines (quantize-then-noise). vqSGD (geometric constructions) suffers variance growing with dimension.
- SSTQ claim: privacy + compression as one object, lower total error than separate steps → better accuracy at same privacy, or same accuracy at tighter budget.

## Deployment Decision Test

For a running federated system (e.g., 50-site hospital network):
1. Identify current pipeline: quantize-then-DP? What baselines are in your stack?
2. Check whether your baseline appears in SSTQ's comparison; if not, the "favorable utility" claim doesn't transfer automatically — run your own A/B.
3. Domain check: images proven; text/tabular/non-IID unproven — pilot before fleet rollout.
4. Adoption gate: wait for the code drop; independent replication of O(2^b) on non-image data is the real signal.

## Position in the FL Privacy Stack (library cross-reference)

| Need | Method | Library skill |
|---|---|---|
| Coordinator can't inspect updates | SecAgg+ / Shamir+Paillier / SMPC | adadp-fedsec, clifti-gpt |
| Formal (ε,δ) output bound | DP-SGD + accounting | dptrainer, adadp-fedsec |
| Bandwidth-constrained edge + privacy | **SSTQ (one-step co-design)** | this skill |
| Encrypt most-sensitive features only | MHE/CKKS selective | HADES |
| Drop-in DP engineering | DPTrainer runtime patching | dptrainer |
| Byzantine-resilient async edge FL | DAG ledger co-design | DAG-AF2L (in library) |
| Cross-silo LLM fine-tuning production | FedLoRA/FedOpt/SecAgg blueprints | (LLMs.blog production patterns, Aug 2026 — incremental to existing FL skills) |

## Companion Context (Aug 2026 privacy landscape, incremental notes)

- **FedGSA** (arXiv:2608.03267): geometry-consistent Grassmann-manifold subspace aggregation for DP federated LoRA; fixes basis-dependence of factor-wise averaging; +2.17%/+2.27% over strongest baseline at ε=6/ε=3; no additional privacy loss (server-side ops are post-processing). Complements FFA-LoRA/FedSVD/LA-LoRA/AS-LoRA line already in library.
- **EFedProx** (Scientific Reports, Aug 5, 2026): multilevel DP + blockchain verification for lung-cancer FL; adaptive proximal term for client drift; 4–9% accuracy gains on LUNA16/IQ-OTH-NCCD; IPFS decentralized storage. Domain-specific incremental.
- **Federated LLM fine-tuning production guide** (LLMs.blog, Aug 23, 2026): FedLoRA payload math (99.8–99.9% WAN reduction: Llama-3-8B 16GB → 28.8MB adapter), FedProx/SCAFFOLD/FedOpt comparison, SecAgg+ masking, six-phase deployment blueprint, Flower vs NVIDIA FLARE vs OpenFedLLM framework comparison. Strong practical checklist; incremental to existing federated skills.

## A-Tech Applications

- Edge-AI product design: any on-device learning feature (keyboards, wearables, automotive) where uplink and privacy budget are both scarce.
- Fleet economics: 256× error reduction at 8 bits ≈ tighter ε for the same accuracy ≈ compliance headroom without hardware upgrades.
- Open-source watch item: code drop → benchmark → adopt pattern; pre-code, treat numbers as theoretical ceiling.
