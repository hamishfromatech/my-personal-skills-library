# CLI Agentic Coding Adoption & Impact — Evidence Base

## Primary Source

Murphy-Hill, Butler & Savelieva (arXiv:2607.01418, 2026) — "Adoption and Impact of Command-Line AI Coding Agents: A Study of Microsoft's Early 2026 Rollout of Claude Code and GitHub Copilot CLI"
- Microsoft internal study, tens of thousands of engineers
- Observation window: January 5 – April 29, 2026 (16 weeks)
- Mixed-methods: telemetry + HR data + developer survey (n=609)

## Study Design

### Adoption Study (Copilot CLI only)
- Sample: Microsoft software engineers eligible to adopt Copilot CLI at rollout (Jan 5, 2026)
- Excluded: Claude Code licensees, two broadly-licensed divisions, retracted users
- Pre-period: Oct 1, 2025 – Jan 4, 2026 (13 weeks) for predictor construction
- Post-period: Jan 5 – Apr 29, 2026 for adoption and retention observation

### Outcomes Study (both tools)
- Adopters: any Copilot CLI or Claude Code activity during Jan 5-11, 2026 (rollout-aligned cohort)
- Controls: PR creators who never used either tool
- Both restricted to active engineers (≥2 merged PRs in 4-week pre-window)
- P5-P95 PR filter on pre-period PR counts

## Predictor Groups (5)

1. **Career stage**: IC2-IC6, M4-M6 (HR records); IC4 reference
2. **Tenure**: <1y, 1-2y, 2-5y, 5-15y (reference), 15+y
3. **Baseline PR activity**: 0 (ref), ≤1, 1-2, 2+ PRs/week over 13-week pre-period
4. **Prior IDE Copilot use**: 0 (ref), 1-14, 15-60, 60+ active days over 13-week pre-period
5. **Social exposure** (time-varying, 14-day rolling):
   - Reviewer peers: share of review-exchange colleagues using Copilot CLI
   - Skip-level peers: share reporting to same manager's manager using Copilot CLI
   - Direct manager: binary indicator

## Controls
- Week fixed effects (calendar-time)
- Broad-division fixed effects
- Engineer-clustered standard errors

## Adoption Results

### Social exposure (strongest predictor)
- Skip-level peers >25%: **+216%** odds of trying (highest signal)
- Direct manager: +82% odds of trying
- Reviewer peers >25%: +54% odds of trying
- Retention tracks similarly except manager (+22% retention vs +82% trying)

### Prior IDE Copilot use (paradoxical)
- Trying: +49% to +83% (more prior use → more likely to try)
- Retention: **−12% to −15%** (more prior use → LESS likely to stick)
- Explanation: IDE users have a fallback; CLI-only users have no fallback

### Baseline PR activity
- Trying: +19% (≤1 PR/week) to +34% (2+ PRs/week)
- Retention: +13% to +31% (monotone; busiest engineers stay most)

### Career stage
- IC2/IC3: −13% to −14% trying (juniors less likely)
- IC5/IC6: +22% trying (seniors more likely)
- M4-M6: no significant difference from IC4
- Retention: noisy; only IC2 statistically significant

### Tenure
- <1 year: +11% trying (newest hires slightly more likely)
- All others: within 2% of reference, not significant

## Outcomes Results

### Part 1: CausalImpact (synthetic control)
- Method: Bayesian Structural Time-Series (BSTS)
- 10 daily-mean regressors from non-adopter PR creators
- Pre-period: 461 days (Oct 1, 2024 – Jan 4, 2026)
- Post-period: 115 days (Jan 5 – Apr 29, 2026)

**Result**: +24.0% lift in PRs/engineer/day [95% CI +14.5%, +33.7%], posterior tail-area p < 0.001

**Persistence**: 
- First week: high (novelty)
- February: +29.4% [95% CI +17.7%, +44.4%]
- March-April: +20.0% [95% CI +7.4%, +35.9%]
- CIs overlap substantially; both exclude zero → sustained, not transient

**Placebo test**: Intervention at 2025-10-06 → −1.1% [−10.6%, +8.6%] (passes)

### Part 2: Within-person dose-response
- Method: Fixed-effects Poisson with engineer + week fixed effects
- Dose: tool-use-days per week, binned {0, 1, 2, 3, 4, 5+}

**Results** (lift vs own zero-day weeks):
| Days/week | PR lift |
|---|---|
| 1 | ~+5% |
| 2 | ~+10% |
| 3 | +15.0% |
| 4 | ~+30% |
| 5+ | +50.1% |

Curve is monotone and well-separated.

### Tool comparison (single-tool users)
- Copilot CLI: +24.9% [+9.4%, +13.6%] (any-use vs own zero-tool weeks)
- Claude Code: +11.4% [+23.0%, +26.8%]
- Difference: 2.2× in favor of Copilot CLI (p < 0.0001)

## Qualitative Evidence (Survey, n=609)

### Themes
1. **Tackling deferred tasks**: "ability to make larger changes that I never would have taken on in the past, like splitting our huge test project into five separate files"
2. **Parallel streams**: "updating our documentation, analyzing it for issues and quality, prototyping app ideas and code samples, creating tools for our team and others to use"
3. **Automating tedious work**: "boilerplate plumbing a lot faster, repetitive unit tests faster"
4. **Role transformation**: "I no longer think about narrow solutions; instead I am able to use agents to think broadly and formulate wholistic approaches"
5. **Persistent productivity**: "I am never going back"

### Why seniors benefit more
- Can break work into smaller chunks for AI to implement
- Better positioned to vet outputs for "correctness, completeness, and sanity"
- Carry work beyond coding (architecture, review, coordination) → agents let them offload coding while staying in those roles
- One principal developer: coding assignments required 5-10 clarification prompts, each 10-15 min, but "now I can just prompt it and switch to another task"

### Why juniors struggle
- "I'm less convinced that junior developers (who 'don't know what they don't know') would be able to use them as effectively"
- Concern: "what these tools mean for junior colleagues and how they can develop a good 'sense' for code"

## Limitations

- Single company (Microsoft); may not generalize
- Microsoft owns GitHub (Copilot CLI) but buys Claude Code → organizational alignment may favor Copilot CLI
- 16-week window; long-horizon decline not captured
- Merged PRs imperfect proxy for throughput (quality costs not measured)
- Adopters self-select; synthetic counterfactual may be biased
- Azure DevOps only; engineers on other ecosystems undercounted

## A-Tech Alignment

- **Open-source AI**: Findings apply to any CLI coding agent; open-source agents (Aider, Cline) may show different patterns
- **Data privacy**: Telemetry is internal; no external data sharing
- **Financial freedom**: +24% PR lift justifies token spend; dose-response shows ROI scales with usage intensity
- **Practical implementation**: Rollout playbook derived from empirical adoption predictors