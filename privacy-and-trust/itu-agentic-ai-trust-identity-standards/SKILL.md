---
name: itu-agentic-ai-trust-identity-standards
description: Navigate and align with the ITU Focus Group on Trust and Identity for Humans and Agentic AI — the UN-level standardization initiative building global frameworks for trusted digital identity, trustworthy AI agent behaviour, and meaningful human control. Use when designing agent identity systems, building agentic AI trust frameworks, preparing for interoperability standards, positioning products for enterprise procurement under emerging agent identity requirements, or mapping the global standards landscape for agentic AI governance.
---

# ITU Agentic AI Trust & Identity Standards

## Overview

The International Telecommunication Union (ITU), the UN specialized agency for digital technologies, launched the Focus Group on Trust and Identity for Humans and Agentic AI on 9 July 2026 at the AI for Good Global Summit in Geneva. This is the first UN-level standardization initiative specifically addressing how autonomous AI agents establish identity, earn trust, and remain accountable throughout their lifecycle while preserving meaningful human control. The Focus Group reports to ITU-T Study Group 17 (security standards), with its first meeting in Paris (November 2026) and second in Geneva (January 2027).

## When to Use

- Designing AI agent identity, authentication, or trust systems
- Building products that will face enterprise procurement requirements for agent identity compliance
- Mapping the global standards landscape for agentic AI governance, trust, or identity
- Positioning open-source AI products to align with emerging UN-level standards before they become procurement requirements
- Designing multi-agent systems where agents must identify and authenticate one another
- Building financial transaction or critical infrastructure systems where agents act on behalf of humans
- Preparing compliance roadmaps for agentic AI products entering regulated industries
- NOT for purely domestic regulatory compliance (use EU AI Act, DPDP, CCPA skills instead)
- NOT for payment protocol specifics (use AP2, x402, Agent Pay skills instead — this is the identity/trust layer above them)

## Core Process / Workflow

### 1. Understand the Standardization Mandate

The Focus Group addresses three core challenge areas:

**A. Trust Management for People and AI Agents**
- How humans establish trust in agents acting on their behalf
- How agents establish trust in other agents
- Trust lifecycle (assurance) models from initial deployment through continuous operation
- Mechanisms for trust repair after agent errors or misconduct

**B. Overall Trustworthiness of Agentic AI Systems**
- Security criteria and benchmarks for continuous assessment of AI agents
- Behavioural accountability throughout the agent lifecycle
- Reference architectures for trustworthy agent systems

**C. Meaningful Human Control**
- Preserving human authority over agent actions, especially for financial transactions and critical infrastructure
- Escalation and override mechanisms
- Authorization scope and constraint enforcement

### 2. Track the Six Deliverables

The Focus Group will develop:

1. **Common terminology and definitions** — The shared vocabulary for agentic AI trust and identity across industry, governments, academia, and civil society
2. **Reference architectures** — For identity, trust, agent discovery, and interoperability
3. **Trust frameworks and lifecycle (assurance) models** — From deployment through continuous assessment
4. **Interoperability mechanisms** — For digital identity and credentials across agent systems
5. **Security criteria and benchmarks** — For continuous assessment of AI agents
6. **Standardization roadmap** — Coordinating action across expert communities

### 3. Map to A-Tech Product Architecture

For each deliverable area, assess alignment:

```yaml
alignment_assessment:
  terminology:
    action: Adopt ITU terminology in A-Tech product docs and API schemas
    priority: Q1 2027 (after first meeting)
  reference_architectures:
    action: Design A-Coder agent identity to be ITU-reference-architecture-compatible
    priority: Q2 2027 (after second meeting)
  trust_frameworks:
    action: Map A-Tech trust design to ITU assurance lifecycle models
    priority: Q2 2027
  interoperability:
    action: Ensure W3C DID + Verifiable Credentials alignment (ITU will likely build on these)
    priority: Ongoing
  security_criteria:
    action: Align A-Coder agent security benchmarks with ITU continuous assessment criteria
    priority: Q3 2027
  standardization_roadmap:
    action: Participate in Focus Group (open to all interested experts)
    priority: November 2026 (first meeting)
```

### 4. Position Open-Source as Structural Advantage

Open-source AI has inherent advantages in the ITU trust framework:

| ITU Requirement | Open-Source Advantage |
|---|---|
| Trustworthiness through transparency | Open code is auditable by community |
| Continuous assessment benchmarks | Public benchmarks, reproducible builds |
| Identity and authentication | Open standards (W3C DID, VC) already aligned |
| Security criteria | Community security audits, CVE tracking |
| Interoperability | Open protocols (MCP, A2A, x402) are standards-native |
| Meaningful human control | Open-source agents are inspectable, modifiable, self-hostable |

### 5. Build the Identity-Trust Stack

Based on the ITU framework, design a layered identity-trust architecture:

```
Layer 5: Human Control & Authorization
  - Human-in-the-loop checkpoints for financial transactions
  - Constraint enforcement (spending limits, merchant whitelists, time windows)
  - Revocation and override mechanisms

Layer 4: Agent Trustworthiness Assessment
  - Continuous behavioural monitoring
  - Performance track records (per-domain)
  - Trust repair protocols after errors

Layer 3: Agent-to-Agent Authentication
  - Mutual identification and authentication
  - Capability negotiation
  - Trust scoring and reputation exchange

Layer 2: Agent Identity & Credentials
  - Agent registration and identity binding
  - Verifiable credentials (W3C VC, SD-JWT)
  - Selective disclosure for privacy

Layer 1: Cryptographic Foundation
  - Key management for agents
  - Signature and verification
  - Audit trail integrity
```

### 6. Privacy-First Trust Architecture

The ITU framework's emphasis on "meaningful human control" aligns with privacy-first architecture:

- **Local-first agent identity**: Agent identity credentials generated and stored on-device; no central identity registry required
- **Selective disclosure**: Agents reveal only the minimum identity information needed for each transaction (SD-JWT, BBS+ zero-knowledge proofs)
- **On-device trust assessment**: Agent trustworthiness evaluated locally using behavioral signals, not centralized surveillance
- **Human-controlled authorization**: Constraint enforcement runs on the user's device; the human retains the key
- **Audit trails stay local**: Tamper-proof logs stored on-device, exported only for dispute resolution with user consent

### 7. Prepare for Enterprise Procurement

Enterprise procurement will increasingly require ITU-aligned agent identity compliance:

1. **Audit current architecture** against the six deliverable areas
2. **Gap analysis**: Identify which deliverables your product already addresses vs. needs to build
3. **Compliance roadmap**: Timeline alignment with ITU meeting schedule (Nov 2026, Jan 2027)
4. **Documentation**: Prepare evidence packs for each trust framework requirement
5. **Open standards adoption**: Verify W3C DID, VC, and relevant protocol alignment
6. **Security benchmarking**: Establish continuous assessment processes before standards are finalized

## A-Tech Application Matrix

### A-Coder
- **Agent identity**: Per-developer agent identity with local-first credential generation; W3C DID for portability
- **Trust assessment**: Per-domain track records with on-device evaluation (connects to trust-calibration-ux-pattern skill)
- **Human control**: Authorization constraints for file system access, code execution, and deployment actions
- **Interoperability**: MCP-compatible tool access; A2A-compatible agent coordination; ITU reference architecture alignment
- **Competitive positioning**: Open-source A-Coder is auditable, self-hostable, and inspectable — structurally aligned with ITU trustworthiness requirements that closed-source agents cannot match

### Be Practical
- **Curriculum module**: "Building Trustworthy Agentic AI: The ITU Standards Landscape" — covers the Focus Group mandate, the six deliverables, and how to design for compliance
- **Case studies**: How open-source agent identity aligns with UN-level standards; privacy-first architecture as ITU trustworthiness advantage
- **Practical exercise**: Map a product architecture against the ITU six deliverable areas; build a compliance roadmap

### Builder's Club
- **Standards participation**: Organize community participation in the ITU Focus Group (open to all experts; first meeting November 2026 in Paris)
- **Open reference implementation**: Community-built open-source reference architecture for ITU-aligned agent identity and trust
- **Benchmark contribution**: Contribute to ITU security criteria and benchmarks for continuous agent assessment
- **Interoperability testing**: Community-driven interoperability testing for agent identity standards

## Anti-Patterns

1. **Waiting for final standards before acting** — The standards process takes 1-2 years; build aligned architecture now using existing open standards (W3C DID, VC, SD-JWT) that ITU will likely build upon
2. **Treating identity as an afterthought** — Agent identity is the foundation; retrofitting it after deployment is far more expensive
3. **Centralized identity registries** — Privacy-first means local-first; avoid building a central agent identity database that creates a surveillance surface
4. **Ignoring agent-to-agent trust** — The ITU framework explicitly addresses agents identifying and authenticating one another; design for this from day one
5. **Conflating trust with usage metrics** — Trustworthiness is about demonstrated competence and accountability, not time spent using the agent (connects to trust-calibration-ux-pattern anti-pattern: vanity trust scores)
6. **Overlooking meaningful human control** — The ITU emphasizes preserving human authority for financial transactions and critical infrastructure; design escalation and override from the start
7. **Building proprietary identity systems** — The ITU will develop interoperability mechanisms; proprietary systems will face isolation

## Cross-References

- **verifiable-intent-agentic-trust-layer** — Mastercard's SD-JWT credential chain is a concrete implementation of the ITU identity-trust layer
- **trust-calibration-ux-pattern** — Per-domain trust assessment aligns with ITU continuous assessment criteria
- **transaction-closure-delegated-ai-governance** — Closure-based governance is a mechanism for meaningful human control
- **contribution-economy-trust-loop** — W3C DID + Verifiable Credentials for community identity aligns with ITU interoperability
- **agent-protocol-stack-2026** — MCP, A2A, AP2 are the protocol layer beneath ITU identity standards
- **agentic-supply-chain-exploit-defense** — ITU security criteria will formalize what this skill defends against
- **agentic-development-security-ads** — Forrester's ADS framework aligns with ITU trustworthiness assessment
- **trust-economy-aeo-framework** — The Trust Economy is the market structure ITU standards will formalize

## References

- See [references/itu-focus-group-detail.md](references/itu-focus-group-detail.md) for the full ITU press release extraction, mandate analysis, timeline, and strategic implications.