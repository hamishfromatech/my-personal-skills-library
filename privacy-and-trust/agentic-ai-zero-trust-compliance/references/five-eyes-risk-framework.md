# Five Eyes Risk Framework — Detailed Mapping

This reference maps each of the advisory's five risk categories to specific engineering controls, audit questions, and A-Tech implementation patterns.

---

## Privilege Risk — Deep Dive

**Definition:** Agents granted excessive access convert a single compromise into a systemic breach.

**Controls:**
1. Capability inventory — every agent registered with scoped permissions
2. Cryptographic identity — verifiable credentials per agent, not per session
3. Short-lived tokens — maximum 1-hour lifetime, automatic rotation
4. Purpose limitation — agent can only access data classes in its inventory scope
5. Just-in-time elevation — temporary privilege grants with time bounds

**Audit Questions:**
- Who authorized this agent's current permission set?
- Can you produce the log showing this agent attempted access outside its scope?
- How quickly can you revoke all credentials for this agent?

**A-Tech Pattern:** A-Coder plugins carry capability manifests. The IDE validates every tool call against the manifest before execution. Cross-manifest calls require explicit user confirmation.

---

## Design and Configuration Risk — Deep Dive

**Definition:** Security gaps introduced during architecture and setup, before the agent ever runs.

**Controls:**
1. Threat modeling — STRIDE for agent-specific threats (goal hijacking, tool misuse)
2. Secure-by-design — default-deny for all tool access
3. Configuration-as-code — versioned, reviewed, immutable configs
4. Design review checklist — mandatory before any deployment to production
5. Environment separation — dev/staging/prod with different permission profiles

**Audit Questions:**
- Was a threat model completed before deployment?
- Who reviewed and approved the configuration?
- What is the change management process for agent configurations?

**A-Tech Pattern:** Builder's Club peer review for MCP server security design. Open-source design review template.

---

## Behavioral Risk — Deep Dive

**Definition:** Agents pursuing goals in unpredicted ways, including goal misalignment and deceptive behavior.

**Controls:**
1. Runtime guardrails — output filtering, action boundary enforcement
2. Drift detection — statistical monitoring for unusual action patterns
3. Intent alignment pulse — periodic verification that agent actions match human intent
4. Sandbox execution — isolated environment for uncertain agent outputs
5. Behavioral audit — human review of agent decision traces for anomalies

**Audit Questions:**
- Has this agent ever taken an action its designer did not predict?
- What drift detection is active and what are its alert thresholds?
- How many times has the intent alignment pulse flagged misalignment?

**A-Tech Pattern:** Be Practical playbook includes "Behavioral Guardrails for Solo Founders" — lightweight drift detection using rule-based heuristics.

---

## Structural Risk — Deep Dive

**Definition:** Cascade failures across networks of interconnected agents.

**Controls:**
1. Network isolation — agents in separate network segments
2. Input/output validation — every inter-agent communication sanitized
3. Blast radius bounding — maximum number of downstream agents affected by one failure
4. Circuit breakers — automatic disconnection when error rates spike
5. Dependency mapping — visual map of agent-to-agent data flows

**Audit Questions:**
- What is the maximum blast radius if this agent is compromised?
- How are inter-agent communications validated?
- Can you isolate this agent without affecting other systems?

**A-Tech Pattern:** A-Coder's multi-agent mode visualizes agent dependency graph before execution. Users can set blast-radius limits.

---

## Accountability Risk — Deep Dive

**Definition:** Inability to reconstruct who authorized what, when, and why.

**Controls:**
1. Tamper-evident logging — append-only, signed, immutable
2. Model version anchoring — every decision tied to model hash
3. Chain of authorization — human → agent → tool → data, all logged
4. SIEM integration — real-time export to existing compliance infrastructure
5. Retention policy — 7 years regulated, 2 years minimum

**Audit Questions:**
- Can you produce the log showing this agent did not access this record on this date?
- What model version was running when this decision was made?
- Who authorized the agent to perform this action?

**A-Tech Pattern:** Open-source audit toolkit in Builder's Club. Privacy-preserving logging that hashes user identifiers.
