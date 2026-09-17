---
name: fglguard-federated-multi-agent-safety
description: Applies FGLGuard — the first privacy-preserving federated graph learning framework for LLM-based multi-agent system safety — to deploy AI-safety guards across organizations WITHOUT pooling prompts, tool outputs, or proprietary workflows. Use when building multi-agent safety detectors across silos, defending agent fleets from prompt injection at scale, or designing cross-organization AI governance that cannot share raw episode data. NOT for single-organization safety evaluation with pooled data (use centralized GNN safeguards) or for model-level safety fine-tuning.
---

# FGLGuard: Federated Graph Learning for Multi-Agent Safety

## Overview
FGLGuard (Yu, Jiang, Li, Liu, Zhang, Zhao, Yu, Chang & Wu, arXiv 2609.02967, Sept 2, 2026; UCLA/UW/UCSD) recasts privacy-preserving safeguarding of LLM multi-agent systems as **graph federated learning**: each operator trains an edge-featured graph attention detector on its own judge-labeled episode graphs and shares only model updates — episodes contain private prompts, tool outputs, and proprietary workflows that no silo should surrender, and no silo alone sees the full attack distribution.

## When to Use
- Building safety detectors for agent fleets across organizations that legally cannot pool episode traces
- Designing cross-silo AI-safety consortia (healthcare agents, financial agents, government agents)
- Auditing whether a deployed MAS safeguard survives distribution shift
- NOT for single-operator deployments where pooling all traces is legal and cheap
- NOT for guardrail prompt-engineering on a single model (no graph topology involved)

## Core Process / Workflow
1. **Cast the safeguard as a graph problem**: agents are nodes; inter-agent communication is edges with edge features; risky agents are localized via the communication graph.
2. **Each operator trains locally** an edge-featured graph attention detector on its own judge-labeled episode graphs — no raw episode leaves the silo.
3. **Share only model updates** using a proximal local objective (non-IID clients) + domain-balanced aggregation.
4. **Calibrate the block threshold** with an over-refusal constraint (safety must not destroy utility).
5. **Corroborate upstream scoring** before a guarded rewrite for blocked answers.
6. **Federation is NOT optional** — the key empirical finding: off-the-shelf transfer collapses under distribution shift (AUROC 0.51–0.70 only after in-domain retraining). A deployable guard must adapt on each site's private traces.
7. Validate across Agent-SafetyBench, R-Judge, and AgentDojo.

## Key Evidence
- Federated FGLGuard **exceeds the in-domain centralized ceiling on all three benchmarks** without pooling any data — where unsupervised anomaly guards and local-only training fail.
- One guard federated across four different-domain operators comes within **0.03 AUROC of multi-domain centralization**; any single-domain guard collapses on the others.
- Live FGLGuard cuts AgentDojo's ground-truth attack-success rate by **43% at near-unguarded utility, zero API cost, and negligible capability loss**.

## Pairs with
`federated-learning-for-privacy-preserving-ai`, `agentic-supply-chain-exploit-defense`, `adaptive-dp-fl-concept-drift-edge`, `ai-agent-evaluation-framework-2026`, `coder-agent-relay-regulated-deployment`

## A-Tech Alignment
- **Open source**: the federated pattern is deployable without enterprise licensing; graph attention detectors are standard PyTorch Geometric constructs.
- **Privacy**: the whole point — safety coordination without sharing prompts or tool outputs. First-party judge labels only.
- **Financial freedom**: zero API cost for the guard layer (43% attack reduction at zero API cost) removes the safety-tax argument.
- **Practical**: the distribution-shift finding (transfer collapses; federate or fail) is the single most actionable rule for any multi-org safety deployment.

*Source: arXiv:2609.02967v1, Sept 2 2026.*