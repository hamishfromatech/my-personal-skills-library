# Proactive AI Interventions at Workflow Boundaries in IDEs — Kuo et al. (IUI 2026)

## Citation
Kuo, F., Sergeyuk, V., Chen, O., & Izadi, S. (2026). ProAIDE: Proactive AI Interventions at Workflow Boundaries in IDEs. *Proceedings of the 2026 International Conference on Intelligent User Interfaces (IUI 2026).* Collaboration between JetBrains, Carnegie Mellon University (CMU), and TU Delft.

## Study Design
- **Method**: Five-day in-the-wild field study in a production IDE (JetBrains Fleet)
- **Participants**: 15 professional developers
- **Interventions**: 229 AI interventions analyzed across 5,732 interaction points
- **System**: ProAIDE — a proactive AI coding assistant that offered code quality suggestions at three workflow trigger points
- **Duration**: Mandatory 5-day usage period; 8/18 participants continued voluntarily beyond this window (sustained engagement signal)

## Key Findings

### 1. Timing Is the Strongest Predictor of Engagement
The single most important variable influencing whether developers engage with proactive AI is *when* the intervention is surfaced — not what it says or how it is presented.

| Trigger Type | N  | Engagement | Dismissal | Ignore |
|-------------|-----|-----------|-----------|--------|
| Post-commit review | 155 | 52% | — | — |
| Ambiguous prompt detection | 35 | 46% | 23% | 31% |
| Declined AI edit follow-up | 39 | 31% | 62% | — |

- Post-commit interventions achieved the highest engagement rate (52%).
- Declined AI edit follow-ups were the least accepted (31% engagement, 62% dismissed) — developers frequently described these as "like an advertisement" or intrusive.
- Ambiguous prompt detection occupied a middle ground (46% engagement).

### 2. Workflow Boundaries Are Receptive Moments
Developers in an evaluative mindset — having just committed code — are significantly more receptive to AI suggestions than developers mid-task. The theoretical basis is the **Index of Opportunity** framework (Iqbal & Bailey, 2005):

- Mental workload decreases at task boundaries → natural windows for interruption.
- **Post-commit**: Working memory has been externalized into the commit (message, diff, staged changes). Attentional resources are freed. The developer transitions from an *implementation* mindset to an *evaluative* mindset.
- **Mid-task**: Cognitive resources are engaged in active implementation. Any intervention competes directly for the same cognitive resources the developer is already using, producing friction, disruption, and resentment.

### 3. Proactively Triggered Suggestions Are Interpreted ~2x Faster
- **Proactive suggestions**: Median interpretation time = 45.4 seconds
- **Reactive (prompt-triggered) suggestions**: Median interpretation time = 101.4 seconds
- Statistical test: Wilcoxon W = 109.00, r = 0.533, p = 0.0016
- Interpretation: Because proactive suggestions arise with full IDE context already loaded (git diff, open files, language server state), developers spend less time re-establishing context before evaluating the suggestion. Reactive prompts require the developer to first articulate and supply context, increasing overhead.

### 4. Four Design Principles
The study distills four design principles for proactive AI coding assistants:

1. **Timely interventions at workflow boundaries**
   - Anchor interventions to natural task boundaries (post-commit, post-build, post-task).
   - Tune down prominence or suppress entirely during active mid-task implementation.
   - The cost of mistiming is high: mis-timed interventions are dismissed and can erode trust.

2. **Contextually relevant suggestions**
   - Augment the AI's context with IDE signals: language server diagnostics, git diffs, recently open tabs, build/test results.
   - Perceived utility is strongly shaped by whether the AI demonstrates contextual understanding of the developer's code. Developers who felt the AI "understood" their code rated suggestions significantly higher.
   - Generic or context-mismatched suggestions reinforced the perception of unreliability.

3. **Explainability and transparency**
   - Provide clear rationales for why a suggestion is being offered at this moment.
   - Previewable changes (diff-style) before acceptance.
   - Confidence indicators where available.
   - Without explanation, proactive suggestions can feel arbitrary or sales-like (as reported for the declined-edit follow-up trigger).

4. **Preserving user control**
   - Lightweight, dismissible pop-ups rather than forced chat invocations.
   - Configurable timing and frequency thresholds.
   - Some developers want more AI autonomy; others want explicit hotkey control. Preferences vary widely — there is no one-size-fits-all setting.
   - Some developers want automatic commit-time review; others find even moderate frequency "excessive." Offer controls.

### 5. Sustained Engagement Beyond Novelty
- 8 of 18 participants continued using ProAIDE voluntarily beyond the mandatory 5-day study period.
- This is a meaningful signal of genuine utility (as opposed to novelty-driven usage that typically decays sharply once a study's mandatory period ends).
- Sustained users were disproportionately those whose workflow included frequent, well-defined commits — i.e., those whose natural workflow generated more post-commit boundaries.

### 6. Usability and Reliability Scores
- **System Usability Scale (SUS)**: 72.8 / 100 — categorized as "good" usability, but with clear room for improvement.
- **Reliability perception**: Only 27% of participants rated AI suggestions as reliable.
- **Acceptance comfort**: 47% reported being comfortable accepting AI suggestions.
- The gap between usability (good) and reliability (low) suggests that the *delivery mechanism* works but the *content quality / trust calibration* is the next bottleneck. Designers should invest in confidence calibration, provenance/traceability, and graceful failure (clear "I'm not sure" signals).

### 7. Context Understanding Drives Perceived Utility
- Developers' perception of the AI's contextual understanding of their code was the strongest predictor of perceived utility.
- Suggestions that referenced recent edits, the staged diff, or project-specific conventions were rated far higher than generic best-practice suggestions.
- **Intent alignment**: 66% of participants reported they could easily understand how a suggestion related to their intended task. The remaining 34% represents a meaningful explanation gap — a target for improvement in rationale presentation.

### 8. Preference Heterogeneity
The study documented wide variation in developer preferences:
- **Autonomy spectrum**: Some participants wanted the AI to auto-apply suggestions; others wanted to review every change manually.
- **Control modality**: Some wanted hotkey-triggered review; others wanted fully automatic commit-time review.
- **Frequency**: Some wanted more interventions; others found the current frequency excessive.
- **Implication for design**: Expose controls. Default to conservative timing (post-commit), but let developers opt into more aggressive triggers. Avoid forcing a single interaction style.

## Three Intervention Trigger Types (Detailed)

### 1. Post-commit review (N=155, 52% engagement)
- Triggered after a developer commits changes.
- The developer has just externalized working memory into the commit — diff, message, staged files are all fresh.
- Suggestion types: code quality improvements, missed edge cases, refactoring opportunities, test coverage gaps.
- Highest engagement of all three triggers.
- This is the canonical "receptive moment" — the developer is in an evaluative mindset, attention is freed, and context is fully available.

### 2. Ambiguous prompt detection (N=35, 46% engagement)
- Triggered when the developer issues a prompt to the AI that lacks sufficient context (e.g., vague intent, missing file references).
- The AI proactively offers guidance to clarify the request rather than guessing.
- Moderate engagement — developers appreciate the help but sometimes the "ambiguity" detection is itself a false positive.
- 23% dismiss, 31% ignore (leave the suggestion visible but do not act).

### 3. Declined AI edit follow-up (N=39, 31% engagement, 62% dismissal)
- Triggered after the developer declines an AI-proposed edit.
- The AI offers an alternative suggestion or explanation for why the original edit was recommended.
- Lowest engagement. Developers frequently described this trigger as "like an advertisement" — the AI appears to be pushing rather than assisting.
- **Design implication**: Use this trigger sparingly or not at all. If used, it must be explicitly dismissible and should never auto-reopen.

## Theoretical Foundation: Index of Opportunity (Iqbal & Bailey, 2005)
- Mental workload is not constant — it varies across the task lifecycle.
- At task boundaries (e.g., commit, build completion, test pass), mental workload drops and attentional capacity is released.
- These low-workload moments are "indexes of opportunity" — natural windows where an interruption is least costly and most likely to be received constructively.
- Within a task (mid-implementation), workload is high and an interruption directly competes for cognitive resources the developer is already using, increasing the cost and friction of the interruption.
- The Kuo et al. (2026) findings empirically validate this framework in the specific context of AI coding assistants: post-commit (a strong task boundary) yields 52% engagement, while mid-task follow-ups yield only 31% engagement with 62% dismissal.

## Practical Implications for Proactive AI Assistant Design

### Do
- Anchor proactive interventions to post-commit and other clear task boundaries.
- Load full IDE context (git diff, open tabs, language server state, build/test results) before surfacing a suggestion.
- Provide clear rationales for *why now* and *what* — make the suggestion's relationship to the developer's intent explicit.
- Preview changes in diff form before acceptance.
- Offer configurability: let developers tune frequency, trigger types, and autonomy level.
- Measure interpretation time and engagement rate by trigger type as core effectiveness metrics.
- Calibrate confidence honestly — if uncertain, signal it. Reliability perception (27% in this study) is the next frontier.

### Don't
- Surface follow-up suggestions immediately after a developer declines an edit — this reads as pushiness.
- Interrupt mid-task with high-prominence UI (modals, forced chat invocations).
- Rely on generic best-practice suggestions divorced from the developer's actual code context.
- Assume one interaction style fits all — preferences vary widely across autonomy, control modality, and frequency.
- Treat high SUS scores as sufficient — usability of the delivery mechanism ≠ trust in the content. Track reliability perception separately.

## Limitations of the Study
- 15 participants is a modest sample; engagement rates should be treated as directional rather than precise.
- Single IDE (JetBrains Fleet); generalization to VS Code, Neovim, or other editors requires further study.
- 5-day mandatory period + voluntary continuation; longer-term usage patterns (months) not captured.
- The study measured engagement and interpretation time, not downstream outcomes (bug reduction, code quality improvement, developer satisfaction over weeks).
- Participant demographics (experience level, language ecosystem) not fully reported in available materials.

## Metrics Reference
| Metric | Value | Notes |
|--------|-------|-------|
| Post-commit engagement | 52% | Highest of three triggers |
| Ambiguous prompt engagement | 46% | Middle |
| Declined-edit follow-up engagement | 31% | Lowest; 62% dismissal |
| Proactive interpretation time (median) | 45.4s | |
| Reactive interpretation time (median) | 101.4s | |
| Wilcoxon test | W=109.00, r=0.533, p=0.0016 | Significant |
| SUS score | 72.8 / 100 | "Good" |
| Reliability rating | 27% rated reliable | Key gap |
| Acceptance comfort | 47% comfortable | |
| Intent alignment | 66% easily understood | 34% explanation gap |
| Sustained users | 8 / 18 | Beyond mandatory period |
| Total interventions | 229 | Across 5,732 interaction points |
| Participants | 15 (study), 18 (recruited) | Professional developers |

## Related Work
- Iqbal, S. T., & Bailey, B. P. (2005). Investigating the effect of cognitive workload on the subjective assessment of interruptibility. *Index of Opportunity* framework.
- Prior work on reactive AI coding assistants (Copilot, Cursor, Tab completion) — establishes the reactive baseline against which ProAIDE's proactive approach is compared.
- Interruption science and notification management literature — broader theoretical grounding for timing interventions.

---
*This reference file summarizes the publicly available findings of the Kuo et al. (IUI 2026) field study for use in designing proactive AI coding assistants. Consult the original paper for full methodological details, statistical analyses, and participant-level data.*