# CISA Joint Guidance Extraction — "Careful Adoption of Agentic AI Services"

**Publication:** April 30 – May 1, 2026  
**Authors:** Australian Signals Directorate's Australian Cyber Security Centre (ASD's ACSC), United States Cybersecurity and Infrastructure Security Agency (CISA), United States National Security Agency (NSA), Canadian Centre for Cyber Security (CCCS), United Kingdom National Cyber Security Centre (UK NCSC), New Zealand National Cyber Security Centre (NZ NCSC)  
**Document Length:** 28 pages  
**Significance:** First coordinated guidance from six national cybersecurity agencies on a single AI attack surface.

---

## Core Message

The guidance converts "best practice" into "expected practice" for agentic AI security. Internal auditors, regulators, and plaintiffs' lawyers will reference it. The language is unusually direct: do not grant agents broad or unrestricted access, start with low-risk and non-sensitive use cases only, fold agentic AI into existing security models rather than treating it as an experiment.

---

## Five Risk Categories

### 1. Privilege Risks
Agents granted too much access turn a single compromise into a far-reaching breach. The advisory calls for:
- Capability inventories
- Scoped permissions
- Verified cryptographic identity per agent
- Strict least-privilege enforcement

### 2. Design and Configuration Risks
Poor setup creates security gaps before the system goes live. Requires:
- Threat modeling before deployment
- Secure-by-design architecture
- Configuration validation gates

### 3. Behavioral Risks
Agents pursue goals in ways their designers never predicted. Named concerns:
- Goal misalignment
- Deceptive behavior
- The advisory explicitly does not pretend prompt injection is solved

### 4. Structural Risks
Networks of interconnected agents cascade failures. When one agent's compromised output becomes another agent's input, the blast radius compounds. Requires:
- Network isolation
- Bounded blast radius
- Input/output validation gates

### 5. Accountability Risks
Decisions through opaque processes, logs that are hard to parse, chains of action that cannot be reconstructed. This is where most programs collapse. Hardest to retrofit. Requires:
- Traceability for every decision and action
- Evidence-quality audit trails
- Reconstructable chains of authorization

---

## Why System Prompts and Runtime Guardrails Will Not Pass an Audit

System prompts are instructions, not controls. Runtime guardrails operate on the host, not the data. Both can be bypassed by:
- Indirect prompt injection
- Model updates
- Adversary control of inputs

Foundational research on indirect prompt injection showed years ago that invisible instructions hidden in retrieved content can hijack LLM-integrated applications. The problem has not been solved, and several major AI companies have publicly conceded it may never be fully solved.

When a model is updated, retired, or replaced, the audit trail tied to that model evaporates. Only data-layer enforcement produces evidence regulators will accept.

---

## The Containment Gap (Kiteworks 2026 Forecast Data)

| Control | Organizations That Cannot Enforce | Advisory Requirement |
|---------|-----------------------------------|----------------------|
| Purpose binding / least privilege | 63% | Least-privilege enforcement |
| Kill switch capability | 60% | Human-in-the-loop guidance |
| Network isolation | 55% | Structural risk containment |
| Input validation | Majority | Behavioral risk mitigation |
| Continuous monitoring | ~40% | Traceability requirement |

---

## Architectural Recommendation: Data-Layer Governance

The only layer that satisfies both the advisory and a downstream auditor is the data layer — where:
- Every access decision is logged
- Every authorization is verified
- Every action is attributable to a human-authorized session
- ABAC policy is independent of the model
- Audit trails are tamper-evident and cryptographically signed

---

## Compliance Implications by Sector

### Healthcare (HIPAA)
Audit-defensibility for AI agent access to PHI now has a named federal standard. Data-layer ABAC and tamper-evident audit trails satisfy both HIPAA minimum-necessary and the joint advisory simultaneously.

### Defense Industrial Base (CMMC Level 2)
CMMC Level 2 AC, AU, and IA families require enforced authorization for AI agents touching CUI. Only 46% of DIB organizations consider themselves prepared. Data-layer governance satisfies all three control families.

### General Enterprise
Microsoft 365 Copilot and similar broadly deployed agents fall under the advisory when used with broad permissions. Organizations need centralized AI data gateway to demonstrate least-privilege and traceability.
