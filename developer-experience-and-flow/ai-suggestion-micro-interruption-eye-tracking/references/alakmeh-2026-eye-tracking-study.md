# Alakmeh et al. (2026) — Eye-Tracking Study of AI Code Suggestion Micro-Interruptions

**Full Reference**: Alakmeh, F., D'Angelo, G., & Fritz, T. (2026). "The Cost of AI Code Suggestions: An Eye-Tracking Study of Micro-Interruptions in Programming." ICSE 2026 — International Conference on Software Engineering. University of Zurich & Google.

**Replication package**: eye-for-ai.hasel.dev

---

## Study Overview

The first in-depth eye-tracking study examining how developers interact with generative AI code suggestions during real programming tasks. The study quantifies the cognitive cost of AI code suggestions as cumulative micro-interruptions that disrupt developer flow.

### Research Question
How do developers visually engage with AI-generated code suggestions, and what is the cognitive cost of these suggestions in terms of micro-interruptions to flow?

---

## Methodology

### Participants
- **33 participants** (17 professional developers, 18 students)
- Average age: 27.23 years
- Recruited from 3 companies + university lab
- Same eye-tracking setup used across all sites for consistency

### Task Design
- 45-minute programming sessions
- Task: TypeScript programming (Chrome Dino game modification)
- Conducted in VS Code Web IDE with GitHub Copilot suggestions enabled

### Apparatus
- **EyeLink Portable Duo** eye tracker: 2000 Hz sampling rate (high-resolution)
- Custom Chrome extension tracking Copilot suggestion events in VS Code Web IDE
- **OBS Studio** (open source) for screen recording
- **WebLink** eye tracker software
- Open-source toolchain throughout

### Data Collected
- 4,444 code suggestions logged across all participants
- Fixation data from eye tracker
- Acceptance/rejection events (active via ESC/click, passive via continued typing)
- Deletion events with timestamps
- Dwell time per suggestion
- Suggestion content and length

---

## Key Findings

### 1. Half of AI Suggestions Are Never Looked At
- **50.5%** of 4,444 suggestions received **zero fixations** (participants never visually attended to them)
- Only 49.5% of suggestions were looked at even briefly

### 2. High Rejection Rate Among Looked-At Suggestions
- Of suggestions that were looked at:
  - **76.7% rejected**
    - 5.5% actively rejected (ESC key or click)
    - 71.2% passively ignored (developer simply continued typing)
  - **23.3% accepted**

### 3. Brief Dwell Times
- Average dwell time on a suggestion: **0.9 seconds** (±0.02 SE)
- Developers make very fast accept/reject decisions

### 4. Micro-Interruption Frequency
- Each suggestion introduces a micro-interruption that disrupts flow
- Participants encounter approximately **51 micro-interruptions per 45-minute session**
- This is **>1 micro-interruption per minute** of programming

### 5. Deletion Rate Spike During Interruptions
- Deletion events occur at **5.8x higher rate** during micro-interruption windows
  - 0.226 Hz during interruption windows
  - 0.039 Hz during rest of session
- Suggests suggestions trigger corrective/deletion behavior, indicating flow disruption

### 6. Fixed Startup Cost for Reading
- **~256ms** fixed startup cost to begin reading any suggestion, regardless of length
- Per-character cost stabilizes at **~11.5 ms** after startup cost is absorbed
- Implication: shorter suggestions are less efficient per-character (startup cost not amortized)

### 7. Length Does NOT Affect Acceptance Rate
- Accepted suggestions: average 27.7 characters
- Rejected suggestions: average 27.3 characters
- No statistically significant difference
- Longer suggestions are not rejected more often — and are more efficient when correct

### 8. Acceptance Increases Over Task/Session Progression
- Acceptance rate by task quartile:
  - First quartile: 19.9%
  - Last quartile: 30.1%
- Developers become more receptive to suggestions as tasks progress (possibly at natural completion boundaries)

### 9. Subjective Ratings
- **66%** rated suggestions as "slightly distracting"
- **40%** rated suggestions as "moderately helpful"
- Developers recognize the distraction cost but still find value

---

## Design Implications

### 1. Reduce Suggestion Volume
Current AI coding assistants present suggestions as an uninterrupted stream. The study shows over half are never looked at and 76.7% of looked-at suggestions are rejected. Reducing volume directly lowers cumulative cognitive overhead.

### 2. Optimize Timing Based on Workflow Phase
Acceptance rates rise from 19.9% (task start) to 30.1% (task end). Suggestion systems should modulate frequency and assertiveness based on where the developer is in their workflow — fewer interruptions during deep work, more at natural boundaries.

### 3. Favor Longer, Complete Suggestions
The fixed ~256ms startup cost means longer, correct suggestions are more efficient per character. When a suggestion is going to be correct, completeness pays off. Length does not reduce acceptance rate.

### 4. Alternative Presentation Formats
Inline ghost text is the current default. Alternatives worth exploring:
- Collapsible previews
- Overlays
- Side panels
These reduce peripheral visual disruption while preserving access to suggestions.

### 5. Attention-Aware Generation
Rather than generating suggestions continuously, systems should model developer attention state to decide **whether** and **when** to generate. Eye-tracking or behavioral signals could inform this.

---

## Statistical Summary Table

| Metric | Value |
|---|---|
| Total suggestions logged | 4,444 |
| Suggestions with zero fixations | 50.5% |
| Of looked-at: rejected (active) | 5.5% |
| Of looked-at: rejected (passive/ignored) | 71.2% |
| Of looked-at: total rejected | 76.7% |
| Of looked-at: accepted | 23.3% |
| Average dwell time | 0.9s (±0.02 SE) |
| Micro-interruptions per 45-min session | ~51 |
| Deletion rate during interruptions | 0.226 Hz |
| Deletion rate rest of session | 0.039 Hz |
| Deletion rate ratio | 5.8x |
| Fixed startup cost (reading) | ~256ms |
| Per-character cost (post-startup) | ~11.5 ms |
| Accepted suggestion avg length | 27.7 chars |
| Rejected suggestion avg length | 27.3 chars |
| Acceptance rate, task Q1 | 19.9% |
| Acceptance rate, task Q4 | 30.1% |
| Rated "slightly distracting" | 66% |
| Rated "moderately helpful" | 40% |

---

## Open-Source Tooling Used
- **OBS Studio** — screen recording
- **Custom Chrome extension** — Copilot suggestion event tracking in VS Code Web IDE
- **WebLink** — eye tracker interface software
- **EyeLink Portable Duo** — hardware eye tracker (2000 Hz)
- Replication package publicly available at eye-for-ai.hasel.dev

---

## Relevance to Open Source AI Ecosystem

This study directly informs the design of open source AI coding assistants (e.g., Continue, Tabby, Cody, FauxPilot). Unlike proprietary tools, open source projects can implement attention-aware suggestion delivery and alternative presentation formats without vendor constraints. The findings argue for:

- Open weight/local coding assistants adding **attention-aware throttling** rather than streaming suggestions continuously
- Open source IDE extensions experimenting with **non-ghost-text presentation** (side panels, collapsible previews, on-demand overlays)
- Community-driven A/B testing of **suggestion timing heuristics** based on workflow phase detection
- The replication package enables open source developers to reproduce the study and test new delivery mechanisms against the same baseline

---

## Limitations to Consider
- Single language (TypeScript) and single task type (game modification) — generalization across languages/domains untested
- 45-minute sessions may not capture long-session fatigue effects
- Copilot-specific behavior may not generalize to all suggestion engines
- Student and professional mix may mask experience-level effects (subgroup analysis not reported in summary)
- Eye-tracking in lab setting may differ from natural work environments