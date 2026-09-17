# Evidence Base: Agentic Coding Prior AI Exposure Moderation

## Source
Agarwal, He & Vasilescu (Carnegie Mellon University, MSR '26)
arXiv:2601.13597v2, published at MSR '26 (23rd International Conference on Mining Software Repositories, April 2026, Rio de Janeiro)

## Study Design
- **Method:** Staggered difference-in-differences (DiD) with propensity-score matched controls
- **Estimator:** Borusyak et al. (2021) imputation-based DiD for staggered adoption
- **Dataset:** AIDev dataset (v3) — links GitHub repos to AI-generated PRs
- **Period:** January 2024 through November 2025 (retrospectively parsed)
- **Treatment:** First agent-generated pull request (agent adoption date)

## Sample Composition

### Agent-First (AF) Repositories
- 401 treated repositories matched to 606 controls
- No traces of AI IDEs throughout collection period
- Older but smaller and less popular (median 68 stars)
- 121.5 agentic PRs per repo (median 28)

### IDE-First (IF) Repositories
- 117 treated repositories matched to 73 controls
- Exhibit IDE activity prior to agent adoption
- More starred, forked, and PR-active (median 423 stars)
- 101.5 agentic PRs per repo (median 37)

## Agent Attribution Taxonomy
Multi-signal cascading strategy:
1. Branch prefixes (cursor/, claude/)
2. PR author logins (codegen-sh, tembo-io)
3. First-commit author names (google-labs-jules[bot], claude[bot])
4. GitHub actor type bot
5. Default classification as human
6. Claude-specific: co-authorship patterns in PR descriptions/comments

## Results

### Development Velocity (RQ1)

| Outcome | AF (% change) | IF (% change) |
|---------|---------------|---------------|
| Commits | +36.25% *** | +3.06% (ns) |
| Lines Added | +76.59% *** | -6.34% (ns) |

**Dynamic effects (AF):**
- t=0 spike: +111% commits, +216% lines added
- Sustained elevation through t=6: +49-109% lines added

**Dynamic effects (IF):**
- Short-lived bump at t=0-2: +16-28% commits
- Returns to zero, eventually negative by t=6: -61% lines, -35% commits

### Software Quality (RQ2)

| Outcome | AF (% change) | IF (% change) |
|---------|---------------|---------------|
| Static Analysis Warnings | +17.73% *** | +19.00% *** |
| Code Complexity | +34.85% *** | +42.87% ** |
| Duplicate Line Density | +7.92% (ns) | -0.94% (ns) |
| Comment Line Density | +4.34% (ns) | +22.30% *** |

**Key finding:** Quality risks are PERSISTENT across both groups regardless of prior AI exposure. Complexity grows over time (AF: +20.7% at t=0 → +49% by t=5).

### Prior AI Exposure Moderation (RQ3)

**Velocity:** Prior AI exposure strongly moderates velocity gains (AF: large and sustained; IF: minimal and short-lived)

**Quality:** Prior AI exposure does NOT moderate quality risks (both groups see ~18% warnings, ~39% complexity)

**Mechanism:** IF repos face higher coordination and integration costs that offset throughput gains. Greater maturity of IF repos constrains how aggressively agentic changes can be merged.

## Comparison with Prior Work
- He et al. (2026) Cursor IDE study: modest productivity gains, mixed quality effects
- This study: autonomous agents AMPLIFY the speed-maintainability trade-off
- In AI-rich environments, agents may magnify complexity without delivering sustained velocity

## Statistical Notes
- Propensity models AUC 0.92-0.99
- Standard errors clustered at repository level
- Isolated significant pre-treatment coefficients in warnings/complexity (concerning but not pervasive enough for clear pre-trend violation)
- Intent-to-treat effects of observable agent adoption

## Implications

### For Teams
1. Don't assume additive productivity when adding agents to AI-saturated workflows
2. Deploy agents selectively for tightly scoped tasks in IDE-first environments
3. Surface maintainability metrics in agent planning and prompting
4. Quality safeguards are needed regardless of prior AI exposure

### For Tool Designers
1. Agent behavior should modulate based on existing AI usage in the repo
2. Complexity-aware review gates for agent PRs
3. Provenance tracking for accountability
4. Human oversight emphasis remains essential

## Replication
- Code: github.com/shyamagarwal13/agentic-coding-impact
- AIDev dataset: Li et al. (2025)