---
name: opal-private-memory-architecture
description: How to build private long-term memory for personal AI agents using oblivious RAM (ORAM) and trusted execution environments (TEEs) — hiding both content and access patterns from the application provider. Covers KG-filtered search, oblivious dreaming, and the privacy-by-architecture pattern that makes personal AI memory tractable at scale. Use when designing privacy-first AI memory systems, evaluating confidential computing for AI, or building personal AI agents that retain user data without surveillance. NOT for general agent memory architecture (see ai-agent-memory-architecture) or federated learning (see privacy-and-trust skills).
---

# Opal: Private Memory Architecture for Personal AI

## The Core Problem

Personal AI systems increasingly retain long-term memory of user activity — documents, emails, messages, meetings, ambient recordings. This creates an unprecedented concentration of personal data at providers whose profiles of user behavior can be shared with third parties, retained for AI training, or compelled by governments.

End-to-end encrypted messaging established private communication as a baseline user expectation. Yet personal AI systems, which accumulate far richer portraits of their users than any messaging app, offer no comparable guarantee.

## The Three Approaches and Their Limits

1. **Trusted hardware for private inference** — Apple, Google, Anthropic now deploy TEEs to keep inference queries hidden. But TEE memory is finite and expensive, driving systems toward persistent long-term memory on untrusted storage. This exposes **retrieval access patterns** that leak private information — observing which memories a system retrieves is nearly as revealing as seeing the query text itself, since retrieved items are semantically similar to the query.

2. **Client-local memory** — Store personal data on the user's device, re-upload relevant context on every request. But personal corpora quickly outgrow a single device, local storage lacks durability for a long-lived memory store, and multi-device access demands a unified backend. Production assistants therefore store memory cloud-natively.

3. **ORAM for access-pattern hiding** — Oblivious RAM hides data access patterns at per-access cost logarithmic in database size. But agentic memory systems achieve high accuracy precisely because they augment semantic search with **query-dependent traversals** (knowledge graphs, temporal filters, multi-hop lookups). ORAM enforces a fixed access budget, precluding these traversals. Efficient obliviousness and accuracy appear fundamentally at odds.

## The Opal Insight

**Decouple all data-dependent reasoning from the bulk of personal data, confining it to the trusted enclave.** Untrusted disk then sees only fixed, oblivious memory accesses.

The enclave-resident component uses a lightweight knowledge graph to capture personal context that semantic search alone misses, and handles continuous ingestion by piggybacking reindexing and capacity management on every ORAM access.

## The Three Novel Contributions

### 1. Knowledge-Graph-Filtered Search

Pure semantic search is insufficient for personal data. Queries like "what did Alice say in last Monday's meeting?" depend on temporal context and entity relationships that embedding similarity alone cannot capture.

**The departure**: instead of a content-rich graph (which produces data-dependent traversals incompatible with ORAM's fixed access budget), maintain a **compact, metadata-only knowledge graph of normalized identifiers** (people, sources, time ranges) that fits entirely inside the enclave and stores no content.

Five node types:
- **Artifact** — logical memory unit (email, meeting, document), with timestamp and modality label
- **Chunk** — identifier for fixed-size content piece; content resides in ORAM
- **Summary** — consolidated digest from periodic summarization
- **Person** — named individuals (email senders, meeting attendees)
- **Project** — workstreams (folder hierarchies, project spaces, recurring meetings)

Four filtration types:
- **Temporal** — resolve date/interval/relative-time expressions into time windows
- **Modality** — scope to source type ("in my emails")
- **Person** — scope to named individual
- **Project** — scope to workstream tags

Query-time traversal: extract structured predicates inside enclave → traverse KG to form admissible set → ANN scores only within that set → single fixed-budget ORAM fetch. All query-dependent reasoning stays inside the enclave; storage provider sees only fixed-size, oblivious access pattern.

### 2. Oblivious Dreaming

Agentic memory systems perform heavy maintenance (compaction, conflict resolution, index repair, summarization) whose access patterns would leak information about the user's data. A fixed maintenance schedule either scans the entire store (expensive) or leaves stale state.

**Oblivious dreaming**: the `Dream()` callback executes inside every batched ORAM access, inspecting blocks already in the stash and updating enclave-resident metadata — **without issuing any extra access**. Like biological memory consolidating during sleep.

Three domains:
- **Adaptive retention** — TTL measured in logical operations; each query access refreshes TTL (LRU-like behavior). Targets steady-state store size; expired items deleted opportunistically as encountered in stash.
- **Sleepy rebalancing** — IVF cluster drift from continuous insertions/deletions. Split/merge clusters without fetching full vectors (reconstruct approximate member vectors from PQ residual codes). Corrections applied lazily during ordinary ORAM accesses.
- **Memory compression** — periodically compress recent chunks into denser summary records. Every T ingestions, retrieve recent chunks, summarize, re-ingest through ordinary ingestion path. No dedicated ORAM fetch.

### 3. Synthetic Personal-Data Pipeline

Real personal data is too sensitive to collect or share at scale. Existing benchmarks generate isolated artifacts but lack temporal dynamics essential for memory evaluation.

Four-stage pipeline producing years of multimodal activity:
1. **Life-state schedule** — deterministic daily schedule modulating arrival intensity by behavioral state
2. **Multivariate Hawkes process** — self-exciting event streams across 6 modalities (email, meeting, document, query, message, ambient); cross-modal excitation (meeting triggers follow-up emails)
3. **Event planning** — scenario planner constructs narrative arcs from social graph
4. **Content generation** — modality-specific LLM generators realize artifacts; scenario-backed events remain causally consistent across modalities

## Evaluation Results

- **Accuracy**: KG-filtered search improves judged accuracy by 13 percentage points over plain ANN, matching Graphiti (state-of-the-art plaintext agentic memory). On Vertex, Opal reaches 60.5% judged accuracy, surpassing Graphiti (56.9%).
- **Throughput**: 29× higher than secure baseline (In-Memory Opal). 16.41 ingests/s, 1.89 queries/s.
- **Cost**: 15× lower infrastructure cost at 1M users ($1.40M vs $21.13M annually).
- **Latency**: Query 2.32s (2.92× faster than In-Memory); Ingest 0.94s (9.05× faster).
- **Bandwidth**: 12–2,700× less query bandwidth than In-Memory baseline across 1K–524K entries.
- **Oblivious dreaming**: Tracks eager LIRE baseline closely (50.6% vs 50.5% judged accuracy). Memory compression effective — summaries are 16.7% of store but fill 39.6% of top-K slots.
- **Deployment**: Under consideration for deployment to millions of users at a major AI provider.

## The Security Guarantee

Indistinguishability-based security definition: for any PPT adversary controlling all computation/storage outside enclave boundaries, the adversary learns neither content of any item/query/response, nor which stored data is relevant to a query, nor which records were accessed. The adversary may learn only whether an operation is Query or Ingest, plus the fixed trace shape (number of fixed-size inter-enclave messages and ORAM batch sizes).

## A-Tech Values Alignment

| Value | Alignment |
|---|---|
| **Open-source AI** | Open-source Gemma models used as "data expert" LLM; ORAM and TEE stacks are open-source; synthetic data pipeline is reproducible |
| **Data privacy** | Core principle — content AND access patterns hidden from provider; no biometric surveillance; user data never leaves enclave boundary in plaintext |
| **Financial freedom** | 15× lower infrastructure cost at scale makes privacy-first memory economically viable, not just ethically preferable; private memory is the appreciating asset that compounds with use |
| **Practical implementation** | Three concrete contributions (KG-filtered search, oblivious dreaming, synthetic pipeline) with quantified results; deployment-ready architecture |

## Practical Implementation for A-Tech

### A-Coder
- The KG-filtered search pattern applies to codebase memory: personal context (which files, which projects, which collaborators) can be captured in metadata-only graphs without exposing code content.
- Oblivious dreaming applies to any persistent memory store — maintenance happens without leaking what's being maintained.

### Be Practical
- The privacy-by-architecture pattern (decouple data-dependent reasoning into enclave; expose only oblivious accesses) is a teachable design principle for privacy-first systems.
- The synthetic personal-data pipeline methodology is independently valuable for evaluating any memory system without real user data.

### Builder's Club
- Open-source reference architecture for privacy-first personal AI memory — community-contributable, auditable.
- The "memory is the asset that compounds; the model is the commodity that depreciates" thesis directly informs Builder's Club marketplace design.

## Cross-References to Existing Skills

- **`ai-agent-memory-architecture`** — The infrastructure layer (benchmarks, multi-signal retrieval, OpenMemory MCP). Opal is the privacy layer on top of that infrastructure — complementary, not overlapping.
- **`privacy-preserving-ai-attribution-framework`** — Attribution framework uses federated learning + DP + HE. Opal uses ORAM + TEE. Different privacy-preserving techniques for different problems (attribution vs memory).
- **`federated-learning-for-privacy-preserving-ai`** — Federated learning keeps raw data at source. Opal keeps data in encrypted ORAM on untrusted storage. Different threat models, complementary approaches.
- **`local-first-web-architecture-2026`** — Local-first principles. Opal is the cloud-hosted version that achieves local-first privacy guarantees despite cloud storage.
- **`privacy-first-ai-pipeline-defense`** — Pipeline defense. Opal is the memory-specific privacy architecture.
- **`post-quantum-privacy-architecture`** — Post-quantum cryptography. Opal's ORAM + TEE approach is a different privacy-preserving paradigm.

## Anti-Patterns

- **Trusting TEEs alone for private memory** — TEEs protect inference but not persistent memory; access patterns on untrusted storage leak which memories are relevant to each query.
- **Content-rich knowledge graphs with ORAM** — Data-dependent traversals are incompatible with ORAM's fixed access budget. Use metadata-only graphs.
- **Fixed maintenance schedules** — Either scan the entire store (expensive) or leave stale state. Use oblivious dreaming to piggyback maintenance on ordinary accesses.
- **Treating closed models as more secure than open** — The Okta 2026 CVE analysis showed authorized retrieval reaching unauthorized recipients in closed systems (Anthropic Slack MCP, MS Copilot EchoLeak, Salesforce ForcedLeak, ServiceNow BodySnatcher — all CVSS 9.3–9.4, all closed systems). Secrecy of the weights did not prevent it.
- **Client-local memory as the privacy solution** — Personal corpora outgrow a single device; local storage lacks durability; multi-device access demands a unified backend.

## The Key Insight

The model is interchangeable. The memory is the asset. A rented model can be deprecated by its lab. A memory held on the enterprise side of the firewall cannot — and it cannot be re-acquired by switching vendors. Privacy-first memory architecture is not just ethical; it is the economic moat that compounds with every interaction while the model layer commoditizes toward zero.