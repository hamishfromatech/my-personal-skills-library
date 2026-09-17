---
name: pcs-deployed-agricultural-fl
description: Applies the Private Computation Space (PCS, Lei et al., arXiv 2609.01667, Sept 1 2026, Cornell) — the first DEPLOYED open-source TEE+DP+asynchronous-FL system for agriculture, running on commodity hardware in fragile rural connectivity with real six-month (NY nitrogen, living plant sensors) and ten-month (CA evapotranspiration) workloads — the first agriculture case the library has beyond the deployed-multi-cluster TEE-FL pattern. Use when [designing privacy-preserving federated learning for rural or connectivity-poor deployments, adapting TEE+DP stacks to commodity hardware, or briefing on agriculture as the open-privacy deployment frontier]. NOT for [the general multi-cluster TEE-FL architecture pattern — use the existing deployed-multi-cluster TEE-FL skill — or DP-FL accuracy methods].
---

# PCS: The Deployed Agriculture TEE+DP FL System

## Overview
PCS (Lei, Abid, Belding, Mosher, Trivedi, Baruah, Wickes-Do, Anderson, Dumba, Whitcraft, Sahajpal, Li, Robbins, Gore, Frank, Wolf, Jones, Stroock, Gold & Weatherspoon, arXiv 2609.01667, Sept 1 2026) is the first **deployed, open-source** ML system provisioning farmer data securely across **multi-cluster orchestration + asynchronous FL + Differential Privacy + Trusted Execution Environments** — running on commodity hardware in fragile rural infrastructure, with six months (nitrogen monitoring via living plant sensors, NY) and ten months (evapotranspiration prediction, CA) of real field workloads.

## Why it matters now
- **Agriculture is the open-privacy deployment frontier:** 69% of US farmers report privacy concerns about sharing data — the paper's motivation. PCS answers with deployed infrastructure, not theory.
- **The hardware constraint is solved in public:** commodity hardware, not specialized accelerators — the DP+TEE stack runs on what farms already have.
- **The connectivity constraint is solved in public:** asynchronous FL tolerates the dropouts of rural links, not just lab-ideal conditions.
- **Six months + ten months of real field workloads** is the longest continuous agriculture FL deployment the library has tracked — beyond the multi-cluster TEE-FL architecture pattern.

## The three-layer defense
1. **Differential Privacy** on local training (farmer data never leaves raw).
2. **Trusted Execution Environments (TEEs)** on aggregation (server-side code attested).
3. **Asynchronous Federated Learning** across multi-cluster orchestration (reliability under rural connectivity).

## A-Tech alignment
- **Open source:** PCS is open-source — the TEE+DP+async-FL stack is reproducible without enterprise licensing.
- **Privacy:** three-layer defense (DP+TEE+async) is the strongest pattern the library tracks for privacy-preserving distributed learning outside the cloud.
- **Financial freedom:** commodity hardware deployment removes the compute-cost barrier for solo/small privacy-first AI startups targeting agriculture, healthcare, and edge sectors.
- **Practical:** the "farmer data never leaves raw + TEE-attested aggregation + async resilience" formula is a one-slide privacy-first pitch for any rural/edge sector.

## When to Use
- Designing FL for **connectivity-poor** environments (rural, maritime, field sensors, remote clinics).
- Adapting TEE+DP stacks to **commodity hardware** rather than specialized cloud instances.
- Briefing on **agriculture, environmental monitoring, or smallholder farming** as the deployment frontier for privacy-first AI.
- Contrasting with `deployed-multi-cluster-tee-fl` (the multi-cluster pattern) — PCS is the agriculture-domain instantiation with the longest real-world run.

## NOT For
- The general multi-cluster TEE-FL architecture pattern — use `deployed-multi-cluster-tee-fl`.
- Pure DP-FL accuracy methods without a TEE layer — use the broader FL-stack skills.
- On-device-only (non-federated) privacy — use the local-first/edge skills.

## Core Process / Workflow
1. **Start from the privacy concern, not the model.** PCS's design rationale is the 69% farmer privacy barrier — begin deployments by measuring your target population's data-sharing resistance.
2. **Layer the defenses.** DP (client-side, formal guarantees) + TEE (server-side attested aggregation) + async FL (tolerates dropouts). Each layer covers a different failure mode; all three are needed for fragile-infrastructure deployments.
3. **Deploy on commodity hardware.** No specialized accelerators — the stack targets what rural sites already run.
4. **Plan for multi-cluster orchestration.** Reliability comes from distributed cluster management, not single-server uptime.
5. **Validate with real workloads.** PCS's credibility comes from 6+10 months of field data, not benchmarks. Plan for longitudinal validation before claiming deployment.

## Honest caveats
- The paper's evaluation is agriculture-specific (nitrogen, evapotranspiration); generalizing the async+TEE+DP recipe to other domains is inference, not finding.
- TEE hardware varies by vendor (Intel SGX vs ARM TrustZone vs others); portability is a deployment question.
- Asynchronous FL complicates convergence guarantees vs synchronous FedAvg; the paper's trade-offs are domain-specific.
- The 69% farmer-privacy figure is a survey estimate, not a causal barrier measurement.

## Pairs with
`deployed-multi-cluster-tee-fl` (the architecture pattern this instantiates for agriculture), `adaptive-dp-fl-concept-drift-edge` (edge DP-FL), `federated-learning-for-privacy-preserving-ai` (the foundation), `xcal-fl-explanation-privacy-calibration` (the fidelity axis PCS leaves unaddressed), `privacy-preserving-local-ai` (the local-first stance).

## References
- See [references/pcs-evidence-base.md](references/pcs-evidence-base.md) for the abstract, deployment details, and the agriculture privacy-barrier framing.