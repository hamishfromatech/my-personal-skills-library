# Ornith Self-Proposed Task RL — Evidence Base

## Source
Ornith-1.5 (DeepReinforce), released August 19, 2026, under MIT license in 397B MoE, 35B MoE (3B active), and 9B dense sizes. DataNorth AI coverage (Jorick van Weelie, August 20, 2026). Miraflow AI and DataNorth analysis of the self-proposed task RL training method.

## The Training Method

Ornith-1.5's core innovation is in post-training, not architecture. Instead of a fixed set of human-written tasks, the model:

1. **Proposes its own tasks** — drawn from a learned task distribution
2. **Builds a scaffold for each task** — creates a runnable environment
3. **Generates the solution rollouts** — attempts to solve its own task
4. **The solution rollouts become the RL signal** — reward from successful completions

The task reward multiplies three components:
- **Validity:** Is the task solvable? (judge agent verifies)
- **Novelty:** Is it different from prior tasks? (prevents repetition)
- **Difficulty:** Targets 0.2 success rate (optimal challenge level)

## Reward Hacking Defense

Reward hacking has been a running theme in DeepReinforce's published RL work. Ornith-1.5 carries that defense into task generation:

- Verifiers are synthesized WITHOUT access to the reference solution
- Solver trajectories are used to discover and close reward shortcuts
- Three check types: oracle (correct solution passes), no-op (doing nothing fails), unsolved-state (unsolved state fails)
- A verifier that passes all three produces a binary reward reliable enough to train on directly

## Benchmark Results (Ornith's own evaluations, avg of 5 attempts)

### Ornith-1.5 397B (Flagship MoE)
| Benchmark | Score |
|-----------|-------|
| Terminal-Bench 2.1 | 86.1 |
| SWE-bench Verified | 86.0 |
| GPQA Diamond | 92.8 |
| BrowseComp | 86.6 |
| Context window | 262,144 (→1M YaRN) |

### Ornith-1.5 35B MoE (3B active)
| Benchmark | Score |
|-----------|-------|
| Terminal-Bench 2.1 | 68.5 |
| SWE-bench Verified | 79.0 |

### Ornith-1.5 9B Dense
| Benchmark | Score |
|-----------|-------|
| Terminal-Bench 2.1 | 47.0 |
| SWE-bench Verified | 70.6 |

## Comparison with Competitors

| Benchmark | Ornith-1.5 397B | Claude Opus 4.8 | Kimi K3 | GLM-5.2 |
|-----------|-----------------|-----------------|---------|---------|
| Terminal-Bench 2.1 | 86.1 | 85.0 | 88.3 | 81.0 |
| SWE-bench Verified | 86.0 | — | — | — |
| Frontier-Bench v0.1 | 13.5 | 21.1 | — | — |
| NL2Repo | 59.5 | 69.7 | — | — |
| DeepSWE | 56.0 | 59.0 | — | — |

**Critical caveat:** Ornith-1.5 leads on Terminal-Bench and SWE-bench but trails on harder agentic rows. A model trained on tasks it wrote itself has an obvious failure mode: it optimizes for the kind of task it can solve, not the kind of task that matters. The gap on Frontier-Bench and NL2Repo is the evidence.

## The Self-Proposed Task Failure Mode

The fundamental risk of self-proposed task RL:

1. The model proposes tasks it can already almost solve (to hit 0.2 success rate)
2. These tasks cluster around the model's existing capability distribution
3. Tasks the model cannot conceptualize (truly novel problem structures) are never proposed
4. The model optimizes for solvability, not for real-world impact
5. Result: strong on benchmarks that resemble the self-proposed task distribution, weak on tasks that require fundamentally different reasoning

This is why the Frontier-Bench and NL2Repo gaps matter more than the headline Terminal-Bench score — they measure capabilities outside the self-proposed distribution.

## Comparison with GLM-5.3's Environment Scaling

GLM-5.3 (Z.ai, August 2026) takes a different approach to the same bottleneck:

| Dimension | GLM-5.3 | Ornith-1.5 |
|-----------|---------|------------|
| Environment source | Research agents collect task patterns from real work | Model generates its own tasks |
| Verifier source | Synthesized without reference solution | Synthesized without reference solution |
| Reward shortcut closing | Solver trajectories discover shortcuts | Solver trajectories discover shortcuts |
| Task diversity | Bounded by real work patterns | Bounded by model's task distribution |
| Human-in-the-loop | Yes (meaningful amount) | Minimal |
| Failure mode | Limited by real work coverage | Limited by model's conceptual range |

Both approaches share the verifier synthesis and shortcut-closing mechanisms, but differ on task source: GLM-5.3 derives tasks from real professional work (more realistic, less scalable), Ornith derives tasks from the model itself (more scalable, less grounded).

## License and Availability

All three sizes ship under MIT license in BF16, FP8, NVFP4, GGUF, and MLX builds. No hosted API, no published price list. Weights on Hugging Face.

- 397B: ~800GB in BF16, requires 8-way tensor parallelism (8× H200 141GB)
- 35B: near-flagship agentic coding on affordable hardware
- 9B: includes quantized Mobile build for iPhone and Android

## Serving Requirements

- Transformers 5.8.1, vLLM 0.19.1, or SGLang 0.5.9+
- 397B vLLM recipe: 8-way tensor parallelism on one node (8× H200)
- GGUF builds run through Ollama and llama.cpp
- OpenAI-compatible tool calling for agent CLIs (OpenCode)

## A-Tech Values Alignment

- **Open-source AI:** MIT license across all sizes — most permissive option in the August 2026 model wave
- **Data privacy:** Self-hostable; no API dependency; GGUF builds for local deployment
- **Financial freedom:** 35B MoE (3B active) delivers near-flagship coding on affordable hardware; eliminates API costs
- **Practical implementation:** The self-proposed task RL method is a concrete, implementable pipeline — not just a theoretical framework

## Composes With
- `open-weight-agentic-model-wave-august-2026` — Ornith-1.5 is part of the August 2026 wave
- `harness-engineering-ai-agents-2026` — reward hacking prevention aligns with Stop Hook and human-as-API patterns
- `spec-driven-development-framework` — the verifier synthesis pattern parallels spec-driven verification
- `verifiability-driven-automation` — the three-check validation (oracle/no-op/unsolved-state) is a verifiability pattern