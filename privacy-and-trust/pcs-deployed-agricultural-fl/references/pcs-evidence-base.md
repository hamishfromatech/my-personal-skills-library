# PCS Evidence Base

**Paper:** Lei, S., Abid, M.S., Belding, J., Mosher, S., Trivedi, M.B., Baruah, S., Wickes-Do, L., Anderson, A., Dumba, B., Whitcraft, A., Sahajpal, R., Li, S., Robbins, K., Gore, M., Frank, M., Wolf, S., Jones, L., Stroock, A., Gold, K., & Weatherspoon, H. (2026). *Private Computation Space: Experience with Trusted Multi-Cluster Federated Learning for Agriculture.* arXiv:2609.01667 (submitted Sept 1, 2026; cs.CR + cs.LG). Cornell University (Weatherspoon lab) with agricultural-science collaborators.

## The privacy barrier that motivates PCS

> "Artificial Intelligence has shown to help improve agricultural practices, yet adoption remains limited: **69% of U.S. farmers have privacy concerns with sharing their data**, and these concerns must be addressed before adoption is widespread."

This is the deployment-framing the library has been missing for agriculture: not "can FL preserve privacy" but "privacy is the *adoption blocker*" — the reason farms don't share data at all, before any model is trained.

## What PCS actually deploys

- **Open-source ML system** to provision and process farmer data securely.
- **Multi-cluster orchestration** for reliability in rural areas.
- **Asynchronous Federated Learning (FL)** — tolerates intermittent rural connectivity, not just lab-ideal conditions.
- **Differential Privacy (DP)** — farmer data never leaves raw.
- **Trusted Execution Environments (TEEs)** — server-side aggregation runs in attested enclaves.
- **Commodity hardware** — no specialized accelerators; the stack runs on what farms already have.

## The two real workloads

| Workload | Location | Duration | Modality |
|---|---|---|---|
| Nitrogen monitoring with **living plant sensors** | New York | **6 months** | In-field biological sensing |
| **Evapotranspiration prediction** from weather stations | California | **10 months** | Weather-station time series |

Six months + ten months of continuous field deployment is the longest real-workload run of a TEE+DP+async FL stack the library has tracked for agriculture — beyond the architecture-pattern coverage in `deployed-multi-cluster-tee-fl`.

## The three-layer defense — why all three

| Layer | Threat covered | Failure mode if missing |
|---|---|---|
| Differential Privacy | Per-farmer data leakage from model updates | Raw-farmer-adjacent reconstruction from gradients |
| TEE on aggregation | Malicious or compromised aggregator | Server reads plaintext updates, injects poisoned aggregates |
| Asynchronous FL | Rural connectivity dropouts | Training stalls when one farm's link drops |

PCS's design rationale: no single layer covers all failure modes in a fragile-infrastructure setting. Agriculture (and by extension, any rural/edge deployment) requires the full stack.

## Positioning against the library's privacy stack

| Existing skill | What it covers | PCS adds |
|---|---|---|
| `deployed-multi-cluster-tee-fl` | Multi-cluster TEE-FL architecture pattern | The agriculture-domain instantiation with the longest real deployment |
| `adaptive-dp-fl-concept-drift-edge` | DP-FL under concept drift at the edge | The TEE layer + commodity-hardware + rural-connectivity constraints |
| `federated-learning-for-privacy-preserving-ai` | FL privacy foundations | The *deployed* agriculture case (most library FL skills are methods, not field systems) |
| `privacy-preserving-local-ai` | Local-first privacy stance | The multi-party (farmer federation) extension of local-first |

## A-Tech content angles

- **Video:** "The first real privacy-preserving AI system deployed on American farms — and it runs on commodity hardware."
- **Slide:** the three-layer defense (DP + TEE + async) against rural failure modes.
- **Bridge:** connects to `xcal-fl-explanation-privacy-calibration` — PCS solves *data* privacy; XCal-FL solves *explanation* privacy. A full agriculture stack needs both.

## Honest caveats

- PCS's evaluation is agriculture-specific; the TEE+DP+async recipe generalizing to other domains is inference.
- TEE hardware (Intel SGX vs ARM TrustZone vs others) varies by vendor; portability is deployment-specific.
- Async FL trade-offs (convergence guarantees vs dropout tolerance) are domain-specific.
- The 69% farmer-privacy figure is a survey estimate — a barrier indicator, not a causal measurement.

## Citation

Lei, S., et al. (2026). Private Computation Space: Experience with Trusted Multi-Cluster Federated Learning for Agriculture. arXiv:2609.01667. https://doi.org/10.48550/arXiv.2609.01667