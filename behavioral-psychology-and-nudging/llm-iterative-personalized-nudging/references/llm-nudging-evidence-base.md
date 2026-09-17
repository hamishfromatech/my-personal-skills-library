# LLM Iterative Personalized Nudging — Evidence Base

## Primary Source

**Li, Z., Liu, Y., Wang, C., Tong, S., Peng, K., & Ji, F. (2026). Enhancing behavioral nudges with large language model-based iterative personalization: A field experiment on electricity and hot-water conservation. arXiv:2604.03881v1.**

### Study Design
- **Setting**: University dormitory, Beijing, China
- **Period**: Nov 2024 – Jan 2025 (4-week baseline + 5-week intervention)
- **Participants**: 233 eligible, 169 electricity / 166 hot-water analytic samples
- **Design**: Three-arm RCT, cluster-randomized by co-participant ties (203 clusters)
- **Delivery**: WeChat-based chatbot (open-source "ChatGPT-on-WeChat" framework)

### Intervention Arms
| Arm | n | Content |
|---|---|---|
| C (Control) | 77 | Text-based: weekly usage report + social comparison via link |
| T1 (Image) | 78 | Same content as C, delivered as visual image report |
| T2 (LLM) | 78 | T1 content + personalized suggestions + behavioral scenarios + quantitative outcome estimates |

### LLM Agent Pipeline
- **Stage 1**: Usage feedback (GLM-4-Plus for retrieval, o1-preview/o1 for generation)
- **Stage 2**: Profile reasoning + suggestion selection (RAG over 3,219 PDF suggestion records)
- **Stage 3**: Quantitative scenario construction (savings → intuitive equivalents)
- **Updating**: Profile refreshed each round with new consumption, prior suggestions, interaction logs

### Primary Results

**Electricity (low-friction behavior)**:
| Metric | C | T1 | T2 |
|---|---|---|---|
| Predicted consumption (kWh/room-day) | 2.58 | 2.53 | 2.03 |
| Adjusted saving rate | 14.1% | 16.0% | 32.4% |
| T2 vs C difference | — | — | -0.56 kWh (p=0.014) |
| T2 vs T1 difference | — | — | -0.49 kWh (p=0.023) |
| Omnibus p | — | — | 0.021 |

**Hot water (high-friction behavior)**:
| Metric | C | T1 | T2 |
|---|---|---|---|
| Predicted consumption (L/person-day) | 34.7 | 32.1 | 31.1 |
| Adjusted saving rate | 4.3% | 11.5% | 14.1% |
| T2 vs C difference | — | — | -3.6 L (p=0.087) |
| Omnibus p | — | — | 0.208 |

**Temporal dynamics (electricity, cumulative saving rate vs C)**:
- Round 1: +8.4pp
- Round 2: +18.3pp (stabilized here)
- Rounds 3-5: 17.9-19.3pp

**Engagement**:
| Metric | C | T1 | T2 |
|---|---|---|---|
| Engagement rate | 57.1% | 58.2% | 69.7% |
| Avg sessions/participant | 45.6 (1.3/day) | — | — |
| T2 session duration | — | 1.90 min | 1.58 min |
| Multiple replies (48h) | 38.0% (T1) | — | 44.7% (T2) |
| Usage data queries | 5.0% (T1) | — | 9.4% (T2) |

### Content Analysis

Topic modeling (5 topics): T2 content shifted from retrospective to prospective:
- Planning-related content: 19.2% (T2) vs 0.4% (conventional)
- Appliance-related content: 16.4% (T2) vs 3.8% (conventional)
- Usage-gap content increased across rounds (19.7 → 25.9 per nudge)
- Post-nudge survey: accuracy 3.74→4.00, actionability 3.72→3.97, satisfaction 3.91→4.17

### Individual Treatment Effects

Meta-learner ensemble (S/T/X/DR-learner, equal-weighted, random forest, 5-fold cross-fitting):
- **Pooled**: T2 mean ITE -0.22 SD, 76.7% negative; T1 mean ITE -0.08 SD, 55.2% negative
- **Electricity**: T2 mean ITE -0.50 kWh, 84.6% negative
- **Top quartile (T2)**: stronger pro-conservation profile (3.77 vs 3.51), higher living budget (2.52 vs 1.84K RMB)

### Behavioral Archetypes (hierarchical clustering, correlation distance)

| Archetype | T2 share | T1 share | C share |
|---|---|---|---|
| Quick responders | 39.1% | 19.6% | 25.6% |
| Gradual responders | 13.0% | 28.3% | 18.6% |
| Rebound responders | — | — | — |
| Late responders | 26.1% | — | 20.9% |
| Adverse responders | 6.5% | 13.0% | 16.3% |

### Predictors (XGBoost feature importance)

| Phase | Top predictor (electricity) | Top predictor (hot water) |
|---|---|---|
| Early | Baseline consumption (36.2%) | Baseline consumption (38.9%) |
| Late | Baseline consumption (54.8%) | Psychological + socio-structural (56.9%) |

Electricity: outcome expectancy strongest psychological predictor (0.09)
Hot water: self-efficacy (0.07) and neighborhood perception (0.07)

## Supporting Literature

- **COM-B framework** (Michie et al., 2011) — capability, opportunity, motivation barriers
- **Social Cognitive Theory** (Bandura, 1986) — self-efficacy, outcome expectancy
- **Dynamic Treatment Regimes** (Chakraborty & Murphy, 2014) — sequential interventions
- **JITAIs** (Nahum-Shani et al., 2018) — just-in-time adaptive interventions
- **Boosts vs nudges** (Grüne-Yanoff & Hertwig, 2016) — competence enhancement
- **LLM persuasion** (Hackenburg et al., 2025, Science) — conversational AI political persuasion
- **LLM mental health** (Heinz et al., 2025, NEJM AI; Allen et al., 2026) — chatbot interventions
- **Habit formation** (Buyalskaya et al., 2023, PNAS) — machine learning on exercise/hygiene habits

## Cross-References

- `llm-agent-nudge-sensitivity` — Cherep et al. (PNAS 2026): LLMs over-comply with nudges
- `nudge-persistence-technology-adoption` — Brandon et al. (Review of Economic Studies 2026): 50% persistence via technology adoption
- `prompt-wait-evaluate-flow-collapse` — LLM nudge timing must respect flow
- `optimal-nudging-resource-rational-framework` — Callaway et al. (2023): resource-rational optimal nudges
- `bottom-nudge-analysis-framework` — BOTTOM framework for nudge classification

## A-Tech Alignment

- **Open-source AI**: GLM-4-Plus (Zhipu AI), o1 (OpenAI); ChatGPT-on-WeChat (open-source); public PDF suggestion library
- **Data privacy**: On-device consumption; data minimization; governance guardrails needed
- **Financial freedom**: 18.3pp higher savings; scalable without coaching; direct cost reduction
- **Practical implementation**: WeChat delivery; RAG+CoT pipeline; 5-week RCT; reproducible