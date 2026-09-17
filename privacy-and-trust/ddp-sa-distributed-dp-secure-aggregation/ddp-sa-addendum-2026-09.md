# DDP-SA Addendum (September 2026): DDP-SA-Adaptive

## What's new (Wei, Jammine & Nait-Abdesselam, Université Paris Cité — arXiv:2608.15153v1, Adaptive clipping successor to arXiv:2604.07125)

The DDP-SA pipeline (Laplace-noised client gradients + full-threshold additive secret sharing over a multi-server architecture) originally used a **static clipping threshold** ∆ for the whole gradient vector, every round. DDP-SA-adaptive replaces it with **round-wise, layer-wise adaptive clipping**:

```
∆_t,i^(l) = median( ||grad_t,i,l(x)||_1 : x ∈ D_i )     # per client, per round, per layer
noise = Lap( 0, ∆_t,i^(l) / ε_t^(l) )                   # noise scale follows the threshold
```

Everything else — fixed-precision encoding, additive secret sharing, PS reconstruction — is unchanged. Privacy analysis (post-processing invariance, basic + advanced composition across rounds) carries over unchanged.

## Reported Results (federated linear regression, PyTorch 1.4.0 + PySyft 0.2.9, GitHub Codespaces)

| Metric | No-private | Static DDP-SA | DDP-SA-adaptive |
|---|---|---|---|
| Communication rounds to convergence | 2082 | 2436 | **2270** (−6.81%) |
| Total training time | 112 min | 203 min | **164 min** (−19.21%) |
| Per-round time | 6.46 s | 10.00 s | **8.67 s** (−13.33%) |
| Test loss | 1e−12 | 0.0055 | **6.94e−5** (−98.74%) |
| Test R² | 0.9999 | 0.9666 | **0.9996** (+3.41%) |
| ε needed for R² = 0.99 | — | ε ≈ 0.4 | **ε ≈ 0.1** (4× stronger privacy at equal utility) |

Why it works: threshold = median of per-sample gradient norms → threshold *and* noise scale both shrink as gradient norms contract over training → late-round updates are much less distorted → faster convergence, higher final accuracy, all without touching the secure-aggregation protocol.

## Design-relevant takeaways

1. **Adaptivity is cheap and stack-compatible.** The mechanism changes only the clipping step; encoding + ASS + aggregation workflow are untouched. If you already run DDP-SA, upgrading is a local client-side patch, not a protocol migration.
2. **Round-wise + layer-wise beats both global-static and global-adaptive** — per-layer norms differ by orders of magnitude across a network, so a single adaptive scalar still leaves accuracy on the table.
3. **The privacy win is not a tradeoff.** The adaptive mechanism simultaneously gets *stronger* ε at equal utility, *lower* compute time, and *fewer* communication rounds — the rare triple improvement. Useful rebuttal to "DP forces a privacy/utility/speed tradeoff" arguments in FL deployments.
4. **Caveats for adoption decisions:** evaluation is limited to a synthetic-ish linear regression / shallow 2-layer model, IID data partition, Laplace (pure-DP) mechanism rather than Gaussian (ε,δ)-DP; no non-IID / deep-network evidence yet in this note. Treat as a mechanism-level proof, not a production benchmark. The authors' own DDP-SA paper (already captured in this skill) covers the architecture itself.

## Relation to adjacent skills in this library

- The adaptive-threshold idea at the server level is shared with `adaptive-verifiable-federated-learning-2026` (adaptive clipping from historical update-statistics + constraint-aware aggregation) and `dp-lac-lightweight-adaptive-clipping`. Where those works adapt clipping on the server side from *update* norms, DDP-SA-adaptive adapts on the client side from *per-sample gradient* medians, preserving the secret-sharing pipeline.
- Same research group's original DDP-SA framework is the architecture parent (see `references/wei-2026-ddp-sa.md`).
- Contrast with `fulcrum-topology-aware-dp`: Fulcrum varies privacy *across clients* (topology-aware MI bound); DDP-SA-adaptive varies *across rounds and layers* for a single client. Complementary axes — a deployment could combine them (per-client bounds ∘ per-layer clipping).

## Suggested SKILL.md patch

Append to the end of the "When to Use" or References section:

```
- **2026-09 update:** the mechanism has been extended to round-wise, layer-wise median-based
  adaptive clipping ("DDP-SA-adaptive", arXiv:2608.15153v1) — 4× stronger ε at equal utility
  (ε≈0.1 vs 0.4 at R²=0.99), −19% training time, −7% communication rounds; client-side
  local change only, no protocol migration. See ddp-sa-addendum-2026-09.md for details,
  reported numbers, and adoption caveats (regression benchmark only, IID data, Laplace
  mechanism, no non-IID evidence).
```