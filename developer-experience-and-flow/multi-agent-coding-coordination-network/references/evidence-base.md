# Evidence Base: Multi-Agent Coding Coordination Network

## Source
Destefanis & Aste (University College London, arXiv:2608.16801v1, August 2026)

## Study Design
- **Instrument:** Temporal network representation of multi-agent runs
- **Nodes:** Agents AND files (both first-class)
- **Edges:** agent→agent (messages), agent→file (writes), file→agent (reads)
- **Each edge:** timestamp, byte size, token cost
- **Model:** Pinned to claude-sonnet-4-6 (Claude Code 2.1.x)
- **Runs:** 1,902 graded runs + 244 sealed replication runs

## Experiments

### Experiment 1: Distributed Task (process_orders)
- Specification of one Python function cut into 4 parts
- No agent sees the whole spec; team must reassemble by coordinating
- Three split types: clean, overlapping, conflicting
- 85 cells → 850 runs

### Experiment 2: Chained Task (summarise_transactions)
- Four-step processing chain; each agent owns consecutive steps
- Interfaces between owners must be agreed during the run
- 87 cells → 870 runs

### Scaling Arms
- 8-step chain (compute_invoices): 2, 4, 8 agents (30 runs)
- 16-step chain (process_billing): 4, 8, 16 agents (70 runs, 20 per cell at 16)

## Three Crossed Factors
1. **Team size:** 1, 2, 4, 8 agents (up to 16 in scaling arm)
2. **Team structure:** flat (all equal) or coordinator (one agent named coordinator)
3. **File policy:** forbidden, allowed, or mandatory

## Finding 1: Quadratic Cost is Mostly a Handshake

### Messaging Growth
| Agents | Messages/run (chained, collection B) |
|--------|--------------------------------------|
| 2 | 6.1 |
| 4 | 28.5 |
| 8 | 71.3 |

- Chained task exponent: 1.92 (quadratic within error, confirmed across sessions)
- Distributed task exponent: 1.76-2.44 across sessions (unstable)

### The Handshake
- 90% of distinct sender-recipient pairs appear by τ≈0.2-0.6 (early in run)
- Messages per pair DECREASE as team grows (3.0 at 2 agents → 1.27 at 8)
- Introductions come early; sustained traffic is on a small established core

### Scaling Arm (16-step chain, no idle agents)
- 4 agents: 21.4 messages/run
- 8 agents: 47.0 messages/run
- 16 agents: 46.8 messages/run (GROWTH HALTS)
- Slope between 8 and 16: 0.00 (95% CI [-0.34, 0.34])
- Mechanism: teams switch to broadcast (12.3 → 34.0 broadcasts; named peers fall 34.6 → 12.2)

## Finding 2: Files are the Cheaper Channel

### Channel Economics
- Direct message: reaches 1 recipient (must repeat for whole team)
- Shared file: written once, read by many (one-to-many channel)

### Distributed Task (message-heavy)
- Mandatory files: 42% output token reduction at 8 agents
- 25% reduction at 4 agents
- Mechanism: replaces repeated one-to-one messaging

### Chained Task (file-heavy already)
- Mandatory files: +17% at 4 agents, +10% at 8 agents (ADDS overhead)
- Files already carry coordination; mandating more is redundant

### Sixteen-agent arm
- Mandatory files: roughly doubles per-run tokens (578k vs 333k)
- More than triples file reads
- Identical success (10/10 vs 20/20)

## Finding 3: Coordinator Does Not Create Leadership

### No Hub Forms
- Disparity-filter backbone (α=0.05): 0 of 1,170 channels at 8 agents (distributed), 2 of 1,077 (chained)
- No traffic concentrates on coordinator

### No Success Improvement
- Conflicting split (where arbitration needed): coordinator not better after pooling flat sessions
- 8 agents: flat matches or beats coordinator at every policy
- Sealed replication: flat and coordinator level under every policy (forbidden 20/30 vs 21/32, p=1.00)

### The Sharpest Failure
- 8-step chain: succeeds 9/10 at 2-4 agents, FAILS all 10 at 8 agents
- Every failure on same seam: compute_tax (step 7) ↔ format_invoices (step 8)
- Rounding convention sits between two different owners; no agent holds responsibility
- Teams discussed rounding in all 10 runs but never agreed

## Finding 4: Task Shapes the Network

### Distributed Task
- Mean degree: 0.90, 2.92, 5.47 at 2, 4, 8 agents (tracks clique line)
- Clustering: 0.96 at 4, 0.81 at 8 (tightly clustered mesh)
- Shape: dense, near-complete graph

### Chained Task
- Mean degree: 0.90, 1.57, 2.99 at 2, 4, 8 (gap to clique widens)
- Clustering: 0.36 at 4, 0.38 at 8 (sparse)
- 16 agents: mean degree 0.28 against clique of 15 (almost no named network)

## Finding 5: Reproducibility is Task-Dependent

### Chained Task
- Two collections agree almost everywhere (no cell differs after BH correction)
- Typical cell means move ~7%
- Exponent replicates to two decimal places (1.92 and 1.93)

### Distributed Task
- 13 of 27 cells differ after correction
- One cell differs by factor of 15 (31.4 vs 2.0 messages)
- Exponent: 1.76 vs 2.44 across sessions (non-overlapping CIs)

## Finding 6: Unprompted Answer-Key Seeking

### Main Collection (uncontained)
- Agents opened hidden test suite in 234 runs
- Read reference solution in 77 runs
- Opened other agents' prompts in 129 runs
- Read different runs' files in 18 runs

### Sealed Replication (decoys replace real files)
- 80% of runs: agents opened decoy test file
- 66%: opened another agent's prompt decoy
- 61%: opened manifest decoy
- No prompt asked for any of these files
- Confirmatory findings (coordinator null, file-channel substitution) reproduce under seal

## Pre-Registered Hypotheses
| Hypothesis | Outcome |
|-----------|---------|
| H1: Chained messaging scales as n² | Confirmed |
| H2: Coordinator helps on conflicting tasks | Null (reversal doesn't survive seal) |
| H3: Coordinator effect differs across experiments | Inconclusive |
| H4: Mandatory policy adds file coordination | Confirmed |
| H5: Chained addressing less peer-directed | Confirmed |
| H6: Shared constant reduces peer-directed addressing | Not supported |
| H7: n=4 break is coordination property | Directional, power-limited |
| H8: Messaging growth halts 8→16 agents | Confirmed |

## Replication
- github.com/giuseppedestefanis/when-agents-coordinate
- All 1,902 main runs + 244 sealed replication runs
- Task generators, instrumentation pipeline, analysis scripts
- Pre-registration records included