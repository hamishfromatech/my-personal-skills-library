# Proactive Coding: arXiv Deep Dive

## Paper: Agentic Coding Needs Proactivity, Not Just Autonomy
**Authors:** Nghi D. Q. Bui, Georgios Evangelopoulos (Google)  
**arXiv:** 2605.06717v1 [cs.SE] 07 May 2026

### Core Thesis
Coding agents have matured from inline completion to autonomous execution. The next frontier is proactivity — agents that notice context shifts, infer what matters, and decide whether to notify, question, draft, or stay silent before a human asks. No deployed coding agent currently documents meaningful interruption-cost reasoning or explicit silence as a learned action.

### Three-Level Taxonomy

#### Level 1: Reactive
- Runs only when developer prompts
- No persistent environmental presence between requests
- Example: Copilot-style inline suggestions

#### Level 2: Scheduled
- Runs on schedules, webhooks, or predefined events
- May filter, batch, or rank outputs within trigger
- Does NOT learn a cross-context, per-developer interruption policy
- Silence is not a learned choice over the full event stream
- Examples: Cursor Automations, Claude Code Routines, Jules Scheduled Tasks

#### Level 3: Situation-Aware
- Monitors continuous event stream (repository, CI, issues, communication, IDE state)
- Computes expected utility minus interruption cost
- Treats "stay silent" as an explicit action
- Updates per-developer model from feedback
- No deployed coding agent documents this capability in 2026

### The Insight as Unit of Behavior
An insight is a context-grounded, time-sensitive hypothesis about what matters next, paired with one of four actions:
1. **Notify** — Surface state change or risk
2. **Question** — Ask for missing intent when ambiguity is high
3. **Draft** — Produce low-ambiguity artifact (patch, comment)
4. **Stay Silent** — Deliberately withhold when interruption cost exceeds benefit

### Interruption-Cost Reasoning
Decision theoretic formulation:
```
action* = argmax [ E[utility(outcome)] - Cost_interruption(state, action) ]
```

Interruption cost varies by:
- IDE focus state (typing, debugging, idle)
- Edit cadence and test activity
- Calendar status and sprint deadlines
- Recent dismiss/defer signals
- Recovery cost by task type (bug fix: 10–15 min; architecture: 30–60 min)

### Agent Comparison Table (from paper)

| System | Level | O1 Cost | O2 Silence | O3 Feedback | O4 Cross-Context | O5 Initiation |
|--------|-------|---------|------------|-------------|------------------|---------------|
| Cursor Background Agents | 1 | ✗ | ✗ | ✗ | ∼ | ✓ |
| GitHub Copilot Coding Agent | 1 | ✗ | ✗ | ✗ | ∼ | ✓ |
| Jules | 2 | ✗ | ✗ | ✗ | ∼ | ✓ |
| Cursor Automations | 2 | ✗ | ✗ | ∼ | ✓ | ✓ |
| Claude Code Routines | 2 | ✗ | ✗ | ✗ | ∼ | ✓ |
| LangChain Ambient Agents | 3† | ∼ | ✓ | ✓ | ∼ | ✓ |

*O1–O5 = testable conditions. † = conceptual reference architecture.*

### Evaluation Metrics

**Insight Decision Quality (IDQ)**
Scores whether the agent chose the right action at the right time:
- Full credit for reference action
- Partial credit for reasonable alternatives
- Penalties for false interruptions, missed opportunities, wrong action types, bad timing
- Scores both shown insights AND deliberate silence

**Context Grounding Score (CGS)**
Is the shown insight supported by the right evidence?
- F1 harmonic mean of evidence precision and recall
- Fact-check: claims in message must be faithful to evidence
- Not applicable when silence is correct action

**Learning Lift (LL)**
Does feedback improve later decisions?
- Compare adapted policy (learns from feedback) vs. frozen policy
- Evaluated on same later decision points
- Positive LL means feedback improved timing or action choice

### Key Citations from Paper
- Horvitz 1999 — Principles of Mixed-Initiative User Interfaces (CHI): foundational interruption-cost model
- Meyer et al. 2024 — Breaking the Flow (ICSE): interruption recovery times
- Mehrotra et al. 2016 — Mobile notification receptivity
- Okoshi et al. 2015 — Reducing perceived mental effort from notifications
- Chen et al. 2025 — Need Help? proactive AI assistants for programming
- Al Awad et al. 2025 — Optimizing LLM code suggestions with feedback-driven timing
- Brady 2026 — Springdrift: auditable persistent runtime with ambient self-perception

### A-Tech Relevance
- A-Coder ambient mode can leapfrog competitors by implementing Level 3 before they document it
- The insight-policy framework provides a measurable, benchmarkable competitive moat
- Local-only telemetry for interruption-cost estimation aligns with privacy-first values
