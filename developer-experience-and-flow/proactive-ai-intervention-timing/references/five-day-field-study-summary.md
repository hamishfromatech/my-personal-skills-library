# Developer Interaction Patterns with Proactive AI: Five-Day Field Study Summary

## Paper Details
- **Title:** Developer Interaction Patterns with Proactive AI: A Five-Day Field Study
- **Authors:** Nadine Kuo, Agnia Sergeyuk, Valerie Chen, Maliheh Izadi
- **arXiv:** 2601.10253 [cs.HC]
- **Venue:** Accepted to IUI'26 (Intelligent User Interfaces 2026)
- **Date:** Submitted 15 Jan 2026
- **URL:** https://arxiv.org/abs/2601.10253

## Methodology
- **Participants:** 15 professional developers
- **Duration:** 5 days in-the-wild
- **System:** Proactive feature of an AI assistant integrated into a production-grade IDE offering code quality suggestions based on in-IDE developer activity
- **Dataset:** 229 AI interventions across 5,732 interaction points

## Key Findings

### 1. Boundary Engagement Effect
Interventions at workflow boundaries (e.g., post-commit) achieved **52% engagement rates**. Mid-task interventions (e.g., on declined edit) were dismissed **62% of the time**.

### 2. Interpretation Time Reduction
Well-timed proactive suggestions required significantly less interpretation time than reactive suggestions:
- **Proactive (well-timed):** 45.4 seconds
- **Reactive:** 101.4 seconds
- **Statistical test:** W = 109.00, r = 0.533, p = 0.0016
- **Effect:** 55% reduction in interpretation time for well-timed proactive suggestions

### 3. Cognitive Alignment
The authors interpret the timing effect as "enhanced cognitive alignment": when developers are at workflow boundaries, they are already transitioning mentally, making them more receptive to contextual suggestions. When immersed in a task, context reconstruction is required, increasing interpretation burden.

### 4. Systematic Receptivity Patterns
Receptivity to proactive suggestions is not random. It follows predictable patterns tied to workflow phases:
- Post-completion moments (commit, push, test finish) = high receptivity
- Active editing and debugging = low receptivity
- Post-dismissal = lowered receptivity for a cooldown period

## Implications for Design

### Timing Rules Derived from Data
1. Target workflow boundaries (post-commit, pre-push, post-test, idle) for non-critical insights
2. Avoid mid-task interruption during active editing, debugging, or immediately after dismissal
3. Format suggestions to match the developer's current cognitive state (summary at boundaries, inline only when explicitly requested)
4. Use interpretation time as a proxy for cognitive alignment — fast processing indicates good timing

### Limitations Acknowledged by Authors
- 15 developers is a modest sample; replication with larger populations needed
- Single IDE environment; generalizability to other tools uncertain
- Code quality suggestions only; other insight types (security, architecture, learning) may have different timing profiles
- Five-day window may not capture long-term trust calibration effects

## A-Tech Relevance
- A-Coder can differentiate by implementing boundary-targeted proactive suggestions before competitors document them
- The 45.4s vs. 101.4s interpretation time difference provides a concrete UX metric for proactive feature success
- The 52% engagement rate sets a benchmark: any proactive feature below 40% boundary engagement is underperforming
- Dismissal tracking and cooldown protocols should be built into the insight policy from day one
