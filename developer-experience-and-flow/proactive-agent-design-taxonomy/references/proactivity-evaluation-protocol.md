# Proactive Coding Agent Proactivity — Evaluation Protocol Deep Dive

## Source

Bui, N.D.Q. & Evangelopoulos, G. (2026). "Agentic Coding Needs Proactivity, Not Just Autonomy." arXiv:2605.06717v1 [cs.SE]

## The Decision-Theoretic Formulation

The paper formalizes proactive agent behavior using a decision-theoretic model rooted in Horvitz's (1999) mixed-initiative interaction principles:

```
a* = argmax_{a ∈ A} E_{p(o|a,s_t,E_t)} [U(o;θ)] - Cost_int(s_t, a; θ)
```

Where:
- `s_t` = developer state at time t (open buffer, branch, recent commits, calendar, sprint deadlines, ticket status, communication context)
- `E_t` = cross-context event stream since last decision (code, project, communication, infrastructure, developer behavior)
- `A = {notify, question, draft, stay silent}` = insight action space
- `o` = developer-observable outcome of action a
- `U(o;θ)` = utility function scoring outcome usefulness to the developer
- `θ` = learned per-developer model
- `Cost_int(s_t, a; θ)` = cost of interrupting the developer in their current state

The agent picks the action whose expected payoff exceeds the interruption cost. When no action clears the bar, it chooses "stay silent."

## Practical Cost_int Proxy Signals

The paper suggests combining observable signals:
- IDE focus state (typing, debugging, idle)
- Edit cadence
- Test or incident activity
- Calendar status
- Recent dismiss/defer signals
- Local scoping for sensitive telemetry

## Empirical Interruption Cost Data

From the literature cited:
- On-screen interruptions degrade code comprehension; recovery ranges from 10-15 minutes for bug fixes to 30-60 minutes for architecture/security tasks (Meyer et al., 2024)
- Self-interruptions can be more disruptive than external ones (Shakeri Hossein Abad et al., 2018, 4,910 tasks, 17 developers)
- State-aware IDE assistant: 90% preference vs. 47% for persistent variant (Chen et al., 2025, 65 participants)
- Feedback-delayed LLM code suggestions: acceptance lifted from 4.9% to 18.6%, wasted inference calls cut by 75% over two months (Al Awad et al., 2025)

## Five Practical Criteria for Agent Comparison

| Criterion | Description |
|-----------|-------------|
| O1: Cost of interruption computed | System observes developer state and decides when to show a message |
| O2: Stay silent as explicit action | System can detect a candidate action and still choose not to show it |
| O3: Per-developer feedback updates policy | System records responses and changes future decisions |
| O4: Cross-context observation | System ingests events from ≥3 context categories |
| O5: Initiation channel | System can show messages without requiring a separate tool |

## Agent Comparison Results

| System | Level | O1 | O2 | O3 | O4 | O5 |
|--------|-------|----|----|----|----|-----|
| Cursor Background Agents | 1 | ✗ | ✗ | ✗ | ~ | ✓ |
| GitHub Copilot Coding Agent | 2 | ✗ | ✗ | ✗ | ~ | ✓ |
| Jules (Google) | 2 | ✗ | ✗ | ✗ | ~ | ✓ |
| Cursor Automations | 2 | ✗ | ✗ | ~ | ✓ | ✓ |
| Claude Code Routines | 2 | ✗ | ✗ | ✗ | ~ | ✓ |
| LangChain Ambient Agents | 3† | ~ | ✓ | ✓ | ~ | ✓ |

† = Conceptual reference architecture. No deployed coding agent documents meaningful interruption cost or explicit silence action.

## Insight Acceptance Criteria

A candidate insight must satisfy four checks:

1. **Relevant now:** Expected developer benefit exceeds interruption cost; policy chooses silence when the same fact can safely wait
2. **Grounded:** Reviewer can recover files, diffs, tickets, logs, messages, or behavioral signals supporting the claim
3. **Action-matched:** notify for awareness, question for missing intent, draft for low-ambiguity work, stay silent for low-value or poorly-timed items
4. **Learnable:** Acceptance, dismissal, deferral, edits, and later delegation change future timing or framing

## Three Evaluation Metrics

### Insight Decision Quality (IDQ)

```
IDQ(π) = (1/T) Σ_{t=1}^{T} S_dec(a_t^π, a_t*, s_t)
```

Where S_dec ∈ [0,1] gives full credit to the reference action, partial credit to reasonable alternatives, and penalties for false interruptions, missed opportunities, wrong action types, and bad timing. Unlike acceptance rate, IDQ scores both shown insights and deliberate silence.

### Context Grounding Score (CGS)

```
CGS = (1/|M|) Σ_{t∈M} F1(G_t, G_t*) · 1[faithful(m_t, G_t)]
```

Where M is the set of times where the action is not "stay silent," F1 is the harmonic mean of evidence precision and recall, and the faithfulness indicator equals 1 only when factual claims in the message are supported by the provided grounding.

### Learning Lift (LL)

```
LL = IDQ(π_adapted) - IDQ(π_frozen)
```

Where π_adapted may update from earlier developer feedback and π_frozen cannot. Both are evaluated on the same later decision points. Positive LL means feedback improved future timing or action choice.

## Long-Horizon Benchmark Design

A long-horizon scenario should include:
- Early weak signals
- Distractors that should be ignored
- Later confirming evidence
- A moment where silence is correct
- A later response that should affect future behavior

This structure tests whether an agent can wait when evidence is thin, connect signals across tools when the case becomes stronger, and remember feedback without overreacting to one dismissal or acceptance.

## Example Scenario: API Deprecation

1. A payment provider announces an older API version retires in two weeks
2. Initial reference action: **stay silent** (warning only in release note, no failing tests, developer working on unrelated incident)
3. Later: developer's current branch touches checkout flow, Jira ticket depends on same endpoint, CI shows deprecation warnings, Slack confirms migration planned
4. Reference action becomes **notify** (grounded in release note, affected call sites, Jira ticket, CI warning, Slack thread)
5. If developer asks agent to handle migration: later state makes **draft** the correct action
6. If developer dismisses low-priority dependency alerts during focus blocks: LL tests whether agent learns to delay similar messages while still showing urgent migration risks

## Evaluation Data Collection

- Collect as timestamped, replayable developer-workflow traces (not completed task records)
- Align repository diffs, issue/PR events, CI logs, dependency notices, communication snippets, and privacy-scoped IDE state
- Sample decision points from both candidate alerts AND quiet intervals (silence must be in the denominator)
- For privacy: store hashed event identifiers, redacted evidence windows, and provenance — not raw IDE snapshots or chat text
- Shadow mode logging: record candidate decisions even when nothing is shown

## Product Design Implications

A Level 3 coding agent should be an **accountable decision surface**:
- Inbox/agent-management view showing shown and deferred insights
- Developers can accept, edit, defer, or dismiss suggestions
- Responses recorded as feedback
- Interface shows evidence for why an insight appeared now
- Detailed behavioral telemetry kept out of team-visible channels
- Boundary between private developer state and shared project context
- Interruption tolerance and dismissal patterns remain private
- Evidence (failing tests, affected files, linked issues) remains auditable

## Reporting Requirements

A credible proactivity claim should state:
- Which event streams were observed
- How candidate insights were filtered
- How often the system chose to stay silent
- What feedback was collected
- Whether feedback changed later surfacing decisions
- Silence rates, delayed-message rates, feedback categories, and learning lift (with the denominator of candidate insights considered but not shown)