# Brandebusemeyer et al. (2026) — Interaction Type Selection in AI-Assisted Coding

## Full Citation
Brandebusemeyer, C., Zunic, A., Zimmermann, A., Schimmer, D., & Arnrich, B. (2026). *Interaction Type Selection in AI-Assisted Coding: A Mixed-Methods Field Study with GitHub Copilot*. ICSE SEIP 2026 / arXiv preprint. Conducted at SAP SE and Hasso Plattner Institute, University of Potsdam.

## Study Overview
Mixed-methods field study examining how professional developers interact with GitHub Copilot and which interaction types (in-code suggestions vs. chat) yield the best outcomes. The study combined controlled benchmark tasks with uncontrolled real-world work and collected multimodal data including physiological signals.

### Research Questions
The study investigated:
1. How do developers distribute their use between in-code suggestions and chat?
2. How does each interaction type (and combinations thereof) affect efficiency, workload, and task completion?
3. What is the relationship between interaction intensity and outcomes?
4. How does perceived cognitive load and productivity change during AI-assisted development?
5. What team-dynamic effects emerge when GenAI is added to the developer workflow?

## Methodology

### Participants
- **N = 22** professional SAP employees
  - 12 developers
  - 8 senior developers
  - 1 QA specialist
  - 1 architect
- All used GitHub Copilot as their AI coding assistant
- Personality traits assessed (used in correlation analyses)

### Study Design: 4-Day Protocol
- **Day 1**: Controlled session — benchmark tasks (HumanEval-X in Java)
- **Days 2–3**: Uncontrolled real-world work — 445 working tasks documented
- **Day 4**: Controlled session — repeat benchmark tasks + retrospective
- Ethics approved by University of Potsdam

### Data Collection (Multimodal)
| Modality | Instrument |
|---|---|
| Questionnaires | Demographics, personality, post-task, post-study |
| Screen recordings | Interaction type, frequency, duration |
| Keyboard/mouse logging | Interaction intensity, timing |
| Physiological wristband | EmbracePlus — autonomic arousal / stress signal |
| Benchmark tasks | HumanEval-X (Java) — task completion + correctness |
| Workload | NASA-TLX (perceived workload) |
| Productivity/satisfaction | SPACE framework (Copilot evaluation) |

### Metrics
- **Task duration**: wall-clock time per task
- **Task completion likelihood**: whether the task was completed (binary)
- **Perceived workload**: NASA-TLX composite and subscales
- **Perceived productivity**: SPACE framework self-report
- **Perceived cognitive load**: during-task self-report
- **AI helpfulness**: developer rating of AI output quality
- **Interaction intensity**: count of in-code suggestions accepted and chat prompts submitted per task
- **Interaction type distribution**: proportion of task time in in-code vs. chat vs. combined

## Key Findings

### Finding 1: In-Code Suggestions Dominate Day-to-Day Use
- 14 of 20 participants (70%) used in-code suggestions for more than 50% of their AI interaction time
- In-code suggestions were rated higher than chat for both **satisfaction** and **perceived productivity**
- This aligns with the intuition that the majority of day-to-day development consists of coding-heavy, local-context tasks where inline completion is the lowest-friction option

### Finding 2: Single Interaction Type Is Best — Combining Modes Provides No Benefit
This is the central, counterintuitive finding of the study.

- Using **in-code suggestions alone** improved efficiency and reduced perceived workload vs. no Copilot
- Using **chat alone** improved efficiency and reduced perceived workload vs. no Copilot
- **Combining both interaction types within a single task** produced:
  - **NO additional benefits** over single-mode use
  - Task **durations comparable to not using Copilot at all**
  - **Perceived workload comparable to not using Copilot at all**

**Interpretation**: The cognitive overhead of context-switching between inline completion and a chat panel — switching attention, reformulating intent across modalities, reconciling two streams of AI output — appears to fully negate the efficiency gains each mode provides individually. The study's authors propose that developers should pick one interaction type per task and commit to it.

### Finding 3: Chat Improves Task Completion Likelihood
- Only **chat-based interaction** significantly increased **task completion likelihood** vs. no Copilot
- In-code suggestions did not show a statistically significant increase in completion likelihood
- **Interpretation**: Chat is better suited to tasks where the developer is stuck or the path to completion is unclear — chat can provide explanations, alternative approaches, and reasoning that unblock the developer. In-code suggestions accelerate tasks the developer could likely complete anyway.

### Finding 4: Interaction Intensity Has a Sweet Spot
- **Moderate** AI interaction improves efficiency
- **Excessive** interaction — especially frequent chat prompts or frequent mode-switching — introduces overhead that **diminishes or reverses** benefits
- Observed efficiency thresholds (heuristics, not hard limits):
  - **In-code suggestions**: efficient up to ~13 interactions per task
  - **Chat**: efficient up to ~6 interactions per task
- Beyond these thresholds, the cost of reviewing/managing AI output and the latency of interaction begin to dominate

### Finding 5: Rule of Thumb for Interaction Type Selection
The authors propose a decision rule based on task characteristics:

| Dimension | → In-Code Suggestions | → Chat |
|---|---|---|
| Task type | Coding-related | Non-coding or higher-level reasoning |
| Context needed | Little (local, around cursor) | Broad (multi-file, project-level) |
| Explanation needed | None beyond inline comments | Explanations/reasoning desired |
| Example tasks | Boilerplate, documentation, unit tests, small modifications | Debugging, brainstorming, summaries, information retrieval, code comprehension |

### Finding 6: Cognitive Load During AI-Assisted Development
- During **development-heavy tasks** with AI assistance, **perceived cognitive load INCREASES** (small-to-moderate effect, **d = 0.34**) while **perceived productivity does NOT significantly change**
- This is not a failure mode — it reflects the active engagement of coding with AI (evaluating suggestions, integrating output)
- The critical moderator is **AI helpfulness**:

| AI Output Perceived As… | Cognitive Load | Productivity |
|---|---|---|
| Helpful | Stays the same | **Significantly increases (d = 0.83, large effect)** |
| Not helpful | Elevated (d = 0.34) | No significant change |

**Interpretation**: The productivity lever is output helpfulness, not interaction volume. If developers report high load and low productivity with AI, the fix is improving output quality (better prompts, better context, better task-AI fit) — not forcing more AI usage.

### Finding 7: Personality Correlation
- **Emotional stability** positively correlated with **duration of GenAI use** (Kendall's τ = 0.38, p = 0.035)
- Developers higher in emotional stability tended to use GenAI for longer stretches
- **Interpretation**: Developers lower in emotional stability may find sustained AI interaction more taxing; this is a descriptive finding, not a prescriptive one, but it suggests individualized guidance may matter

### Finding 8: Team Dynamics
- Developers **turn to GenAI faster** than team members for feedback (lower latency to first feedback)
- However, developers still rate **colleagues' feedback as more helpful** for **complex problems**
- **Interpretation**: GenAI serves as a rapid first-pass sounding board; human colleagues remain superior for nuanced, complex review. This is complementary, not substitutive.

### Finding 9: High Intent to Change Behavior Post-Study
- **73% of participants** intended to change their GenAI interaction patterns after the study
- Indicates strong latent demand for evidence-based guidance on how to use AI coding assistants effectively
- Suggests that default/ad-hoc interaction patterns are suboptimal and developers recognize this when shown the data

## Statistical Summary Table

| Finding | Effect Size / Statistic | Direction |
|---|---|---|
| Single mode vs. no Copilot (efficiency, workload) | Significant | Improvement |
| Combined modes vs. no Copilot (efficiency, workload) | Not significant | No improvement — comparable to baseline |
| Chat vs. no Copilot (task completion likelihood) | Significant | Improvement |
| In-code vs. no Copilot (task completion likelihood) | Not significant | No improvement |
| Cognitive load during dev-heavy tasks with AI | d = 0.34 | Increase (small-to-moderate) |
| Productivity when AI output helpful | d = 0.83 | Increase (large) |
| Emotional stability ↔ GenAI use duration | τ = 0.38, p = 0.035 | Positive correlation |
| Developers planning to change interaction post-study | 73% | Behavioral intent |

## Limitations
- **Sample size**: N = 22 from a single organization (SAP); generalization to other companies, languages, and tools should be validated
- **Tool specificity**: Study used GitHub Copilot; other assistants (Cursor, Continue, Tabnine, etc.) may have different in-code vs. chat quality and thus shift the optimal balance
- **Duration**: 4-day study captures short-term patterns; long-term habituation and skill development with AI are not measured
- **Intensity thresholds**: The ~13 (in-code) and ~6 (chat) interaction counts are observed medians from this study, not universal constants — they are heuristics to be validated elsewhere
- **Self-report bias**: Workload (NASA-TLX) and productivity (SPACE) are self-reported; physiological data (EmbracePlus) provides corroborating signal but is not directly interpretable as workload without further calibration
- **Language scope**: Benchmark tasks were in Java (HumanEval-X); interaction patterns may differ in dynamically-typed or less-verbose languages
- **No long-term quality measure**: The study measured perceived productivity and workload, not defect rates, code maintainability, or long-term code quality
- **Combined-mode mechanism**: The study identifies that combining modes is no better than baseline but does not fully isolate the mechanism (attention switching, intent reformulation, output reconciliation, or tool latency) — further work needed

## Practical Implications for Agent Skill Design
1. **Default to single-mode per task** — the strongest actionable finding; design workflows that encourage committing to one interaction type
2. **Route by task type** — use the rule-of-thumb table as a task classifier to pre-select the interaction mode, reducing developer decision cost
3. **Gate on helpfulness, not volume** — if productivity isn't improving, the lever is output quality, not more interactions
4. **Respect intensity limits** — flag when interaction counts exceed heuristic thresholds and suggest decomposing the task or switching modes cleanly
5. **Acknowledge individual differences** — personality traits (e.g., emotional stability) correlate with sustained AI use; guidance may need to be personalized rather than one-size-fits-all
6. **Position AI as a first-pass, not a replacement for team review** — especially for complex problems where human feedback remains rated as more helpful