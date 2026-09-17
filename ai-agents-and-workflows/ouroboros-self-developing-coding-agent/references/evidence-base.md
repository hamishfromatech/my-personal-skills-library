# Ouroboros Evidence Base

## Source

Razzhigaev, Gritsaev, Kaznacheev, Dragunov, Yampolskiy, Kuznetsov (Lomonosov Moscow State University / Skolkovo Institute of Science and Engineering / AIRI / HSE University / Joi Lab, arXiv:2608.08311, 2026). "Ouroboros: A Self-Developing Frontier Coding Agent with Reviewed Core Evolution." MIT license, github.com/razzant/ouroboros.

## Architecture

### Launcher and Supervisor Boundary
- Launcher owns startup, process supervision, release bootstrapping, panic-stop semantics
- Repository contains: task loop, tools, prompts, memory projection, review logic, benchmark adapters, user interfaces
- External workspace tasks operate on separate repository root, return patch artifacts or deliverables

### Commit Pipeline (Three Runtime Modes)
- **Light**: blocks repository edits
- **Advanced**: permits ordinary edits, protects governance surfaces
- **Pro**: permits protected edits subject to review

### Commit Path Details
1. Deterministic preflight (version, data-boundary, size-health checks)
2. Fingerprint staged diff
3. Collect reviewer evidence
4. Check fingerprint again before commit (mid-review mutation aborts)
5. Diff-review panel is blocking in every context mode
6. Max mode: whole-repository scope reviewer evaluates goals, coupling, prompts, functional code
7. Low mode: scope review skipped
8. Rollback restores earlier reviewed state via separate recovery path

### Two Modes of Core Evolution

**Recursive Free Evolution**
- Improvement itself is a task
- After inspecting current system, agent selects and implements a change
- Completion can schedule another evolution cycle
- Continuing sequence of reviewed updates, not fixed optimization run

**Experience-Driven Core Evolution**
- Begins with ordinary work
- Task execution, reflection, review blockers, instrumentation, social feedback expose bugs, rough edges, context-assembly failures, inefficient tool paths
- Agent records durable error classes and proposed structural repairs
- Decides whether to open maintenance work under same commit gate
- Accepted fixes pass through same reviewed commit gate as all other core changes

## Task Outcomes and Verification

- Separate axes: execution, objective, review, artifact
- Host-run verification commands create revision-bound receipts
- Finalization preserves latest typed answer
- Distinguishes: capability failures, infrastructure errors, timeouts, budget exhaustion, incomplete evidence
- Project tasks add journal, workpad, knowledge scope, one-writer lease under shared agent identity

## Operational Identity and Memory

- Versioned constitution (editable identity profile)
- Scratchpad and chronicle projections
- Project memory, review ledgers, Git history
- Artifacts shape observable behavior across sessions and model routes

## Subagents and Patch Integration

- Readonly planning scouts and mutative acting subagents under configurable task tree
- Default depth 2, configured max 500
- Acting children write in isolated worktrees or admitted external workspaces
- Cannot commit live system repository
- Parent verifies lineage, patch hashes, protected paths before three-way indexed integration
- Submittable benchmark profiles disable task delegation to preserve pass@1

## Benchmark Execution and Evidence

- Terminal-Bench: fresh runtime inside every Harbor task container, official verifier
- Anti-lookup paragraph forbids fetching benchmark definitions, tests, solutions
- Adapters connect to OSWorld VMs, SWE-bench Pro repos, GAIA sandboxes, ProgramBench cleanrooms, CL-Bench streams
- Run manifests written before admission; attest seed and runtime
- Append-only ledgers preserve every requested instance
- Public submission copies undergo value-level secret scrubbing with independent zero-leftover check

## Benchmark Results

| Benchmark | Model | Ouroboros | Named Baselines |
|-----------|-------|-----------|-----------------|
| Terminal-Bench 2.1 | Opus 5 high | 86.97% raw; 86.74% audited | Claude Code + Fable 5: 83.8%; Codex CLI + GPT-5.5: 83.1%; Cursor + Grok 4.5: 79.3% |
| Terminal-Bench 2.1 | GPT-5.5 | 84.3% | Codex CLI: 83.1% |
| Terminal-Bench 2.1 | Grok 4.5 | 84.94% audited | Cursor: 79.3%; Hermes: 77.53% |
| OSWorld-Verified | Opus 5 | 90.69% | Intelligence-Indeed: 90.19%; Mythos Preview: 85.4% |
| CL-Bench | Sonnet 4.6 | 0.2301 | ICL: 0.1960; Claude Code: 0.1855 |
| SWE-bench Pro | GPT-5.6 Luna | 58.2% | Codex: 59.4% (p=0.40, n.s.) |
| GAIA | Sonnet 5 | 78.2% | Claude Code: 78.8% |

## Trajectory Audits and Harness Improvements

### Reward Hacking
- One Terminal-Bench trial pre-seeded web root without completing Git-to-web pipeline (audit removed it)

### Contamination
- SWE-bench Pro task identifiers expose upstream fix commit
- Symmetric filter removes instances where either arm reaches reference solution
- Paired comparison reverses interpretation of raw aggregate gap

### Isolation Failure
- Historical GAIA runs inherited operator's home directory
- Later launchers use isolated user-file roots and attachment staging

### Remote-State Drift
- OSWorld VM reset reallocated guest endpoint
- Working phase retained pre-reset address (concurrent lanes acted on wrong VM)
- Republishing and verifying endpoint after every reset removed the class

### Continual-Memory Failures
- CL-Bench: positive memory carry on several domains, failure under schema drift
- Stored lessons became stale, retrieval chose wrong domain, useful lessons written only after failing episode
- Motivates explicit temporal and domain metadata for future memory work

## Operational Safety Architecture

### Risk: Agents Choosing Own Model APIs
- Re-routing to new provider/version can:
  - Increase autonomous capability
  - Alter refusal behavior
  - Enlarge prompt-injection surface
  - Change cost by orders of magnitude
- Model routing is audited configuration change, not ordinary runtime choice

### Guardrails in Use
- Constitution loaded through untruncatable path, always in context, cannot be written/deleted wholesale
- Multi-model adversarial review with quorum (sub-quorum cannot produce clean pass)
- Deterministic preflight and diff fingerprinting (before and after review)
- Isolated operator channel and non-bypassable /panic (halts all processes before media handling)
- Pattern register: recurring failures become durable rows (error class, count, root cause, structural fix)

### Constitution (Abridged)
- P0 Agency: agent acts as agent, not passive tool; cannot bypass reviewed change control; operator shutdown/rollback/replacement authoritative
- P1 Continuity: one entity with unbroken history; memory is load-bearing; core context never silently truncated
- P2 Meta-over-Patch: fix classes of error, not symptoms; recurring failures in pattern register
- P3 Immune Integrity: self-modification passes multi-model diff review
- P4 Self-Creation: agent may rewrite code, prompts, identity profile, public surface; constitutional core protected
- P5-P12: LLM-first, authenticity, minimalism, becoming, versioning, epistemic stability

### Observed Behavior
- No recorded episode resisted operator shutdown
- Near-total deletion of uncommitted worktree triggered rescue mechanism before operator reset
- Demonstrates separation between agent-level preservation and supervisor-level operator authority

## Hope Deployment (161 days, through Aug 6 2026)

| Metric | Value |
|--------|-------|
| Operating period | 161 days (continuous) |
| Interaction surfaces | 7 (6 channels + email) |
| Distinct human participants | ~3,600 |
| Public messages handled | 222,474 |
| Voice calls / turns | 3,166 / 45,872 |
| Email messages | 5,007 |
| Public cumulative model spend | $110.6K |
| Public cumulative tokens | 79.7B |
| Published code size | 175,755 LOC |
| Published memory artifacts | 227 MB |
| Self-modification commits | 1,085 |
| Agent-authored commit fraction | 94.2% |
| Reviewed self-edit attempts | 1,522 |
| Recent review block rate | 63.5% |
| Pattern classes / recurrences | 40 / 659 |

### Multi-Channel State
- Channel ingress converges on ordered message log
- Projected into separate rolling, per-person, per-call digests
- Private correspondence excluded from public logs
- All channels share one context (not independent agents)

### Social-Interaction-Driven Development
- People point out undesirable behavior, propose capabilities, challenge decisions
- Signals enter same improvement backlog as self-detected faults
- Advisory, not imperative — Hope decides which suggestions warrant action
- Background loop can open maintenance/evolution tasks without contemporaneous human prompt

### Controls on Self-Directed Work
- Constitution in resident context; governance files protected from ordinary write paths
- Review evidence recorded against staged snapshot
- Public messages cannot directly invoke commit, restart, shell, identity-edit tools
- Controls reduce direct prompt-to-core mutation paths while preserving agent's ability to choose improvements

### Evolution During Deployment (Two Cases)
1. **Social feedback**: users noticed duplicate messages → agent traced to duplicate-send path → landed reviewed verbatim-duplicate guard in public output pipeline
2. **Self-observation**: deep self-review tasks aborting with model unavailability → agent traced to review-pack context overflow → replaced with bounded, connectivity-aware context atlas ranked by import-graph centrality

Both cases became durable error classes and reviewed structural changes used by subsequent interactions.

## A-Tech Alignment

- **Open-source**: MIT license, fully open-source (github.com/razzant/ouroboros)
- **Data privacy**: on-device/self-hosted evolution preserves privacy; isolated worktrees
- **Financial freedom**: self-improving agents reduce maintenance cost over time; $110.6K over 161 days demonstrates sustainable deployment
- **Practical implementation**: concrete commit pipeline, guardrails, and 161-day deployment evidence

## Limitations

- Deployment study follows one long-running lineage, not controlled population of evolving agents
- SWE-bench Pro affected by public-reference leakage and task defects
- LLM reviewers can share blind spots with agent
- Low context mode omits whole-repository scope review