# Expertise Returns: Evidence Base & Methodology

This reference documents the measurement frameworks and classifications underlying the findings in `SKILL.md`. It draws on Hitzig et al. ("Agentic Coding and Persistent Returns to Expertise," Anthropic, June 2026) and Wu et al. ("How Do Developers Interact with AI?", arXiv:2604.16393).

> **Companion file:** `anthropic-expertise-study-evidence.md` in this same directory contains the raw source-citation details, the exact key statistical findings table, session-structure stats, adoption context, and the study's own limitations list. This file focuses on the *frameworks and methodology*; that file focuses on the *numbers and provenance*.

---

## 1. The S-IASE Model for Programming Behavior

Wu et al. introduce the **S-IASE** model to describe developer programming behavior at a fine-grained, sequential level. Each unit of programming activity is decomposed into:

- **S — Situation:** The context in which the activity occurs (the current state of the codebase, the task goal, the surrounding environment).
- **I — Intention:** What the developer is trying to accomplish at this moment (the immediate sub-goal). Intentions are higher-level than actions; a single intention may spawn several actions.
- **A — Action:** The concrete thing the developer (or agent) does — editing a file, running a command, issuing a prompt, inspecting output.
- **S — Supporting tool:** The tool or artifact mediating the action (the editor, terminal, the AI agent, a test runner, a search result). Note which tool is involved is itself informative about the division of labor.
- **E — Emotion:** The affective state associated with the activity — frustration, satisfaction, confusion, confidence. Captured via self-report or inferred from behavioral signals (e.g., repeated failed attempts, rapid re-prompts).

### Why it matters here

The S-IASE decomposition lets the study separate *intention* (a human planning signal) from *action* (an execution signal). The "~70% of planning decisions are human, ~80% of execution decisions are Claude" finding is built on classifying session events along the Intention/Action axis and attributing each to the human or the agent. Emotion signals help distinguish productive sessions from flailing ones — a component of the failure-signal detection (see §3).

### Reading the S-IASE sequence

A typical agentic coding session looks like a chain of S-IASE tuples:

1. Human sets situation (opens repo, describes task) → **Intention: "fix the login bug"**
2. Agent proposes action (reads files, edits auth module) → **Action: file edits**; **Supporting tool: Claude Code**
3. Human inspects result → **Intention: "verify the fix"**; **Emotion: tentative**
4. Human prompts again with refinement → **Intention: "also handle the expired-token case"**

The study aggregates these chains to compute the planning/execution split and to classify the *mode of work* (see §5).

---

## 2. Expertise Classifier Definitions

The study classifies users into three expertise tiers — **Novice**, **Intermediate**, and **Expert** — based on behavioral signals from their sessions rather than self-report. The classifier uses features such as prompt specificity, use of domain terminology, autonomy of verification (running tests vs. asking the agent), and the complexity of tasks attempted.

### Novice

- Prompts are vague or underspecified ("make it work," "fix this," "add a feature").
- Little use of domain-specific language or correct technical terminology.
- Rarely runs verification commands independently; relies on the agent to self-check.
- Tasks are small in scope and often isolated (single-file edits, one-off scripts).
- Session behavior shows high re-prompting without directional progress (flailing).
- Output per prompt is low (~600 words); actions per prompt ~5.

### Intermediate

- Prompts include concrete scope and constraints ("add an endpoint at `/users` that returns the list, with pagination, using the existing `db` module").
- Uses correct technical terminology and references specific files/modules/functions.
- Runs some verification independently (tests, builds) but may still lean on the agent.
- Tasks span multiple files and involve integration with existing systems.
- Output per prompt and actions per prompt are markedly higher than novice.
- Verified success rate jumps to ~28–33%.

### Expert

- Prompts are precisely scoped with implicit context ("wire the new auth middleware into the existing router chain and update the OpenAPI spec; the integration tests should cover the 401 path").
- Fluent domain language; references architecture, conventions, and edge cases unprompted.
- Independently runs verification and reviews agent output critically (reads diffs, checks tests, audits for regressions).
- Tasks are architecturally ambitious (cross-system refactors, new subsystems, non-trivial debugging of emergent behavior).
- Output per prompt ~3,200 words; actions per prompt ~12.
- Verified success rate ~28–33% — **not meaningfully higher than intermediate**, consistent with concave returns.

### Classifier signals

The classifier looks for three signals:
1. **How precisely the user frames their directions** — specificity, constraints, edge cases.
2. **What they ask Claude to verify** — whether the user directs verification or accepts output unchecked.
3. **Whether the user tends to correct Claude, or Claude tends to correct the user** — a proxy for who holds the stronger model of the task.

### Classifier validation

The classifier was validated against a hand-labeled subset of sessions (drawn from the SWE-chat public dataset, among other sources). Because it is behavior-based, it avoids the self-report bias that plagues expertise surveys (people overrate themselves). The key caveat: the classifier measures *observable agentic-coding expertise*, which the study argues correlates strongly with *task-specific domain expertise* rather than raw coding skill. Expertise is explicitly **task-specific**: a senior engineer asking their first Rust question is a beginner at that task; an accountant specifying reconciliation rules and catching edge cases is an expert at that task.

---

## 3. Success Measurement Methodology

The study defines three outcome categories:

### Judged Success

- A model-based classifier reads the full transcript and assigns one of: **succeeded / partially succeeded / failed / no clear goal**.
- Broader than verified success — includes cases where the task was clearly completed but no formal verification artifact was produced (e.g., a doc draft, a config change that "looks right").
- Higher rates than verified success by construction.
- Useful for capturing the full range of valuable agentic work, especially non-code tasks (writing, analysis).
- Sessions with "no clear goal" (~7.7% of the sample) are **excluded** from the success analysis.

### Verified Success

- Requires judged success **AND** at least one **hard verifiable signal**, e.g.:
  - Git commits / PRs matching the work performed.
  - Passing tests (the agent or user ran the test suite and it passed).
  - Successful build / compile.
  - Explicit user affirmation of completion.
- Verifiable-signal strength is scored 0–5.
- Stricter and more reliable than judged success.
- The ~15% (novice) → ~28–33% (intermediate+) figures in `SKILL.md` refer to **verified success**.
- This is the conservative measure and the one used for cross-occupation and cross-expertise comparisons.

### Failure Signals and Troubled Sessions

- **Failure signal score (0–5):** based on errors, failed tests, retries, and the user pushing back on output.
- **Troubled session:** failure signal > 3 — verifiable evidence of failure.
- **Abandoned:** judged as failed **AND** zero lines of code written.

These are used both as negative outcome categories and as features for the success classifier.

### Important caveat on conditioning on trouble

Troubled sessions are not a random subset: experts hit trouble less often, and their troubled sessions are likely *harder problems* (the average estimated task value of a troubled session roughly doubles from the bottom to the top of the expertise scale). So comparisons of success *within* troubled sessions conflate expertise with problem difficulty. The headline novice→intermediate+ gap is therefore reported on **all sessions**, not on the troubled subset.

### Why two success measures?

Judged success captures "did they get something useful done?" — important for understanding the *value* of agentic coding across all work modes (including writing and analysis). Verified success captures "did they produce a provably working result?" — important for comparisons where rigor matters (e.g., the expertise and occupation breakdowns). The gap between the two measures is itself informative: it reflects how often users skip verification.

---

## 4. Occupation Inference Methodology

The study infers each user's occupation from behavioral and contextual signals, then maps inferences to the **U.S. SOC (Standard Occupational Classification) taxonomy**.

### Signal sources

- **Project context loaded at session start** — the type of repo/project worked in.
- **File names and structure** — e.g., infrastructure repos suggest DevOps/SRE; data-analysis notebooks suggest analyst roles.
- **Referenced artifacts** — legal filings, clinical data, financial reports, curriculum, etc. These are strong occupational signals.
- **Vocabulary** — domain terminology and the framing of problems (business-language framing → management/analyst; systems-language framing → engineering).
- **Task patterns / modes of work** (see §5) — debugging-heavy + backend → likely SWE; writing/analysis-heavy → likely analyst or manager.

### Critical guardrail

The classifier is **explicitly instructed NOT to treat the act of coding as evidence of a coding profession.** A lawyer building a script to flag missing clauses in contracts is mapped to **Legal Occupations** even if the work is primarily software. This prevents the trivial inference "they're using a coding tool, therefore they're a software engineer" and is what makes the occupation comparison meaningful.

### Mapping to SOC

Inferred occupational profiles are mapped to the **23 major groups** in the U.S. BLS Standard Occupational Classification (SOC) taxonomy, including:
- **15-0000 Computer & Mathematical Occupations** (incl. 15-1250 Software Engineers, 15-2010 Data Scientists)
- **11-0000 Management Occupations**
- **13-0000 Business & Financial Operations**
- **19-0000 Life, Physical, & Social Science**
- **17-0000 Architecture & Engineering**
- Other major groups as sample size allows

Occupation is inferred in ~70% of sessions; the remainder are left unclassified when no signal is present.

### Key caveats

- Inference is probabilistic; individual misclassification is expected but averages out at the group level given the ~235k sample.
- The study reports group-level comparisons, not individual occupation claims.
- The finding that "every major occupation is within ~7 points of software engineers on verified success" is robust to the inference noise because it compares *group means*, not individuals.

---

## 5. The Nine Modes of Work

Each session is classified into the **single mode** that best describes what the session is trying to accomplish. A model reads the transcript and assigns one mode. Classification is validated against telemetry: >90% of sessions classified as creating/modifying code show corresponding code changes in the telemetry.

### The nine modes (as defined in the study)

**Code-centric:**
1. **Building something new** — creating new functions, modules, features from scratch (~25% of sessions).
2. **Fixing something broken** — debugging, diagnosing and resolving errors, crashes, unexpected behavior (~26%).
3. **Testing code + orchestrating other agents** — writing/running tests; coordinating multi-agent workflows (~5%).

**Operations:**
4. **Operating software** — deploying, configuring, running pipelines, monitoring, running scripts/tools (~17%).

**Exploration:**
5. **Understanding how an existing system works** — asking the agent to explain code, explore an unfamiliar codebase.
6. **Planning a change before making it** — scoping and designing before implementation.
   - (Modes 5 + 6 together: ~14%.)

**Non-code:**
7. **Analyzing data** — data exploration, analysis, summarization.
8. **Writing prose-based documents and presentations** — drafting docs, reports, slides.
   - (Modes 7 + 8 together: ~13%.)
9. **(Residual / other)** — sessions that don't cleanly map to the above.

### Note on the S-IASE connection

Wu et al.'s S-IASE framework (§1) informs the *sequential* decomposition of behavior within a session, while these nine modes describe the *dominant intent* of a session as a whole. A single session may contain S-IASE events spanning several modes, but is assigned the one mode that best characterizes its overall goal.

### How the composition shift reads

Over Oct 2025 → Apr 2026:

| Mode | Oct | Apr | Interpretation |
|---|---|---|---|
| Fixing something broken | 33% | 19% | Users stop fighting broken code; the agent handles more fixes autonomously. |
| Operating software | 14% | 21% | Users shift toward running/configuring/extending real systems. |
| Writing + data analysis (non-code) | ~10% | ~20% | Non-code generative work doubles — docs, data exploration, analysis. |
| (Other modes, incl. building new) | ~43% | ~40% | Roughly stable residual. |

The trend is from *reactive repair* toward *active operation and generation*. This is consistent with users learning to steer the agent toward higher-value, forward-looking work rather than using it primarily as a fix-it tool.

---

## 6. Statistical Methodology and Regression Controls

### Baseline comparisons

The headline figures (success rates, actions/prompt, output/prompt, composition shares) are descriptive statistics computed over the relevant session/user subsets. They are large-sample (hundreds of thousands of sessions) so sampling noise is small; the main threat to validity is *selection* (who opts into Claude Code telemetry), not sampling variance.

### Regression framework

For the expertise-returns and occupation comparisons, the study runs regressions of the form:

```
outcome ~ expertise_tier + occupation_group + controls + ε
```

where:
- **outcome** is verified success (binary), actions/prompt, or output/prompt.
- **expertise_tier** is the Novice/Intermediate/Expert classifier output.
- **occupation_group** is the SOC major group.
- **controls** include:
  - **Session length** (longer sessions have more chances to succeed/fail).
  - **Task complexity / mode of work** (debugging vs. implementation vs. writing).
  - **Time period** (Oct vs. Apr) to absorb the composition shift and maturation trend.
  - **Repo characteristics** (language, size, age) where available.
  - **Prompt count** (to separate "more prompting" from "better prompting").

### Key robustness checks

- **Expertise effect persists controlling for occupation:** the novice→intermediate jump survives when occupation is held constant, confirming it is not just "engineers are both more expert and more successful."
- **Occupation effect is small controlling for expertise:** once expertise tier is in the model, occupation adds little explanatory power — the "management ≈ SWE" result holds within expertise tiers, not just across the pooled sample.
- **Concavity test:** the intermediate→expert coefficient is significantly smaller than the novice→intermediate coefficient, confirming the concave returns-to-expertise claim (this is the "competence, not mastery" finding).
- **Time-trend separation:** the composition shift and task-value rise are estimated separately from the expertise effects to avoid confounding "users got better over time" with "experts do better."

### Effect sizes vs. significance

With ~400k sessions, nearly any difference is statistically significant. The study therefore emphasizes **effect sizes** (the magnitude of differences) over p-values. The practically important findings — the 2.4x/5x amplification, the ~7-point occupation spread, the concave expertise curve — are all large enough to matter, not merely detectable.

### Task value (economic proxy)

- **Task value** is approximated by asking what the work would cost on a freelance marketplace, calibrated against a public dataset of real postings.
- Used primarily for **relative comparison over time**, not absolute dollar values. Price estimates are explicitly described as coarse.
- This is the measure behind the "~27% rise in task value over 7 months" figure (and the per-mode breakdowns: building +43%, operating +34%, fixing +32%).

### Limitations acknowledged

- **Single tool, single provider:** all data is Claude Code (CLI, Claude.ai, desktop app). Third-party IDEs, SDKs, and headless mode are excluded. Generalization to other agents or IDE assistants is not established.
- **Opt-in telemetry sample:** users who opt in may differ from the broader population.
- **Occupation inference noise:** individual-level occupation is imperfectly measured (~70% of sessions inferred); group comparisons are robust, individual claims are not.
- **Success is session-scoped:** long-running, multi-session projects are not captured as a single outcome; a project that spans many sessions may succeed overall even if individual sessions show mixed verified-success rates.
- **No counterfactual:** the study observes agentic coding but does not compare against the same users doing the same tasks without the agent (no pure control). The returns-to-expertise finding is about *differences across users within agentic coding*, not about agentic coding vs. manual coding.
- **Conditioning on trouble:** experts hit trouble less often, and their troubled sessions are likely harder problems — so within-troubled-session comparisons conflate expertise with problem difficulty.
- **Positionality:** the study is from Anthropic, which makes Claude Code.
- **Real-world outcomes unmeasured:** the study cannot tell whether produced code was actually used, discarded, or economically valuable.
- **Classifier dependence:** all classifications depend on a model's reading of the transcript; classifiers are validated against telemetry where possible, but ground-truth human labels are hard to establish at this scale.

See `anthropic-expertise-study-evidence.md` for the study's verbatim limitations list and future-research directions.

## Source Provenance

- **Primary:** Hitzig et al., "Agentic Coding and Persistent Returns to Expertise," Anthropic, June 16, 2026. URL: https://www.anthropic.com/research/claude-code-expertise
- **Supporting:** Wu et al., "How Do Developers Interact with AI?" arXiv:2604.16393 (S-IASE model, modes-of-work classification).
- **Prior work referenced by the study:** Sarkar (2026, Cursor IDE sessions); Baumann et al. (2026, public agentic coding sessions); METR time-horizon evaluations.
- **Companion evidence file:** `anthropic-expertise-study-evidence.md` (raw findings, citations, session-structure stats, adoption context).