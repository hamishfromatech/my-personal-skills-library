# Microsoft CLI Agentic Coding Study — Full Evidence Base

**Source:** Murphy-Hill, E., Butler, J., & Savelieva, A. (July 1, 2026). "Adoption and Impact of Command-Line AI Coding Agents: A Study of Microsoft's Early 2026 Rollout of Claude Code and GitHub Copilot CLI." arXiv:2607.01418v1 [cs.SE]. Microsoft.

**License:** CC BY 4.0

---

## Study Design

### Setting
Microsoft's early-2026 rollout of two sanctioned agentic command-line tools: GitHub Copilot CLI (GA Feb 25, 2026; preview access earlier) and Anthropic Claude Code (managed access program from late 2025). Observation window: January 5 – April 29, 2026 (~4 months). Rollout boundary: January 5, 2026.

### Key distinction from prior work
- **First field study** to use developer-level telemetry (not surveys) for agentic CLI tools specifically (prior telemetry studies covered IDE-based tools).
- Enterprise setting provides the full eligible-adopter population (the "denominator" missing from public-repository signal studies).
- Separates initial use from retention (the factors differ).
- Compares two tools (Claude Code vs. Copilot CLI).

### Two studies
1. **Adoption study** (Copilot CLI only, well-defined eligible population): RQ1 who tries, RQ2 who retains.
2. **Outcomes study** (both tools): RQ3 does output rise, RQ4 does the tool matter, RQ5 who benefits most.

---

## Adoption Study — Predictors

### Sample
Microsoft software engineers eligible to adopt Copilot CLI at rollout. Excluded: engineers with Claude Code access (different adoption decision), retracted licensees. Pre-period: Oct 1, 2025 – Jan 4, 2026 (13 weeks). Post-period: Jan 5 – Apr 29, 2026.

### Model
- **Initial use:** discrete-time logistic regression on engineer-week panel (hazard model). Week fixed effects, division fixed effects, clustered SEs on engineer.
- **Retention:** cross-sectional logistic regression on adopters. Retained = active ≥5 of 14 days from first use.

### Five predictor groups (ranked by strength on initial use)

#### 1. Social exposure (strongest)

Three time-varying signals over prior 14 days:

| Signal | Top-bucket initial-use odds lift | Retention lift |
|---|---|---|
| Skip-level peers (25%+ used) | **+216%** | +66% |
| Direct manager (binary) | +82% | +22% |
| Reviewer peers (25%+ used) | +54% | +54% |

Social exposure could reflect peer influence or homophily (similar engineers cluster). Fixed effects cannot fully separate these. The authors note prior work showing learning developer tools from peers is highly effective (Murphy-Hill & Murphy 2011; Xiao, Witschey & Murphy-Hill 2014).

#### 2. Prior IDE Copilot use (opposite directions)

| Days of prior IDE use | Initial-use odds | Retention odds |
|---|---|---|
| 1–14 days | +49% | −12% |
| 15–60 days | ~+65% | −15% |
| 60+ days | +83% | −15% |

Interpretation: IDE-savvy engineers are open to AI and try the CLI tool, but have a familiar fallback, so they don't build a sustained CLI habit. First-time AI users have no fallback and stay longer if they stay at all.

#### 3. Baseline PR activity

| PRs/week (pre-period) | Initial-use odds | Retention odds |
|---|---|---|
| ≤1 | +19% | +13% |
| 1–2 | ~+25% | +14% |
| 2+ | +34% | **+31%** |

The busiest engineers both try and stay. Retention scales with prior output.

#### 4. Career stage

| Stage | Initial-use odds vs. IC4 | Retention |
|---|---|---|
| IC2 (junior) | −13% | negative (sig. for IC2) |
| IC3 | −14% | noisy |
| IC4 (reference) | — | — |
| IC5 | +22% | ~0 |
| IC6 | ~+20% | ~0 |
| M4–M6 (managers) | no difference | no difference |

Gentle seniority gradient among ICs. Seniors decompose, vet, and parallelize better. Juniors "don't know what they don't know."

#### 5. Tenure (barely matters)

| Tenure | Initial-use odds vs. 5–15y |
|---|---|
| <1 year | +11% |
| 1–2y, 2–5y, 15+y | within couple % of reference (not significant) |

Newest hires slightly more likely to try; everyone else indistinguishable.

### Qualitative survey (609 responses from "Agentic Engineering Day")

Themes:
- Broader task scope: "updating our documentation, analyzing it for issues and quality, prototyping app ideas and code samples, creating tools for our team"
- Parallel work streams: "I can be working on multiple things at once"
- Tackling previously deferred tasks: "the ability to make larger changes that I never would have taken on in the past"
- Senior advantage: "an especially good fit for experienced, senior developers, who can break work down into smaller chunks"
- Junior concern: "what these tools mean for junior colleagues and how they can develop a good 'sense' for code"
- Durable shift: "Using GitHub CLI has entirely changed the way that I approach all my projects... I am never going back."

---

## Outcomes Study — Part 1: Synthetic Control (CausalImpact)

### Design
Bayesian Structural Time-Series (BSTS). Rollout-aligned adopter cohort (any tool activity Jan 5–11, 2026). Control: 10 daily-mean regressors from non-adopter PR creators, randomly partitioned. Pre-period: Oct 1, 2024 – Jan 4, 2026 (461 days). Post-period: Jan 5 – Apr 29, 2026 (115 days). P5–P95 PR filter.

### Result
**+24.0% lift in PRs/engineer/day** [95% CI +14.5%, +33.7%], posterior tail-area p<0.001.

### Persistence (no fade)
| Period | Lift | 95% CI |
|---|---|---|
| First week | (high, novelty) | — |
| Rest of January | — | — |
| February | +29.4% | [+17.7%, +44.4%] |
| March 1 – Apr 29 | +20.0% | [+7.4%, +35.9%] |

Intervals overlap substantially; both exclude zero. The drop is within sampling noise → **sustained, not transient**.

Contrast: He et al. (2026) found Cursor's lift faded by month 2–3. Two hypotheses for the difference: (a) tool generation (2026 agentic CLI vs. 2024–25 IDE); (b) unit of analysis (within-person immune to compositional drift vs. repo-level average reverting as marginal users adopt).

### Placebo test
Placebo intervention at 2025-10-06: −1.1% [−10.6%, +8.6%]. Passes.

---

## Outcomes Study — Part 2: Within-Person Analyses

### Common dataset
All engineers with any Copilot CLI or Claude Code activity, Jan 5 – Apr 29, 2026. Outcome: merged PRs/engineer/week (merged = completes within 28 days). Engineer FE + week FE (Poisson). SEs clustered on engineer.

### RQ3: Dose-response (Equation 3)

log E[PRs] = α_i + τ_w + Σ β_k 1[d=k] + β_5+ 1[d≥5]

| Tool-use days/week | PR lift vs. 0-day weeks |
|---|---|
| 0 | (reference) |
| 1 | ~+5% |
| 2 | ~+10% |
| 3 | **+15.0%** |
| 4 | ~+30% |
| 5+ | **+50.1%** |

Monotone, well-separated, convex. More use → disproportionately more output.

### RQ4: Tool comparison (single-tool subset)

| Tool | Any-use-week PR lift | 95% CI |
|---|---|---|
| Claude Code | +11.4% | [+9.4%, +13.6%] |
| Copilot CLI | **+24.9%** | [+23.0%, +26.8%] |

Copilot CLI ≈ 2.2× the lift of Claude Code (p<0.0001). Surprising given public sentiment favoring Claude Code for autonomous agentic work. Two hypotheses: (a) different task mixes; (b) Copilot CLI better aligned with Microsoft's internal workflow (Microsoft owns GitHub).

### RQ5: Who benefits most (interactions at 3 days/week)

**Career stage:** IC4 reference = +21.3% [+19.6%, +23.0%]. More junior ICs and more senior managers show larger lifts ("C" shape). Benjamini-Hochberg FDR correction applied.

**Tenure:** 5–15y reference = +21.5% [+19.9%, +23.1%]. Again a "C" shape — low- and high-tenure engineers benefit more than mid-tenure. The <1y point estimate is large but possibly conflated with onboarding ramp-up (despite excluding <6-month hires).

---

## Token-Spend Context

- Fortune reports Meta employee usage exceeded 60 trillion tokens in 30 days; highest individual averaged 281 billion tokens. At Claude Opus 4.6 pricing ($5/M tokens), that one user ≈ $1.4M/month.
- At organizational scale, token spend can reach millions annually. Misreading adoption, retention, or impact makes a rollout expensive without changing velocity.

---

## Threats to Validity (selected)

- **Construct validity:** Retention threshold (5/14 days) and 28-day merge window are arbitrary; re-ran at 3/14 and 7/14 — model fits at all three. Merged PRs reward small frequent PRs; may miss quality costs (complexity).
- **Internal validity:** Adoption study is cross-sectional — cannot separate peer influence from homophily. Outcomes within-person design addresses engineer-specific confounds but heavier-use weeks may carry lighter task mix.
- **External validity:** One company, one early-2026 window. Microsoft is large and diverse but findings generalize only insofar as settings resemble it. PRs measured from Azure DevOps only.
- **Researcher positionality:** Authors are Microsoft employees; Microsoft sells AI tools and owns GitHub (maker of Copilot CLI). The Copilot CLI > Claude Code result may reflect organizational alignment.

---

## Key References Cited in the Study

- He et al. (2026) — Cursor lift fades at month 2, gone by month 3 (MSR).
- Heilman, Kyllo & Murphy-Hill (2026) — GitHub Copilot dose-response observational analysis (within-person template).
- Daniotti et al. (2026) — AI-assisted coding global diffusion (Science).
- StackOverflow (Dec 2025) — 49,000+ respondents; developer trust in AI falling; "willing but reluctant."
- Pragmatic Engineer survey (early 2026) — Claude Code most popular AI dev tool among respondents.
- Demirer, Musolff & Yang (2026, NBER) — "Writing code vs. shipping code" across AI tool generations.
- Reyes-Reina et al. (2026) — systematic review: all 25 prior adoption studies rely on surveys/interviews, none on observational telemetry.