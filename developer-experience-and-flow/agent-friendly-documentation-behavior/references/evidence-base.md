# Evidence Base: Agent-Friendly Documentation Behavior

## Source
Gao & Chen (arXiv:2608.20195, August 2026)

## Datasets

### SWE-chat Dataset
- 557 agentic coding sessions
- 94,813 development events
- 3,033 documentation interactions
- Public dataset of agentic coding sessions

### AIDev Dataset
- 33,097 agentic pull requests
- 690,260 classified file-level change records
- Links GitHub repositories to AI-generated PRs

## Four Key Findings

### Finding 1: Agents Prefer Agent-Facing Artifacts

| Documentation Type | Share of Interactions |
|-------------------|----------------------|
| Instruction files and working notes (agent-facing) | 60.5% |
| Classical technical documentation | 10.6% |
| API references | 1.3% |

- Agents primarily consult artifacts written for them, not human-facing docs
- Classical documentation and API references are rarely accessed
- Implication: current human-centric documentation is inefficient for AI agents

### Finding 2: Weak Documentation-to-Code Link

**Adjacent transition probability** (doc consultation → immediate code editing): 0.002
- The direct link between reading documentation and immediately editing code is very weak

**Unadjusted three-event lift:** 1.05
**Stage-adjusted model:** OR 1.33 [1.09, 1.62] (above unity when accounting for stage)

**Documentation creation:** lift 1.67 (elevated unadjusted, but adjusted interval includes unity)

**Testing association:** documentation consultation associated with LESS immediate testing
- Lift 0.23 (cluster CI 0.08-0.45)
- Adjusted OR 0.39 [0.25, 0.60]

### Finding 3: Self-Initiated, Not Failure-Driven

| Trigger | Share |
|---------|-------|
| Self-initiated | 70.2% |
| Failure-driven | 7.5% |

- Agents consult documentation on their own initiative, not after errors
- Documentation trails code: in multi-commit PRs changing both, code is touched first 4.7x more often
- No explicit documentation-based validation sequence was observed

### Finding 4: "Agent-Friendly" Properties Lack Support

Two widely assumed properties of agent-friendly documentation:
1. **Actionability** (docs that tell agents what to do) — no consistent behavioral support
2. **Verifiability** (docs that let agents check their work) — no explicit validation sequence observed

## The Two-Lobed Cycle Model

Agent documentation interaction is described as a two-lobed cycle, NOT a linear journey:

**Lobe 1: Code-focused work**
- Agents write and modify code
- Documentation consultation trails code changes
- Code touched first 4.7x more often in multi-commit PRs
- Doc reading does not immediately trigger code changes

**Lobe 2: Documentation-focused work**
- Agents create documentation
- Often unadjusted from existing patterns
- Documentation creation elevated (lift 1.67) but adjusted interval includes unity

## Methodology

### Event Classification
- Pipeline released for classifying development events
- Coding scheme for documentation interactions
- Event-level data released

### Analysis Approach
- Behavior-grounded study (not survey or assumption-based)
- Transition probabilities between event types
- Stage-adjusted models for lifting analysis
- Cluster confidence intervals for testing association

## Implications

### For Documentation Design
1. Prioritize agent-facing instruction files over classical documentation
2. Place instructions where agents naturally explore early in sessions
3. Don't expect documentation reading to immediately trigger code changes
4. Documentation informs later work, not immediate edits
5. Don't over-invest in "actionable" or "verifiable" properties without behavioral validation

### For Agent Tool Design
1. Agents self-initiate documentation consultation — make discovery natural
2. Documentation consultation reduces immediate testing — agents may be reading instead of testing
3. Agents create documentation during their workflow — support this with good templates

### For Repository Maintainers
1. AGENTS.md / CLAUDE.md / .cursorrules files are the primary documentation agents read
2. Working notes accumulated during tasks are the second-most consulted type
3. Classical README and docs are rarely consulted by agents
4. API references are almost never consulted directly

## Cross-References
- **SE Agent Building Practice:** "Agents retrieve written, not unsaid" (76% agreement) — agents work with what is explicitly written in instruction files, not implicit knowledge
- **SWE-chat Dataset:** Most common user intent is understanding existing code (19%), not writing code (13.4%) — documentation serves understanding, not generation
- **MCP Code Execution:** Agents explore filesystems progressively, not through error-driven lookups — documentation must be discoverable through normal exploration
- **Agent-Friendly API Documentation 2026:** Existing skill covers API documentation design; this skill covers broader documentation behavior patterns

## Released Artifacts
- Documentation interaction pipeline
- Coding scheme for event classification
- Event-level data from both datasets