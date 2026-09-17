# Lego-RL: Evidence Base

## Paper Details

- **Title**: LEGO-RL: Harness-Native Reinforcement Learning for Coding Agents
- **Authors**: Yiming Du, Yuxin Jiang, Tao Yuan, Jianbo Dai, Shaowei Wang, Jierun Chen, Chaofan Tao, Xianzhi Yu, Lifeng Shang, Kam-Fai Wong, Xiaohui Li, Haoli Bai
- **arXiv**: 2608.17393 (August 2026)
- **License**: Apache 2.0
- **Code**: github.com/LegoX/Lego-RL
- **Docs**: lego-rl.pages.dev/docs

## Headline Results

Model: Qwen3.5-35B-A3B, trained for 3 epochs (126 steps) on 2,699-task OpenSWE-derived index.

### SWE-bench Verified Results

| Harness | Base Model | After Lego-RL | Gain |
|---------|-----------|--------------|------|
| OpenHands SDK | 64.0% | 70.4% | +6.4 |
| Claude Code | 62.4% | 68.2% | +5.8 |
| OpenCode | 57.2% | 66.6% | +9.4 |

Evaluation protocol: temperature 0.7, 200 turns, 200K context budget, same harness version.

### Cross-Harness Comparison

| Model | OpenHands SDK | Claude Code | OpenCode |
|-------|--------------|-------------|----------|
| Qwen3.5-35B-A3B (base) | 64.0% | 62.4% | 57.2% |
| Qwen3.6-35B-A3B (next gen) | 67.4% | 63.4% | 60.6% |
| KAT-Coder-V2.5-Dev (post-trained) | 67.0% | 66.8% | 61.2% |
| **Lego-RL-Qwen3.5-35B-A3B** | **70.4%** | **68.2%** | **66.6%** |

### KAT-Coder Harness-Specific Gains

KAT-Coder-V2.5-Dev (post-trained from Qwen3.5-35B-A3B):
- Claude Code (its authors' harness): +3.4 points over base
- OpenCode: +0.6 points over base
- OpenHands SDK: -0.4 points over base

This asymmetry demonstrates harness specificity: a model post-trained for one harness does not transfer evenly to others.

## Architecture Details

### Agent Loop Interface

The framework connects three native coding agents through thin adapters:

1. **Claude Code** — Anthropic's CLI coding agent
2. **OpenHands** — All-Hands-AI's open-source agent
3. **OpenCode** — sst's open-source coding agent

Custom scaffolds speaking OpenAI or Anthropic API can use the same agent-loop interface.

### In-Process Proxy

Records token ids, masks, and log-probabilities at generation time. Key features:
- History rewrites (for multi-turn conversations)
- Serves OpenAI and Anthropic interfaces used by supported agents
- Faithful rollout capture (no approximation)

### RL Backends

| Algorithm | Frameworks |
|-----------|------------|
| PPO | FSDP, VeOmni, Megatron |
| GRPO | FSDP, VeOmni, Megatron |
| GSPO | FSDP, VeOmni, Megatron |

Training modes:
- Synchronous
- Fully asynchronous

MoE support: R3 routing replay
Trajectory filtering: removes broken or over-long rollouts from loss

### Sandbox Backends

| Backend | Use Case |
|---------|----------|
| Kubernetes | Multi-node, production |
| Docker | Single-machine, development |

Task execution in isolated Harbor environments:
- Task verifier provides reward
- Image caching available
- Reward-hacking checks for supported task sets

## Training Setup

### Demo Run (1/16 scale)

```bash
# 8 prompts × 4 responses = 32 trials/step
git clone https://github.com/LegoX/Lego-RL.git && cd Lego-RL
bash scripts/setup_env.sh

cp scripts/train/examples/demo.env scripts/train/configs/demo.env
# Edit CHANGEME values: checkpoint, train/val index, kubeconfig

PREFLIGHT_ONLY=1 bash scripts/train/train.sh scripts/train/configs/demo.env  # validate
bash scripts/train/train.sh scripts/train/configs/demo.env                  # launch
```

Requirements: 8× A100/H100-class GPUs, uv, policy checkpoint, two task indexes, Kubernetes cluster.

### Claude Code Integration

```bash
# Within Claude Code session
/rl:run scripts/train/configs/demo.env
/rl:status
/rl:dashboard
```

### Full-Scale Training

- 256 prompts × 16 responses per step
- 3 epochs (126 steps for 2,699-task index)
- Model: Qwen3.5-35B-A3B

## Live Dashboard

```bash
bash webui/start_dashboard.sh
```

Features:
- Training curves
- Validation metrics
- Per-task results
- Individual trajectory inspection

## Key Insight: Harness as Part of Policy

The evidence from Lego-RL aligns with a broader pattern in the DevEx research cluster:

1. **Tool architecture affects behavior** (Xu et al., arXiv:2608.11386): 6 architectures × 3 actors × 11,700 trajectories show Atomic tools improve consistency 4.7x
2. **Multi-agent coordination is task-shaped** (Destefanis & Aste, arXiv:2608.16801): Task shapes the network, not coordinator assignment
3. **Prior AI exposure moderates adoption** (Agarwal, He & Vasilescu, MSR '26): Agent-first vs IDE-first repos show different velocity/quality profiles
4. **Documentation behavior is harness-dependent** (Gao & Chen, arXiv:2608.20195): Agents prefer agent-facing artifacts 60.5% over classical docs

Lego-RL extends this pattern to **training**: the harness is not just deployment context but training context. A model trained in harness X is optimized for harness X's interface, control flow, and error patterns.

## Why Harness-Native Beats Harness-Agnostic

### The Transfer Gap

Standard RL post-training:
1. Train model on generic coding tasks
2. Deploy in specific harness
3. Expect transfer

Problem: the harness changes the task distribution. Tool formats, context management, retry logic, and error handling all shape what "good" looks like.

### Lego-RL's Solution

1. Train model IN the target harness
2. Reward = task verifier outcome (harness-mediated)
3. Model learns harness-specific patterns

Evidence: same model (Qwen3.5-35B-A3B) scores 64.0% / 62.4% / 57.2% across three harnesses BEFORE training. After harness-native training: 70.4% / 68.2% / 66.6%. The base variation across harnesses (6.8pp spread) narrows after training.

## Limitations

1. **GPU requirements**: 8× A100/H100 for training
2. **Three harnesses supported**: Claude Code, OpenHands, OpenCode (extending requires adapter)
3. **Training cost**: Dominated by sandbox execution (real repository tasks are slow)
4. **Benchmark scope**: SWE-bench Verified only; other benchmarks untested
5. **Asynchronous mismatch**: Policy-version mismatch mitigated by truncated importance sampling

## Code Availability

- **Source**: github.com/LegoX/Lego-RL
- **License**: Apache 2.0
- **Built on**: verl (RL training), Harbor (sandboxed execution)
- **Agent support**: Claude Code, OpenHands, OpenCode

## Citation

```bibtex
@misc{du2026legorlharnessnativereinforcementlearning,
  title={LEGO-RL: Harness-Native Reinforcement Learning for Coding Agents},
  author={Yiming Du and Yuxin Jiang and Tao Yuan and Jianbo Dai and Shaowei Wang and Jierun Chen and Chaofan Tao and Xianzhi Yu and Lifeng Shang and Kam-Fai Wong and Xiaohui Li and Haoli Bai},
  year={2026},
  eprint={2608.17393},
  archivePrefix={arXiv},
  primaryClass={cs.AI},
  url={https://arxiv.org/abs/2608.17393},
}
```