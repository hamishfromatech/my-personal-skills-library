# PPC Evidence Base

## Source
Khanzadeh, S., Platnick, D., Alirezaie, M., Rahnama, H. (2026). "Share No More Than the Request Requires: Federated Disclosure for Perspective-Aware AI." ACM AISec 2026 (19th ACM Workshop on Artificial Intelligence and Security), The Hague, Netherlands. arXiv:2607.22953.

## Formal Model Summary

### Chronicle Definition
A Chronicle for entity u is the time-ordered sequence of Situation Graph snapshots:
```
𝒞_u = ⟨G_{t1}, G_{t2}, ..., G_{tn}⟩  with t1 < t2 < ... < tn
```
Each G_{ti} = (V_{ti}, E_{ti}) where:
- V_{ti} holds entities, attributes, contextual objects
- E_{ti} ⊆ V_{ti} × 𝒫 × V_{ti} holds directed predicate-labeled edges (s, p, o) with p ∈ 𝒫 (domain ontology)
- A distinguished situation node s_{ti} anchors the moment

Consolidated view: G_{𝒞_u} = ⊕_{i=1}^{n} G_{ti} (provenance-preserving merge with entity alignment; co-referent nodes unified; every node/edge keeps time index + provenance pointer).

### Authorization Function
```
𝒜: Rel × Purp × (∪_i V_{ti} ∪ ∪_i E_{ti}) → {permit, deny}
```
- Rel: requester–holder relationship space (treating physician, insurance auditor, opposing counsel)
- Purp: stated purpose space (drug_interaction_assessment, discovery, etc.)
- Evaluates locally at holder against policies typed by domain-expert ontology
- Driven by GDPR Art. 5(1)(b) purpose limitation + HIPAA relationship-based access

### Authorized Evidence Subgraph (Eq. 1)
```
min |S|  s.t.
  (i)   S ⊆ G_{𝒞_u}
  (ii)  ∀ x ∈ V_S ∪ E_S: 𝒜(r,p,x) = permit
  (iii) Sufficient(S, Q)
```

### Sufficiency Validator (Algorithm 1) — decides constraint (iii); does NOT search
1. Check every element passes 𝒜 (auth leak → false)
2. For each information need i_j: check PathCov (directed walk matching path template T_j up to ontology subsumption)
3. Check PredComplete (predicate-type counts meet req(τ, Q))
4. Check connectivity (Paths(S, 𝒜) not disconnected)
5. Check ProvComplete (every element has provenance pointer)
6. If LLM consumer + local 𝒞_a available: check distributional refinement D(π_S || π_a) ≤ ε
7. Else: return structural result

### Two-Phase Protocol
**Phase 1**: Request → Request planning → Local compilation (auth filter → relevance retrieval → minimality pruning) → Assembly → Delivery (text + provenance refs)
**Phase 2**: Artifact release (re-evaluate 𝒜, add artifact-release policy, explicit per-artifact holder approval)

## Medical Domain Worked Example

Patient Alice, holder u. Dr. Chen (treating cardiologist, Hospital A) requests drug-interaction assessment.

**Policy rules (R1–R6)**:
- R1: user_type=treating_specialist ∧ purpose=drug_interaction_assessment → evaluate
- R2: predicate_category=billing → deny
- R3: predicate_category=session_note → deny
- R4: predicate_category=diagnosis ∧ subcategory=psychiatric → deny
- R5: predicate_category ∈ {prescription, allergy} → permit
- R6: predicate_category=diagnosis ∧ subcategory=cardiac → permit

**Chronicle fragment G_HospA** (10 triples):
- (s_Jun24, situationOf, Alice) — context — permit
- (Alice, hasDiagnosis, AFib) — diagnosis/cardiac — permit
- (Alice, hasDiagnosis, MDD) — diagnosis/psychiatric — DENY (R4)
- (Alice, prescribed, Warfarin) — prescription — permit
- (Warfarin, hasDosage, 5mg) — prescription — permit
- (Alice, prescribed, Amiodarone) — prescription — permit
- (Amiodarone, hasDosage, 200mg) — prescription — permit
- (Alice, hasAllergy, Penicillin) — allergy — permit
- (Alice, attendedSession, Note447) — session_note — DENY (R3)
- (Alice, billed, Claim8821) — billing — DENY (R2)

**Compilation**:
- (a) Auth filter: remove MDD, Note447, Claim8821 → 7 edges remain
- (b) Relevance retrieval: match i_1 (⟨prescribed, hasDosage⟩) to both chains; i_2 (⟨hasAllergy⟩) to allergy triple
- (c) Structural sufficiency: 2 prescription walks + 1 allergy walk; PredComplete on Π(Q)
- (d) Minimality pruning: drop authorized AFib diagnosis (not matched by path template); retain s_Jun24 as minimal provenance connector

**Result S*** (6 triples released):
1. (s_Jun24, situationOf, Alice)
2. (Alice, prescribed, Warfarin)
3. (Warfarin, hasDosage, 5mg)
4. (Alice, prescribed, Amiodarone)
5. (Amiodarone, hasDosage, 200mg)
6. (Alice, hasAllergy, Penicillin)

**Key tension surfaced**: Dropping AFib is structurally minimal but clinically insufficient — AFib is why amiodarone was prescribed and shifts anticoagulation reasoning. The "smallest sufficient" set is only as good as the information-need decomposition 𝒜(Q). This is the open question, not a settled default.

## Litigation Domain Worked Example

Opposing counsel (user type opposing_counsel, purpose discovery) requests evidence from corporation's Chronicle about Contract X.

**Ontology**: contract_term, communication, internal_memo, financial_record, privileged (attorney–client)

**Request planning**: "communications concerning Contract X in discovery window" → needs:
- i_1: ⟨governs, referencedIn⟩ (contract → messages referencing it)
- i_2: ⟨sentOn⟩ (bound messages to date range)

**Policy**: opens contract terms + non-privileged communications in window; denies attorney–client material + out-of-scope memos. Privilege behaves like inference closure: a memo quoting privileged advice must be denied even though its predicate type is communication.

**Same tension**: a single contract clause pulled free of its surrounding negotiation thread can be technically responsive yet misleading. "Smallest sufficient" for fair discovery may be larger than literal template match returns.

## Capability Comparison (Table 1)

| System | Hold.Sov. | Ledger-free | Chronicle | Purpose | Relation | Min.Sub. |
|---|---|---|---|---|---|---|
| Solid | ✓ | ✓ | × | ~ | ~ | × |
| MedRec | ~ | × | × | ~ | ~ | × |
| Secret Network | × | × | × | × | × | × |
| IPFS / libp2p | ~ | ✓ | × | × | × | × |
| Secure Scuttlebutt | ✓ | ✓ | × | × | × | × |
| **PPC** | **✓** | **✓** | **✓** | **✓** | **✓** | **✓** |

## Four Notions of Sufficiency

1. **Structural sufficiency** — path coverage, predicate-type completeness, connectivity, provenance (Algorithm 1). The only tier the protocol guarantees.
2. **Task sufficiency** — does S* let the consumer complete its task?
3. **Clinical/legal sufficiency** — domain-competence + fairness norms (AFib indication, discovery negotiation thread)
4. **LLM-answer sufficiency** — distributional tier: consumer's answer distribution over S* tracks that over full authorized view 𝒞_a

Structural sufficiency lower-bounds the others without implying them. Closing the gap is the open question.

## Related Constructs

- **Situation Graphs** (PAi): snapshot one situated moment as predicate-labeled knowledge graph
- **Chronicles** (PAi): longitudinal knowledge graph from compiled Situation Graphs; powers agents in social simulation and extended reality
- **Solid pods** (Berners-Lee): holder-local storage + WebID access control — but no Chronicle semantics, no exchange-time minimization
- **Verifiable Credentials** (W3C): bind identity/role/affiliation for cross-domain trust — PPC uses these for the request VP, complementary not replacement
- **Hippocratic databases** (Agrawal 2002): purpose as first-class DBMS element — PPC generalizes to temporal, predicate-labeled graphs
- **Contextual integrity** (Nissenbaum 2004): information flows appropriate when conforming to context-relative transmission principles — PPC is the compile-time counterpart for structured identity
- **AirGapAgent** (Bagdasarian 2024): restricts conversational agent's working context to task-necessary data — PPC is the compile-time analog for structured Chronicles

## Open Problems

1. Cross-holder entity alignment (privacy-preserving record-linkage integration with Chronicle structure)
2. Formalizing the compilation problem (efficient approximation of Eq. 1)
3. Prompt injection into the planner (malicious Q steering 𝒜(Q) over-broad)
4. Provenance-reference leakage (proving IDs reveal nothing without authorized Phase-2 release)
5. Closing the structural→task→clinical→LLM sufficiency gap
6. Byzantine coordinator (current model assumes honest-but-curious)