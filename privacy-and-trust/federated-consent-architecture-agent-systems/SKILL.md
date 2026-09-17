---
name: federated-consent-architecture-agent-systems
description: A privacy architecture for cross-tenant federated learning in multi-agent systems that separates agent communication from learning coordination, enforcing consent propagation, secure aggregation, differential privacy, and privacy accounting across training rounds. Use when designing AI agent systems that learn from distributed operational data across tenants, when building privacy-preserving agent training pipelines, or when compliance requires verifiable consent and audit trails for federated model training (A-Coder enterprise licensing, Be Practical federated curriculum, Builder's Club cross-org learning).
---

# Federated Consent Architecture for Agent Systems

## Why This Skill Exists

Agent systems increasingly operate across tenants, organizations, and trust domains. Agents produce operational traces — tool invocation outcomes, retrieval quality signals, planner decisions, task completion signals, safety interventions, user feedback — that can improve later behavior. But centralizing these traces conflicts with tenant isolation, data protection law, and enterprise security policy. This skill defines how to learn from distributed agent data *without* centralizing it, while maintaining verifiable consent, audit trails, and privacy guarantees.

The architecture draws from the IETF Internet-Draft "Privacy-Preserving Federated Learning for Agent Systems" (draft-kale-agntcy-federated-privacy-02, July 2026) and the federated consent system pattern for training data provenance. It is scoped to cross-tenant and cross-organization settings.

## The Core Principle

**Separate agent communication from learning coordination.** Existing agent protocols (MCP, A2A, SLIM, ADS) provide discovery, messaging, authentication, and transport. The learning layer adds: cohort formation, update submission, secure aggregation, differential privacy, privacy accounting, auditability, and model distribution. The learning layer does not define a new agent protocol — it defines metadata and requirements that any carrying protocol must represent.

## Architecture Roles

| Role | Responsibility |
|------|---------------|
| **Tenant Participant** | Performs local training on tenant-controlled data; submits updates or shares |
| **Coordinator** | Creates learning tasks, selects cohorts, distributes model parameters, maintains task state |
| **Aggregation Service** | Aggregates participant updates; sees only the aggregate, never individual updates |
| **Privacy Accountant** | Tracks privacy loss across training rounds and model releases |
| **Model Registry** | Stores model versions and release metadata |
| **Audit Log** | Records task config, round state, participant events, privacy accounting, release decisions |
| **Policy Authority** | Defines tenant eligibility, data-use constraints, privacy limits, release rules |
| **Verifier** | Optionally evaluates attestation evidence; optionally checks aggregation integrity |

Roles can be separate services or combined. Combining roles simplifies operations but changes the trust model — document which roles are combined and why.

## The Learning Task Configuration

Every learning task MUST define:

- Task identifier and purpose
- Model being trained + initial model version
- Allowed participant population
- **Privacy unit** (record, user, session, device, tenant, or organization)
- Privacy budget (epsilon, delta) and accounting method
- Update type (full gradient, full parameters, statistics, or LoRA adapter)
- Clipping rule
- Noise mechanism (if DP used)
- Secure aggregation method (if used)
- Minimum cohort size
- Maximum number of rounds
- Update schema
- Model release criteria
- Retention policy for updates, aggregates, logs, checkpoints

Participants MUST be able to inspect the task configuration before contributing. A participant MUST reject a task if the configuration is missing required fields or conflicts with local policy.

## Training Round Flow

1. Coordinator creates a round for the configured learning task
2. Coordinator selects eligible participants
3. Each participant authenticates the coordinator and verifies the task configuration
4. Coordinator sends current model version + round metadata
5. Each participant trains locally using data permitted by local policy
6. Each participant clips its update per task configuration
7. Each participant applies configured privacy mechanism (if required at participant)
8. Each participant submits update (or share of update) for aggregation
9. Aggregation service computes the aggregate
10. Privacy accountant updates privacy-loss state
11. Coordinator determines whether aggregate can update the global model
12. Model registry records new model version + release metadata

## Privacy Mechanisms

### Secure Aggregation (RECOMMENDED for cross-tenant)
The aggregation service learns only an aggregate value for a valid cohort — never individual updates. Properties:
- Participant update bound to one task, one round, one model version
- Failed participant does not cause disclosure of another's update
- Protocol handles dropout within configured limits
- Rejects replayed or duplicated updates
- Authenticates participants and aggregation messages

**Critical caveat**: secure aggregation alone does NOT provide differential privacy. The aggregate can still leak, especially with small cohorts, repeated rounds, or correlated updates. Reconstruction of individual contributions from aggregates has been demonstrated even with quantized updates when cohorts are small.

### Differential Privacy
Three deployment models:
- **Local DP**: participants perturb updates before submission (reduces trust, may reduce utility)
- **Central DP**: trusted component adds noise after aggregation (improves utility, requires trust)
- **Distributed DP**: participants add shares of noise that combine to required distribution (reduces trust, preserves utility, adds complexity)

For agent systems, **user-level or tenant-level privacy** is often more relevant than record-level. Record-level privacy understates risk when one user/tenant contributes many records.

### Privacy Accounting
Federated learning is iterative. A single round does not describe full privacy risk. The accountant MUST track:
- Task ID, model ID/version, round number, cohort size, sampling rate
- Clipping bound, noise multiplier/distribution, privacy unit, accounting method
- Cumulative privacy loss, release decision per model version

Use an accountant suitable for iterative training (Renyi DP or equivalent). Training MUST stop when the configured privacy budget is exhausted.

### Cohort Controls
Small cohorts increase disclosure risk. The coordinator MUST NOT complete a round if valid participants fall below the configured minimum. Minimum cohort size should reflect: privacy unit, update dimension, expected participant correlation, number of rounds, secure aggregation method, whether DP is applied before or after aggregation.

## Parameter-Efficient Fine-Tuning (PEFT / LoRA)

For agent systems, the model is often a large language model. Full fine-tuning across tenants is rarely practical. Updates are usually LoRA adapters, not full gradients.

**LoRA-specific concerns:**
- Clipping and noise apply to the adapter parameters actually transmitted
- Adding noise to both LoRA factors independently and averaging them does NOT produce the average of intended updates — it can amplify noise. Use aggregation that is correct for the adapter structure (freeze one factor, or aggregate the reconstructed update)
- Heterogeneous adapter ranks across participants require rank-aware aggregation — element-by-element averaging of different-rank adapters is undefined
- The privacy unit and accounting method do not change because the update is an adapter

## Agent-Specific Data Risks

Agent traces differ from ordinary telemetry:
- User prompts and requests
- Agent plans and intermediate reasoning
- Tool names, inputs, outputs
- Retrieval queries and retrieved content
- Memory reads/writes
- Safety policy decisions
- Human approval/rejection events
- Task outcome labels
- Latency, error, resource-use metrics

Some categories contain secrets, regulated data, or third-party content. **Local preprocessing SHOULD remove fields not required for the learning task. Tool outputs and retrieved content SHOULD NOT be included unless the task has explicit policy allowing that use.**

### Prompt Injection and Tool Poisoning
Agent traces can be influenced by prompt injection, malicious documents, compromised tools, and adversarial retrieval content. If those traces are used for learning, the attack moves from one tenant into a shared model. Mitigations:
- Separate trusted labels from untrusted content
- Record provenance for training examples
- Reject training data from sources that do not meet task policy

## Consent Layer

Beyond the IETF federated learning architecture, a federated consent system propagates and enforces creator licensing and revocations across marketplaces, pipelines, and auditors.

### Consent Record Fields
- consent_id (UUID)
- subject_id (creator's DID or account ID)
- content_fingerprint (robust perceptual hash)
- scope (training, commercial-deploy, internal-research)
- duration (start, expiry, renewable)
- terms_url / license_id
- issued_at, issued_by (issuer DID), signature
- status (active, revoked, suspended)
- revocation_record (timestamp, reason, operator_id)
- audit_proof (Merkle root or ledger reference)

### Consent Tokens
Issue cryptographically signed tokens (JWT or W3C Verifiable Credential) representing consent scope and constraints. Tokens travel with datasets and models. Publish public keys at `/.well-known/jwks.json`.

### Propagation Patterns
- **PULL-first**: query consent-store before each use. Simple, latest truth, but latency/load.
- **PUSH-first**: consent stores push events to subscribed platforms. Low latency, eventual consistency.
- **Hybrid (recommended)**: PUSH events signal updates; PULL queries confirm before irreversible actions (e.g., model release).

### Revocation SLOs
- Event delivery to 99% of subscribers within 60s
- Subscribers must ACK within 120s or re-query
- Model training systems re-check consent at checkpoints and before snapshot/export

### Privacy-Preserving Auditability
- **Merkle-based proofs**: commit consent records into a Merkle tree, publish root, provide per-record inclusion proofs
- **Selective disclosure**: use Zero-Knowledge Proofs to prove a model used only data with valid consents without revealing raw content
- **Redacted logs**: store hashed identifiers in shared logs; keep re-identification keys in HSMs

## Threat Models (Three-Level Taxonomy)

| Level | Threat | Defense |
|-------|--------|---------|
| TM-1 | Honest-but-curious server (follows protocol, infers from updates) | Secure aggregation, server-side DP |
| TM-2 | Colluding clients (cooperate to infer honest clients' data) | Local DP, secret sharing (secure aggregation alone is insufficient) |
| TM-3 | Active/Byzantine adversary (submits manipulated updates) | Byzantine-robust aggregation (Krum, trimmed mean, Bulyan) |

**Critical**: applying the wrong defense for the threat model is a real and documented error. Applying Krum (TM-3 defense) in a TM-1 setting incurs up to 15% accuracy reduction for no privacy benefit. Match the defense to the threat.

### Coordinator Compromise Attack
A compromised coordinator can select a cohort where every participant except the target is controlled by the attacker. The attacker's contributions are known, so the target's contribution is recoverable by subtraction. Repeating across rounds defeats secure aggregation without breaking cryptography.

**Mitigations:**
- Cohort selection verifiable or constrained (committed random seed)
- Participants observe participant-set commitments and detect unusual cohort placement
- Sybil controls limit attacker's ability to populate cohorts
- Distributed noise ensures residuals are DP-protected even against adversarial cohort remainder

## The Tension: Update Privacy vs. Update Inspection

Two goals pull in opposite directions:
- Privacy mechanisms (secure aggregation, local DP) prevent any party from seeing individual updates
- Poisoning/backdoor defenses usually rely on inspecting individual updates for anomalies

A deployment cannot fully satisfy both with the same mechanism. Choose explicitly:
- **Prioritize privacy**: rely on secure aggregation + DP; limit poisoning defense to aggregate checks and model evaluation
- **Prioritize inspection**: allow trusted component to examine individual updates; weakens privacy, requires stronger trust assumption
- **Hybrid**: use VDAF-style validity proofs, TEE-based inspection, or anomaly checks on protected values

Record the chosen approach in the task configuration. State the residual poisoning risk.

## Deletion, Departure, and Unlearning

Retention policies govern stored artifacts. They do NOT remove a participant's influence from a trained model. When a tenant departs or invokes deletion rights:

1. **Retrain from checkpoint** predating the participant's first contribution. Only approach that fully removes influence. Cost grows with rounds since checkpoint; consumes additional privacy budget.
2. **Approximate unlearning** (FedEraser-style). Reduces but does not provably eliminate influence. Residual is hard to quantify.
3. **DP as bound**. If privacy unit is tenant and budget is meaningful, dependence is already bounded. Whether this satisfies legal obligations is a question for counsel.
4. **Contractual handling**. Participation terms state contributions to released models are not removable; departure affects future rounds only.

**The chosen approach MUST be recorded in the task configuration and disclosed to participants before enrollment.**

## A-Tech Applications

### A-Coder Enterprise Licensing
- **Privacy unit**: tenant (organization)
- **Update type**: LoRA adapter (rank-aware aggregation)
- **DP model**: distributed (participants add shares of noise)
- **Cohort minimum**: 25 tenants (configurable)
- **Consent layer**: enterprise customers consent to specific training tasks; revocation propagates via webhooks
- **Audit trail**: Merkle-based inclusion proofs for compliance
- **Threat model**: TM-1 (honest-but-curious coordinator) for enterprise; TM-2 awareness for competing tenants
- **Competitive differentiator**: verifiable consent + privacy accounting + audit trail — "your code never leaves your tenant, and we can prove it"

### Be Practical Federated Curriculum
- **Privacy unit**: learner (user-level, not record-level — one learner contributes many sessions)
- **Update type**: LoRA adapter for curriculum personalization
- **Consent layer**: learners consent to specific curriculum improvement tasks; can revoke and depart
- **Practical consideration**: retraining from checkpoint is the only full-unlearning path; disclose this before enrollment
- **Privacy-first design**: behavioral signals (progress, completion, hesitation) stay on-device; only model updates are aggregated

### Builder's Club Cross-Organization Learning
- **Privacy unit**: organization (tenant-level)
- **Consent layer**: organizations consent to community model improvement; marketplace-style consent tokens
- **Cohort controls**: minimum 10-25 organizations per round
- **Audit trail**: community governance requires transparency — release metadata available to participants
- **Threat model**: TM-2 awareness (competing organizations may collude); distributed DP + cohort controls

## Worked Privacy Accounting Example

Configuration: privacy unit = tenant; 25 tenants selected from 250 eligible (sampling rate 0.1); 100 rounds max; L2 clip bound 1.0; delta = 1e-6.

Composed epsilon at delta = 1e-6 over 100 rounds (Renyi DP accountant):
- sigma = 1.1 → epsilon ≈ 7.5 (exceeds budget of 3.0 more than twofold)
- sigma = 1.5 → epsilon ≈ 4.4
- sigma = 2.0 → epsilon ≈ 2.9 (within budget of 3.0)

**Key insight**: a noise multiplier that looks reasonable for a single round (1.1 is common in literature for one-shot training) exceeds the budget more than twofold when composed over 100 rounds. Per-round intuition is not a substitute for composed accounting.

## Implementation Checklist

- [ ] Define privacy unit before training begins
- [ ] State DP model (local, central, distributed)
- [ ] Configure privacy accountant (Renyi DP or equivalent)
- [ ] Set minimum cohort size based on privacy unit, update dimension, rounds
- [ ] Implement secure aggregation with dropout tolerance
- [ ] For LoRA: use rank-aware aggregation; do not average factors independently
- [ ] Local preprocessing removes fields not required for the learning task
- [ ] Consent tokens issued with public key endpoint
- [ ] Revocation SLOs defined and published
- [ ] Audit trail uses Merkle inclusion proofs or equivalent
- [ ] Deletion/unlearning approach recorded in task configuration and disclosed to participants
- [ ] Threat model documented (TM-1, TM-2, TM-3) with matched defenses
- [ ] Coordinator compromise mitigations in place (verifiable cohort selection, Sybil controls)
- [ ] Tension between update privacy and update inspection resolved and documented

## Complementary Skills
- `privacy-preserving-ai-attribution-framework` — attribution layer (this skill is the learning layer)
- `separable-expert-architecture-deletable-personalization` — structural unlearning via deletable proxy
- `opal-private-memory-architecture` — memory layer for private agent state
- `ai-agent-memory-architecture` — infrastructure memory for agent systems
- `federated-llm-on-device-personalization` — federated alternative for on-device personalization
- `sheld-fl-self-learning-heterogeneous-dp-framework` — DP framework for federated learning
- `eu-ai-act-developer-compliance-2026` — regulatory compliance driver