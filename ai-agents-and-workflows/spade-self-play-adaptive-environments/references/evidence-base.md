# SPADE: Evidence Base

## Paper Details

- **Title**: SPADE: Self-Play in Adaptive Synthetic Executable Environments
- **Authors**: Bo Liu, Simon Yu, Yiding Jiang, Ao Qu, Andrew Zhao, Zichen Liu, Junsu Kim, Zijian Zhou, Seungone Kim, Tongzheng Ren, Mickel Liu, Hanfei Yu, Zhaorun Chen, Weiyan Shi, Paul Pu Liang, Luke Zettlemoyer, Yejin Choi, Natasha Jaques
- **arXiv**: 2608.19197 (August 2026)
- **License**: MIT
- **Code**: github.com/spade-rl/spade
- **Models & Data**: huggingface.co/spade-rl
- **Paper**: arxiv.org/abs/2608.19197

## Core Architecture

### Shared Policy, Two Roles

A single language model plays both roles in each cycle:

**Environment Designer:**
1. Samples grounding context from pretraining corpus + environment memory
2. Writes complete executable environment (Python with reset() and step() interfaces)
3. Generates privileged hint
4. Generated code passes structural and runtime validation before entering training pool

**Reasoning Agent:**
1. Plays each environment WITH the hint
2. Plays each environment WITHOUT the hint
3. Task return trains the agent role
4. Hint-based regret trains the designer role
5. Per-role advantage normalization keeps joint update stable

### Hint-Based Regret

The key mechanism for adaptive curriculum:

- **Regret** = Return_with_hint - Return_without_hint
- High regret → environment is at capability frontier (hint helps significantly)
- Zero regret → too easy (both succeed without hint) or too hard (both fail with hint)
- Designer trained to maximize regret → generates frontier-calibrated environments
- This steers generation toward environments at the agent's capability frontier while keeping them feasible

### Per-Role Advantage Normalization

Since one policy plays both roles:
- Agent role advantage: from task return
- Designer role advantage: from hint-based regret
- Normalization prevents one role's gradient from dominating the other

## Backend Architecture

### Slime/SGLang Integration (Primary)
- SGLang: inference
- Megatron-LM: policy updates
- Ray: orchestration
- Located in spade/core/ and spade/slime/

### Tinker Integration (Alternative)
- Thinking Machines' distributed training framework
- Located in spade/tinker/
- Supports: Qwen3-4B-Instruct, Qwen3-8B, Qwen3-8B-Base, Qwen3-30B-A3B-Instruct, GPT-OSS-20B

## Supported Models

| Family | Models | Trained in Paper |
|--------|--------|-----------------|
| **Qwen3** | Qwen3-4B-Instruct-2507, Qwen3-8B, Qwen3-30B-A3B-Instruct-2507, Qwen3-32B | 4B, 8B, 30B-A3B |
| Qwen3.5 | Qwen3.5-4B, Qwen3.5-9B, Qwen3.5-35B-A3B | — |
| GPT-OSS | gpt-oss-20b, gpt-oss-120b | — |
| Nemotron | NVIDIA-Nemotron-3-Nano-30B-A3B-BF16, NVIDIA-Nemotron-3-Super-120B-A12B | — |
| GLM | GLM-5.2, GLM-5.3 | — |

Model-agnostic: any chat-capable policy the backend can fine-tune.

## Training Settings

### Two Settings

1. **Cognitive games**: Multi-turn reasoning games with verifiable win/loss conditions
2. **Multi-turn tool use**: Tool-calling tasks with executable verification

### Training Commands

```bash
# Installation
git clone --recurse-submodules git@github.com:spade-rl/spade.git && cd spade
python -m venv .venv && source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -e .
# Python 3.10-3.12 supported

# SPADE games self-play
bash cmd/games/train_spade_4b.sh
bash cmd/games/train_spade_8b.sh
bash cmd/games/train_spade_30b.sh

# SPADE tool use self-play
bash cmd/tool_use/train_spade_4b.sh
bash cmd/tool_use/train_spade_8b.sh
bash cmd/tool_use/train_spade_30b.sh

# Fixed-environment baselines (for comparison)
bash cmd/games/train_fixed_gpt55_4b.sh   # GPT-5.5 static corpus
bash cmd/games/train_fixed_gpt55_8b.sh
bash cmd/games/train_fixed_gpt55_30b.sh
bash cmd/games/train_fixed_rlve_4b.sh    # RLVE corpus
bash cmd/games/train_fixed_rlve_8b.sh
bash cmd/games/train_fixed_rlve_30b.sh

# Ablations (30B-A3B)
bash cmd/ablations/*.sh
```

### Environment Variables

```bash
export MODEL_ROOT=/path/to/model/checkpoints
export WORKSPACE_DIR=/path/to/spade/workspace
export CORPUS_FILE=/path/to/grounding.jsonl
export WANDB_API_KEY=...
export WANDB_ENTITY=your-wandb-entity
```

### Training Scale

- 400 rollouts on single 8-GPU node for 30B-A3B
- GRPO algorithm
- Both roles trained simultaneously

## Released Artifacts

### August 20, 2026
- SPADE checkpoints: 4B, 8B, 30B-A3B (games and tool use)
- Grounding corpora
- Generated environments on Hugging Face (spade-rl)

### August 11, 2026
- Self-play codebase
- Static GPT-5.5 environment corpus

### Static GPT-5.5 Corpus
- 7,872 validated Python environments
- Six cognitive skills
- Pinned revision with per-environment SHA-256 checksums
- Apache 2.0
- Available: huggingface.co/datasets/spade-rl/SPADE-Environment-Pool-GPT5.5-Games

## Evaluation

```bash
# Offline benchmark suites
python -m eval_offline.run_offline_eval --help

# Format results
python -m eval_offline.render_table --help
```

Benchmark suites: AIME, BFCL (Berkeley Function Calling Leaderboard), PRIME evaluation utilities.

## Key Results

### Outperforming Fixed-Environment Baselines

SPADE models improve on held-out math, science, code, and procedural-reasoning benchmarks **past the saturation point** of fixed-environment baselines.

The critical finding: fixed-environment baselines (GPT-5.5 corpus, RLVE corpus) saturate — the model masters all available environments and learning stops. SPADE's adaptive curriculum keeps generating new environments at the capability frontier, enabling continued improvement.

### Adaptive Curriculum Properties

1. **Progressive difficulty**: Designer produces harder environments over training
2. **Increased interactivity**: Environments become more multi-turn over time
3. **Frontier-tracking**: Difficulty follows the agent's improving capability
4. **No saturation**: Unlike fixed pools, the curriculum evolves with the learner

## Comparison with Related Approaches

### vs Fixed-Environment RLVR

| Dimension | Fixed-Environment RLVR | SPADE |
|-----------|----------------------|-------|
| Environment source | Hand-built pool | Model-generated |
| Adaptivity | None | Hint-based regret |
| Saturation risk | High | Low |
| Curriculum | Static | Adaptive |
| Coverage | Limited to pool | Open-ended |

### vs Ornith Self-Proposed Task RL

| Dimension | Ornith | SPADE |
|-----------|--------|-------|
| Task generation | Model proposes tasks | Model designs environments |
| Reward signal | Validity × novelty × difficulty | Hint-based regret |
| Frontier calibration | Target 0.2 success rate | Regret = with/without hint gap |
| Environment type | Tasks | Executable Python (reset/step) |
| Risk | Solvability optimization | Same risk (self-proposed) |
| Scale | 397B/35B/9B | 4B/8B/30B-A3B |

Key distinction: SPADE's hint-based regret directly measures whether the environment is at the capability frontier. Ornith's validity × novelty × difficulty is a proxy that may generate solvable-but-irrelevant tasks.

### vs IBM Granite 4.2 Real-Sandbox RL

| Dimension | Granite 4.2 | SPADE |
|-----------|-------------|-------|
| Environment source | Real repos, shells, web | Synthetic Python |
| Task authenticity | Real-world | Synthetic |
| Adaptivity | Fixed task set | Adaptive curriculum |
| Transfer risk | Low (real distribution) | Medium (synthetic) |
| Reproducibility | Documented harness | Model-generated |

These approaches are complementary: SPADE for scalable curriculum generation, Granite 4.2 for real-world task distribution.

## Ablation Studies

Available in cmd/ablations/ for 30B-A3B. Key ablations:
- Without hint-based regret (designer reward = task return only)
- Without per-role normalization
- Without grounding corpus
- Different regret thresholds

## Limitations

1. **Self-proposed task risk**: Optimizes for solvability/frontier-calibration, not real-world impact (shared with Ornith)
2. **Synthetic environments**: Python reset()/step() may not capture real-world messiness
3. **Minimum capability**: Self-play requires 4B+ parameters for meaningful environment design
4. **Grounding dependency**: Designer quality depends on grounding corpus quality
5. **Safety review needed**: Open-ended generation can produce unexpected environments
6. **Training cost**: 400 rollouts per step on 8-GPU node for 30B

## Citation

```bibtex
@article{liu2026spade,
  title={SPADE: Self-Play in Adaptive Synthetic Executable Environments},
  author={Liu, Bo and Yu, Simon and Jiang, Yiding and Qu, Ao and Zhao, Andrew and Liu, Zichen and Kim, Junsu and Zhou, Zijian and Kim, Seungone and Ren, Tongzheng and Liu, Mickel and Yu, Hanfei and Chen, Zhaorun and Shi, Weiyan and Liang, Paul Pu and Zettlemoyer, Luke and Choi, Yejin and Jaques, Natasha},
  journal={arXiv preprint arXiv:2608.19197},
  year={2026}
}
```

## Acknowledgments

- Distributed RL: Slime (THUDM), SGLang, Megatron-LM
- Miles (radixark) RL post-training framework
- Thinking Machines: Tinker framework, tinker-cookbook
- Modal: compute and model serving
- Evaluation: RLVE, Berkeley Function Calling Leaderboard, PRIME
- Base models: Qwen3