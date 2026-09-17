# DAG-AF2L Evidence Base

## Primary Source
He, L. (2026). "A DAG-based asynchronous federated learning framework with byzantine-resilient privacy preservation for edge IoT." *Discover Internet of Things*, Springer Nature. DOI: 10.1007/s43926-026-00461-0. Published 09 August 2026. License: CC BY-NC-ND 4.0.

Author affiliation: Department of Information Management, Guangdong Justice Police Vocational College, Guangzhou, China.

## Core Framework Architecture

### DAG Ledger Substrate
- Fully decentralized: no central server; updates form blocks in a directed acyclic graph
- Enables concurrent, non-blocking model updates
- Each block references predecessor blocks (like IOTA Tangle or blockchain but optimized for FL)

### Three Co-Designed Mechanisms

#### 1. Staleness-Aware Aggregation
- Staleness = time delay between client update computation and global incorporation
- Stale updates weighted less to prevent convergence derailment
- Part of unified convergence bound

#### 2. Multi-Dimensional Trust Evaluation
- Dimensions include: data quality, update timeliness, consistency with peers, historical behavior
- Graph-coupled reputation: trust propagates along DAG edges
- A client's trust score informed by validators of its updates

#### 3. Trust-Aware Personalized DP
- Higher-trust clients → less DP noise (preserves utility)
- Lower-trust/ suspect clients → more DP noise (protects against poisoned contributions)
- Aligns noise with filtering mechanism → eliminates destructive interference

## Unified Convergence Analysis
The paper's key theoretical contribution: a single convergence bound that explicitly includes all three terms:
- Byzantine attack residual impact (after filtering)
- DP noise degradation
- Asynchronous staleness

The trinity tradeoff: robustness × privacy × efficiency. Co-design makes this surface more favorable than the product of independent tradeoffs.

## Experimental Setup
- **Datasets**: Three datasets (CINIC-10, and others)
- **Models**: ResNet-18 for image classification
- **Attack types**: Four — Gaussian noise (GN), sign flipping (SF), label flipping (LF), adaptive attack (AA)
- **Baselines**: FedAsync, FedBuff, DP-FedAvg, DAG-ACFL, DAG-EnseFL, REPChain-FL, ACSFL, ASO-Fed, FABA, Krum
- **Metrics**: Convergence rate, robustness (accuracy under attack), privacy-utility tradeoff, detection metrics

## Key Results
- Consistently outperforms all baselines in convergence, robustness, and privacy-utility tradeoff
- Joint co-design eliminates destructive interference between DP noise and Byzantine filtering
- Empirical tradeoff surface corroborates theoretical analysis
- Edge testbed deployment confirms practicality on resource-constrained devices

## Funding
Four projects:
1. 2025 Educational Teaching Reform Research (Guangdong Vocational Education Electronic Information, Grant No. 25)
2. 2025 Teaching and Educational Reform (Guangdong Provincial Vocational School Competition, Grant No. 2025SZWGZ115)
3. 2025 Research Project Special Committee on Educational Digitalization (Grant No. HED-2025-09)
4. Special Project "AI Empowered Vocational Education Teaching Reform" (Grant No. DJ2026B001)

## License
CC BY-NC-ND 4.0 (Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International)
- Non-commercial use, sharing, distribution permitted
- No adaptations/derivatives allowed

## Limitations
- Single-author paper from a vocational college (smaller research group)
- CC BY-NC-ND license restricts commercial derivative works (relevant for A-Tech's open-source-but-commercial-aware positioning)
- No specific epsilon/delta values reported in abstract/summary
- Full text not fully extracted (preprint/unedited version noted by publisher)

## Relationship to Existing A-Tech Skills
- **`federated-byzantine-robust-partial-participation`** (DMA, ICML 2026): DMA solves the robustness-efficiency conflict in centralized FL with partial participation. DAG-AF2L extends to decentralized async FL for edge IoT — the next frontier.
- **`sheld-fl-self-learning-heterogeneous-dp-framework`**: SHELD-FL uses self-learning privacy budgeting (dynamic ε by gradient sensitivity). DAG-AF2L uses trust-aware personalization (DP noise follows trust score, not gradient sensitivity). Complementary approaches.
- **`adaptive-dp-fl-concept-drift-edge`** (FedDriftGuard): FedDriftGuard handles concept drift in edge FL. DAG-AF2L handles Byzantine attacks in edge FL. Different edge FL challenges but same deployment context.
- **`google-parfait-open-privacy-ai-stack`**: Google's stack is production-scale and centralized (Gboard). DAG-AF2L is research-stage and decentralized. Different deployment models.

## A-Tech Relevance
- Decentralized architecture = no central server = aligns with open-source community-governed AI
- Edge IoT focus = enables AI on commodity hardware = financial freedom
- Byzantine-resilient = safe community model training
- Trust-aware DP = privacy-preserving without destroying utility for trusted contributors
- DAG ledger = auditable model update history (every update is a block with references)
