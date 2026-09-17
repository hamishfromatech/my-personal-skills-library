# Productivity-Experience Paradox & Supervisory Engineering — Evidence Base

## Primary Source

Vella, A. & Blincoe, K. (2026). "The Impact of AI Coding Assistants on Software Engineering: A Longitudinal Study." University of Auckland, New Zealand. Manuscript submitted to ACM.

## Study Design

- **Design**: Longitudinal mixed-methods, two questionnaires 6 months apart
- **Time points**: Q1 (October 2024), Q2 (April 2025), each open 4 weeks
- **Sample**: 224 responses → 158 eligible Q1, 101 eligible Q2, 95 matched longitudinal cohort (60% retention)
- **Eligibility**: Professional software engineers currently using AI coding assistants
- **Recruitment**: Convenience + referral-chain sampling via LinkedIn, X, Facebook, Discord, Slack communities; 33 organizations contacted, 9 agreed to distribute
- **Ethics**: University of Auckland Human Participants Ethics Committee (UAHPEC27902)
- **Analysis**: R (complete-case); Wilcoxon signed-rank tests; Spearman correlations; Holm-Bonferroni correction; reflexive thematic analysis (NVivo); alluvial diagrams

## Attrition Analysis

Fisher's exact tests compared retained vs. lost participants across 6 variables: gender, experience, role, programming language, company size, country. No significant differences (all p > 0.05), Cramér's V ≤ 0.23. Attrition was not systematic.

## Participant Demographics (Table 1)

| Variable | Q1 n (%) | Q2 n (%) |
|---|---|---|
| Gender: Man | 135 (85%) | 86 (85%) |
| Gender: Woman | 19 (12%) | 12 (12%) |
| Age 25-34 | 55 (35%) | 33 (33%) |
| Age 35-44 | 63 (40%) | 45 (45%) |
| Junior | 21 (13%) | 13 (13%) |
| Mid-Career | 77 (49%) | 51 (50%) |
| Senior | 59 (37%) | 37 (37%) |
| Fullstack | 57 (36%) | 32 (32%) |
| Backend | 37 (23%) | 28 (28%) |
| NZ | 65 (41%) | 48 (48%) |
| Netherlands | 16 (10%) | 10 (10%) |
| UK | 14 (9%) | 11 (11%) |

28 countries represented.

## Study Context

### AI Tools Used
- 37 unique tools mentioned across both time points
- GitHub Copilot and ChatGPT: 70% adoption at Q1, 58% at Q2
- Mean tools per participant: 1.9 (Q1) → 2.9 (Q2)
- 82% of matched participants changed tool combinations
- Daily usage: 57% (Q1), 61% (Q2) — stable (paired Wilcoxon p=0.17)

### Attitudes
- Initial impressions (Q1): predominantly positive
- Anticipated disappointment if removed (Q2): more evenly distributed
- Correlation between measures: Spearman ρ=0.30, p=0.003 (moderate positive)

### Primary Concerns (ranked 1-7)
- Quality: dominant at both time points (44% → 36%)
- Security: second at both time points
- **Maintainability: ONLY significant change** (3% → 19%, paired Wilcoxon p=0.003, moderate effect)
- Interpretation: AI optimizes for "works now" not "maintains later"

## RQ1 Results: Task Focus Shifts

### Cross-sectional (Figure 5)
| Task | Q1 Mean | Q2 Mean | Direction |
|---|---|---|---|
| Writing code | 2.10 | 1.93 | Strongest reduction (82% less time by Q2, 2% more) |
| Refactoring | 2.48 | 2.39 | Below neutral (many report less) |
| Testing | 2.52 | 2.77 | Below neutral but trending up |
| Reviewing | 3.03 | 3.15 | Above neutral (only task > 3.0) |
| Designing | 2.94 | 2.89 | Near neutral |
| Debugging | 2.84 | 2.84 | Neutral |

### Longitudinal (n=88 matched, paired Wilcoxon, Holm-Bonferroni)
| Task | Q1 Mean | Q2 Mean | r (effect) | p |
|---|---|---|---|---|
| Writing code | 2.10 | 1.92 | -0.35 (moderate) | 0.263 |
| Testing | 2.52 | 2.77 | +0.29 (small) | 0.263 |
| Reviewing | 2.93 | 3.16 | +0.26 (small) | 0.333 |
| Others | — | — | ≤ 0.11 | n.s. |

No individual task reached statistical significance, but directional effects present.

### Task Group-Level Shift (Table 4, n=88)
| Group | Q1 Mean | Q2 Mean | Δ | r | p |
|---|---|---|---|---|---|
| Creation | 2.51 | 2.40 | -0.11 | -0.24 | 0.092 |
| Verification | 2.77 | 2.92 | +0.16 | +0.30 | 0.060 |
| **Balance (V-C)** | **0.26** | **0.53** | **+0.27** | **+0.39** | **0.006** |

**Significant shift toward verification activities** (moderate effect). 42% shifted on at least one dimension. Most common: decreased creation only (20%), both increased (17%), both decreased (15%).

Transition patterns:
- Writing code: 30% less, 56% stable, 15% more
- Testing: 22% less, 36% stable, 42% more
- Reviewing: 23% less, 38% stable, 40% more

### Qualitative Themes (RQ1)

**Theme 1: Compressing routine creation work**
- 1A: Reducing manual implementation effort (boilerplate, small functions, repetitive structures)
- 1B: Supporting small-scale debugging and error handling (error message → ChatGPT → primer)

**Theme 2: Rerouting information seeking and comprehension into AI interactions**
- 2A: From search engines to real-time assistance ("basically replaced google for me")
- 2B: Accelerated learning, onboarding, and code understanding ("Much easier to pick up new tools")

**Theme 3: From doing to supervising AI-generated code**
- Shift from direct production to oversight: directing, evaluating, deciding accept/modify/discard
- "I ask AI to make changes first and review them before accepting or rejecting"
- "Mostly reading the code and directing AI on the right way"

**Theme 4: Verification and trust calibration moderating perceived benefits**
- Variation in willingness to accept AI output
- "I almost never let it write production code, or at least never leave in code that I don't understand"
- "I do not trust it for strategic or critical code"
- Q2 reflection: "6 months ago, I was using the tools a lot more cautiously with a lot more fact checking"

## RQ2 Results: Developer Experience and Productivity

### Developer Experience (Figure 6, Tables 5-6)

Cross-sectional:
- Q1: Cognitive load and feedback loops similar improvement; flow state lagged
- Q2: Feedback loops greatest improvement; cognitive load and flow state smaller

Attitude correlations (Table 5, Spearman):
| Dimension | Q1 Initial Impression | Q2 Anticipated Disappointment |
|---|---|---|
| Feedback loops | 0.32*** | 0.33** |
| Flow state | 0.27** | 0.26* |
| Cognitive load | 0.22** | 0.23* |

Feedback loops consistently strongest association.

Longitudinal (n=94 matched, paired Wilcoxon, Holm-Bonferroni):
| Dimension | Q1 Mean | Q2 Mean | Δ | r | p | Direction |
|---|---|---|---|---|---|---|
| Feedback loops | 3.73 | 3.95 | +0.21 | +0.38 | **0.038** | SIGNIFICANTLY IMPROVED |
| Cognitive load | 3.74 | 3.60 | -0.15 | -0.26 | 0.195 | Non-sig decline |
| Flow state | 3.54 | 3.36 | -0.18 | -0.24 | 0.195 | Non-sig decline |

Individual variation: feedback loops 33% improved; cognitive load 19% improved / 29% declined; flow state 27% improved / 35% declined.

### Cohort Analysis (Figure 7) — THE KEY FINDING

| Cohort | Q1 | Q2 | Change |
|---|---|---|---|
| Positive (all dimensions 4-5) | — | 37% retention | Poor retention |
| Negative (any dimension 1-2) | 14% | 27% | **NEARLY DOUBLED** |
| Mixed/Neutral | — | — | — |
| Complete stability (all 3) | — | 9% | Rare |

**No participant who started Negative recovered to fully Positive by Q2.**

Within Negative cohort:
- Flow state: predominant issue, 54% (Q1) → 76% (Q2)
- This is the most vulnerable dimension.

### Productivity

Cross-sectional:
- Q1: Mean=4.08, SD=0.65, 84% report improvement
- Q2: Mean=4.03, SD=0.59, 84% report improvement (identical)
- No participant transitioned to negative perception

Cross-sectional correlations with DevEx (Holm-Bonferroni):
| Dimension | Q1 ρ with productivity | Q2 ρ with productivity |
|---|---|---|
| Flow state | 0.49*** (strongest) | 0.20* (weakened) |
| Feedback loops | — | 0.37*** (strongest at Q2) |
| Cognitive load | — | — |

**Key shift: Flow state was strongest DevEx correlate at Q1 but dropped to small effect by Q2; feedback loops replaced it.**

Longitudinal (n=95 matched):
- Paired Wilcoxon: Δ=-0.05, p=0.326, r=-0.22 (small effect, n.s.)
- 77% maintained identical ratings; 14% decreased, 9% increased
- **CRITICAL: Changes in DevEx dimensions did NOT correlate with changes in productivity** (smallest p=0.313)

### Qualitative Themes (RQ2)

**Theme 1: Accelerated throughput with uneven day-to-day experience**
- "Boring mechanical tasks like refactoring files, updating tests, adding translations, creating test fixtures are 10x less effort"
- "This does break the flow, but still speeds up the overall process"
- "less 'in the zone' time, because while code is being produced, I now find myself switching context more often"
- Throughput gains ≠ uniformly smoother or easier work

**Theme 2: Expanded perceived capability and willingness to engage with unfamiliar work**
- Lower threshold for unfamiliar technologies, languages, domains
- Faster path from uncertainty to workable first step
- CAUTION: "Destroys process of learning something new — you get a working solution, but not understanding why and how it may work"

**Theme 3: Verification and trust management as ongoing engineering work**
- 3A: Individual oversight — skepticism, accountability, steering
  - "You need to always be fully in control, and guide the AI into the direction you want"
  - "Identifying when the LLM has lost the thread of the problem and halting it is essential"
  - "break it down into smaller more specific 'modules' and then tie them together"
- 3B: Collective trust, reputation, and collaborative implications
  - "If I know that a dev is using AI heavily I trust them much less to know what exactly their code is doing"
  - "When left unsupervised with AI, junior colleagues sometimes have a tendency to pursue inappropriate solutions to the wrong problems much further"

## The Productivity-Experience Paradox — Formal Definition

**The productivity-experience paradox**: sustained output perceptions alongside degrading experience, potentially decoupling the established relationship between the two.

Prior research found:
- Uninterrupted focus is central to developers' sense of productivity (Meyer et al., 2014)
- Satisfaction and productivity are bidirectionally linked (Storey et al., 2021)

The DevEx framework treats its three dimensions as complementary drivers of productivity. AI assistance may reshape their relative contributions, with faster feedback compensating for increased cognitive friction.

**Open question**: Whether this reconfiguration is sustainable, or whether erosion in flow and cognitive load eventually undermines the gains.

## Supervisory Engineering Work — Proposed New SDLC Category

Not captured by traditional categories (designing, coding, testing, reviewing):
- **Directing**: specifying intent, crafting prompts, providing context, iterating when output misses
- **Evaluating**: reading AI output, deciding accept/modify/reject
- **Correcting**: fixing errors, integrating output, maintaining consistency

Why standard categories miss it: Engineers may not recognize verification of AI output as "testing" or "code review" in the traditional sense.

## Threats to Validity

### Internal
- 40% attrition (158→95); demographic comparisons show no significant differences but unmeasured differences possible
- Disillusioned engineers may have been less motivated to complete follow-up → overestimate stability, underestimate negative trajectories
- First author is practicing software engineer (managed via reflexive memoing, counter-example seeking, supervisor discussions)

### External
- Only continuing users included (survivor bias): 84% = "84% of continuing users", not "84% of all who tried"
- 85% men, predominantly English-speaking → limits generalizability
- Temporal: reflects AI assistants as of late 2024 / early 2025; rapid tool evolution
- Confounded with rapidly changing tool landscape

### Construct
- Perception-based measures (not objective performance)
- Recall bias, recency effects, shifting internal benchmarks
- Interpretivist stance: perceptions treated as meaningful data in their own right

## Implications

### For Individual Engineers
- Prepare for shift from creation to supervisory work (differently allocated, not freed-up)
- Develop verification and trust calibration skills (transferable across tools)
- Knowing when to accept/reject AI output is key emerging skill

### For Organizations
- Output metrics (lines, PRs) may mask experiential erosion
- Monitor DevEx alongside productivity for early warning
- Enable tool experimentation, don't mandate single tool
- Team restructuring: verification, judgment, trust calibration central
- Hiring profiles: emphasis on judgment over implementation speed

### For Educators
- Teach directing, evaluating, correcting AI output
- Evolve assessment: evaluate understanding, not just output quality
- Students can now produce working code without understanding it

## Future Research Directions

1. Whether supervisory engineering work is empirically distinct from existing SDLC activities
2. Whether DevEx dimensions contribute differently in AI-assisted contexts
3. Whether experiential erosion manifests as burnout or turnover over longer horizons
4. Whether supervisory work is intrinsically satisfying or diminishes professional fulfilment
5. Team-level dynamics: how individual-level findings aggregate

## Cross-References

- `prompt-wait-evaluate-flow-collapse` — flow disruption mechanism (junk flow, 39-point perception gap)
- `supervisory-engineering-work` — work category identification (if separate skill exists)
- `ai-productivity-long-term-factors` — long-term productivity factors
- `spurious-productivity-space-redistribution` — hidden costs, SPACE redistribution
- `developer-ai-ambidexterity-shift` — exploration/exploitation rebalancing
- `devex-verification-bottleneck-framework` — verification bottleneck
- `agentic-cognitive-engagement-decline` — cognitive engagement decline (Bloom's Taxonomy)
- `calm-technology-ai-coding` — calm technology principles
- `ai-fatigue-scale-design` — fatigue measurement
- `ai-review-fatigue-mitigation` — review fatigue

## A-Tech Alignment

| A-Tech Value | Alignment |
|---|---|
| Open-source AI | Applies to Aider, Cline, OpenHands, any open agent; open tools enable self-hosting = data sovereignty over developer telemetry |
| Data privacy | Internal telemetry only; no sensitive data required; privacy-preserving DevEx measurement |
| Financial freedom | Productivity gains justify tool spend; experiential erosion signals sustainability risk that affects retention cost |
| Practical implementation | 158→95 longitudinal, professional engineers, 28 countries, mixed-methods validated |