# Agentic AI Adoption: Evidence from Codex — Evidence Base

## Source
Johnston, Holtz, Martin, Ong, Tambe & Chatterji (OpenAI / Columbia Business School / University of Pennsylvania Wharton / Duke University Fuqua School). "The Shift to Agentic AI: Evidence from Codex." arXiv:2606.26959, June 2026.

## Data
- **Source**: OpenAI Codex usage data
- **Populations**: Individual (personal accounts), Organizational (Business/Enterprise), OpenAI workers
- **Timeframe**: Codex launched April 2025; data through June 11, 2026
- **Analysis**: Automated privacy-protecting pipeline with LLM classifiers
- **Sampling**: 3% sample for some analyses; 4% for task classification; 0.1% for complexity estimation

## Adoption Metrics (as of June 11, 2026)

### Codex Share of Output Tokens (28-day)
| Population | Share |
|---|---|
| Individual | 16.5% |
| Organizational | 63.3% |
| OpenAI workers | 99.8% |

### Growth
- Weekly active Codex users: 5× growth Jan 1 – June 1, 2026
- Skill use: 5.4% (March 1) → 26.6% (June 11) = ~5× in 3 months

## Persona Classification
Three personas based on recent Codex requests:
- **Developer**: software development activity (writing, reviewing, refactoring code)
- **General Knowledge Worker**: non-coding work (documents, spreadsheets, memos, communication)
- **Personal**: non-work usage (hobbies, finance, education)

Validation: >90% of engineers classified as Developer; >90% of sales as General Knowledge Worker.

## Adoption by Job Function (Organizational Users)
| Function | Codex Token Share (avg user) | Codex Token Share (of total) |
|---|---|---|
| Engineering | 26.8% | 88.3% |
| Data/Analytics | 15.2% | — |
| Legal | 1.9% | 17.6% |

- Engineers: 5× increase since start of year
- Non-technical: large proportional increase but small absolute share
- Intensive users within each function dominate total tokens

## Adoption by Seniority (Organizational)
Codex share rises across the seniority distribution — agentic tooling is not only for ICs.

## OpenAI Internal Adoption Timeline
- Engineers: >50% Codex share by Jan 2026; >90% by March 2026
- Legal/Recruiting: ~0% in Jan 2026 → ~20% by early April → 75% by May 2026
- Convergence: most functions >90% by mid-2026
- Output tokens: median legal worker 13× (June vs Nov 2025); median researcher 50×

## Task Taxonomy (First-Level)
| Category | Description |
|---|---|
| Code Understanding | Code Q&A, Planning |
| Code Implementation | Backend, Frontend, App Prototypes, Game Dev, Refactoring, Bug Fixing |
| Code Validation | Code Review, Security Audit, Testing |
| Engineering Operations | Repo Management, Build/Release, Environment Config, Job Management, Production Monitoring |
| Data Analysis | Queries, Interpretation, Processing, Labeling |
| Research | Internal Knowledge, Market/Competitive, Financial Markets, Web Search |
| Knowledge Artifacts | Documents, Presentations, Spreadsheets, Multimedia, PDFs |
| Collaboration | Message Drafting, Summaries, Scheduling, Task Management, Meeting Support |
| Business Function Workflows | Finance, Sales, Marketing, Product/Design, Customer Support, Recruiting, Legal/Compliance |
| Application Management | Codex Configuration, Data Entry, Browser Actions |

## Task Complexity Growth (Individual Users, 0.1% sample)
| Threshold | Dec 2025 | May 2026 |
|---|---|---|
| ≥1 hour estimated human time | 35.4% of users | 70.2% |
| ≥8 hours estimated human time | 2.1% of users | 25.6% |

- First turn of thread: >2× more likely to be ≥1 hour vs 4th turn
- Complexity classifier validated on 1,000 Codeforces problems (r=0.65 with fastest human solve, r=0.69 with difficulty score)

## Concurrency (Week of June 11, 2026)
| Population | No Concurrent Turns | 5+ Concurrent Agents |
|---|---|---|
| Individual | 63.9% | rare |
| Organizational | 67.4% | rare |
| OpenAI workers | 10.7% | 28.6% |

- Concurrent turns = overlapping turns in different threads, >30s overlap

## Long-Running Agents (June 11, 2026)
| Percentile | OpenAI | Organizational | Individual |
|---|---|---|---|
| Median daily runtime | 2.5 hrs | lower | lower |
| P99 daily runtime | 71 hrs | +25% since April | +50% since April |
- P99 OpenAI grew 88% since April 7, 2026
- Median OpenAI: intermittent (2.5 hrs/day); P99: continuous multi-agent

## Skill Use (7-day window ending June 11, 2026)
| Population | Any Skill Use |
|---|---|
| Individual | 25.7% |
| Organizational | 30.4% |
| OpenAI workers | 96.2% |

### Skill Sources
- Preinstalled (bundled with Codex)
- Curated (standalone OpenAI-distributed)
- Plugin (bundled in plugin + app integrations)
- Custom plugin (recognized plugin, non-catalog)
- Custom (user/org-specific, not OpenAI-distributed)

- Custom skills most valuable in high-context org environments
- Growth driven by plugins and custom skills
- Skill use by task area: concentrated in technical workflows externally; diffused broadly within OpenAI (50.9% of collaboration conversations invoke skills)

## Codex Skills and Plugins Architecture
- **Skill**: authoring format for reusable, task-specific workflow — directory with required SKILL.md (name + description metadata) + optional scripts, references, assets
- **Plugin**: installable distribution unit (.codex-plugin/plugin.json manifest) packaging skills + app integrations + MCP configuration + hooks + assets
- Skills specify workflow; plugins package and distribute capabilities
- Shareable across users and organizations

## Four Stylized Facts
1. **Rapid but uneven shift**: 5× WAU growth; smallest among individuals, largest at OpenAI
2. **Delegated production**: users ask Codex to do work, not just provide advice
3. **Anchored in software but broadening**: software is leading edge; extends to research, planning, communication, data analysis at frontier
4. **Intensive users organize around parallel workflows**: concurrency, long-running agents, skill systematization

## Key Contrasts with Conversational AI
| Dimension | Conversational AI | Agentic AI |
|---|---|---|
| Primary interaction | Ask questions, get responses | Delegate tasks, get work done |
| Unit of analysis | Conversation | Delegated workflow |
| Key metrics | Active users, chats, messages | Task complexity, runtime, workflow reuse, concurrency, production output |
| User role | Asker | Delegator, monitor, reviewer, coordinator |
| Adoption barrier | Model capability | Organizational complements (file access, management expectations, review processes) |

## Workforce Implications
- Jobs shift toward directing, monitoring, integrating agent outputs
- Senior workers use for planning/review/delegation
- Non-technical roles adopt rapidly once frictions removed
- Team composition, hiring, career ladders may shift
- Productivity gains depend on workflow redesign, not just tool deployment

## A-Tech Alignment
- **Open-source AI**: applies to any agentic AI tool (open or closed); patterns are tool-agnostic
- **Data privacy**: privacy-protecting pipeline, no researcher reads underlying messages
- **Financial freedom**: understanding adoption curves helps organizations plan investment
- **Practical implementation**: production-scale data (millions of users), validated classifiers, Codeforces benchmark