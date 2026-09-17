# Core vs. Peripheral Developer Agent Usage — Evidence Base

## Primary Source

Cynthia, Das & Roy, "Core vs. Peripheral Developers: How Do They Use AI Coding Agents Differently?"
- **Venue:** MSR '26 (Mining Software Repositories)
- **arXiv:** 2601.20106
- **Scale:** 9,427 agentic PRs, 1,701 developers, 1,391 repositories (public GitHub)

## Study Design

### Data Collection
- **Source platform:** GitHub (public repositories only)
- **Unit of analysis:** The pull request (PR), with developer-level aggregation for frequency analysis
- **PR classification:** Agentic PRs identified via indicators of AI coding agent involvement (agent-attributed commits, agent tool signatures in PR metadata, agent-assisted labels or commit messages)
- **Developer classification:**
  - **Core developers:** Maintainers or high-frequency contributors with merge privileges and sustained commit history in the repository
  - **Peripheral developers:** Occasional or external contributors without maintainer status, lower commit frequency, no merge privileges
- **Repository scope:** 1,391 public GitHub repositories spanning multiple languages and project sizes

### Research Questions (RQs)
1. **RQ1 — Usage frequency and purpose:** How frequently do core and peripheral developers use agents, and for what types of tasks?
2. **RQ2 — Review dynamics:** How do review interactions differ between core and peripheral developers' agentic PRs?
3. **RQ3 — Modifications:** How are agentic PRs modified post-generation, and do modification patterns differ by developer group?
4. **RQ4 — CI verification:** How do core and peripheral developers differ in CI verification of agentic PRs?

### Analysis Methods
- **Quantitative analysis** of PR metadata: commit counts, review comment counts, modification line counts, CI check outcomes
- **Task type classification:** PRs categorized by primary purpose (bug fixing, feature addition, documentation, testing, refactoring, other)
- **Review comment classification:** Comments categorized by concern type (evolvability, code organization, alternative solution approaches, correctness, style, other)
- **Modification analysis:** Comparison of added/deleted lines and modification types (refactor, bug fix, documentation improvement, test addition, other) between groups
- **CI outcome analysis:** Success/failure/no-run rates for CI checks on agentic PRs, broken down by developer group

## RQ1 — Usage Frequency and Purpose

### Frequency

| Metric | Core Developers | Peripheral Developers |
|---|---|---|
| Median agentic PRs per developer | 2.0 | 2.0 |
| Mean agentic PRs per developer | 3.73 | 6.08 |

- The equal medians (2.0) indicate that the *typical* developer in both groups uses agents at the same baseline rate.
- The divergent means (3.73 core vs. 6.08 peripheral) reveal a **heavy-tailed distribution among peripheral developers**: a subset of peripheral developers uses agents disproportionately more often, pulling the mean well above both the median and the core developer mean.
- This power-user subset is a key finding — it suggests that some peripheral developers are compensating for skill or experience gaps through heavier agent reliance.

### Merge Rate

| Metric | Core Developers | Peripheral Developers |
|---|---|---|
| Agentic PRs merged into main/master | 85.8% | 77.8% |

- Core developers' agentic PRs merge at a rate **8.0 percentage points higher** than peripheral developers' (85.8% vs. 77.8%).
- This reputation/privilege effect persists **even when the code is agent-generated** — agents do not flatten the social hierarchy of merge decisions.
- The gap is consistent with prior literature on core/peripheral contribution dynamics in OSS, indicating that the agent does not erase the structural advantage of being a known, trusted contributor.

### Task Delegation Patterns

**Peripheral developers — even distribution across task types:**

| Task Type | Peripheral Share |
|---|---|
| Bug fixing | ~19.8% |
| Feature addition | ~19.3% |
| Documentation | ~18.7% |
| Testing | ~18.9% |
| (Other/refactoring) | remainder |

Peripheral developers delegate broadly across all task categories at roughly equal rates (~18.7–19.8% each). Their agent use is undifferentiated — they use agents for whatever task is at hand, whether it is a bug fix, a feature, docs, or tests.

**Core developers — concentrated on toil:**

| Task Type | Core Share |
|---|---|
| Documentation | 20.9% |
| Testing | 21.8% |
| Bug fixing | lower than peripheral |
| Feature addition | lower than peripheral |

Core developers concentrate agent delegation on **documentation (20.9%)** and **testing (21.8%)** — the two task categories that constitute "toil": necessary work that developers view as low-satisfaction, low-creative-content, but essential for project health. They retain direct manual control over bug fixing and feature addition, where judgment and domain expertise matter most.

**Interpretation:** The task-distribution divergence is one of the most interpretable findings. It shows that experienced developers are strategic about *what* they delegate — offloading toil to agents while keeping judgment-intensive work for themselves. Less-experienced developers delegate everything, which may reflect either (a) a broader range of needs (they need help with everything) or (b) a lack of the meta-skill of knowing what to delegate vs. what to do manually.

## RQ2 — Review Dynamics

### Review Engagement

| Metric | Core Developers | Peripheral Developers |
|---|---|---|
| Median review comments per PR | 3.6 | 2.0 |

- Core developers engage in review discussions at nearly **twice the rate** of peripheral developers (median 3.6 vs. 2.0 comments per PR).
- This higher engagement holds for agent-generated PRs — core developers do not relax their review intensity when an agent produced the code under review.
- The gap reinforces the "quality gatekeeper" role: core developers invest more review effort regardless of the code's origin.

### Review Concern Types

| Concern Type | Core Developers | Peripheral Developers |
|---|---|---|
| Evolvability issues (primary) | 52.8% | 59.3% |
| Code organization focus | lower | higher |
| Alternative solution approaches focus | higher | lower |

- **Both groups primarily raise evolvability concerns** (maintainability, future changeability, technical debt) — 52.8% (core) and 59.3% (peripheral). This is the dominant review concern for both groups.
- **Peripheral developers** focus more on **code organization** — where code should be placed, how it should be structured, modularity concerns. This is a surface-level structural focus.
- **Core developers** focus more on **alternative solution approaches** — whether the chosen implementation strategy is the right one, whether a different approach would be better. This is a higher-level architectural judgment.

**Interpretation:** The review-focus divergence mirrors the task-delegation divergence. Core developers bring architectural judgment to review (is this the right approach?), while peripheral developers bring structural judgment (is this organized correctly?). The core developer's review focus is harder to automate or delegate — it requires deep project context and design experience.

## RQ3 — Modifications to Agentic PRs

### Acceptance Without Modification

| Metric | Both Groups |
|---|---|
| Agentic PRs accepted without modification | 74.1% |

- **74.1% of agentic PRs are accepted as-is** across both core and peripheral developers — no human edits after agent generation.
- This is a high acceptance rate, indicating that agents generally produce work that is "good enough" to merge without further intervention.
- The 25.9% that require modification are where the group differences emerge.

### Modification Patterns When Changes Are Made

| Modification Type | Core Developers | Peripheral Developers |
|---|---|---|
| Refactoring (common to both) | yes | yes |
| Bug fixing | — | yes (additional) |
| Documentation improvement | yes (additional) | — |

- **Both groups commonly refactor** agent-generated code when modifying it — restructuring without changing behavior.
- **Peripheral developers** also fix bugs in agent-generated code — suggesting that agents sometimes produce code with functional defects that less-experienced reviewers catch.
- **Core developers** also improve documentation in agent-generated code — extending the agent's output with better explanations, consistent with their documentation-heavy delegation pattern (they care about docs quality).

### Modification Size

| Metric | Core Developers | Peripheral Developers |
|---|---|---|
| Mean added lines (when modified) | 55.4% | lower |
| Mean deleted lines (when modified) | 38.2% | lower |

- Core developers make **larger modifications** when they intervene — 55.4% mean added lines and 38.2% mean deleted lines (as a proportion of the original PR's line count).
- This is consistent with the quality-gatekeeper role: when core developers touch agent output, they make substantial changes — restructuring, expanding, or reworking the contribution — rather than making small surface fixes.

**Interpretation:** The modification data paints a coherent picture: agents produce mostly-acceptable code (74.1% accepted), but when core developers do intervene, they make bigger, more structural changes, while peripheral developers make smaller, bug-fix-oriented changes. This reflects the difference between architectural review (core) and correctness review (peripheral).

## RQ4 — CI Verification

### CI Outcomes

| Metric | Core Developers | Peripheral Developers |
|---|---|---|
| CI success rate on agentic PRs | 51.2% | 43.1% |
| Merged without running any checks | 11.2% | 19.1% |

- Core developers achieve a higher CI success rate (51.2% vs. 43.1%) — their agent-generated PRs pass CI more often.
- **Peripheral developers are nearly twice as likely to merge without running any CI checks at all** (19.1% vs. 11.2%). This is the most striking CI finding: nearly one in five peripheral-developer agentic PRs is merged with zero verification.
- Core developers follow the **"all checks must pass"** principle more consistently — they run checks, and they require them to pass before merging.

### Risk Compounding

The CI data reveals a compounding risk pattern:
1. Peripheral developers are the heavier agent users (mean 6.08 PRs)
2. Peripheral developers delegate across all task types (including bug fixing and features)
3. Peripheral developers are nearly twice as likely to merge without any CI verification (19.1%)
4. Peripheral developers' agentic PRs have a lower CI success rate when checks *are* run (43.1%)

This means the group most reliant on agents is also the group with the weakest verification practices — a combination that elevates the risk of merging incorrect or low-quality agent-generated code into production branches.

**Interpretation:** The CI gap is the most actionable finding in the study. Unlike review engagement or modification patterns (which are harder to mandate), CI policy is a mechanical enforcement point. Projects can require CI passage on all PRs — especially agent-generated ones — and eliminate the 19.1% no-checks merge rate entirely.

## Summary Statistics Table

| Metric | Core Developers | Peripheral Developers | Gap |
|---|---|---|---|
| Median agentic PRs/dev | 2.0 | 2.0 | 0 |
| Mean agentic PRs/dev | 3.73 | 6.08 | +2.35 (peripheral higher) |
| Merge rate (to main/master) | 85.8% | 77.8% | +8.0 pp (core higher) |
| Median review comments/PR | 3.6 | 2.0 | +1.6 (core higher) |
| Evolvability issues (primary) | 52.8% | 59.3% | +6.5 pp (peripheral higher) |
| PRs accepted without modification | ~74.1% | ~74.1% | ~0 |
| Mean added lines (when modified) | 55.4% | lower | core higher |
| Mean deleted lines (when modified) | 38.2% | lower | core higher |
| CI success rate | 51.2% | 43.1% | +8.1 pp (core higher) |
| Merged with no checks | 11.2% | 19.1% | +7.9 pp (peripheral higher) |

## Methodology Notes

### Developer Classification
- Core/peripheral classification based on repository contribution history: commit frequency, merge privileges, sustained participation over a defined observation window.
- The classification is repository-specific: a developer may be core in one repository and peripheral in another.
- The study does not use self-reported expertise — classification is behavioral (based on actual contribution patterns).

### Agentic PR Identification
- Agentic PRs identified through indicators of AI coding agent involvement in the PR lifecycle (agent-attributed commits, agent tool signatures, agent-assisted labeling).
- The study focuses on PRs where an agent was involved in code generation or modification, not PRs where an agent was used only for review or comment generation.

### Limitations
- **Public GitHub only:** Findings may not generalize to private/corporate repositories with different review and CI practices.
- **Observational, not experimental:** The study observes correlations between developer type and agent usage patterns; it does not establish causation (e.g., it cannot prove that agent use *causes* the skill-gap risk — only that the pattern is consistent with it).
- **Agent identification:** The method for identifying agentic PRs depends on detectable agent signatures; PRs where agents were used but not labeled may be missed, potentially undercounting agent usage.
- **Cross-sectional:** The study captures a snapshot of usage patterns; it does not track longitudinal skill development in peripheral power-users to confirm the hypothesized learning-risk effect.

## Cross-Reference Notes

- **`cli-agentic-coding-adoption-impact`**: Complementary perspective — that study examines *who adopts* CLI agents (social-network effects, +24% PR lift); this study examines *how* different contributor types *use* agents once adopted. Together they cover the adoption → usage → outcome pipeline.
- **`agentic-coding-returns-to-expertise`**: That study finds domain expertise (not coding proficiency) drives agentic success; this study's core/peripheral finding is consistent — core developers (who have more project-specific domain expertise) get better merge outcomes and use agents more strategically (toil absorption) than peripheral developers.
- **`ai-engineering-culture-amplifier`**: This study is a concrete instance of the culture-amplifier thesis — agents do not flatten existing hierarchies (reputation, review rigor, CI discipline); they amplify the existing differences between core and peripheral contributors.
- **`botsitting-botshitting-cycle`**: The 19.1% no-checks merge rate for peripheral developers is a botshitting risk vector — merging agent output without verification is the over-trusting phase of the cycle. The core developer's "all checks must pass" stance is the corrective.
- **`human-oversight-agentic-systems-practice`**: Core developers' review behavior (3.6 comments/PR, solution-approach focus, larger modifications) maps to the "post hoc review" oversight form in that framework. The CI gap reflects a weaker "a priori control" stance among peripheral developers.