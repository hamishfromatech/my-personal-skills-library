# MI-at-Scale Evidence Base

## Source
Chopra, F., Haaland, I., Roever, N., & Roth, C. (2026). "Evaluating Behavioral Interventions at Scale with AI." CESifo Working Paper No. 12410. January 17, 2026.

## Study Design

### Sample
- Recruited: 2,800 participants (Prolific, Dec 18-20, 2025)
- UK and US users with Instagram or TikTok installed
- Exclusions: unusual typing speed (>10 char/sec), audio screener failure, fastest/slowest 1%
- **Final sample: 2,719 participants**
- 47% US, 53% UK; mean age 41; 60% female; 62% college-educated
- Mean household income: $80,270
- 92% Instagram/TikTok users; mean 183 min/day social media (SD 111)
- 85% spend ≥1 hour/day; 64% report "somewhat addicted" to ≥1 app

### Treatment Arms (4 arms, equal proportions)
1. **Change Talk** — MI protocol; evokes pro-change arguments; redirects sustain talk
2. **Decisional Balance** — MI protocol; explores pros/cons symmetrically
3. **Direct Persuasion** — Non-MI; unsolicited advice + information + concrete strategies
4. **Control** — Neutral time-use interview (no social media discussion)

### Conversation Structure (all arms)
- ~14 minutes average duration
- Common structure: explore habits → scaling questions → planning
- Voice or text input (16% used voice messages)
- GPT-5.2 model for generation
- Pasting disabled to prevent AI-tool use by participants

## Key Results

### Motivation to Change (Primary Outcome)
| Treatment | Effect (SD) | p-value |
|---|---|---|
| Change Talk | +0.52 | <0.001 |
| Direct Persuasion | +0.43 | <0.001 |
| Decisional Balance | +0.21 | <0.01 |

**Change Talk > Decisional Balance** (p < 0.001)
**Change Talk vs Direct Persuasion**: not significant (p = 0.101)

### Cost-Benefit Perceptions
| Treatment | Effect (SD) | p-value |
|---|---|---|
| Change Talk | +0.45 | <0.001 |
| Direct Persuasion | +0.20 | <0.01 |
| Decisional Balance | +0.13 | <0.05 |

**Change Talk > Direct Persuasion** (p < 0.001)
**Change Talk > Decisional Balance** (p < 0.001)

### Willingness to Pay (Incentivized)
| Treatment | Effect ($) | p-value | % of control mean |
|---|---|---|---|
| Change Talk | +$0.82 | <0.01 | 27% (control $3.00) |
| Direct Persuasion | +$0.35 | n.s. | 12% |
| Decisional Balance | +$0.15 | n.s. | 5% |

### Self-Reported Social Media Time (Follow-up, 2+ weeks)
| Treatment | Effect (min/day) | p-value |
|---|---|---|
| Change Talk | -11.2 | <0.10 |
| Decisional Balance | -11.9 | <0.05 |
| Direct Persuasion | **-23.8** | <0.001 |

**Direct Persuasion > Change Talk** (p < 0.05) — the motivation-behavior gap

### Persistence (2-week follow-up)
| Treatment | Motivation (SD) | Perceived Costs (SD) |
|---|---|---|
| Change Talk | +0.15** | +0.17*** |
| Direct Persuasion | +0.16*** | +0.12** |
| Decisional Balance | +0.07 (n.s.) | +0.05 (n.s.) |

### Strategy Adoption (Follow-up)
| Strategy | Change Talk | Direct Persuasion |
|---|---|---|
| Set specific rules/goals | +9.4 pp | +8.5 pp |
| Turned off notifications | Higher | — |
| Replaced with other activities | Higher | — |
| Built-in phone settings | — | Higher |
| Third-party app blocker | — | +7.3 pp** |
| Technology-based strategies (any) | marginal | +7.3 pp** |
| Behavioral strategies (any) | +11.0 pp*** | +9.3 pp*** |
| No steps taken | -9.4 pp*** | -8.5 pp*** |

## MITI 4.2.1 Fidelity Validation

### LLM-Based Scoring Pipeline
- Prompt template per global score (cultivating change talk, softening sustain talk, partnership, empathy)
- Contains: task description, MITI manual instructions, full transcript, output constraints
- Model: GPT-5.1 (GPT-4.1-nano produces similar results)

### Validation Against Human Ground Truth
- 14 annotated transcripts (official MITI training materials)
- **Global correlation: 0.72** (pooled across 4 scores, N=56)
- Mean bias: -0.24 (LLM slightly more conservative than humans)
- Partnership: r = 0.89
- Empathy: r = 0.75
- Cultivating change talk: r = 0.66
- Softening sustain talk: r = 0.45

### Applied to Study Transcripts
- 1,369 MI interviews (Change Talk + Decisional Balance) scored
- Both arms score highly on all 4 global measures
- Scores strongly concentrated (low variance) — AI delivery highly consistent
- Addresses historical MI scaling problem: interviewer variance

## Topic Analysis (BERTopic)

### Methodology
- 25,952 documents (participant responses)
- Embeddings: all-mpnet-base-v2 sentence transformer
- Dimensionality: UMAP
- 30 topics identified, 4 merged, GPT-4.1-mini for labeling

### Key Topic Differences
- **Decisional Balance**: 95% discuss "Balancing Connection and Social Media Use" vs 75% Change Talk (+20pp)
- **Direct Persuasion**: higher frequency of "Physical Strategies to Limit Phone Use" and "App and Notification Use Management"
- **MI arms**: higher "Confidence in Changing Social Media Use" and "Challenges in Sustaining Personal Change"
- **Change Talk**: distinct spike in "Preference for Gradual Social Media Reduction"

## Heterogeneity Analysis

### By Actual-Ideal Gap (pre-treatment)
| Outcome | Low gap | High gap |
|---|---|---|
| Predicted time reduction (Change Talk) | -12.5 min | -31.9 min |
| Predicted time reduction (Direct Persuasion) | -21.0 min | -52.8 min |
| Actual time reduction (Direct Persuasion) | -19.7 min | -30.3 min |

**Insight**: Treatments capitalize on existing self-control perception; effects larger when gap is bigger.

## Emotional States (PANAS subset)

| Emotion | Change Talk | Decisional Balance | Direct Persuasion |
|---|---|---|---|
| Determination | +0.9-1.1 SD | moderate | moderate (weaker) |
| Encouragement | +1.1 SD | moderate | moderate (weaker) |
| Interest | elevated | elevated | elevated |
| Irritation | moderate | low | notably higher |
| Upset | moderate | low | notably higher |

**Direct Persuasion** produces most emotionally conflicted response — consistent with reactance theory predictions.

## Implications for A-Tech

1. **Protocol selection by outcome goal**:
   - Want motivation change → Change Talk (MI)
   - Want actual behavior change → Direct Persuasion (concrete strategies)
   - Want balanced exploration → Decisional Balance (but smallest effects)

2. **Open-source MI delivery**: Open-weight LLMs can deliver MI protocols; fidelity validation pipeline (LLM-based MITI scoring) is itself open-source-able

3. **The motivation-behavior gap is the key design challenge**: Systems that only increase motivation without providing implementation strategies will underperform

4. **Strategy type matters**: Technology-based strategies (app blockers, settings) drove the largest actual behavior change in Direct Persuasion; behavioral strategies (willpower, replacement) drove Change Talk's motivation gains

5. **Scalability**: AI delivery eliminates interviewer variance — a historical barrier to MI scaling