---
name: fulcrum-topology-aware-dp
description: Applies Fulcrum (Rangwala, Sinnott & Buyya, University of Melbourne, 2026) — topology-aware differential privacy allocation for federated learning that gives every client the same worst-case privacy bound while improving on uniform DP-SGD whenever network leverage is non-uniform. Use when designing privacy-preserving federated systems over heterogeneous network topologies (hierarchical, decentralized, cross-silo), defending against topology-exploiting membership-inference attacks, or deciding whether to adopt topology-aware DP.
---

# Fulcrum: Topology-Aware Differential Privacy for Federated Learning

## Overview

Fulcrum (Rangwala, Sinnott & Buyya, University of Melbourne, arXiv preprint 2026, GPL-3.0, research codebase) introduces a structural blind spot in federated learning privacy: DP-SGD treats all clients identically, but clients differ in *structural leverage* — how much an adversary can learn about them from network topology. The paper pairs a passive attack (TADI — Topology-Aware Distributional Inference) that quantifies per-client leakage of sensitive-class concentration with a defence: **per-client asymmetric DP-SGD noise allocation** σ*²ᵢ = a/(K★ − ℓ°ᵢ), where ℓ°ᵢ is the client's uncontrollable prior-coupling (structural leverage) term and K★ is a uniform worst-case mutual-information bound. The allocation degenerates to uniform DP-SGD on symmetric topologies — so it can be adopted unconditionally — and strictly improves on uniform whenever leverage is non-uniform: privacy-bound gaps up to 0.871 nats on Erdős–Rényi (p=0.5) and 1.193 nats on Barabási–Albert (m=2) graphs, saturating the analytic asymptote, with TOST equivalence tests showing no measurable utility cost across all tested settings.

## When to Use

- Designing federated learning over networks where topology is genuinely unequal: hierarchical FL (hub clients see more), decentralized FL (graph degree varies), cross-silo FL (dataset-size influence varies)
- Threat-modeling membership inference that exploits structural position rather than gradient values alone
- Deciding whether to adopt topology-aware DP: the degeneracy test (symmetric topology → stick with uniform) is built into the method
- Auditing an existing FL deployment for topology-induced privacy asymmetry (clients in structurally exposed positions receive the same noise but carry more risk)
- NOT for: secure aggregation / cryptographic protection (orthogonal layer — combine with secure-aggregation skills); DP accountancy mechanics or clipping strategies (use `dptrainer-drop-in-differential-privacy`, `slaclip-adaptive-clipping-dp-sgd`, `dp-lac-lightweight-adaptive-clipping`); adaptive *budget-over-time* allocation (use `adadp-fedsec-adaptive-dp-secure-aggregation`); server-side trust scoring (use `dag-af2l-dag-ledger-async-fl-edge-iot`)

## Core Process / Workflow

### 1. Quantify Your Topology's Leverage Asymmetry

Three leverage proxies cover the main FL regimes:

| Regime | Leverage proxy ℓ°ᵢ | Symmetric case |
|---|---|---|
| Hierarchical FL | SBM group size (cluster membership exposure) | Balanced hierarchies |
| Decentralized FL | Graph degree | Ring, k-regular |
| Cross-silo FL | FedAvg dataset-size influence | Equal-size silos |

Compute ℓ°ᵢ per client. If leverage is uniform (ring, balanced hierarchy, equal silos), Fulcrum provably degenerates to uniform DP-SGD — adopt uniform and stop.

### 2. Run the TADI Attack to Ground the Threat Model

TADI is the empirical justification: a passive shadow-training attack with four channel ablations (A₁ parameter channel; A₂ᵗᵒᵖᵒ/A₂ᵒʳᵍ/A₂ᶠᵘˡˡ prior-coupling channels). Key calibration facts: the parameter channel is bounded by DP-SGD in all settings; the prior-coupling channels realize the theoretical bound under matched priors (perfect AUROC at η=0.75 in Setting C) but are conservative under realistic public-proxy adversaries (no positive lift on Fed-Heart-Disease). Run TADI on your topology before engineering the defence — if the attack shows no positive lift under your adversary model, uniform DP-SGD is already sufficient.

### 3. Apply the Closed-Form Allocation

The defence is a single formula: σ*²ᵢ = a/(K★ − ℓ°ᵢ) — clients with higher structural leverage receive *less* noise budget headroom consumed by the uncontrollable term, so their controllable mechanism term T_max·C²/(2σᵢ²|B|²) tightens until every client hits the same worst-case bound K★. Implementation exists in the codebase as an Opacus per-client wrapper (`fulcrum/dp/`). Three empirical headline results to anchor expectations: strict improvement at every tested (U, T_max) cell on Fed-ISIC2019 (real cross-silo, hierarchical), Fed-Heart-Disease (real cross-silo, tabular), and CIFAR-10 (statistical vehicle) — relative gap up to 26.6% on Fed-ISIC2019; TOST equivalence (p<0.05 at ±0.5pp) confirms no utility cost.

### 4. Position It in the DP-FL Stack

Fulcrum is the *spatial* allocation axis; it composes rather than competes:

- **Across time (budget schedules):** `adadp-fedsec-adaptive-dp-secure-aggregation` (adaptive ε over training rounds), `dp-lac-*` clipping adaptation
- **Across clients (this skill):** topology-aware noise allocation
- **Across the wire:** secure aggregation (`zk-proof-federated-learning-trust`, `ddp-sa-distributed-dp-secure-aggregation`)
- **Engineering adoption path:** `dptrainer-drop-in-differential-privacy` removes the integration barrier for any Opacus-based deployment — Fulcrum's per-client wrapper plugs into the same ecosystem

### 5. Adoption Decision Test

Adopt topology-aware DP when ALL hold: (1) your topology has measurable leverage asymmetry (degree spread, hub structure, or silo-size variance); (2) your threat model includes topology-exploiting adversaries (not just gradient-inversion); (3) you operate DP-SGD (per-sample clipping + Gaussian noise) so the per-client noise multiplier is a real control. Otherwise: uniform DP-SGD with good accounting remains the right answer — and Fulcrum's degeneracy guarantee means you lose nothing by re-evaluating as the network evolves.

## Cross-Domain Linkages (A-Tech)

- **Privacy/trust:** fills the topology dimension of the FL privacy stack; pairs with `adadp-fedsec-adaptive-dp-secure-aggregation` (temporal axis) and `dag-af2l-dag-ledger-async-fl-edge-iot` (trust-weighted DP on DAG ledgers — related spirit, different mechanism).
- **Open-source values:** GPL-3.0 research codebase with full deployment docs, attack pipelines, and figure reproduction — a complete open replication package, exactly the A-Tech transparency pattern.
- **Financial freedom:** the degeneracy guarantee means small federations on simple topologies avoid unnecessary engineering; allocation math is closed-form (no extra training cost).
- **Practical implementation:** Opacus-compatible wrapper; three-benchmark validation incl. real cross-silo medical data; TOST equivalence testing is a methodological template for any privacy-mechanism claim.

## Limitations

- Preprint with arXiv ID TBD; results are author-run — the 0.871/1.193-nat gaps and TOST equivalence await independent reproduction
- Threat model is *passive* adversary with the stated channel ablations; active/topology-manipulating adversaries are out of scope
- Sensitive-class concentration inference (classification settings); regression/other inference channels not studied
- Research codebase (GPL-3.0), not a production library

## References

- Rangwala, M., Sinnott, R.O. & Buyya, R. (University of Melbourne) — "Topology-Aware Differential Privacy in Federated Learning," 2026 (arXiv preprint; Fulcrum codebase, GPL-3.0, github: murtazahr — docs/EMPIRICAL_RESULTS.md carries the full claim-by-claim write-up). Theoretical core: additive per-client MI bound (controllable mechanism term + uncontrollable prior-coupling term); Theorem 5.3 min-max optimal allocation; TADI attack with four channel ablations.
- Related existing skills: `adadp-fedsec-adaptive-dp-secure-aggregation`, `dptrainer-drop-in-differential-privacy`, `dag-af2l-dag-ledger-async-fl-edge-iot`, `slaclip-adaptive-clipping-dp-sgd`, `zk-proof-federated-learning-trust`, `ddp-sa-distributed-dp-secure-aggregation`, `fl-governance-procedural-relational-structural`.
