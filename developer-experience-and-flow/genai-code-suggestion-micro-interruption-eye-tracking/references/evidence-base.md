# Evidence Base: Generative AI Code Suggestions & Micro-Interruption Eye-Tracking

## Source

Alakmeh, D'Angelo & Fritz — University of Zurich / Google — ICSE 2026

The first in-depth eye-tracking study examining developer interaction with generative AI code suggestions during real programming tasks. This document captures the full experimental setup, eye-tracking methodology, statistical results, suggestion engagement flow, temporal analysis, design implications in detail, and comparison with traditional autocomplete studies.

---

## 1. Experimental Setup

### 1.1 Participants

- **33 developers** recruited for the study.
- Participants performed a realistic TypeScript coding task in a web-based IDE environment.
- Sessions lasted **45 minutes** per participant.
- The task was designed to be representative of typical programming work — not artificial toy problems — to ensure that suggestion engagement patterns generalized to real development contexts.

### 1.2 Total Suggestions Logged

- **4,444 code suggestions** were generated and logged across all participants and sessions.
- Each suggestion event captured the full lifecycle: generation, presentation, first gaze, dwell duration, acceptance/rejection, and subsequent typing behavior.

### 1.3 Coding Environment

- A **web-based IDE** running TypeScript was used to ensure a controlled but realistic editing environment.
- A **custom Chrome extension** was built specifically for this study to intercept and log suggestion lifecycle events. The extension captured:
  - Timestamp of suggestion generation.
  - Timestamp of suggestion becoming visible (rendered as ghost text or inline).
  - Timestamp of suggestion dismissal or acceptance.
  - Suggestion content and length (in characters).
  - Suggestion type (single-line, multi-line, block).
  - Surrounding code context at the point of suggestion.

### 1.4 Task Design

- 45-minute sessions provided enough time for developers to enter deep work states, encounter repeated suggestion cycles, and exhibit both early-task and late-task behavior — enabling the temporal/quartile analysis.
- TypeScript was chosen as a widely-used language with strong AI suggestion tooling support, ensuring the suggestions were representative of what developers encounter in production tooling.

---

## 2. Eye-Tracking Methodology

### 2.1 Hardware

- **EyeLink Portable Duo** eye tracker.
- Sampling rate: **2000 Hz** (2000 gaze samples per second).
- This high sampling rate is critical for capturing micro-saccades and brief fixations on transient UI elements like ghost-text suggestions, which may be visible for only ~1.5 seconds.
- The EyeLink Portable Duo is a research-grade tracker with high spatial accuracy (typically <0.5° visual angle), suitable for reliably detecting whether a developer's gaze landed on a specific screen region (the suggestion) versus the surrounding code.

### 2.2 Calibration

- Standard 9-point calibration performed before each session.
- Calibration validated and repeated if drift exceeded acceptable thresholds.
- Gaze data quality monitored throughout the 45-minute session.

### 2.3 Gaze-to-Suggestion Mapping

- The suggestion rendering area was treated as a **region of interest (ROI)**.
- A suggestion was classified as "looked at" if at least one fixation fell within the suggestion ROI during its visible lifetime.
- "Never looked at" means zero fixations within the suggestion ROI for the entire duration the suggestion was on screen.
- **Dwell time** was computed as the sum of all fixation durations within the suggestion ROI during a single suggestion event.

### 2.4 Suggestion Lifecycle Logging

The Chrome extension and eye-tracker streams were synchronized via timestamps, enabling the reconstruction of each suggestion's full lifecycle:

```
Suggestion Generated → Rendered Visible → [Gaze On/Off ROI] → Accepted / Rejected / Timed Out → Post-Suggestion Typing Behavior
```

### 2.5 Micro-Interruption Definition

A **micro-interruption** was operationalized as any suggestion event where the developer's gaze was captured by the suggestion ROI, interrupting their primary task of reading/writing code. The interruption window was defined from the first fixation on the suggestion to the resumption of typing or gaze return to the primary editing region.

---

## 3. Statistical Results

### 3.1 Suggestion Engagement Summary

| Outcome | Count / Percentage | Notes |
|---|---|---|
| Total suggestions logged | 4,444 | Across 33 participants, 45-min sessions |
| Never looked at | 49.5% | Zero fixations on suggestion ROI |
| Looked at | 50.5% | At least one fixation on suggestion ROI |
| — Accepted | 23.3% (of looked-at) | Suggestion adopted into code |
| — Rejected (total) | 76.7% (of looked-at) | |
| —— Actively rejected | 5.5% (of looked-at) | Explicit dismiss/escape action |
| —— Passively rejected | 71.2% (of looked-at) | Ignored, typed over, or timed out |

### 3.2 Timing Metrics

| Metric | Value | Notes |
|---|---|---|
| Avg. suggestion visible duration | ~1.5 s | Time from render to dismissal/acceptance |
| Avg. dwell time (when looked at) | ~0.9 s | Total fixation time within suggestion ROI |
| Micro-interruptions per 45-min session | ~51 | More than 1 per minute |
| Fixed startup cost (reading) | 256 ms | Intercept of reading time vs. length regression |
| Per-character reading cost | 11.5 ms/char | Slope of reading time vs. length regression (after startup) |

#### Reading Time Model

The relationship between suggestion length and reading time was modeled as:

```
reading_time = 256 ms + (11.5 ms × character_count)
```

The 256 ms intercept represents a **fixed cognitive startup cost** — the time to orient attention to the suggestion and begin parsing regardless of length. The 11.5 ms/char slope represents the incremental reading cost once parsing has begun.

Key implication: even a 1-character suggestion costs 256 ms + 11.5 ms = 267.5 ms of developer attention. A 50-character suggestion costs 256 ms + 575 ms = 831 ms. The fixed cost dominates for short suggestions.

### 3.3 Deletion Rate Analysis

| Condition | Deletion Rate (Hz) | Notes |
|---|---|---|
| Baseline (normal typing, no suggestion) | 0.039 | Deletions per second |
| During micro-interruption window | 0.226 | Deletions per second |
| Multiplier | ~6× | Deletion rate is 6× higher during micro-interruption windows |

The deletion rate spike indicates that engaging with a suggestion frequently leads to **rework behavior** — developers delete, undo, or restructure code at a substantially elevated rate immediately after a suggestion event. This holds even for rejected suggestions, suggesting that the act of reading and evaluating a suggestion perturbs the developer's editing trajectory.

### 3.4 Suggestion Length vs. Acceptance

| Outcome | Avg. Length (chars) |
|---|---|
| Accepted | 27.7 |
| Rejected | 27.3 |

The difference (0.4 characters) is **not statistically significant.** Suggestion length is not a predictor of acceptance. This challenges the common design heuristic of "make suggestions shorter to increase acceptance" — the data shows length is not the lever.

### 3.5 Temporal / Quartile Analysis

| Task Quartile | Acceptance Rate |
|---|---|
| Q1 (start of task) | 19.9% |
| Q2 | ~mid-range |
| Q3 | ~mid-range |
| Q4 (end of task) | 30.1% |

Acceptance rate rises from **19.9% in the first quartile to 30.1% in the fourth quartile** — a relative increase of ~50%. Possible explanations:

1. **Cognitive load accumulation**: As the task progresses and mental fatigue builds, developers may become more willing to accept suggestions to reduce effort.
2. **Context maturation**: The AI's context window becomes richer later in a task (more surrounding code, clearer intent), producing more relevant suggestions.
3. **Task structure**: Later phases of a task may involve more boilerplate or repetitive code where suggestions are more useful.
4. **Trust calibration**: Developers may need time to calibrate trust in the suggestion system before accepting more frequently.

The temporal trend is a strong signal that **delivery timing** is a first-class design lever, not a secondary concern.

---

## 4. Suggestion Engagement Flow (Detailed)

```
                        ┌─────────────────────────┐
                        │   Suggestion Generated   │
                        │   & Rendered Visible     │
                        └────────────┬────────────┘
                                     │
                    ┌────────────────┼────────────────┐
                    │                                   │
              (49.5%)                           (50.5%)
           Never Looked At                      Looked At
           (zero fixations                  (≥1 fixation
            on suggestion ROI)               on suggestion ROI)
                    │                                   │
                    ▼                                   ▼
          ┌─────────────────┐              ┌─────────────────────┐
          │   Discarded      │              │  Developer Reads    │
          │   (no cognitive  │              │  & Evaluates        │
          │    engagement)   │              │  (dwell ~0.9 s)     │
          └─────────────────┘              └──────────┬──────────┘
                                                       │
                                  ┌────────────────────┼────────────────────┐
                                  │                                         │
                            (23.3%)                                   (76.7%)
                            Accepted                                  Rejected
                                  │                                         │
                                  ▼                          ┌──────────────┼──────────────┐
                         ┌────────────────┐                  │                             │
                         │ Code Adopted    │            (5.5%)                        (71.2%)
                         │ into edit       │         Actively                     Passively
                         │  (may be edited)│         Rejected                      Rejected
                         └────────────────┘         (explicit                    (ignored,
                                                       dismiss)                   typed over,
                                                       │                          or timed out)
                                                       │                             │
                                                       ▼                             ▼
                                              ┌────────────────┐       ┌──────────────────────┐
                                              │ Suggestion     │       │ Suggestion fades     │
                                              │ dismissed by   │       │ or is overwritten    │
                                              │ user action    │       │ by continued typing  │
                                              └────────────────┘       └──────────────────────┘
```

### Key Observations from the Flow

1. **The majority path is "generate → never look at → discard."** Nearly half of all suggestions follow this path, representing pure waste in terms of compute and rendering.
2. **The second most common path is "generate → look at → passively reject."** The developer spends ~0.9 s reading, decides it's not useful, and continues typing — the suggestion is overwritten. This is the micro-interruption path.
3. **Active rejection is rare (5.5%).** Developers rarely bother to explicitly dismiss suggestions; they simply ignore them and keep typing. This means the "reject" signal is mostly passive and inferred from behavior, not explicit user action.
4. **Acceptance is the minority path (23.3% of looked-at, ~11.7% of all suggestions).** The suggestion system's success rate, when measured against all suggestions generated, is low.

---

## 5. Temporal Analysis (Detailed)

### 5.1 Within-Task Temporal Pattern

The study divided each 45-minute session into quartiles by elapsed task time and measured acceptance rate within each quartile:

- **Q1 (0–25% of task time):** 19.9% acceptance — lowest.
- **Q4 (75–100% of task time):** 30.1% acceptance — highest.
- The increase is monotonic across quartiles, indicating a consistent trend rather than a single spike.

### 5.2 Interpretation

The temporal pattern has two major design implications:

1. **Suppress early, accelerate late.** Suggestion systems that flood suggestions from the moment a developer opens a file are fighting against the data — early suggestions are the least likely to be accepted. A warm-up period with reduced suggestion frequency could cut wasted interruptions without sacrificing acceptance.

2. **Context maturity matters.** Later in a task, the AI has more context (more code written, clearer intent patterns), and the developer has a more stable mental model. Both factors likely contribute to higher acceptance. Systems that explicitly track "context readiness" before suggesting could improve the signal-to-noise ratio.

### 5.3 Session-Level vs. Task-Level Effects

The 45-minute session was treated as a single task for the quartile analysis. Whether the temporal effect holds at finer granularity (e.g., within a 10-minute sub-task) or at coarser granularity (e.g., across multiple sessions in a day) is an open question for future work.

---

## 6. Design Implications (Detailed)

### 6.1 Reduce Suggestion Volume

**Evidence:** 49.5% of suggestions are never looked at; 76.7% of looked-at suggestions are rejected. Combined, only ~11.7% of all generated suggestions are accepted.

**Principle:** The default "always-on, stream-everything" strategy imposes cognitive costs (micro-interruptions, screen clutter, deletion-rework spikes) that are not justified by the adoption rate. Suggestion systems should:

- Set a higher confidence threshold before surfacing suggestions.
- Suppress suggestions during active typing bursts (high keystroke rate).
- Batch or queue suggestions and surface them at natural pause points.
- Provide a "suggestion density" control so developers can tune frequency to their preference.

### 6.2 Temporal Awareness

**Evidence:** Acceptance rate rises from 19.9% (Q1) to 30.1% (Q4) across task quartiles.

**Principle:** Suggestion frequency should be temporally adaptive:

- **Early task / fresh file:** Reduce suggestion frequency. The developer is building a mental model; interruptions are most costly here.
- **Mid task:** Gradually increase suggestion availability as context stabilizes.
- **Late task:** Allow higher suggestion frequency; acceptance is highest and cognitive fatigue may make assistance more welcome.

Implementation could use simple heuristics (time since session start, lines of code written, keystroke velocity) or learned models that predict "context readiness."

### 6.3 Context-Adaptive Suggestion Style

**Evidence:** Suggestion length does not impact acceptance (accepted: 27.7 chars, rejected: 27.3 chars, not significant).

**Principle:** The lever is not length but **contextual fit.** Suggestion systems should adapt the style and granularity of suggestions to the editing context:

- **Boilerplate / repetitive code:** Full-line or block suggestions may be appropriate (high likelihood of correct completion).
- **Novel logic / algorithmic code:** Suppress or reduce to single-token suggestions (low likelihood of correct guessing, high interruption cost).
- **Refactoring context:** Offer structural suggestions (e.g., extract function) rather than inline completions.
- **Debugging context:** Suppress suggestions entirely; the developer is reading, not writing.

### 6.4 Alternative Presentation Formats

**Evidence:** Inline ghost text imposes a micro-interruption on every appearance (~51/session, >1/min). The fixed 256 ms startup cost means even brief ghost-text appearances tax attention.

**Principle:** Move away from always-visible inline ghost text. Explore:

| Format | Mechanism | Benefit |
|---|---|---|
| **Collapsible preview** | Suggestion hidden behind a clickable/expandable indicator | Zero interruption cost until developer chooses to look |
| **Overlay popover** | Suggestion appears in a popover on explicit trigger (e.g., key combo, hover) | Developer controls when to engage |
| **Side panel** | Suggestions queued in a dedicated panel, not inline | Never occludes active editing region; developer browses on demand |
| **Deferred delivery** | Suggestions queued and surfaced at natural break points (end of statement, save event, idle detection) | Aligns suggestion delivery with cognitive break points |
| **Ghost text with gaze-gating** | Ghost text appears but is dimmed/transparent until gaze is detected on the suggestion region | Reduces visual clutter; only "lights up" when developer is already looking |

The gaze-gating concept is particularly interesting given this study's methodology — it suggests a feedback loop where the system uses gaze data (if available via webcam-based tracking) to adapt suggestion presentation in real time.

### 6.5 Micro-Interruption Mitigation

**Evidence:** Deletion rate spikes to 0.226 Hz (6× baseline) during micro-interruption windows.

**Principle:** Beyond reducing frequency, systems should mitigate the *cost* of each interruption:

- **Undo-aware suggestions:** If a suggestion is rejected and the developer deletes code, offer a quick "revert to pre-suggestion state" action.
- **Non-destructive presentation:** Suggestions that don't overwrite or displace existing code until explicitly accepted.
- **Flow-state detection:** If the system detects a high-velocity typing burst (high keystroke rate, low pause time), suppress all suggestions until velocity drops.

---

## 7. Comparison with Traditional Autocomplete Studies

### 7.1 Traditional Autocomplete vs. Generative AI Suggestions

| Dimension | Traditional Autocomplete | Generative AI Suggestions (this study) |
|---|---|---|
| Suggestion content | Single token / symbol / line from language model of codebase | Multi-token, multi-line, sometimes full blocks |
| Suggestion frequency | Very high (triggered on most keystrokes) | Lower frequency but longer content |
| Acceptance mechanism | Tab/Enter to accept | Tab/Enter or explicit accept |
| Typical acceptance rate | Higher (~25–40% in prior studies) | Lower (~23.3% of looked-at, ~11.7% overall) |
| Interruption character | Brief, predictable, single-token | Variable length, unpredictable, higher cognitive load |
| Reading cost | Low (single token, often already partially typed) | Higher (256 ms fixed + 11.5 ms/char) |
| Deletion/rework risk | Low (single token, easy to undo) | Higher (6× deletion rate during interruption windows) |

### 7.2 Key Differences

1. **Traditional autocomplete benefits from predictability.** Developers build a mental model of when and what the autocomplete will suggest (e.g., after typing `obj.`, expect method names). This predictability reduces the cognitive startup cost. Generative AI suggestions are less predictable — the developer must read and evaluate each one from scratch, incurring the full 256 ms startup cost every time.

2. **Traditional autocomplete has higher signal-to-noise.** Because it suggests from the known symbol space (codebase API, language keywords), the suggestions are more likely to be relevant. Generative AI suggestions cover a wider space but with lower per-suggestion accuracy, leading to the 76.7% rejection rate.

3. **Generative AI suggestions are more disruptive when wrong.** A wrong traditional autocomplete suggestion is a single token — easy to ignore, low deletion risk. A wrong generative AI suggestion may be a multi-line block that the developer has invested 0.9+ seconds reading, and the elevated deletion rate (0.226 Hz) suggests that rejecting a substantial suggestion perturbs the editing flow more than rejecting a token.

4. **The micro-interruption concept is more acute for generative AI.** Traditional autocomplete interruptions are shorter and more routine. Generative AI suggestions, by virtue of their length and unpredictability, impose a higher per-interruption cognitive cost, making the ~51/session figure more consequential.

### 7.3 Prior Autocomplete Eye-Tracking Studies

Prior eye-tracking studies of traditional autocomplete (e.g., work by Bragdon et al., Proksch et al.) generally found:
- Developers look at autocomplete suggestions more frequently (higher look-at rate).
- Acceptance rates are higher.
- Dwell times are shorter (single-token suggestions are faster to evaluate).

This study's contribution is extending the eye-tracking methodology to **generative** AI suggestions, where the content is longer, less predictable, and imposes a fundamentally different cognitive cost structure. The 256 ms fixed startup cost and the 6× deletion spike are findings specific to the generative AI context and were not prominent in traditional autocomplete studies.

---

## 8. Limitations and Open Questions

- **Single language (TypeScript):** Results may not generalize to all languages. Dynamically-typed languages or languages with different boilerplate profiles may show different acceptance/interruption patterns.
- **45-minute sessions:** Longer sessions or multi-day studies may reveal fatigue effects or adaptation patterns not captured here.
- **Single IDE/environment:** The web-based IDE and specific suggestion tooling may have presentation characteristics that influenced results.
- **Participant population:** The 33 participants may not represent all developer demographics, experience levels, or domains.
- **No causal intervention:** The study is observational — it measures existing behavior, not the effect of changing suggestion UX. The design implications are evidence-informed hypotheses, not validated interventions.
- **Gaze-gating future work:** The study used gaze data for analysis; using gaze data for real-time suggestion adaptation (gaze-gated presentation) is a proposed future direction, not something tested here.

---

## 9. Replication and Open Resources

The study provides:
- **Replication package** — analysis scripts, statistical models, and data processing pipeline.
- **Dataset** — anonymized suggestion logs and gaze data (subject to ethical/privacy review for access).
- **Chrome extension** — the custom suggestion lifecycle logging tool used in the study.

These resources enable independent verification, extension to new languages/IDEs, and replication of the eye-tracking methodology in other developer-tool contexts — aligning with open-source research practices.

---

## 10. Citation

Alakmeh, D'Angelo & Fritz. "Generative AI Code Suggestions and Developer Micro-Interruptions: An Eye-Tracking Study." ICSE 2026. University of Zurich / Google.