# Codex Agentic AI Shift Evidence Base

## Source

Johnston, Holtz, Martin Richmond, Ong, Tambe, Chatterji (OpenAI / Columbia Business School / University of Pennsylvania Wharton / Duke University Fuqua School, arXiv:2606.26959, 2026). "The Shift to Agentic AI: Evidence from Codex."

## Dataset

- OpenAI Codex usage data (agentic coding/work platform, released April 2025)
- Three populations: individual users (personal plans), organizational users (Business/Enterprise), OpenAI workers
- Automated, privacy-protecting pipeline (no researcher reads underlying messages)
- Data through June 11, 2026

## Adoption by Population

### Weekly Active Users and Token Share

| Population | Codex Share of Active Users (28-day) | Codex Share of Output Tokens |
|-----------|--------------------------------------|------------------------------|
| Individual | <1% | 16.5% |
| Organizational | 17.3% | 63.3% |
| OpenAI workers | ~100% | 99.8% |

- Individual adopters are unusually intensive (high tokens per user)
- Organizational adoption broader; Codex majority of output tokens
- OpenAI: Codex largely replaced ChatGPT for work-related AI use

### Growth by Persona

- Developers remain important share, especially among individual/organizational accounts
- Growth faster among Non-developers (General Knowledge Workers + Personal)
- Within OpenAI: usage diffused from engineering to all departments

### Adoption by Job Function (Organizational Users)

| Job Function | Codex Token Share (avg user) |
|-------------|------------------------------|
| Engineering | 26.8% (quintupled since year start) |
| Data/Analytics | 15.2% |
| Legal | 1.9% (but 17.6% among legal users who adopt) |

- Technical roles adopt earlier; non-technical growing proportionally
- Adoption across seniority levels (not just junior ICs)

### Adoption Within OpenAI

- Engineers: >90% of tokens on Codex by March 2026
- Legal, recruiting: near zero in Jan 2026 → ~75% by April 2026
- Rapid convergence across functions after internal adoption push

## Task Composition

### First-Level Task Taxonomy

- Software-related: code implementation, code understanding, code validation, engineering operations, application management
- Knowledge work: data analysis, research, knowledge artifacts, collaboration, business function workflows
- Across all account types, software production dominates
- OpenAI users show more research, business function workflows, engineering operations

### Task Complexity Over Time (Individual Users)

| Threshold | Dec 2025 | May 2026 | Change |
|-----------|----------|----------|--------|
| ≥1 hour task | 35.4% | 70.2% | +34.8pp |
| ≥8 hour task | 2.1% | 25.6% | +23.5pp (~10x) |

- Most complex queries concentrated at start of sessions (first turn 2x+ likely to be >1hr)
- Complexity increases as models improve and users gain experience

## How People Use Agentic AI

### Turn Concurrency (Week ending June 11, 2026)

| Population | 0 concurrent | Peak 2 | Peak 5+ |
|-----------|--------------|--------|---------|
| Organizational | 67.4% | majority of rest | — |
| Individual | 63.9% | majority of rest | — |
| OpenAI | 10.7% | — | 28.6% |

- OpenAI workers manage portfolios of parallel agentic work
- External users predominantly single-threaded

### Long-Running Agents (Cumulative Daily Runtime)

| Population | Median | P99 |
|-----------|--------|------|
| OpenAI | 2.5 hours | 71 hours |
| Organizational | lower | +25% since April |
| Individual | lower | +50% since April |

- P99 OpenAI users have several agents running concurrently at any hour
- Intensive users rapidly expanding delegated work

### Skill Use (Systematization)

| Metric | March 1 | June 11 |
|--------|---------|---------|
| Any skill use (active users) | 5.4% | 26.6% |
| Individual | — | 25.7% |
| Organizational | — | 30.4% |
| OpenAI | — | 96.2% |

- Growth from plugins (recurring task domains) and custom skills (local procedural context)
- Custom skills highest in organizational settings (team standards, recurring reports, org-specific workflows)
- 50.9% of collaboration conversations invoke ≥1 skill (within OpenAI)

## Output Growth Within OpenAI

- Median worker output tokens rose 10x+ in every job function (Nov 2025 → June 2026)
- Legal: median 13x more monthly output tokens
- Researcher: median 50x more
- Growth reflects shift from conversational to agentic + more intensive persistent use

## Theoretical Implications

- Agentic AI is more than a more capable version of conversational AI
- Users delegate work (producing artifacts, modifying systems, executing workflows) not just ask questions
- Adoption depends on organizational complements (workflow redesign, review processes, skills)
- Long-run gains require reorganizing production around new capabilities (electrification analogy)

## A-Tech Alignment

- **Open-source**: findings apply to open-source agent frameworks (OpenHands, SWE-agent, Codex CLI)
- **Data privacy**: on-device agent workflows preserve privacy while enabling delegation
- **Financial freedom**: parallel agent orchestration multiplies individual productivity
- **Practical implementation**: skill systematization framework directly applicable to A-Tech skill design

## Limitations

- OpenAI is an unusually favorable environment (frontier familiarity, cheap marginal usage, high buy-in)
- Individual/organizational adoption remains lower and more uneven
- Data is observational, not experimental — cannot establish causal effects
- Codex usage may not generalize to all agentic AI tools