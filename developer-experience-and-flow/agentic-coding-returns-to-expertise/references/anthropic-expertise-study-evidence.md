# Anthropic Expertise Study — Evidence Base

## Source

**Title:** Agentic coding and persistent returns to expertise
**Authors:** Zoe Hitzig, Maxim Massenkoff, Eva Lyubich, Shaoyi Zhang, Ryan Heller, Peter McCrory
**Date:** June 16, 2026
**URL:** https://www.anthropic.com/research/claude-code-expertise
**Citation:** @online{hitzig2026agentic, author = {Zoe Hitzig and Maxim Massenkoff and Eva Lyubich and Shaoyi Zhang and Ryan Heller and Peter McCrory}, title = {Agentic coding and persistent returns to expertise}, date = {2026-06-16}, year = {2026}, url = {https://www.anthropic.com/research/claude-code-expertise}}

## Dataset

- ~400,000 interactive Claude Code sessions
- ~235,000 distinct people
- Period: October 2025 – April 2026 (7 months)
- Surfaces: CLI, Claude.ai, Claude Code desktop app
- Excluded: Third-party IDEs, SDKs, headless mode (`claude -p "<prompt>"`)
- Privacy-preserving analysis tool: No researcher reads individual transcripts; occupation labels never linked to identifiable users; only aggregates over a minimum number of distinct users observed.

## Study Framework Components

### 1. Nine Work Modes

Sessions classified into the single mode that best describes what the session is trying to accomplish. A model reads the transcript and classifies it. Validated against telemetry (whether lines of code were added/deleted). Agreement: >90% of sessions classified as creating/modifying code showed code changes in telemetry.

**Code-centric:**
1. Building something new (25% of sessions)
2. Fixing something broken (26%)
3. Testing code + orchestrating other agents (5%)

**Operations:**
4. Operating software — deploying, configuring, running pipelines, monitoring (17%)

**Exploration:**
5. Understanding how an existing system works
6. Planning a change before making it
(Together: 14%)

**Non-code:**
7. Analyzing data
8. Writing prose-based documents and presentations
9. (8 + 9 together: 13%)

### 2. Decision Attribution Classifier

A privacy-preserving classifier lists all meaningful decisions in a session. Decisions separated into:
- **Planning:** what to do, which approach to take, what counts as done
- **Execution:** which files to change, what code to write, what language, which commands to run

Each decision attributed to Claude or the user. Produces two numbers per session: user's share of planning decisions, user's share of execution decisions.

**Typical session:** User makes ~70% of planning decisions, ~20% of execution decisions. Claude makes ~80% of execution decisions.

### 3. Expertise Classifier (Five-Point Scale: Novice → Expert)

Looks for three signals:
1. How precisely the user frames their directions
2. What they ask Claude to verify
3. Whether the user tends to correct Claude or Claude tends to correct the user

Expertise is task-specific: A senior engineer asking their first Rust question is a beginner at Rust. An accountant specifying reconciliation rules and catching edge cases is an expert at that task.

Examples (from SWE-chat public dataset):
- **Novice:** Generic instructions with no implied domain-specific knowledge.
- **Expert:** Deep knowledge of the codebase and technical environment; precise specification of constraints and edge cases.

### 4. Success Measurement

- **Judged success:** Classifier reads full transcript; decides (succeeded / partially succeeded / failed / no clear goal).
- **Verified success:** Judged success AND ≥1 hard verifiable signal (git commits/PRs matching work, tests passing, explicit user affirmation). Strength scored 0-5.
- **Failure signal:** Errors, failed tests, retries, user pushing back on output. Scored 0-5.
- **Troubled session:** Failure signal > 3 (verified evidence of failure).
- **Abandoned:** Judged as failed AND zero lines of code written.
- Sessions with "no clear goal" (~7.7% of sample) excluded from success analysis.

### 5. Occupation Inference

Each user's occupation inferred from session transcript, mapped to 23 major groups in BLS Standard Occupational Classification (SOC) taxonomy.

Signals used:
- Project context loaded at session start
- File names and structure
- Referenced artifacts (legal filings, clinical data, financial reports, curriculum)
- Vocabulary

Explicitly instructed NOT to treat the act of coding as evidence of a coding profession. A lawyer building a script to flag missing clauses is mapped to Legal Occupations even if the work is primarily software.

Occupation inferred in ~70% of sessions. Left unclassified when no signal.

### 6. Task Value Estimation

Economic value approximated by asking what the work would cost on a freelance marketplace, calibrated against a public dataset of real postings. Used primarily for relative comparison over time, not absolute dollar values. Price estimates are coarse.

## Key Statistical Findings

### Expertise → Output Amplification

| Expertise Level | Actions per Prompt | Words of Output per Prompt |
|---|---|---|
| Novice | ~5 | ~600 |
| Intermediate | ~8-10 | ~1,400-2,000 |
| Expert | ~12 | ~3,200 |

Regression controls: work mode, task value, month, occupation, model family; standard errors clustered by user. Both upward trends statistically significant (p < 0.001). Each adjacent-level step significant. Effect remains significant at +9% actions and +13% output per expertise level after controls.

### Expertise → Success

| Outcome | Novice | Intermediate+ |
|---|---|---|
| Verified success (all sessions) | 15% | 28-33% |
| At least partial success (all sessions) | 77% | 91-92% |
| Verified success (troubled sessions) | 4% | 15% |
| At least partial success (troubled sessions) | 60% | 80-81% |
| Abandoned (troubled sessions) | 19% | 5-7% |

Most gain is novice → intermediate. The gap between intermediate and expert is modest (slope decreases).

Adjusted rates compare only sessions sharing the same work mode, task-value band, month, task subject, and occupation type (software-related or not).

### Occupation → Success (Code-Producing Sessions)

| Occupation Group | Verified Success | At Least Partial Success |
|---|---|---|
| Management | ~31% (highest) | — |
| Computer & Mathematical | ~30% | 89% |
| Other major occupations | ~26-29% | ~88% |

Every one of the ten largest occupations lands within 7 points of software engineers in verified success. The five-point gap (30% vs 26%) has neither widened nor narrowed over seven months.

### Work Composition Shift (Oct 2025 → Apr 2026)

- Fixing broken code: 33% → 19% (nearly halved)
- Operating software: 14% → 21%
- Writing + data analysis: ~10% → ~20% (~doubled)
- Task value (estimated): rose ~27% on average across all work types
  - Building: +43%
  - Operating: +34%
  - Fixing: +32%

### Session Structure

- Typical session: ~4 turns
- Each prompt sets off ~10 agent actions on average (sometimes 100+)
- ~2.4% of sessions average >100 actions per prompt
- ~1 in 270 average >200 actions per prompt
- ~1 in 2,300 average >500 actions per prompt
- Each turn: ~2,400 words of agent output
- User controls execution (>80%): ~8 actions per turn
- Agent controls planning (>80%): ~16 actions per turn

### Adoption Context

- GitHub projects with coding agent activity: more than doubled since late 2025
- Claude Code users: average 20 hours/week of active tool time
- Fastest-growing non-software occupation groups: management, sales, legal

## Prior Work Referenced

- Sarkar (2026) — study of Cursor IDE sessions
- Baumann et al. (2026) — study of publicly available agentic coding sessions
- METR time-horizon evaluations — frontier model autonomy benchmarks
- Prior Anthropic report on Claude Code autonomy measures and how Claude Code changes work at Anthropic

## Limitations (From the Study)

1. Cannot measure real-world outcomes (code used/discarded, economically valuable artifacts)
2. Non-interactive usage (headless, third-party IDEs, SDKs) excluded — substantial share of activity
3. All classifications depend on model's reading of transcript; classifiers validated against telemetry but challenging to validate at scale
4. Session transcripts may be too long/complex for human labels to serve as ground truth
5. Conditioning on trouble selects different sessions for different users (experts hit trouble less often; their troubled sessions are likely harder problems — average estimated value of troubled session roughly doubles from bottom to top of expertise scale)
6. Positionality: Study is from Anthropic, which makes Claude Code

## Future Research Directions (From the Study)

- If returns to expertise decrease over time → models are supplying the judgment users currently bring (gains broadening beyond domain experts)
- If share of coding sessions completed by non-software occupations continues to grow → software production becoming part of ordinary work in every field
- Developing a framework to measure non-interactive usage is a priority for future work

## Implications for A-Tech

### The Core Thesis

Coding agents are not substituting for domain expertise — the more understanding a worker brings to an agent, the more quality work the agent can do. A person with domain command, in any field, may now be able to do technical work they previously could not. A person without any expertise will get far less from the same tool.

### The Competence Threshold

The gains come mostly from competence, not mastery. A working grasp of the domain captures most of the benefit; deep specialization adds only a bit more beyond that. This means:
- The onboarding target is "intermediate" not "expert"
- The largest success-rate jump is novice → intermediate (15% → 28-33%)
- Beyond intermediate, diminishing returns

### The Labor Market Signal

If these patterns hold across the economy, agentic coding tools are absorbing implementation-heavy work while rewarding those with firm understanding of the problems they solve. The most valued skill is problem understanding, not coding proficiency.