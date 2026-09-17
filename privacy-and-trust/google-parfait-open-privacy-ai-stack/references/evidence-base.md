# Google Parfait Evidence Base: Repository Details, Production DP Guarantees, Algorithm Internals, ODP Architecture, and Server Design

## Introduction

This reference document provides the deeper technical details supporting the `google-parfait-open-privacy-ai-stack` skill. It expands on each of the seven open-source repositories, presents the full Gboard differential privacy guarantee table, details the MF-DP-FTRL algorithm, documents the Android On-Device Personalization (ODP) architecture, describes the Federated Compute Server components, and provides a reference for the DP configuration parameters.

All content is drawn from the public `google-parfait` GitHub organization (launched January 2025), Google Research publications, Android Privacy Sandbox documentation, and RFC 9334.

---

## Part 1: Detailed Repository Descriptions

### 1. `federated-language`

**Purpose:** The core domain-specific language (DSL) and runtime abstraction for expressing federated computations.

**Key characteristics:**
- **Platform-independent:** Federated algorithms are expressed once and can execute on multiple backend runtimes. First-class backends include JAX and TensorFlow.
- **Federated types:** Introduces a type system that distinguishes `CLIENTS`-placed data (per-device values) from `SERVER`-placed data, enabling static reasoning about where computation occurs and what data crosses the network boundary.
- **Federated comprehensions:** Provides functional constructs (`tff.federated_map`, `tff.federated_aggregate`, `tff.federated_broadcast`) for expressing distributed computations declaratively.
- **Role:** This is the foundational layer that `tensorflow-federated` and `federated-compute` build upon. It is what makes Parfait portable and what allows the same algorithm to be simulated locally, run on a research cluster, or deployed to production Android devices.

**Why it matters:** Portability across ML frameworks means federated algorithms are not locked into TensorFlow or JAX. The typed DSL also enables formal analysis of data flow — critical for proving that raw data never leaves the client.

---

### 2. `tensorflow-federated` (TFF)

**Purpose:** High-level Python interfaces for federated learning and federated analytics, built on TensorFlow and JAX.

**Key modules:**
- **`tff.learning`:** The FL training API. Provides `tff.learning.build_federated_averaging_process` (FedAvg), DP-aware training (DP-SGD, DP-FTRL integration), model construction helpers, and evaluation APIs. Supports Keras and JAX models.
- **`tff.analytics`:** The federated analytics API. Provides building blocks for private heavy hitters, private histograms, and cardinality estimation with DP guarantees.
- **`tff.simulation`:** A simulation toolkit for local, single-machine experimentation. Enables developers to prototype FL/FA pipelines on a laptop before deploying to a real cross-device cluster. Includes data partitioning helpers and toy datasets.
- **`tff.aggregators`:** Composable aggregation factories, including secure aggregation (SecAgg), mean aggregation, and custom aggregation strategies.

**Why it matters:** TFF is the primary developer entry point. A practitioner can express a federated learning experiment in a few dozen lines of Python, test it locally with `tff.simulation`, and then port the same computation to `federated-compute` for production cross-device execution.

---

### 3. `federated-compute`

**Purpose:** The cross-device execution system — the production orchestration layer that runs federated computations across real (or simulated) populations of devices.

**Key components:**
- **Federated Compute Server:** The server-side orchestration system (detailed in Part 5). Manages task lifecycle, device assignment, aggregation, and model updates.
- **Android client libraries:** On-device runtime that executes the client-side of federated computations on Android devices. Integrates with Android Private Compute Core for isolation. Handles scheduling (device conditions: charging, idle, on Wi-Fi), local training, and secure upload of aggregated updates.
- **Reference demo:** The canonical "federated learning of emojis" example — a minimal, runnable end-to-end FL pipeline that trains an emoji-prediction model across simulated devices. Serves as the getting-started tutorial for the stack.

**Why it matters:** This is the bridge from research to production. The same federated computation expressed in TFF can be packaged and deployed via `federated-compute` to real Android devices at scale.

---

### 4. `confidential-federated-compute`

**Purpose:** Extends `federated-compute` with Trusted Execution Environment (TEE) based confidential computing, enabling verifiable privacy guarantees.

**Key characteristics:**
- Aggregation and DP noise injection execute inside attested hardware enclaves (e.g., Intel SGX, AMD SEV-SNP, or equivalent TEEs).
- The server operator cannot observe raw device updates or pre-DP aggregates — only the final, DP-noised, sealed output is visible.
- Enclave computations produce attestation evidence (per RFC 9334 RATS) that external auditors can verify to confirm the enclave ran the expected, DP-correct code on unmodified inputs.
- Enables "external verifiability" (Pillar 4): a third party can verify that DP was correctly applied without needing to trust the server operator or access raw data.

**Why it matters:** This is what distinguishes Parfait from stacks that assert DP in software but cannot prove it. Confidential federated compute makes the DP guarantee a verifiable property, not a trust assumption.

---

### 5. `trusted-computations-platform`

**Purpose:** The secure enclave platform runtime for stateful, multi-step computations inside TEEs.

**Key characteristics:**
- **Stateful computations:** Supports multi-round, stateful workflows inside the enclave (e.g., maintaining running model state across FL rounds), not just stateless function calls.
- **Sealing:** Enclave state can be sealed (cryptographically bound to the enclave identity) so it persists across enclave restarts and cannot be read or tampered with outside the enclave.
- **Attestation:** Produces and verifies remote attestation evidence per RFC 9334, allowing clients and auditors to confirm the enclave's identity, code hash, and configuration before submitting data.
- **Runtime:** Underpins `confidential-federated-compute` and can be used for broader confidential computing workloads beyond FL (e.g., confidential analytics, confidential inference).

**Why it matters:** Stateless enclaves are insufficient for FL, which is inherently iterative. The trusted-computations-platform provides the stateful, sealed, attested runtime that confidential federated compute requires.

---

### 6. `raft-rs`

**Purpose:** A Rust implementation of the Raft consensus algorithm.

**Key characteristics:**
- **Raft consensus:** A well-understood, leader-based consensus protocol for replicated state machines. Ensures a cluster of nodes agrees on a single, ordered log of operations despite failures.
- **Rust implementation:** Memory-safe, high-performance, suitable for deployment in critical infrastructure paths.
- **Role in Parfait:** Used within the `trusted-computations-platform` and `federated-compute` server infrastructure to provide fault-tolerant, replicated state for orchestration services (e.g., consensus on task state, aggregation state, model checkpoints). Enables crash-recoverable coordination across the distributed compute fabric.
- **Why not just a database?** FL orchestration has specific consistency and liveness requirements (e.g., exactly-once round execution, deterministic aggregation) that benefit from an embedded consensus engine rather than a generic external datastore.

**Why it matters:** Production FL at scale cannot tolerate a single point of failure in orchestration. `raft-rs` provides the replicated, fault-tolerant substrate that makes the Federated Compute Server reliable.

---

### 7. `dataset_grouper`

**Purpose:** Scalable tooling for producing group-structured (non-IID) federated dataset pipelines.

**Key characteristics:**
- **Group-structured partitioning:** Partitions datasets by natural grouping keys (e.g., per-user, per-organization) rather than producing IID random shuffles. This mirrors the real-world FL condition where each device's data is non-IID and skewed.
- **Scalable pipelines:** Built to handle large-scale dataset construction (millions of groups, billions of examples) efficiently, suitable for production-scale FL experimentation.
- **Realistic evaluation:** Enables researchers and engineers to evaluate FL algorithms and DP guarantees under non-IID conditions — the regime where naive FL often degrades and where DP tuning is most sensitive.

**Why it matters:** IID benchmarks systematically overestimate FL performance and underestimate DP utility loss. `dataset_grouper` enables honest, production-representative evaluation, which is essential for setting realistic ε targets and clipping thresholds.

---

## Part 2: Gboard Differential Privacy Guarantee Table

The Gboard deployment is the flagship production evidence for Parfait. As of the latest published data, 30+ next-word-prediction neural network language models have been trained with FL + DP across 7+ languages and 15+ countries. All models use δ = 10⁻¹⁰.

The following table reports representative ε values per language-locale model. The Portuguese (Brazil) and Spanish (Latin America) models are the first production DP models to achieve ε < 1, via MF-DP-FTRL.

| Language | Locale / Region | Algorithm | ε (privacy budget) | Devices per Round | Notes |
|----------|-----------------|----------|---------------------|-------------------|-------|
| Portuguese | Brazil (pt-BR) | MF-DP-FTRL | **0.994** | 12,000+ | First production DP model with ε < 1 |
| Spanish | Latin America (es-419) | MF-DP-FTRL | **≤ 1.0** | 12,000+ | First production DP model with ε < 1 (LATAM) |
| English (US) | United States (en-US) | DP-FTRL | ~1.5–4.0 (representative) | thousands | Early DP-FTRL deployment |
| English (India) | India (en-IN) | DP-FTRL | representative | thousands | |
| French | France (fr-FR) | DP-FTRL | representative | thousands | |
| German | Germany (de-DE) | DP-FTRL | representative | thousands | |
| Japanese | Japan (ja-JP) | DP-FTRL | representative | thousands | |
| Korean | Korea (ko-KR) | DP-FTRL | representative | thousands | |
| Hindi | India (hi-IN) | DP-FTRL | representative | thousands | |
| (additional) | (15+ countries total) | DP-FTRL / MF-DP-FTRL | up to 13.69 | thousands | Range spans 0.994 to 13.69 across all models |

**Notes on the table:**
- ε values range from **0.994 to 13.69** across the full set of 30+ models. The strongest guarantees (ε < 1) are achieved by the MF-DP-FTRL models.
- δ = 10⁻¹⁰ for all models — a negligibly small probability of privacy failure.
- "representative" entries indicate that the exact per-model ε was not enumerated in the public summary; the range 0.994–13.69 bounds all models.
- 12,000+ devices per round for the ε < 1 models reflects the larger cohort sizes needed to maintain utility under tighter privacy budgets.
- The table should be treated as indicative; exact values are published in Google's AI blog posts and accompanying papers and may be updated as new models launch.

---

## Part 3: MF-DP-FTRL Algorithm Details

### Background: DP-FTRL

**DP-FTRL (Differentially Private Follow-The-Regularized-Leader)** is a streaming/online optimization algorithm adapted for the federated setting. Unlike DP-SGD (which assumes IID minibatches), DP-FTRL is designed for the sequential, non-IID nature of federated rounds, where each round's participant set is a fresh sample from the device population.

**Key properties:**
- Treats the sequence of federated rounds as a stream of gradients.
- Maintains a running model state updated via a regularized leader-following rule.
- DP noise is added to the stream of updates, with the privacy cost analyzed via advanced composition over the stream.
- Well-suited to the tree-aggregation (`TREE`, `ADAPTIVE_TREE`) DP mechanisms, which provide efficient continual release under DP.

### Matrix Factorization DP-FTRL (MF-DP-FTRL)

**MF-DP-FTRL** is the variant that enabled the ε < 1 production milestone. The core idea:

1. **Matrix factorization of the update sequence:** The full sequence of model updates across all rounds is represented as a matrix operation. The update history matrix is factorized into a lower-rank product, which reduces the sensitivity of the aggregate to any single round's contribution.

2. **Tighter sensitivity bounds:** By factorizing the update sequence, the effective sensitivity of the released model to any single device's contribution is reduced relative to naive per-round DP-SGD composition. This tightens the privacy-utility trade-off: for the same ε, utility is higher; equivalently, for the same utility, ε can be made smaller.

3. **Result:** MF-DP-FTRL achieved **ε ≤ 1** for the Portuguese (Brazil) and Spanish (Latin America) Gboard models — the first time a production DP model trained directly on user data reached Google's Tier 1 "strong privacy guarantee" threshold (ε < 1).

### Why MF-DP-FTRL matters for A-Tech
- It demonstrates that ε < 1 is achievable in production, not just in research — validating the feasibility of strong DP for real on-device personalization.
- It provides a concrete algorithm practitioners can adopt (open-source via Parfait) rather than a theoretical aspiration.
- It shows the value of algorithmic innovation (matrix factorization) in improving the privacy-utility frontier, complementing hardware (TEE) and systems (SecAgg) approaches.

---

## Part 4: Android On-Device Personalization (ODP) Architecture

ODP is Google's Privacy Sandbox initiative for on-device, privacy-preserving personalization. It is the application-layer embodiment of Parfait's principles on Android.

### 4.1 Paired-Process Architecture

ODP uses a **paired-process** design to enforce isolation between sensitive model execution and the rest of the system:

**ManagingProcess:**
- Handles lifecycle management, networking, and non-sensitive orchestration.
- Cannot directly access model data or inference inputs/outputs.
- Communicates with the IsolatedProcess via a narrow, policy-enforced IPC (inter-process communication) boundary.

**IsolatedProcess:**
- Sandboxed process that holds the model and executes inference and on-device personalization.
- Has access to the model and to permitted (policy-approved) data.
- Cannot exfiltrate data except through the policy-enforced IPC boundary.

**Why paired-process?** It creates a hard isolation boundary. Even if the ManagingProcess is compromised, it cannot read the model's sensitive inputs or outputs. The IsolatedProcess is the trust boundary for model execution.

### 4.2 Policy Engine

The policy engine is the gatekeeper for data flow across the isolation boundary:

- **Data ingress rules:** Define what data may enter the IsolatedProcess (e.g., permitted feature vectors, sanctioned model inputs). Rejects unauthorized data types.
- **Data egress rules:** Define what may leave the IsolatedProcess (e.g., only aggregated, DP-noised, or non-sensitive outputs). Blocks exfiltration of sensitive inputs or intermediate computations.
- **Enforcement:** Rules are enforced at the IPC boundary. Any data crossing the boundary is checked against the policy; violations are blocked and logged.

**Why it matters:** The policy engine makes the isolation boundary programmable and auditable. Rather than relying on the developer to "be careful," the system enforces data-flow constraints by construction.

### 4.3 TEE-Based Aggregation

For federated aggregation, ODP routes aggregation and DP noise injection through a Trusted Execution Environment:

- **Central DP via TEE sealing:** The TEE receives raw aggregated updates from devices, applies DP noise, and **seals** the result. The server only ever observes the sealed, post-DP output — it cannot see pre-DP aggregates.
- **Sealing:** Cryptographically binds the output to the enclave identity, ensuring the sealed output cannot be forged or tampered with outside the enclave.
- **Attestation (RFC 9334 RATS):** The enclave produces attestation evidence conforming to the RFC 9334 Remote ATtestation procedureS architecture. This evidence allows external auditors to verify:
  - The enclave's identity and code hash (what code is running).
  - The enclave's configuration (DP parameters, privacy budget).
  - That the computation ran on unmodified inputs inside the attested TEE.

**Why it matters:** TEE sealing + RFC 9334 attestation makes the central DP guarantee externally verifiable. An auditor does not need to trust the server operator — they can verify the enclave's integrity and the DP computation's correctness from the attestation evidence.

### 4.4 ODP Privacy Model: "Privacy via Confidentiality"

ODP formalizes its privacy reasoning as **"privacy via confidentiality"**:

- **Consumer-producer DAG:** The full computation is modeled as a directed acyclic graph (DAG) of consumer-producer relationships. Each node consumes inputs and produces outputs; edges represent data dependencies. This makes the full data flow explicit and analyzable.
- **Post-processing for DP:** DP analysis is applied via the post-processing property: if an input to the DAG satisfies (ε, δ)-DP, then any function of that input (downstream in the DAG) also satisfies (ε, δ)-DP. This allows DP guarantees to propagate through the computation graph without additional privacy cost.
- **Advanced composition:** When multiple DP mechanisms compose within the DAG (e.g., multiple rounds or multiple statistics), advanced composition theorems are used to compute the total privacy cost across the DAG, tighter than naive composition.
- **Central DP via TEE sealing:** The central DP guarantee is enforced by the TEE sealing mechanism — the only observable output of the confidential computation is the sealed, post-DP result.

**Why it matters:** The DAG + post-processing + advanced composition framework gives ODP a principled, composable privacy analysis. It is not ad-hoc — it is a formal model that can be machine-checked and audited.

---

## Part 5: Federated Compute Server Architecture

The `federated-compute` server is the production orchestration system for cross-device FL/FA. Its core components:

### 5.1 Task Management
- **Role:** Defines, schedules, and tracks federated computations across their full lifecycle.
- **Responsibilities:**
  - Task definition: spec (model, algorithm, DP config, hyperparameters), population targeting, round count, convergence criteria.
  - Scheduling: when tasks run, round cadence, resource allocation.
  - Lifecycle tracking: round-by-round status, failure handling, checkpoint management.
- **Persistence:** Task state is replicated via `raft-rs` consensus for fault tolerance.

### 5.2 Task Assignment
- **Role:** Selects eligible devices for each round and dispatches task specs.
- **Responsibilities:**
  - Device selection criteria: population definition, eligibility (OS version, device capabilities), sampling rate (how many devices per round).
  - Dispatch: pushes task specs (model checkpoint, client-side computation graph) to selected devices via the Android client libraries.
  - Round management: tracks which devices accepted, completed, or failed the round; manages timeouts and retries.
- **Why it matters:** Device selection directly affects both utility (enough participants for stable aggregation) and privacy (sampling rate enters the DP analysis via privacy amplification by subsampling).

### 5.3 Aggregator
- **Role:** Receives device updates, applies secure aggregation, and (in confidential deployments) routes through the TEE for DP noise injection.
- **Responsibilities:**
  - Secure aggregation (SecAgg): cryptographically combines device updates so the server sees only the sum, never individual contributions. Protects against a curious server even before DP is applied.
  - Confidential aggregation: in TEE deployments, raw (SecAgg'd) updates enter the enclave; the enclave applies DP noise and seals the output.
  - Dropout handling: aggregates over the devices that actually reported, adjusting for participant dropout.
- **Why it matters:** The aggregator is the privacy-critical path. SecAgg + TEE + DP together ensure that even a malicious server cannot recover individual data or bypass DP.

### 5.4 Model Updater
- **Role:** Applies aggregated updates to the global model and prepares the next round.
- **Responsibilities:**
  - Server-side optimizer state: maintains momentum, adaptive learning rate state (e.g., FedAdam, FedAvgM) across rounds.
  - Model checkpointing: saves the updated global model for the next round and for evaluation.
  - Convergence monitoring: tracks loss/accuracy metrics (computed privacy-safely) to decide when to stop.
- **Why it matters:** The model updater is where the aggregated, DP-noised update becomes the next global model. Correctness here determines whether the DP guarantee on the update propagates to the released model (it does, via post-processing).

### Server-Wide Properties
- **Fault tolerance:** Critical state (task state, model checkpoints) is replicated via `raft-rs` consensus, so the server survives node failures without losing training progress.
- **Horizontal scalability:** Designed for millions of concurrent device participants; the aggregator and task assignment services scale horizontally.
- **Confidential option:** In confidential deployments, the aggregator and model updater execute inside TEEs, so the server operator never sees pre-DP data.

---

## Part 6: Differential Privacy Configuration Parameters

Parfait supports multiple DP mechanisms, configurable per federated task. These determine how noise is injected and how the privacy budget is consumed across rounds.

### FIXED_GAUSSIAN
- **Mechanism:** Standard Gaussian mechanism. Noise ~ N(0, σ²) is added to each aggregated update, with σ calibrated to the per-round privacy budget (ε_round, δ).
- **Composition:** Total privacy cost = sum of per-round costs (basic composition) or tighter advanced composition.
- **Use case:** Simple, predictable, well-understood. Good baseline and for tasks where the privacy budget is generous relative to the number of rounds.
- **Trade-off:** Fixed σ may over- or under-spend the budget depending on round-to-round variability.

### ADAPTIVE_GAUSSIAN
- **Mechanism:** Gaussian mechanism with adaptive noise scaling. σ is adjusted per round based on observed round statistics (e.g., number of participants, update norms) to better utilize the total privacy budget across all rounds.
- **Composition:** Uses advanced composition with adaptive budget allocation.
- **Use case:** Tasks with variable round sizes or where maximizing utility under a fixed total ε is critical.
- **Trade-off:** More complex; requires careful calibration to avoid under-spending (wasted budget) or over-spending (privacy violation).

### TREE
- **Mechanism:** Tree-based (hierarchical) aggregation for streaming/continual release. Builds a binary tree of partial sums; noise is added to tree nodes. Enables efficient DP release of a running aggregate over a stream of updates.
- **Use case:** DP-FTRL-style streaming algorithms; continual release of model updates or statistics. Well-suited to the sequential nature of federated rounds.
- **Trade-off:** Higher per-update computational overhead than flat Gaussian, but tighter privacy-utility trade-off over long streams.

### ADAPTIVE_TREE
- **Mechanism:** Adaptive variant of tree aggregation. Tunes the tree structure and noise allocation per round based on observed data, improving utility for non-stationary (non-IID) data streams.
- **Use case:** MF-DP-FTRL and other advanced streaming DP algorithms; the variant that enabled ε < 1 for Gboard Portuguese (Brazil) and Spanish (Latin America).
- **Trade-off:** Most complex to configure; offers the best privacy-utility frontier for production streaming FL.

### Configuration Considerations
- **δ:** Typically set to 10⁻¹⁰ for production (negligibly small probability of privacy failure). The same δ is used across all mechanisms.
- **Clipping threshold (C):** Bounds the ℓ2-norm of per-device updates, controlling sensitivity. Must be tuned per model/task; too low truncates signal, too high wastes noise budget.
- **Sampling rate (q):** The fraction of eligible devices selected per round. Enters the DP analysis via privacy amplification by subsampling (tighter bounds for smaller q).
- **Number of rounds (T):** Total rounds over which the budget is spent. More rounds = more composition cost; mechanisms like TREE/ADAPTIVE_TREE amortize this more efficiently.

---

## Part 7: Production Evidence Summary

| Dimension | Value |
|-----------|-------|
| Language models (Gboard) | 30+ |
| Languages | 7+ |
| Countries | 15+ |
| ε range | 0.994 – 13.69 |
| Strongest ε | 0.994 (Portuguese/Brazil, MF-DP-FTRL) |
| First ε < 1 production DP | Portuguese (Brazil), Spanish (Latin America) |
| δ | 10⁻¹⁰ (all models) |
| Devices per round (strongest DP) | 12,000+ |
| Algorithms | DP-FTRL, MF-DP-FTRL |
| Other deployments | Android Private Compute Core, Google Maps |
| Repositories | 7 (all Apache 2.0) |
| Contributors | 100+ |
| GitHub org | `google-parfait` (launched January 2025) |

---

## Part 8: Cross-References to Related Skills

- `google-gboard-private-fl-dp` — Gboard-specific FL+DP production deep dive (companion skill)
- `federated-learning-for-privacy-preserving-ai` — FL fundamentals
- `privacy-by-design-generative-ai` — Privacy-by-design for generative models
- `dataguard-hardware-dp-guarantee` — Hardware-enforced DP (complementary hardware approach to TEE-based DP)
- `federated-learning-as-a-service-2026` — FL-as-a-service architectures
- `privacy-first-ai-pipeline-defense` — Software-level DP enforcement
- `convergent-dp-analysis-federated-learning` — DP analysis for FL
- `ddp-sa-distributed-dp-secure-aggregation` — Distributed DP + secure aggregation

---

## Sources

- Google Research, `google-parfait` GitHub organization: https://github.com/google-parfait (launched January 2025).
- Repositories: `federated-language`, `tensorflow-federated`, `federated-compute`, `confidential-federated-compute`, `trusted-computations-platform`, `raft-rs`, `dataset_grouper`.
- Google AI blog posts on Gboard federated learning and differential privacy (DP-FTRL, MF-DP-FTRL).
- Android On-Device Personalization (ODP) documentation, Privacy Sandbox initiative.
- RFC 9334 — Remote ATtestation procedureS (RATS) architecture.
- Apache 2.0 open-source license.
- 100+ contributors across the Parfait organization.