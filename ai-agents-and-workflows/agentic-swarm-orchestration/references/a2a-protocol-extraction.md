# A2A Protocol Extraction — Agent-to-Agent Interoperability

## Core Concepts (Google A2A, April 2026)

The Agent-to-Agent (A2A) protocol defines how agents discover capabilities, negotiate tasks, and share results without human intermediation.

### Agent Card (Discovery)
Every agent exposes a JSON card at `/.well-known/agent.json`:

```json
{
  "name": "security-scanner-agent",
  "version": "2.1.0",
  "capabilities": ["dependency-scan", "secret-detection", "sbom-generation"],
  "input_schema": { /* OpenAPI fragment */ },
  "output_schema": { /* OpenAPI fragment */ },
  "authentication": ["did-vc", "api-key"],
  "endpoint": "https://agents.atech.dev/security"
}
```

### Task Lifecycle
1. **Request** — Task description + required capabilities
2. **Negotiate** — Capability matching, terms, constraints
3. **Execute** — Asynchronous task with progress streaming
4. **Deliver** — Result artifact(s) with provenance
5. **Rate** — Quality signal feeds reputation graph

### Key Design Principles
- **Capability-centric** — Agents matched by what they can do, not who they are
- **Asynchronous by default** — No blocking waits across agent boundaries
- **Human-in-the-loop optional** — Escalation is explicit, not default
- **Cryptographically verifiable** — All messages signed, all actions logged

## Interoperability Challenges
- Protocol versions diverge between vendors
- Rate limiting and resource contention in dense swarms
- Semantic ambiguity in capability descriptions
- Reputation portability across platforms

## A-Tech Implementation Path
Phase 1: Internal agents use A2A within Builder's Club infrastructure  
Phase 2: Open A2A gateway for community-contributed agents  
Phase 3: Cross-platform agent marketplace with portable reputation
