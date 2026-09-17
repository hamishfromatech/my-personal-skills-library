# Faros AI Engineering Report 2026: Evidence Base

## Study Design

- **Source:** Faros AI, "The AI Engineering Report 2026: The Acceleration Whiplash" (April 2026)
- **Data:** Two years of telemetry from 22,000 developers and 4,000+ teams on the Faros platform
- **Method:** Tracking metric change between each organization's periods of lowest and highest AI adoption (within-org longitudinal comparison, not cross-sectional)
- **Scope:** Seven areas examined: adoption, throughput, context switching, code complexity, pre-merge quality, workflow efficiency, production quality
- **Coverage:** Finance, healthcare, infrastructure, and other sectors where software runs critical operations
- **Published:** April 12, 2026 (takeaways); full report available at faros.ai/research/ai-acceleration-whiplash

## The Ten Takeaways (Full Detail)

### Takeaway 1: AI crossed a threshold — it is now the primary author of code

This happened not as a deliberate decision but as adoption scaled, acceptance rates climbed, and agent-mode tools began applying changes directly rather than waiting for developer approval.

- 80% of teams now exceed the 50% weekly active user threshold for AI tools
- AI code acceptance rate rose from 20% to 60%
- AI is not assisting developers; in most organizations it is leading them

### Takeaway 2: The business value is real — roadmaps are finally moving

The 2026 data is not all bad news. Real delivery acceleration is happening:

- Epics completed per developer: up 66%
- Task throughput per developer: up 33.7%
- PR merge rate per developer: up 16.2%
- More features shipped, more initiatives completed, more code entering the codebase than at any prior point

### Takeaway 3: The throughput numbers have an asterisk — code churn up 861%

Code churn = ratio of lines deleted to lines added for merged code in a given quarter.

- Under high AI adoption: +861% (nearly 10x prior rate)
- Significantly more code being removed relative to what is being added

Three plausible explanations (all consistent with data; the right one varies by organization):
1. Developers accepting AI code quickly and returning to replace it when it proves insufficient in practice
2. AI enabling teams to finally tackle large-scale refactoring previously too slow or costly to staff
3. Engineers moving faster to improve code they were never fully satisfied with at shipping time

**The critical diagnostic question:** With Git-level line provenance data, determine whether deleted lines were written recently (suggesting rework of AI-generated code) or represent legacy code being productively refactored.

**Key insight:** Throughput measures what was shipped, not what survived. The 861% is the asterisk on every output number in the report.

### Takeaway 4: Production incidents more than tripled per PR

- Incidents-to-PR ratio: up 242.7% (more than 3x the low-AI-adoption baseline)
- An incident = an outage, security event, or system failure reaching real users in production
- This is a ratio, not a probability: a single PR can link to multiple incidents; not every incident traces directly to the most recent merge
- Monthly incidents: up 57.9%
- What started as a productivity conversation has become a reliability problem

### Takeaway 5: Bugs are accelerating, not stabilizing

- 2025 report: bugs per developer up 9% as AI adoption grew
- 2026 report: that figure has risen to 54%
- The relationship between AI adoption and defect rate is NOT flattening as organizations mature — it is steepening
- More AI-generated code in the codebase correlates with more bugs per developer, and the relationship is strengthening

### Takeaway 6: AI made starting easy, finishing hard

- Daily PR contexts per developer: up 67.4% (more parallel threads of work)
- Work restarts (tasks returning to in-progress after moving to another stage): up 13.8%
- In-progress tasks showing no activity for 7+ days: up 26% (work started, claimed capacity, then stalled)
- The individual-level picture is acceleration; the workflow data shows more threads opened, more work abandoned mid-flight

### Takeaway 7: The senior engineer tax

AI-generated code presents a specific challenge for reviewers:
- It is often superficially convincing: idiomatic, well-named, stylistically consistent with the surrounding codebase
- It looks like code written by someone who knows what they are doing
- The structural and logical failures, when they exist, are beneath the surface
- Catching them requires a reviewer to read carefully, reason about intent, and reconstruct the problem the code was meant to solve — not scan for obvious errors
- This is slow, expensive cognitive work

Review time metrics:
- Median time to first PR review: up 156.6%
- Average time spent in code review: up 199.6%
- Median time in review: up 441.5%

The engineers with the deepest system knowledge are spending their most valuable hours unraveling plausible-looking code that should never have reached them in the state it did.

### Takeaway 8: More code enters production with no review at all

- Pull requests merged without any review (human or agentic): up 31.3%
- Not a deliberate decision to bypass oversight — reviewers cannot keep pace with AI-generated code volume
- Code reaching production with no oversight at a meaningfully higher rate than before high AI adoption
- Combined with the production incident data, this defines the core risk of the acceleration whiplash

### Takeaway 9: Strong engineering foundations do not protect you

- DORA's 2025 State of AI-Assisted Software Development report concludes (from survey data) that strong engineering foundations amplify AI's benefits and offer protection against downsides
- Two years of telemetry across thousands of teams tells a different story
- High-performing engineering organizations (mature DevOps, high DORA scores, disciplined delivery) experience the same downstream deterioration as everyone else
- Surveys capture how developers feel about their work. Right now, developers feel more productive because, at the individual level, they are.
- What surveys cannot capture: what happens downstream — review queues backing up, incidents accumulating, bugs reaching customers
- Perception lags reality. Telemetry does not.

### Takeaway 10: Headcount-cut warning

- AI engineering impact data shows output is up
- The work required to ensure that output is safe, correct, and maintainable has NOT decreased — it has increased substantially
- Engineers considered for cuts are in many cases the ones absorbing the quality gap AI is creating
- Every organization cutting engineering headcount on the basis of AI output gains should read this report

## Metric Definitions

| Metric | Definition |
|--------|------------|
| Code churn | Ratio of lines deleted to lines added for merged code in a given quarter |
| Incidents-to-PR ratio | Production incidents per merged PR (ratio, not probability) |
| Work restarts | Tasks that return to in-progress after moving to another stage |
| Daily PR contexts | Number of distinct PRs a developer touches per day (parallelism) |
| Unreviewed PR merge | PR merged without any human or agentic review |
| AI acceptance rate | Percentage of AI-suggested code accepted by developers |

## Methodology Notes

- The report measures what AI is actually producing across the full SDLC, not how developers feel
- Metric change is tracked between periods of lowest and highest AI adoption within each organization (within-org comparison)
- The organizations in the dataset already have granular telemetry visibility (Faros platform users) — this is NOT representative of all organizations
- The data represents organizations that CAN see the whiplash; most organizations cannot see it because they lack the telemetry infrastructure
- The gap between knowing and acting is the only gap that matters now

## Relationship to Existing A-Tech Skills

| Existing Skill | Relationship |
|----------------|-------------|
| botsitting-botshitting-cycle | The hidden labor dimension (6.4 hrs/week botsitting); this skill is the telemetry/system dimension showing what that labor produces downstream |
| ai-productivity-measurement-gap-2026 | The measurement infrastructure gap (89% trust metrics, 94% say incomplete); this skill provides the empirical data that the gap hides |
| unified-devex-measurement-stack-2026 | The framework integration (DORA+SPACE+DX Core 4+AI Attribution+Business); this skill validates why Layer 4 (AI Attribution) and quality tracking are critical |
| ai-productivity-output-volume-paradox | The output-volume mechanism (27% new-work ratio, Anthropic internal data); this skill is the telemetry proof at scale |
| ai-engineering-culture-amplifier | The cultural amplifier thesis (AI multiplies pre-existing practices); this skill shows what culture amplifies — the whiplash hits everyone, but culture determines recovery velocity |
| comprehension-debt-framework | The comprehension cost of AI code; this skill shows the downstream signal (bugs, incidents, review time) |
| untrainable-corner-pricing-moat | The 180%/30% code-volume-to-shipping gap IS the whiplash; the untrainable corner is where the quality verification work lives that prevents the whiplash |

## Supporting Context: METR Self-Reported Productivity Survey (May 2026)

METR's survey of 349 technical workers provides the self-reported complement to Faros's telemetry:

- Median self-reported value change: 1.4x–2x (closer to what matters than speed)
- Median self-reported speed change: 3x (expected to be higher than value; confirms substitution effect)
- Retrospective estimate: 1.3x value in March 2025 → 2x in March 2026 → forecast 2.5x for March 2027
- Reasons to be skeptical: METR staff (familiar with perceived-vs-actual gaps) report the lowest value gains of any subgroup; early qualitative investigation of high estimates suggests overstatement
- Previous METR RCT: developers overestimated AI effect on time spent by 40 percentage points on average
- Public survey estimates have consistently exceeded field experiment estimates

**Synthesis:** The self-reported 1.4–2x value gain coexists with the telemetry showing +54% bugs, +242.7% incidents-to-PR, +861% code churn. Both can be true: individual value-perceived productivity rises while organizational quality costs compound downstream. This IS the whiplash — the individual feels faster; the system gets slower.

## Supporting Context: METR Uplift Update (February 2026)

METR's follow-up to their original 19% slowdown finding:

- Original study (early 2025): AI caused 20% slowdown for experienced developers (+2% to +39% CI)
- Follow-up (late 2025): for original developers, estimated speedup of -18% (-38% to +9% CI); new developers -4% (-15% to +9% CI)
- BUT: severe selection effects — developers refuse to participate without AI, refuse to submit tasks that would be painful without AI
- 30–50% of developers choosing not to submit some tasks because they don't want to do them without AI
- The estimate is likely a lower bound; true speedup among most active adopters could be much higher
- METR is redesigning the study (shorter intense experiments, higher pay, fixed-task designs, developer-level randomization)

**Synthesis with whiplash:** METR's selection effect IS the whiplash's perception dimension. Developers self-report high speedup, refuse to work without AI, and feel more productive — while the telemetry shows the downstream costs they don't see. The individual experience is real; the system cost is also real. The whiplash is the gap between them.

---

*Evidence base compiled from Faros AI Engineering Report 2026 (April 12, 2026), METR Self-Reported Impact Survey (May 11, 2026), and METR Uplift Update (February 24, 2026).*