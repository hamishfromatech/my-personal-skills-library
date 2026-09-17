# SWE-chat Evidence Base

## Dataset Overview
- **Source:** Baumann et al., Stanford University, arXiv:2604.20779, April 2026
- **Collection Method:** Entire.io CLI installs git hooks on opt-in public GitHub repositories; automatically logs coding agent sessions and links them to commits with line-level human vs. agent attribution
- **Scale (April 2026):** 6,000 sessions, 13,000 checkpoints, 63,000+ user prompts, 355,000+ agent tool calls, 2.7M logged events
- **Agents Covered:** Claude Code (~85%), OpenCode, Gemini CLI, Cursor, Factory AI Droid

## Coding Mode Definitions

| Mode | Definition | Share | Agent-Authored Code |
|------|-----------|-------|---------------------|
| Human-only | All committed code by human | 22.7% | 0% |
| Collaborative | Human and agent jointly contribute | 36.5% | 0-99% |
| Vibe coding | Agent authors >99% of committed code | 40.8% | >99% |

## Code Survival and Efficiency

| Metric | All Modes | Collaborative | Vibe Coding |
|--------|-----------|---------------|-------------|
| Code survival rate | 44.3% | 38.2% | 59.0% |
| Coding efficiency | 50.3% | 44.1% | 64.6% |
| Agent self-overwrite | 44.3% | 38.2% | 59.0% |
| Human overwrite | 9.3% | 10.1% | 7.1% |
| Human deletion | 42.2% | 46.9% | 30.9% |

## Cost and Time Efficiency (per 100 committed lines)

| Metric | Human-only | Collaborative | Vibe Coding |
|--------|------------|---------------|-------------|
| Token cost | ~$0.07 | ~$0.05 (best) | ~$0.13 |
| Time (min) | 8.6 | 4.8 (best) | 12.6 |
| Agent runtime (min) | ~7.3 | ~4.1 | ~10.7 |

## Security Findings (per 1,000 committed lines, Semgrep)

| Metric | Human-only | Collaborative | Vibe Coding | Overall |
|--------|------------|---------------|-------------|---------|
| Vulnerabilities introduced | 0.08 | 0.14 | 0.76 | 0.11 |
| Vulnerabilities fixed | 0.04 | 0.08 | 0.52 | 0.06 |
| Net increase | 0.04 | 0.06 | 0.24 | 0.05 |

**Vulnerability Types:** Path traversal, command injection (CWE-78), unsafe format strings (CWE-134), SQL injection (CWE-89), missing integrity checks (CWE-353)

## Interaction Statistics

### User Intent Distribution
| Intent | Share |
|--------|-------|
| Understand existing code | 19.0% |
| Create new code | 13.4% |
| Git operations | 13.4% |
| Debug | 13.0% |
| Refactor | 9.2% |
| Connect/integrate | 1.0% |
| Test | 4.0% |
| Other | 26.6% |

### Tool Call Distribution
| Tool | Share |
|------|-------|
| Bash commands | 33% |
| File read/edit/search | 48% |
| Git/gh | 11.9% |
| Write | 2.9% |
| Web fetch/search | 0.5% |
| MCP | 1.9% |

### Oversight Rates
| Action | Rate |
|--------|------|
| Agent asks for clarification | 1.1-2.6% |
| User interrupts agent | 3.3-6.0% |
| User pushback (corrections, rejections, failure reports) | 39-41% |

## User Persona Distribution
| Persona | Share |
|---------|-------|
| Expert Nitpicker | 39.7% |
| Vague Requester | 33.5% |
| Mind Changer | 19.9% |
| Other | 7.0% |

## Comparison with Existing Datasets

SWE-chat is the first dataset combining:
- Real user prompts (not curated benchmark tasks)
- Agent tool-use trajectories
- Code diffs with human vs. agent authorship attribution
- Complete interaction context

Existing datasets (SWE-smith, CoderForge, SERA, etc.) capture agent trajectories but lack human prompts and code attribution.

## Temporal Trends
- Vibe coding share doubled from 20% to 40% over 3-month observation window
- Agent turn 99.9th percentile duration now exceeds 100 minutes (growing)
- Pushback and interruption rates remain stable over time

## Limitations
- Data from developers who opt into Entire.io (early adopter bias)
- ~85% from Claude Code (limited agent diversity)
- Failed sessions (where user abandons agent output) may not be captured
- LLM-as-judge annotations are imperfect (validated against human gold labels)
- Sample skewed toward Python repositories on GitHub

## A-Tech Alignment
- **Open-source AI:** Enables realistic benchmarks for open-source coding agents
- **Data privacy:** Code attribution without exposing proprietary data
- **Practical implementation:** Living dataset design supports continuous evaluation
- **Financial freedom:** Cost-efficiency insights inform pricing of agent infrastructure