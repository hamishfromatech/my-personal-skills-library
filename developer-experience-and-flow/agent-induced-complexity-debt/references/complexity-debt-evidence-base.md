# Agent-Induced Complexity Debt — Evidence Base

## Primary Source
**Agarwal, S., He, H., & Vasilescu, B. (2026).** "AI IDEs or Autonomous Agents? Measuring the Impact of Coding Agents on Software Development." *23rd International Conference on Mining Software Repositories (MSR '26)*, April 2026, Rio de Janeiro. DOI: 10.1145/3793302.3793589

### Methodology
- **Dataset:** AIDev dataset (v3) linking GitHub repositories to AI-generated pull requests
- **Design:** Staggered difference-in-differences (DiD) with propensity-score matching
- **Treatment:** First agent-generated pull request (Claude, Copilot, Cursor, Devin, Codex, Jules, OpenHands, Codegen, Cosine, Tembo)
- **Samples:** 401 Agent-First (AF) repos matched to 606 controls; 117 IDE-First (IF) repos matched to 73 controls
- **Outcomes:** Commits, lines added, static-analysis warnings, duplicate line density, cognitive complexity, comment density (via SonarQube)
- **Estimator:** Borusyak et al. (2021) imputation-based DiD (avoids biases from traditional two-way fixed effects under staggered adoption)

### Results (Average Post-Adoption Treatment Effects)

| Outcome | AF β | AF SE | AF % Change | IF β | IF SE | IF % Change |
|---|---|---|---|---|---|---|
| Commits | 0.309*** | 0.051 | +36.25% | 0.030 | 0.092 | +3.06% |
| Lines Added | 0.569*** | 0.103 | +76.59% | −0.066 | 0.189 | −6.34% |
| Duplicate Line Density | 0.076 | 0.044 | +7.92% | −0.009 | 0.056 | −0.94% |
| Comment Line Density | 0.042 | 0.028 | +4.34% | 0.201*** | 0.055 | +22.30% |
| Static Analysis Warnings | 0.163*** | 0.048 | +17.73% | 0.174 | 0.114 | +19.00% |
| Code Complexity | 0.299*** | 0.059 | +34.85% | 0.357** | 0.114 | +42.87% |

### Dynamic Effects (6-month event study)
- **AF velocity:** Spike at t=0 (+111% commits, +216% lines), persisting (+49–109% lines through t=6)
- **IF velocity:** Short-lived bump (+16–28% at t=0–2), then near-zero or negative (~−61% lines, ~−35% commits by t=6)
- **AF complexity:** +20.7% at t=0 → ~+49% by t=5; persistent
- **IF complexity:** Elevated at t=−2, ~+15–62% through t=6; persistent
- **AF warnings:** ~+22–31% by t=4–5
- **IF warnings:** ~+25% at t=4–6

### Key Conclusion
"Autonomous agents offer meaningful velocity gains only in new-to-AI settings while consistently raising complexity and warning levels across contexts, reinforcing a speed-maintainability trade-off. Prior exposure to AI IDEs moderates benefits but not risks, underscoring the need for selective deployment and active oversight."

## Supporting Evidence

### He, Miller, Agarwal, Kastner & Vasilescu (2026, MSR)
"Speed at the Cost of Quality: How Cursor AI Increases Short-Term Velocity and Long-Term Complexity in Open-Source Projects"
- Prior work on Cursor IDE adoption: modest productivity gains, mixed quality effects
- Autonomous agents amplify the speed-maintainability trade-off relative to IDE-based tools

### Becker, Rush, Barnes & Rein (2025, arXiv:2507.09089)
"Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity"
- Controlled experiment: experienced open-source developers using Cursor Pro with Claude models
- **19% slower** task completion vs working unaided
- Despite strong benchmark performance (Claude 3.5 Sonnet: 92% HumanEval, 70.3% SWE-Bench Verified)
- Accuracy ≠ productivity; flow disruption explains the disconnect

### Bauer et al. (2025, arXiv:2510.10165)
"AI-Assisted Programming May Decrease the Productivity of Experienced Developers by Increasing the Technical Debt and Maintenance Burden"
- Agents biased toward producing more code (production is cheap) and local fixes (global redesign is expensive)
- Long-term studies of repository health under sustained agent contribution urgently needed

### Watanabe et al. (2025, arXiv:2509.14745)
- Maintainers merge 83.8% of 567 Claude Code pull requests
- High merge rate, but quality concerns persist beneath the surface

### Bhati (2026, arXiv:2604.26275)
"Agentic AI in the Software Development Lifecycle"
- SWE-bench Verified: 1.96% (Oct 2023) → 78.4% (Apr 2026)
- Productivity: 13.6%–55.8% time savings across controlled studies
- **Open problem #3: The Technical-Debt Hypothesis** — "Agents are biased toward producing more code (because production is cheap) and toward local fixes (because global redesign is expensive in tokens). Long-term studies of repository health under sustained agent contribution are urgently needed."

### Xu et al. (2025, arXiv:2510.10165)
"AI-Assisted Programming May Decrease the Productivity of Experienced Developers by Increasing Maintenance Burden"
- Complementary evidence that maintenance burden erodes short-term velocity gains

## Ethical Considerations
"Increased automation shifts accountability and maintainability burdens; provenance tracking, transparency of agent-generated changes, and review practices that emphasize human oversight are still essential to avoid debt." (Agarwal et al., 2026)

## Implications for A-Tech

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | The study uses open-source data (AIDev, GHArchive); reproducible methodology; replication package publicly available |
| **Practical implementation** | Concrete mitigation framework (complexity-aware review, refactoring mandates, automated test gates, selective deployment) |
| **Developer experience** | Complexity debt is a DevEx tax — cognitive load rises, review burden increases, ownership of code erodes |
| **Financial freedom** | Complexity debt has real economic cost (maintenance burden, slower feature delivery over time); quantifying it enables informed adoption decisions |