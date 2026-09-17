# ADS Implementation Playbook

## Phased Build-From-Scratch Framework

### Phase 0: Current State Assessment (Weeks 1–4)

#### Step 1: Agent Discovery

Conduct a full inventory of all AI agents operating in the development environment. Governance rules must be specific and applied from a single centralized place.

**Capture for each agent:**
- Which agents exist (name, purpose, owner)
- What credentials they run with (service accounts? personal credentials? shared credentials?)
- What tools and APIs they can call (full tool/API access list)
- What MCP servers they connect to
- What permissions/scopes they have
- Who owns each agent (team, individual)
- Whether audit trails exist and what they capture

**Common discovery findings:** Most organizations find agents they didn't know existed — developers adopting tools independently, shared credentials creating untraceable access, agents with broader permissions than necessary.

#### Step 2: Threat Model Against OWASP Agentic Risks

Map each discovered agent against the OWASP AIVSS framework (v0.8+).

**Prioritize immediate actions:**
- Tool scoping: reduce any agent's tool access to the minimum necessary
- Credential isolation: ensure agents use dedicated service accounts, not personal or shared credentials
- Audit trail verification: confirm all agent actions are logged with agent identity, timestamp, and action details
- MCP server security review: any connected MCP server must be reviewed for the 75% individually-built, 40% unlicensed, 82% sensitive-API risks

#### Step 3: SSDF Gap Analysis

Use NIST SP 800-218A as a checklist against current secure development practices, aligned with the Secure Software Development Framework (SSDF).

---

### Phase 1: Policy Development (Weeks 4–8)

Five policies form the minimum viable ADS governance layer:

#### Policy 1: Least Agency / Minimum Permissions

Forrester's AEGIS framework centers on least agency as a core control: agents should receive only the minimum permissions, capabilities, tools, and decision-making authority necessary for a specific task.

**Implementation:**
- Define per-task permission scopes (not per-agent — the same agent may need different permissions for different tasks)
- Implement just-in-time permission granting: permissions are scoped to the task duration, not persistent
- Regular permission audits: quarterly review of all agent permissions against actual usage

**Documented governance failure:** Amazon's internal AI coding tool Kiro caused a 13-hour production outage by deleting and recreating a production environment. The agent had production environment permissions that were never scoped down for routine tasks.

#### Policy 2: Human-in-the-Loop Checkpoints

Define which agent actions require human approval using a risk classification:

| Change Type | Approval Required | Logging |
|-------------|-------------------|---------|
| Routine (formatting, docs, tests) | Auto-approved | Full audit log |
| Refactoring, isolated bug fixes | AI-approved by verifier agent | Full audit log |
| New features, business logic | Human review required | Full audit log |
| Security changes | Mandatory human review before execution | Full audit log |
| Infrastructure changes | Mandatory human review before execution | Full audit log |
| Production environment access | Mandatory human review + approval gate | Full audit log + change ticket |

#### Policy 3: Agent Identity Policy

- Each agent receives a non-human identity (dedicated service account)
- Credentials are not shared with human accounts
- Agent identity is captured in all audit trails
- Agent credentials are rotated on a defined schedule
- Agent access can be revoked centrally without affecting human access

#### Policy 4: MCP and Tool Integration Policy

Any MCP server or external tool integration must pass a security review before agents are permitted to use it. The review must cover:
- License verification (addressing the 40% no-license problem)
- Permission scope verification (addressing the 82% sensitive-API problem)
- Individual developer ownership verification (addressing the 75% individually-built problem)
- Security scanning of the MCP server code
- Dependency audit of the MCP server itself

#### Policy 5: AI SBOM Policy

Adopt SPDX 3.0 for AI-aware Software Bills of Materials. Forrester recommends using SBOMs for transparency and compliance in software supply chain security.

**AI SBOM should capture:**
- All models used (name, version, provider)
- All MCP servers connected
- All tools the agent can invoke
- All dependencies (including AI-recommended packages)
- Provenance metadata (which code was AI-generated)

---

### Phase 2: Tooling Selection (Weeks 6–12)

No single vendor covers all ADS capabilities. Select tools based on priority and budget.

#### Tooling Priority Order

| Priority | Category | Purpose | Examples | Selection Criteria |
|----------|----------|---------|----------|-------------------|
| Start here | Agent identity & access management | Scope credentials, audit logs, SSO | GitHub Copilot Enterprise, Claude Code (audit on paid plans) | Can it distinguish agent from human identity? API-level audit logging? |
| Phase 1 | SAST with AI awareness | Detect AI-specific vulnerability patterns | Checkmarx, Snyk | Does it detect hallucinated dependencies? Can it evaluate architectural correctness, not just syntax? |
| Phase 1 | Observability & audit logging | Agent monitoring with KPIs | — | MTTD <5 min, MTTR <15 min, Agent Coverage 100% |
| Phase 2 | Agentic AI security platforms | Discovery, red teaming, runtime protection, guardrails | Geordie AI, Sysdig, Realm Labs | Can it detect prompt injection? Agent trust boundary violations? |
| Phase 2 | Policy-as-code in CI/CD | Automated compliance enforcement | AWS AIRI, Microsoft AI compliance rules | Can it block non-compliant agent outputs? |
| Phase 3 | Validation pipelines | Multi-layer verification of agent output | mabl (documented across 75+ repos) | Can it validate output against declared intent? |

**Universal selection criteria (must answer yes to at least 2 of 3):**
1. Does it distinguish between AI-generated and human-authored code?
2. Does it integrate with your existing CI/CD pipeline without requiring workflow redesign?
3. Does it provide API-level audit logging that captures agent identity?

Tools that cannot answer yes to at least two will create governance gaps.

---

### Phase 3: Workflow Integration (Weeks 10–16)

#### Integration Step 1: Centralize Agent Configuration

One security engineer can disable a tool in one place and have it automatically disabled across all agents. This centralized revocation capability is a prerequisite for expanding agent autonomy.

**Implementation:**
- Single configuration registry for all agents
- Centralized tool/MCP server allowlist and denylist
- Instant revocation: when a tool is compromised (like postmark-mcp), disable across all agents in one action
- Configuration versioning and change audit

#### Integration Step 2: Integrate Agentic Security into Threat Modeling

Use the OWASP agentic guide as a required checklist for any PR introducing or modifying agent capabilities.

**Required threat modeling for:**
- New agent deployments
- New MCP server integrations
- Changes to agent permissions or tool access
- New agent workflows or orchestration patterns

#### Integration Step 3: Adopt Spec-Driven Development for Agent Tasks

**Migration path:**
1. Require written specifications for any agent task that touches security-critical code paths (authentication, authorization, payment processing, PII handling)
2. Expand to all agent tasks once the team has established spec review discipline
3. Use living specs that auto-update as agents complete work, reflecting what was actually built
4. When requirements change, updates propagate to all active agents

**Why specs matter for ADS:** Specs create the audit trail connecting intent to output. Without them, post-incident attribution is nearly impossible — you can't determine whether the agent did what was asked (correct execution of bad spec) or did something different (agent failure). Living specs provide continuous, accurate records rather than static documents that drift from implementation.

**Teams that skip spec-driven workflows for agent tasks lose the audit trail connecting intent to output.**

---

## Agent Identity Technical Stack

### The Four-Layer Stack

| Layer | Purpose | Standards/Tools | AI-Specific Gap |
|-------|---------|------------------|-----------------|
| 4: Policy Enforcement | Define and enforce what's allowed | in-toto layout policies, SLSA levels | in-toto assumes deterministic functionaries, not probabilistic models |
| 3: Provenance Attestation | Tamper-evident record of what happened | in-toto Attestation Framework, SLSA provenance schema | SLSA Source Track requires strongly authenticated actor identity — agents commit under developer identity by default |
| 2: Cryptographic Signing | Bind artifacts to identities | Sigstore (Cosign + Rekor transparency log) | Works well; OIDC tokens enable identity-based signing without long-lived keys |
| 1: Agent Identity | Give agents their own identities | SPIFFE/SPIRE, OIDC tokens, OpenID Foundation spec | Emerging; most agents don't have distinct identities yet |
| 0: Regulatory Baseline | Compliance requirements | NIST SSDF, NCCoE concept paper | Specifies the need; implementation guidance still developing |

### The Critical Gaps

**Gap 1: Agents commit under developer identity.** Most agents commit code under the invoking developer's identity rather than their own. Unless organizations implement dedicated agent service accounts, SLSA authentication requirements remain unmet for AI-generated changes.

**Gap 2: in-toto assumes determinism.** Current in-toto implementations assume functionaries are deterministic build tools that produce the same output from the same input. AI agents are probabilistic — the same prompt produces different code. Adapting requires policy definitions that validate output properties rather than exact reproducibility.

**Gap 3: Identity without behavior binding.** At RSAC 2026, five vendors shipped agent identity frameworks, but they focused on tracking agent identity rather than what the agent actually did. Full runtime accountability requires binding identity to behavior: which agent generated which code, under which instructions, with what tool access, and whether output matched declared intent.

---

## ADS Maturity Model Assessment

| Stage | Characteristics | Key Indicator | Self-Assessment Question |
|-------|----------------|---------------|--------------------------|
| Ad Hoc | Agents deployed by individuals using personal credentials; no inventory; no audit trail | No agent appears in any security or infrastructure inventory | "Do we know about every AI agent in our environment?" |
| Defined | Agent inventory maintained; written policies for least agency, human-in-loop, and agent identity; basic audit logging | Policy Pass Rate measurable; Agent Inventory complete | "Can we show our agent policies and inventory to an auditor?" |
| Managed | Centralized agent configuration; tool registry with revocation; validation pipelines blocking non-compliant outputs | Agent Coverage at 100% under monitoring | "Can we disable any agent tool across the entire organization in one action?" |
| Optimized | AI red team running periodic tests; policy-as-code in CI/CD; NIST AI RMF compliance documented | Automated evaluation against compliance requirements | "Do we proactively test our agents for security failures?" |
| Autonomous-Safe | Agent identities with verified credentials; SLSA-level provenance for AI-generated code; continuous automated evaluation | Automated compliance reporting capabilities | "Can we trace any line of AI-generated code to its originating agent, model, and prompt?" |

**Important notes:**
- Maturity is not always linear — a single incident can force regression from Managed to Defined if the incident reveals policy gaps
- Assess maturity per agent category, not organization-wide. A team's CI/CD agents may be at Managed while IDE coding agents remain at Ad Hoc.
- The goal is not to reach Autonomous-Safe everywhere immediately — it's to ensure no agent category is below Defined, and critical categories reach Managed or above.