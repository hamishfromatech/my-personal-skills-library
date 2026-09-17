---
name: provenance-preserving-chronicles-minimum-disclosure
description: Apply minimum-necessary disclosure for perspective-aware AI Chronicles — temporal, predicate-labeled knowledge graphs representing a user's lived context. Use when designing federated agent identity exchange, selective disclosure across autonomous agents, relationship-and-purpose-aware data minimization, or privacy-preserving personal-context sharing. NOT for generic access control, document-level permissions, or static data stores without temporal Chronicle semantics.
---

# Provenance Preserving Chronicles (PPC) — Minimum-Necessary Disclosure

## Core Principle

**Share no more than the request requires.**

PPC compiles each holder's Chronicle (a temporal sequence of predicate-labeled Situation Graphs) into a compact *authorized evidence subgraph* governed by one rule: release the smallest sufficient fragment for a given requester relationship, stated purpose, and specific task.

This is the compile-time counterpart to contextual integrity: the transmission principle (relationship + purpose) is evaluated *before* anything crosses the holder boundary, over temporal, predicate-labeled structure. "Task-necessary" is enforced as a subgraph-minimization objective, not a prompt-time heuristic.

## When to Use

- Designing federated agent networks where one node's agent queries another user's Chronicle without centralizing data
- Building privacy-preserving personal-context exchange (medical, legal, financial)
- Implementing GDPR Art. 5(1)(c) data minimisation or HIPAA minimum-necessary standards at exchange time
- Creating relationship-aware access control that goes beyond role-based or attribute-based models
- Designing two-phase disclosure: provenance-linked text first, raw artifacts only after explicit holder approval

## The Three Constructs

### 1. Chronicle (𝒞_u)
A time-ordered sequence of Situation Graphs for entity u:
```
𝒞_u = ⟨G_{t1}, G_{t2}, ..., G_{tn}⟩
```
Each G_{ti} is a predicate-labeled knowledge graph anchoring one situated moment. Consolidation yields a single temporal, provenance-annotated identity graph — not a bag of disconnected snapshots.

### 2. Authorization Function (𝒜)
```
𝒜: Rel × Purp × (V ∪ E) → {permit, deny}
```
Takes a requester–holder relationship r (e.g., treating physician, insurance auditor), a stated purpose p (e.g., drug_interaction_assessment), and one node or edge from the Chronicle. Decides: for this requester, under this purpose, is it allowed out? Evaluation runs locally at the holder against policies typed by a domain-expert ontology.

Two regulatory principles drive the function:
- **Purpose limitation** (GDPR Art. 5(1)(b), HIPAA TPO): data collected for p1 may only be disclosed for p2 if p2 is compatible
- **Relationship-based access**: the requester's relationship maps to a user type that gates which predicate categories are in bounds

### 3. Authorized Evidence Subgraph (S*)
```
min |S|  s.t.
  (i)   S ⊆ G_{𝒞_u}                    (drawn from Chronicle)
  (ii)  ∀ x ∈ V_S ∪ E_S: 𝒜(r,p,x)=permit  (authorization)
  (iii) Sufficient(S, Q)                (sufficiency for request Q)
```

Sufficiency has two tiers:
- **Structural baseline** (guaranteed): path coverage, predicate-type completeness, connectivity, provenance — decidable and enforced by the protocol
- **Distributional refinement** (aspirational): the consumer's answer distribution over S* tracks that over the full authorized view — a research target, not a shipped mechanism

## The Two-Phase Protocol

### Phase 1: Authorized Evidence Compilation
1. **Request**: requester's agent sends (Q, VP) — request + verifiable presentation binding identity/role/affiliation to a DID
2. **Request planning**: planner decomposes Q into schema-level information needs (path templates from ontology). Holders receive patterns, not instance-level content — the ask itself does not leak.
3. **Local compilation** (per holder):
   - (a) Authorization filtering → 𝒞_{i,a}
   - (b) Relevance retrieval (entity linking, embedding similarity, temporal traversal)
   - (c) Minimality pruning (e.g., Prize-Collecting Steiner Tree with authorization-weighted penalties)
4. **Assembly**: coordinator merges {S_1...S_k}, resolves entity alignment, re-runs sufficiency validator on assembled S*
5. **Delivery**: compiled evidence returned with provenance references; if consumer is LLM-based, S* may serialize into a grounded prompt

### Phase 2: Artifact Release (Holder-Approved)
- Phase 1 returns text + provenance pointers, NOT raw artifacts
- Follow-up references provenance IDs from Phase 1
- Re-evaluates 𝒜 on artifact node + any new edges (Phase-1 auth necessary but not sufficient)
- May add artifact-release policy (higher user-type bar)
- ALWAYS requires explicit, per-artifact holder approval (mandatory human gate)

## Domain Instantiation (3 Knobs)

The same protocol plugs into different regulated domains by swapping:
1. **Domain-expert ontology** (predicate types: prescription, allergy, diagnosis/cardiac, diagnosis/psychiatric, session_note, billing)
2. **User-type taxonomy** (treating_specialist, insurance_auditor, opposing_counsel)
3. **Disclosure policy** (which predicates each user type may see under each purpose)

### Medical Example
- Patient Alice's Chronicle spans fragments at multiple hospitals
- Dr. Chen (treating cardiologist) requests drug-interaction assessment
- Policy: permit prescription/allergy/cardiac-diagnosis; deny psychiatric/billing/session_note
- Compilation: two prescription chains + one allergy edge; AFib diagnosis permitted but pruned (not matched by path template) — retained only as minimal provenance connector
- Result: 6 triples released; psychiatric and billing data never cross the boundary

## Threat Model

**In scope:**
- Malicious requester exceeding authorization → access controller enforces 𝒜 at holder boundary
- Consumer inference threats → inference closure (deny edges that enable high-confidence inference of denied nodes), decoupled disclosure (policy at compile time, not post-hoc redaction)
- Prompt injection into LLM-based consumer → two-phase flow limits blast radius

**Out of scope (assumptions/open problems):**
- Honest-but-curious coordinator (Byzantine coordinator = future work)
- Forged credentials (assumes VP/DID layer is sound)
- Holder collusion to reconstruct denied data
- Provenance-reference leakage (treat IDs as opaque capability handles)
- Prompt injection into the planner (malicious Q steering 𝒜(Q) over-broad)

## Key Tensions (Surfaced, Not Resolved)

**Minimality vs. utility**: aggressive minimization optimizes for disclosure safety but can strip context a competent consumer needs. The AFib indication is clinically load-bearing for anticoagulation reasoning, yet a structural minimizer drops it. Four notions of sufficiency:
- Structural sufficiency (guaranteed by protocol)
- Task sufficiency (does S* let consumer complete its task?)
- Clinical/legal sufficiency (domain-competence + fairness norms)
- LLM-answer sufficiency (distributional tier)

Structural sufficiency lower-bounds the others without implying them. Closing that gap is the open question.

## A-Tech Value Alignment

| Value | Alignment |
|---|---|
| Open-source AI | Protocol is open; ontologies and policies are inspectable; no black-box access decisions |
| Data privacy | Holder sovereignty by construction; minimum-necessary is the optimization objective, not an afterthought; two-phase flow limits blast radius |
| Financial freedom | Reduces legal/compliance cost of cross-organization data exchange; enables new classes of privacy-preserving agent services |
| Practical implementation | Reference protocol with concrete algorithms (Prize-Collecting Steiner Tree, ontology-typed policies); instantiable in medical/legal domains by swapping 3 knobs |

## Implementation Notes

- **Ontology**: predicate types come from a domain-expert ontology (medical SNOMED/ICD, legal contract/communication/privileged)
- **Policy language**: realizable as ontology-typed, context-aware policy in the Semantic Web tradition (ODRL, WAC, SPARQL CONSTRUCT/DESCRIBE)
- **Subgraph solver**: approximates Eq. (1) — call the output *compact*, not provably smallest; the validator (Algorithm 1) decides sufficiency, the compiler searches
- **Cross-holder entity alignment**: assumes privacy-preserving record-linkage primitive (e.g., Bloom-filter matching); integration with predicate-rich temporal Chronicle structure is an open problem
- **Blockchain for audit**: PPC can adopt a ledger for tamper-evident logging of consent decisions, but disclosure itself must NOT run over global consensus

## References

See `references/ppc-evidence-base.md` for the full formal model, sufficiency validator pseudocode, worked medical and litigation examples, and capability comparison table across Solid/MedRec/Secret Network/IPFS/Secure Scuttlebutt.