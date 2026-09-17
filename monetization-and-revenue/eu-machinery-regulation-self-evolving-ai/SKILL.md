---
name: eu-machinery-regulation-self-evolving-ai
description: Applies Europe's Machinery Regulation (replaces the Machinery Directive, effective Jan 20, 2027) as the SELF-EVOLVING-BEHAVIOUR REGIME pattern — the first regulatory category written around machine learning: machinery with self-evolving behavior and AI-based safety functions now require notified-body conformity assessment instead of manufacturer self-declaration, and a model that performs tasks absent from its training data (e.g., Skild AI's S1 learning a task from a single video) sits at the center of the definition. Use when [assessing EU compliance exposure for AI-driven robotics or adaptive machinery, designing robotics products for the EU market, or evaluating robot-foundation-model vendors against the notified-body timer]. NOT for [general AI Act GPAI obligations — use eu-ai-act-developer-compliance-2026 — or EU cyber-resilience reporting — use eu-cyber-resilience-act-compliance-2026].
---

# The Machinery Regulation: Self-Evolving Behaviour Enters Law

## Overview
Europe's Machinery Regulation replaces the Machinery Directive on **January 20, 2027**, and for the first time regulates **AI-based safety functions** and **machinery with self-evolving behaviour**. Both now need a **notified body** rather than a manufacturer's own declaration. Nobody has assessed one of these machines yet — the regime does not start for sixteen months (as of the Sept 10, 2026 reporting).

The regulatory definition is precise: a machine that *changes what it does after it ships*. A robot-foundation-model vendor whose selling point is performing work absent from its training data is close to the center of the definition.

## When to Use
- Assessing EU market-entry compliance for robotics, humanoid systems, industrial automation, or any ML-based safety function
- Evaluating robotics/AI vendors against the notified-body timeline (whether they sell into Europe, what conformity path they will need)
- Designing products that must remain within a fixed post-deployment behavior envelope (the compliance-safe alternative)
- NOT for: general-purpose AI Act obligations (`eu-ai-act-developer-compliance-2026`), CRA vulnerability reporting (`eu-cyber-resilience-act-compliance-2026`), medical-device or automotive regimes

## Core Process / Workflow

### The mechanism
1. **Learning-after-deployment is the trigger, not "contains AI."** The regime distinguishes machines whose behavior is fixed at ship-time (traditional conformity) from machines that keep adapting. Self-evolution + safety function = notified body.
2. **Evidence shifts from design documents to empirical behavior.** Conformity assessment must now handle a machine whose failure modes include behaviors that did not exist in training data — the notified body must observe, not just review.
3. **The market signal is vendor silence.** Skild AI — $100M run-rate, hundreds of robots at 60+ companies, S1 model learning tasks from a single video (96% on seen tasks, 66% on unseen, one demo ≈ 380 post-training examples) — has not said whether it sells into Europe. The compliance question is now a market-segmentation question: which vendors' products can pass a notified-body assessment, and which will route around Europe.

### The assessment ladder
| Machine class | Regime | Evidence |
|---|---|---|
| Fixed post-deployment behavior, no AI safety function | Manufacturer self-declaration | Design + test docs |
| AI-based safety function | Notified body | Empirical validation of the safety function |
| Self-evolving behavior | Notified body | Post-deployment change governance + validation of out-of-distribution behavior |

### Design responses
- **Constrain the envelope:** freeze or gate model updates post-ship; keep adaptation within assessed bounds (this is where federated/on-device update controls become a compliance surface, not just a privacy feature)
- **Separate the safety layer:** keep certified deterministic safety functions separate from the adaptive policy model, so the safety function is assessable without assessing general learning
- **Plan the evidence pipeline:** logging, incident traces, and post-deployment monitoring become conformity-assessment artifacts under this regime

## References
- Nearest neighbors: `eu-ai-act-developer-compliance-2026`, `eu-cyber-resilience-act-compliance-2026` (the EU regulatory family), `humanoid-embodied-ai-open-stack-2026` (the open robotics stack this regime will assess), `agentic-coding-production-characterization` (post-deployment behavior evidence as a general pattern).
- Honest caveats: regime is enacted but not yet enforced (Jan 20, 2027); no notified-body assessment of a self-evolving AI machine has occurred, so practical thresholds are unproven; scope is EU market placement — non-EU deployments are unaffected unless they ship into Europe; "self-evolving" definitions will be tested in notified-body guidance, not the regulation text.

*Source: TNW, "Skild AI hit $100M in run-rate revenue selling robots a brain that learns from one video" (Sept 10, 2026), covering Bloomberg's revenue report and the Machinery Regulation timeline; EU Machinery Regulation (2023/1230), application from Jan 20, 2027.*