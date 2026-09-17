# Personalized Coding Agent Skills — Evidence Base

## Source
Huang, Du & Lan (UMass Amherst / OpenRefinery.ai). "Do Personalized Skills Help Coding Agents? An Empirical Study of Developer Interaction Histories." arXiv:2608.10319, August 2026.

## Dataset
- **Source**: SWE-chat (Baumann et al., 2026) — public CLI coding-agent sessions via Entire.io from GitHub
- **Filtered**: 206 sessions from 13 developers (strict filtering for reproducible replay)
- **Split**: 80% evolution set (164 sessions), 20% held-out test set (42 sessions)
- **Task categories**: code review/targeted fixes 28.6%, feature implementation 20.9%, testing/build/DevOps 15.0%
- **Model**: Codex with GPT-5.5 for all components
- **Seeds**: 5 random seeds for data split

## Experimental Conditions
- **A**: No skill (baseline)
- **B**: Target developer's personalized skill
- **C**: Personalized skill from another random developer
- **D**: Generic skill pooled across all developers

## Quantitative Results

| Condition | Score | Follow-up Rate | Win/Tie/Loss vs A |
|---|---|---|---|
| A: No skill | 65.02 ± 3.24 | 24.76% (52/210) | — |
| B: Personalized | 65.99 ± 2.14 | 30.95% (65/210) | 41.43/14.76/43.81% |
| C: Random developer | 65.94 ± 3.66 | 27.62% (58/210) | 43.33/17.62/39.05% |
| D: Generic | 68.80 ± 2.26 | 30.00% (63/210) | 50.95/14.76/34.29% |

- Generic skill: +3.78 over baseline (p=.063, not significant but consistently stronger)
- Personalized: +0.97 over baseline (p=.399, not significant)
- Random developer: +0.92 (p=.451, not significant)
- Personalized ≈ Random developer → limited evidence of developer-specific benefit

## Impact of Relevant Interaction History

| Relevant Sessions | N | B-A (Personalized vs No) | B-C (vs Random Dev) | B-D (vs Generic) |
|---|---|---|---|---|
| 0 | 3 | -6.33 | +15.00 | -8.00 |
| 1–2 | 16 | 0.00 | -1.38 | -3.81 |
| 3–5 | 11 | +0.10 | +0.20 | -7.20 |
| ≥6 | 12 | **+10.17** | **+8.92** | **+5.67** |

**Key finding**: Personalized skills become effective only when ≥6 relevant historical sessions exist. Below that, generic skills consistently outperform.

## Developer Simulator Effectiveness
- 171/210 replay instances had substantive follow-ups from both real developer and simulator
- Exact match: 59.65%
- Partial match: 29.82%
- Mismatch: 10.53%
- At least partially consistent: 89.47%
- Consistent across all 4 experimental conditions (86.96%–92.50%)

## Interaction and Execution Metrics

| Metric | A: No Skill | B: Personalized | C: Random | D: Generic |
|---|---|---|---|---|
| Agent turns | 1.29 | 1.37 | 1.33 | 1.35 |
| Unresolved follow-ups | 0.29 | 0.37 | 0.33 | 0.35 |
| Tool calls | 8.47 | 9.46 | 8.69 | 8.98 |
| Agent tokens | 442K | 597K | 521K | 643K |
| Execution time (s) | 91.76 | 106.16 | 99.62 | 104.57 |
| Files changed | 1.65 | 2.22 | 1.79 | 1.99 |
| Patch churn | 29.08 | 37.11 | 32.32 | 37.98 |
| Test command groups | 0.56 | 0.97 | 0.86 | 0.93 |
| Validation groups | 1.77 | 2.23 | 2.06 | 2.16 |
| Successful validation | 43.1% | 58.9% | 54.1% | 57.4% |

**Insight**: Skills lead to more extensive execution and more systematic validation, not less interaction.

## Skill Content Statistics

| Property | Personalized (avg) | Generic |
|---|---|---|
| Rules per skill | 14.15 | 25.00 |
| Words per rule | 16.70 | 15.32 |
| Rule words per skill | 236.38 | 383.00 |
| Similarity to generic | 0.517 | 1.000 |
| Similarity between developers | 0.443 | — |
| Unique developer-specific rules | 64.7% | — |

### Rule Category Distribution

| Category | Personalized | Generic |
|---|---|---|
| Communication | 23.4% | 20.0% |
| Workflow | 30.4% | 28.0% |
| Validation | 10.3% | 12.0% |
| Follow-up | 28.3% | 24.0% |
| Commit | 7.6% | 16.0% |

## Evidence-Grounded Refinement Effect
- Bootstrap skill: 65.71 ± 1.65
- Bootstrap + LLM refinement: 65.99 ± 2.14 (+0.28, p=.792, not significant)
- Refinement modestly improves but needs richer histories for reliable gains

## Developer-Level Variation
- Only 6/13 developers had higher scores with personalized vs no-skill (B > A)
- Only 8/13 had personalized > mismatched skill (B > C)
- 11/13 had generic > no-skill (D > A) — generic most consistent
- Developers with fewer held-out sessions had wider confidence intervals

## Skill Generation Pipeline Detail

### Bootstrap Construction
- Fixed pattern analysis template
- Asks backbone LLM to identify recurring patterns in:
  - Communication style
  - Work style
  - Follow-up handling
  - Validation preferences
  - Commit behavior
- Excludes: repository names, file paths, issue IDs, commands, task-specific details
- Output: task-independent SKILL.md

### Evidence-Grounded Refinement
- Second LLM pass: bootstrap skill + original evolution sessions
- Treats evolution sessions as primary evidence, bootstrap as candidate rules
- Verifies each rule against interaction history
- Retains rule only if supported by ≥2 independent user turns from different sessions
- Focus: communication style, work style, follow-up handling
- Avoids: programming languages, frameworks, interfaces, task types
- Must not: introduce new task requirements, prescribe environment-specific commands, justify implementing less than requested

## Replay Framework Detail
1. Task summary from all developer-authored messages (excludes agent reasoning)
2. Developer simulator: conditioned on task summary + current conversation
3. After each agent response: simulator issues focused follow-up OR "No further requests"
4. Skill injected before original first-turn request; unchanged during session
5. Max 6 interaction turns per replay
6. Scoring: SWE-chat 100-point rubric via LLM-as-judge
7. 5 random seeds for data split robustness

## A-Tech Alignment
- **Open-source AI**: applies to any open-source coding agent with skill support (Codex, Claude Code, Aider, Cline, OpenHands)
- **Data privacy**: uses only developer interaction traces, no prompt content required for skill generation
- **Financial freedom**: generic skills reduce infrastructure cost (no per-developer pipelines needed for most orgs)
- **Practical implementation**: reproducible replay framework, 206 real sessions, 13 developers, 5 seeds