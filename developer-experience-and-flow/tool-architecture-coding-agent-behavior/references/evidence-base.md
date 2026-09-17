# Tool Architecture Evidence Base

## Source

Xu, Saghir, Wu, Côté, Wang, Lakkaraju, Pei (Purdue University / Microsoft Research / University of Chicago, arXiv:2608.11386, 2026). "The Devil Is in the Interface: Evaluating How Tool Architecture Shapes Coding Agent Behavior."

## Experimental Design

- 6 tool architectures with matched capabilities (BashOnly, Atomic, NLSearch, Python, HypoTrack, Scratchpad)
- 3 actor models: Qwen3Coder-30B, Kimi-K2.5, Claude Sonnet 4.5
- 11,700 total trajectories
- 65 problem instances from SWE-bench Live (25 repos, ≤5 issues per repo)
- 10 independent rollouts per instance per actor-setup pair
- Evaluation dimensions: task resolve rate, consistency (pass^k), exploration (Jaccard read diversity, CodeBLEU solution diversity), efficiency (input tokens, output tokens, steps)
- Additional validation on SWE-bench Verified, SWE-bench Pro, debugging split (2 open-weight actors, 5 rollouts)

## Finding 1: Atomic Improves Consistency

| Actor | Setup | pass^5 | pass^7 | pass^9 |
|-------|-------|--------|--------|--------|
| Qwen3Coder-30B | BashOnly | 0.046 | 0.031 | 0.020 |
| | Atomic | 0.106 (+0.059) | 0.097 (+0.067) | 0.094 (+0.074) |
| Kimi-K2.5 | BashOnly | 0.290 | 0.277 | 0.266 |
| | Atomic | 0.304 (+0.014) | 0.289 (+0.013) | 0.280 (+0.014) |
| Sonnet-4.5 | BashOnly | 0.296 | 0.270 | 0.252 |
| | Atomic | 0.313 (+0.017) | 0.297 (+0.027) | 0.283 (+0.031) |

- Atomic is the ONLY setup with uniformly positive pass^k deltas across all three actors
- Mechanism: reduces environment-interaction errors (mis-edit: 1.64→0.19 for Qwen; wrong-syntax: 0.96→0.01)
- Gains largest for weakest actor (Qwen3Coder-30B): +4.7x improvement at pass^9

## Finding 2: NLSearch Improves Exploration

- NLSearch is the ONLY setup that consistently increases read diversity across all three actors
- Early search query diversity: Jaccard 0.85/0.87/0.86 (NLSearch) vs 0.69/0.63/0.68 (BashOnly)
- Recall of high-relevance files: +0.025 to +0.064 across actors
- Precision trade-off: -0.046 to -0.104 (more noise)
- Solution diversity changes are small and less uniform — exploration gains weakly translate to diverse final solutions

## Finding 3: Python Code Execution Improves Efficiency

| Actor | Setup | Steps | Input Tokens | Output Tokens |
|-------|-------|-------|-------------|---------------|
| Qwen3Coder-30B | BashOnly | 77 | 980K | 11,444 |
| | Python | 46 | 662K (-32%) | 9,672 (-15%) |
| Kimi-K2.5 | BashOnly | 65 | 950K | 9,556 |
| | Python | 47 | 1,141K (+20%) | 9,659 (+1%) |
| Sonnet-4.5 | BashOnly | 80 | 2,140K | 25,357 |
| | Python | 55 | 2,576K (+20%) | 25,089 (-1%) |

- Efficiency gain primarily from step reduction (fewer API calls), not per-step cost
- Python allows bundling multiple operations in a single executable block
- For strong actors (Kimi, Sonnet), Atomic can fragment compound bash workflows into more turns
- 97% of Python actions correspond to operations already available in BashOnly

## Finding 4: Lightweight Cognitive Scaffolding Limited

- Scratchpad: BLEU scores vs baseline reasoning content are high (>0.6 for many entries) — externalizes existing reasoning rather than introducing new state
- HypoTrack: true branching over multiple competing hypotheses is rare; most trajectories record 0 or 1 hypothesis
- Neither tool adds retrieval, memory management, new task information, or different reasoning policy
- Interpretation: text-only scaffolding alone is insufficient to reliably alter actor reasoning

## Generalization to Additional Tasks

Tables 12-15 confirm directional consistency across SWE-bench Verified, feature implementation (SWE-bench Pro), and debugging:
- Atomic improves pass^5 for both open-weight actors overall (+9.5% and +2.5%)
- NLSearch increases read diversity across tasks (+13.0% and +9.4%)
- Python reduces input-token cost (-0.6M and -0.4M) and steps (-25.9 and -15.1)

## A-Tech Alignment

- **Open-source**: findings apply to open-weight models (Qwen3Coder-30B, Kimi-K2.5) and open-source agents (OpenHands, SWE-agent)
- **Data privacy**: code-execution interfaces enable local, privacy-preserving agent workflows
- **Financial freedom**: 56.3% token reduction and 41.6% step reduction lower agent operating costs
- **Practical implementation**: concrete decision framework by goal (consistency, exploration, efficiency)

## Limitations

- Study uses SWE-bench Live subset (65 instances); results validated on 72 additional instances
- Only 3 actor models; frontier closed models not tested
- Cognitive scaffolding result is specific to lightweight text-based tools — richer scaffolding (retrieval, memory) not evaluated
- Atomic's effect depends on actor's natural interaction style (compound vs atomic bash behavior)