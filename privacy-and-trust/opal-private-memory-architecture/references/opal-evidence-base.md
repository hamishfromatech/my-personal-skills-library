# Opal Private Memory Architecture — Evidence Base

## Primary Source

Kaviani, D., Ozdarendeli, A.E., Zhu, J., Ding, Y., Popa, R.A. — "Opal: Private Memory for Personal AI" (arXiv:2604.02522, 2026). UC Berkeley + Google DeepMind. Fetched full text. Under consideration for deployment to millions of users at a major AI provider.

## The Problem in Detail

Personal AI systems retain long-term memory of user activity (documents, emails, messages, meetings, ambient recordings). Microsoft Recall continuously screenshots user activity; Google Gemini ingests emails and photos; AI memory tools capture notes, meetings, conversations. Embodied AI agents in robotics and wearables capture richer ambient streams.

The result: unprecedented concentration of personal data at providers. Outsourced workers were recently found reviewing sensitive videos captured by Meta's Ray-Ban smart glasses (BBC News, 2026). Compromising the application provider's infrastructure could expose this data in its entirety.

End-to-end encrypted messaging established private communication as a baseline user expectation, yet personal AI systems — which accumulate far richer portraits — offer no comparable guarantee.

## Why Existing Approaches Fail

### Trusted hardware alone
Apple (Private Cloud Compute, 2024), Google (Private AI Compute, 2025), Anthropic (Confidential Inference via Trusted VMs, 2025) deploy TEEs for private inference. But LLM context windows and TEE memory are finite and expensive, driving systems toward persistent long-term memory on untrusted storage. Observing which memories a system retrieves is nearly as revealing as seeing the query text itself (Song & Raghunathan, 2020; Morris et al., 2023; Li et al., 2023).

### Client-local memory
Stores personal data on user's device, re-uploads context on every request. But personal corpora outgrow a single device (Dumais et al., 2015; Gemmell et al., 2006), local storage lacks durability (Schroeder & Gibson, 2007; Wunder et al., 2025), and multi-device access demands a unified backend (Dearman & Pierce, 2008; Yuan et al., 2022). Production assistants store memory cloud-natively (OpenAI, 2024; Woodward, 2026; Apple, 2024).

### ORAM alone
ORAM hides access patterns at O(log N) per-access cost. But agentic memory systems achieve accuracy by augmenting semantic search with query-dependent traversals (knowledge graphs, temporal filters, multi-hop lookups). ORAM enforces a fixed access budget, precluding these traversals. Compass (Zhu et al., 2025) co-designs ANN search with ORAM, but does not support query-dependent traversals.

## The Opal Architecture

### System Model
Three components: client, set of trusted enclaves, untrusted disk. All private computation inside enclaves; encrypted ORAM state on untrusted disk.

Three enclaves:
- **Enclave_Opal** (controller): orchestrates query/ingestion, maintains KG, ANN index metadata, ORAM client state
- **Enclave_Emb** (embedding): serves embedding model
- **Enclave_LLM** (LLM): serves LLM model

Two ORAM databases on disk:
- **ORAM_ANN**: vector embeddings for finding relevant matches
- **ORAM_Data**: raw data chunks passed to LLM for answer generation

### Threat Model
Malicious host with uncompromised enclave (established TEE threat model: Signal SVR3, Occlum, Gramine-TDX; industry systems from Apple, Google, Anthropic). Attacker cannot view/tamper with data/computation inside enclave, but can observe, drop, replay, reorder, tamper with all communication/data/computation outside enclave. Focus on hiding what the user is doing, not that the user is present.

### Security Guarantees
- **Confidentiality**: All user data, queries, responses hidden from adversary. Adversary learns whether operation is Query or Ingest, but neither content nor which stored data is relevant nor which records were accessed.
- **Integrity and Freshness**: Tampering/rollback of ORAM-backed storage detected on every request. Client state held in enclave RAM, periodically sealed to disk; freshness relative to provider's last checkpoint.

### Security Definition
Indistinguishability-based, relative to leakage function L. For any request sequence, L reveals: public parameters Π (ANN fetch count n, reranking budget K, summarization period T, tree depth L, bucket capacity Z), operation type at each step, and resulting fixed trace shape (number of fixed-size inter-enclave messages and ORAM batch sizes).

**Theorem 2.1**: For any PPT stateful adversary A, the Opal protocol satisfies condition 1 (indistinguishability) with probability at most 1/2 + negl(λ) and condition 2 (correct execution) with probability at most negl(λ), in the G_att-hybrid model, when instantiated with durable monotonic client counter, secure KDF, SUF-CMA secure MAC, collision-resistant hash, and AEAD scheme.

## KG-Filtered Search (Detail)

### Enclave-Resident Graph
Compact in-enclave control plane. Raw text and embeddings remain in ORAM. Graph stores only identifiers, sparse metadata, structural relationships — deterministically extracted from application metadata at ingest time without LLM call.

Five node types: Artifact (logical memory unit, timestamp, modality label), Chunk (identifier for content piece; content in ORAM), Summary (consolidated digest), Person (named individuals), Project (workstreams).

Multiple chunks from same artifact share single artifact record → graph scales with number of distinct artifacts, not individual chunks.

### Filtration Types
- **Temporal**: resolve to time windows; retain only artifacts within range
- **Modality**: scope to source class; percolate along source-link edges for linked artifacts of different modalities
- **Person**: filter on canonical participant identifiers
- **Project**: filter on workstream tags from organizational structure

### Query-Time Traversal
Single retrieval round (variable rounds would leak query information). Confidence assigned to each predicate: only high-confidence become hard constraints; low-confidence widened or dropped. Relaxation cascade: person → project → modality, widen temporal windows until enough candidates.

## Oblivious Dreaming (Detail)

### Adaptive Retention
TTL in logical operations. Each query access refreshes TTL (LRU-like). Targets steady-state store size N:

TTL = ⌈N / (w · c · η)⌉, where η = 1 + (1-w)·ln(K)

w = write ratio, c = chunks per logical item, K = reranking budget. All parameters public or derivable from public workload statistics. Expired items deleted opportunistically as encountered in stash.

### Sleepy Rebalancing
IVF clusters drift from continuous insertions/deletions. When cluster exceeds split threshold: reconstruct approximate member vectors from PQ residual codes, run 2-means, install two new centroids. No items fetched; split invisible externally. Pending corrections applied lazily during ordinary ORAM accesses. 5.7% of 2.22M pending reassignments actually required cluster change.

### Memory Compression
Every T ingestions: retrieve recent chunks via KG.Traverse(recent), summarize with LLM, re-ingest through ordinary path. Summary is another memory item with own identifier, embedding, KG links. Summaries: 16.7% of store, 39.6% of top-K slots.

## Synthetic Personal-Data Pipeline (Detail)

### Four Stages
1. **Life-state schedule**: deterministic daily schedule (asleep, commuting, focus time, free time, weekend). State modulates baseline arrival intensity of every modality.
2. **Multivariate Hawkes process**: 6 modalities (email, meeting, document, query, message, ambient). Each event triggers causally related follow-on events. Branching matrix sparse by design. Email self-excitation 0.40 (Enron reply rate 45.6%, adjusted down). Message self-excitation 0.60 (thread lengths, adjusted down).
3. **Event planning**: scenario planner constructs narrative arcs from social graph (5-person close circle, 15-person regular contact layer; 58% of social energy to closest 5).
4. **Content generation**: modality-specific LLM generators. 60% of emails noise, 70% of documents noise, 85 personal messages/day, 245 workplace messages/day, 3.4 meetings/day.

### Calibration Sources
- Enron corpus reply rate (Fox et al., 2016): 45.6%
- Reply time median 47 min (Kooti et al., 2015)
- 50% of corporate emails non-actionable (Sappelli et al., 2016)
- 68% of enterprise data never used after creation (Seagate, 2020)
- 153 Teams messages/workday received, 92 Slack messages/day sent (Microsoft 2025, Lee 2025)
- 17.1 meetings/week, 27.3% unplanned (Reclaim.ai, 2024)
- Personal desktop search: 6.6% same-day, 21.9% within week, 45.9% within month (Dumais et al., 2015)
- Collective memory: fast exponential ~10 days, then power-law tail (Igarashi et al., 2022)

## Evaluation Results (Detail)

### Accuracy
- KG-filtered search: +13 pp judged accuracy over ANN-only
- Matches Graphiti (state-of-the-art plaintext agentic memory) — 60.5% vs 56.9% on Vertex
- Largest gains on temporal (23-24 pp), person (27-30 pp), modality (16-20 pp) questions
- Prior memory benchmarks place frontier accuracy at 50-70%; Opal matches despite harder multi-modal setting

### Bandwidth
- At 524K entries: Opal 1.71 MiB/query, 0.09 MiB/ingest; In-Memory 4.55 GiB/query, 9.1 GiB/ingest
- 12-2,700× less query bandwidth across 1K-524K sweep

### Throughput
- Opal: 16.41 ingests/s, 1.89 queries/s
- In-Memory: 0.557 ingests/s, 0.146 queries/s
- 29.5× and 13.0× higher respectively
- Users per enclave: 1,932 (Opal) vs 67.0 (In-Memory)

### Cost (at 1M users)
- Infrastructure: $1.40M (Opal) vs $21.13M (In-Memory) — 15.0× lower
- Total (incl model/API): $5.21M vs $24.93M — 4.79× lower

### Latency
- Query: 2.32s (Opal) vs 1.48s (Plaintext) vs 6.78s (In-Memory) — 1.57× overhead, 2.92× faster than secure baseline
- Ingest: 0.94s vs 0.48s vs 8.51s — 1.96× slower, 9.05× faster

### Oblivious Dreaming (3-year replay)
- Active entries rise to capacity, remain stable
- ORAM stash bounded (max 70 blocks)
- Sleepy rebalancing: 50.6% vs 50.5% (eager LIRE) — matches non-oblivious baseline
- 7.33 pp gap vs no-expiration control: 5.88 pp from deleting old chunks, 1.45 pp from ANN index drift

## Deployment

All enclaves co-located on US East host with Tinfoil Intel TDX confidential VMs. Opal runs in Tinfoil Container (4 vCPU, 4 GB RAM) with NVMe drives. LLM (gpt-oss-20b) and embedding (nomic-embed-text) enclaves on NVIDIA B200 GPU with confidential computing. Client on GCP n2-standard-8 in us-central1-b (Iowa), cross-country WAN.

## Corroborating Sources

- **Google Research — "Toward provably private insights into AI use" (Oct 2025)** — Confidential federated analytics (CFA) with LLM-powered structured summarization + differential privacy + TEEs. Open-sourced in Google Parfait. Deployed in Recorder app on Pixel with Gemma 3 4B. User-level ε = 1. Corroborates the TEE + DP + LLM-in-enclave pattern.
- **MIT News — "Enabling privacy-preserving AI training on everyday devices" (April 2026)** — FTTE (Federated Tiny Training Engine): 81% faster training, 80% less on-device memory, 69% less communication. Semi-asynchronous approach for heterogeneous devices. Corroborates the feasibility of privacy-preserving AI on resource-constrained edge devices.
- **Akhmetov et al. — "Personalized Federated Learning for Sovereign Personal AI Agents: A Review" (IEEE, 2026)** — PFL + LLMs + privacy-preserving technologies for sovereign personal AI agents. LoRA adapters, knowledge graphs, DP, secure aggregation. "Sovereign Data Ecosystem" where users retain personal data locally while contributing to global model improvements. Corroborates the sovereign-personal-AI direction.
- **Trncik — "Cognitive Understanding Architecture (CUA) Technical Specification v1.2" (Zenodo, 2026)** — Five-tier memory architecture (Working, Episodic, Semantic, Collective, Meta) with dual-pathway learning: R1 collective learning under (ε,δ)-Replace-One DP with Rényi DP composition, R2 per-user adaptation via LoRA adapters. Three deployment architectures (Edge-Local, TEE-Assisted, Server-Side). Corroborates the TEE-assisted personal AI memory direction.

## Grep-Confirmation of Novelty

Grep across `/home/user/.skills` for "ORAM", "oblivious", "access pattern", "Opal", "TEE" returned no matches in the context of private memory architecture. The ORAM + TEE + KG-filtered search + oblivious dreaming combination is not captured by any existing skill. Adjacent skills:
- `ai-agent-memory-architecture` — infrastructure layer (benchmarks, multi-signal retrieval); no privacy/access-pattern-hiding
- `privacy-preserving-ai-attribution-framework` — federated learning + DP + HE for attribution; different problem
- `federated-learning-for-privacy-preserving-ai` — federated learning; different threat model
- `local-first-web-architecture-2026` — local-first principles; no ORAM/TEE
- `post-quantum-privacy-architecture` — post-quantum crypto; different paradigm

This skill provides the privacy layer (ORAM + TEE + KG-filtered search + oblivious dreaming) that sits on top of the memory infrastructure layer.