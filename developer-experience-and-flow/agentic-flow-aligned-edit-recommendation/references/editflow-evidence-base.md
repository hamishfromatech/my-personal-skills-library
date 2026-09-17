# Agentic Flow-Aligned Edit Recommendation — Evidence Base

## Primary Source
**Liu, C., Lin, Y., Chang, J., Liu, J., Qi, B., Jiang, B., Huang, Z., Dong, J. S. (2026).** "EditFlow: Benchmarking and Optimizing Code Edit Recommendation Systems via Reconstruction of Developer Flows." *Proc. ACM Program. Lang.* 10, OOPSLA1, Article 141.

## Key Empirical Findings

### Flow Violation Prevalence (50 real-world Python commits)
| System | Keep (%) | Jump (%) | Revert (%) | Break (%) |
|---|---|---|---|---|
| Cursor | 28.23 | 9.84 | 6.45 | 55.48 |
| Claude Code | 34.16 | 7.82 | 6.79 | 51.23 |

**68.81% of model recommendations disrupt developers' ongoing mental flow** (Jump + Revert + Break), including 8.83% that are technically correct but ill-timed (Jump).

### Edit Order Recovery Accuracy (RQ1)
| Method | Accuracy (%) | Precision (%) | F1 (%) |
|---|---|---|---|
| Zero-shot | 50.63 | 82.22 | 61.67 |
| Few-shot | 47.42 | 77.13 | 57.96 |
| Hand-crafted | 53.27 | 80.96 | 60.93 |
| DSPy | 53.39 | 62.44 | 57.37 |
| **Auto-tuned** | **87.26** | **88.01** | **87.54** |

63.81% relative improvement over the best baseline.

### Real-World Alignment (RQ2, 500 industrial commits, 3,059 hunks)
| Method | Violations |
|---|---|
| Zero-shot | 195 |
| Few-shot | 203 |
| Hand-crafted | 121 |
| **Auto-tuned** | **30** |

75%+ reduction in violations vs best baseline.

### Flow-Aware Optimization (RQ3, large-scale benchmark)

**Cursor:**
- Keep: 24.00% → 38.49% (+60.4%)
- Break: 60.15% → 51.13% (−15.0%)
- Precision: 33.02% → 42.42% (+28.5%)
- F0.5: 34.22 → 41.45 (+21.1%)

**Claude Code:**
- Keep: 30.76% → 46.89% (+52.4%)
- Break: 53.49% → 45.52% (−14.9%)
- Precision: 40.54% → 50.45% (+24.4%)
- F0.5: 40.32 → 46.75 (+16.0%)

**CoEdPilot:**
- Keep: 13.30% → 33.18% (+149.7%)
- Break: 82.91% → 62.57% (−24.5%)
- Precision: 14.78% → 35.50% (+140.1%)

Average across systems: 66.99% precision improvement, 87.42% boost to flow-keeping edits, 22.42% drop in flow-breaking edits.

### User Study (RQ4, 32 participants, 3 tasks)
- **Task 1 (moderate):** EG2 statistically significant over CG2 (p=0.0318, r=0.525); EG1 marginal over CG1
- **Task 2 (hard):** EG1 statistically significant over CG1 (p=0.0004, r=0.788); EG2 consistent trend (p=0.0004, r=0.840)
- **Task 3 (easy, order-insensitive):** No significant difference (boundary case — nearly any edit ordering aligns with flow)
- **Overall: 25.11% faster task completion** and significantly higher perceived recommendation quality

### Resource Overhead
- Additional latency: 1.71s average (below ~2s flow-disruption threshold)
- Additional tokens: 6.58K average
- Additional cost: $0.03/query average

## Supporting Context: The Productivity Paradox

### Becker et al. (2025, arXiv:2507.09089)
"Measuring the Impact of Early-2025 AI on Experienced Open-Scale Developer Productivity"
- Developers using Cursor Pro with Claude models completed tasks **19% slower** than working unaided
- Despite Claude 3.5 Sonnet achieving 92.0% on HumanEval and 70.3% on SWE-Bench Verified
- **Key insight: accuracy is not equivalent to productivity** — flow disruption explains the disconnect

### Agarwal, He & Vasilescu (2026, MSR '26)
"AI IDEs or Autonomous Agents? Measuring the Impact of Coding Agents on Software Development"
- Longitudinal causal study of agent adoption in open-source repositories (staggered DiD)
- **Velocity:** Large gains only when agents are the first AI tool (+36.3% commits, +76.6% lines); minimal if IDE-AI already present (+3.1%, −6.3%)
- **Quality:** Static analysis warnings +18%, cognitive complexity +39% regardless of prior AI exposure
- **Conclusion:** Agent-induced complexity debt — agents accelerate code introduction that raises long-term cognitive and maintenance load

### Bhati (2026, arXiv:2604.26275)
- SWE-bench Verified: 1.96% (Oct 2023) → 78.4% (Apr 2026)
- Productivity: 13.6%–55.8% time savings across controlled studies
- 49% of jobs saw AI used for at least a quarter of their tasks (Anthropic, 2026)
- Open problem: "the economics of attention" — if an agent produces 10 plausible patches/hour, the rate-limiting resource is human review

## Implications for A-Tech

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | EditFlow is implemented as a VS Code extension wrapping existing assistants — open-source compatible |
| **Practical implementation** | 25.11% faster task completion with $0.03/query overhead — high ROI, low cost |
| **Developer experience** | Flow preservation is the missing dimension in DevEx measurement; SPACE/DevEx frameworks treat flow as first-class but don't measure edit-level flow disruption |
| **Financial freedom** | Reducing flow disruption directly increases developer throughput — the economic value of flow-aware tooling |