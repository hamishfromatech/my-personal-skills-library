---
name: topology-betrays-privacy-sa-attack
description: Applies the lattice-based reconstruction attack on secure aggregation in decentralized federated learning (Yu, Ji, Bjerva & Li, arXiv:2609.08476, Sept 8, 2026) as the TOPOLOGY-BETRAYS-PRIVACY pattern — the first formal demonstration that secure aggregation (SA), long treated as a strong privacy guarantee in federated learning, leaks in decentralized topologies because sparse local-neighborhood aggregation gives colluding semi-honest nodes asymmetric partial views of honest peers' updates; reconstruction is posed as the Hidden Subset Sum Problem and solved with lattice reduction plus structural filtering. Use when [evaluating whether secure aggregation suffices for a decentralized or P2P federated deployment, deciding DP composition for DFL, designing topology defenses, or briefing on FL privacy guarantees that hold in hub-spoke but not sparse decentralized settings]. NOT for [centralized-server FL design — use federated-learning-for-privacy-preserving-ai — or LoRA-specific DP aggregation — use fedgsa-grassmann-dp-fl-full].
---

# When Topology Betrays Privacy: The Secure-Aggregation Attack

## Overview
Yu, Ji, Bjerva & Li (arXiv:2609.08476, Sept 8, 2026) break the assumption that underlies a decade of federated-learning design: that seeing only an *aggregate* of neighbors' updates hides the individual updates. In **decentralized** FL — where each node aggregates only over its neighborhood — sparse topologies give colluding semi-honest nodes asymmetric, overlapping partial views. Multiple hidden linear combinations of an honest node's private states become observable, and lattice reduction recovers them.

## When to Use
- Any DFL / P2P / gossip-FL deployment claiming "secure aggregation" as the privacy story
- Threat-model reviews for cross-device learning where the topology is sparse by design (mesh, ring, random graphs)
- Deciding whether DP must be layered *on top of* SA in decentralized settings (in centralized hub-spoke FL with a trusted server, SA + DP is a stronger combined story)
- NOT for: centralized cross-silo FL, DP optimizer selection (`dptrainer-drop-in-differential-privacy`, `dp-fedsofim-server-side-fisher-preconditioning`)

## Core Process / Workflow

### Why the attack works (the mechanism)
1. **Local aggregation ≠ global aggregation.** Each node sees a weighted sum over its own neighbors. A node's private update enters *multiple* distinct aggregates visible to a coalition.
2. **Collusion assembles overdetermined systems.** k colluding nodes jointly observe several different linear combinations of the same unknown private vector(s).
3. **Reconstruction = Hidden Subset Sum.** With both states and coefficients hidden, recovery is formally equivalent to the Hidden Subset Sum Problem; the authors deploy lattice reduction (LLL/BKZ-class) plus structural filtering (sparsity, coefficient structure) to recover the original updates — and from them, training data.

### Measured result
On image, tabular, and text tasks under sparse DFL topologies, colluding semi-honest nodes **recover honest peers' original local updates**, enabling downstream private-data reconstruction. The attack is *semi-honest* — no deviation from the protocol required; sparse topology alone does the damage.

### Defense checklist (derive from the mechanism)
- **Degree floor / minimum connectivity:** enforce minimum neighborhood size so no node's update appears in too few aggregates (raises the hidden-subset-sum instance's difficulty).
- **Pairwise masking that scales:** if topology is sparse, use SA constructions whose security doesn't degrade with sparse neighborhoods — or abandon SA as the sole guarantee.
- **DP as the load-bearing defense.** In sparse DFL, treat SA as a communication-pattern optimization, not a privacy guarantee; add calibrated DP noise (see `dptrainer-drop-in-differential-privacy`, `fulcrum-topology-aware-dp`) so the reconstruction problem is infeasible even with the aggregate views.
- **Topology audit as part of threat modeling.** For any DFL design, ask: *which coalitions of nodes can assemble enough overlapping aggregates to overdetermine an honest node's state?* If the answer is "a modest colluding set on a sparse graph," SA is decorative.
- **Honest framing in marketing:** "secure aggregation protects your updates" is now falsifiable in decentralized settings; say "secure aggregation + DP" or say nothing.

## References
- Nearest neighbors: `fulcrum-topology-aware-dp` (the spatial/topological DP axis — composes with this attack's topology insight), `flipd-majority-collusion-resistant-secure-aggregation`, `adadp-fedsec-adaptive-dp-secure-aggregation`, `ddp-sa-distributed-dp-secure-aggregation`, `fedex-lora-exact-federated-aggregation` (Chorus implementation discipline), `federated-learning-for-privacy-preserving-ai`, `privacy-preserving-local-ai`.
- Honest caveats: assumes collusion among semi-honest nodes (not full Byzantine adversaries); attack demonstrated on standard tasks under sparse topologies — real-world DFL deployments should re-evaluate their own graph density; the paper establishes the formal connection (Hidden Subset Sum) rather than a drop-in production attack toolkit.

*Source: Yu, W., Ji, C., Bjerva, J., & Li, Q. (2026). "When Topology Betrays Privacy: Lattice-Based Reconstruction Attacks on Secure Aggregation in Decentralized Federated Learning." arXiv:2609.08476, Sept 8, 2026.*