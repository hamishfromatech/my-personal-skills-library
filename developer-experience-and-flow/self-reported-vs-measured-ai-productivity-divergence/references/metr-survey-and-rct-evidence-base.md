# METR Survey and RCT Evidence Base

## Source 1: METR Self-Reported Impact Survey (May 11, 2026)

**Authors:** Joel Becker
**Date:** May 11, 2026
**URL:** https://metr.org/blog/2026-05-11-ai-usage-survey/

### Study Design

- **Sample:** 349 technical workers (after filtering 10 for data quality from 359 raw)
- **Composition:** 87 software engineers, 71 researchers, 129 academics and PhD students, 48 founders and managers
- **Experience:** Average 12 years programming, 19 months using AI for programming, 7 months using agentic AI coding tools
- **Geography:** ~48% US-based, ~25% Europe, ~50% use Claude Code
- **Recruitment:** Convenience sample from GitHub, academic directories, METR networks, X (Twitter). Response rate ~2% for email outreach. ~70% paid to take survey (avg $200).
- **Period:** February–April 2026
- **Innovation:** Distinguishes "value" (how much more value are you creating) from "speed" (how long would it have taken without AI)

### Key Results

**Value uplift (3 measures):**
- "How long would it have taken you to deliver equally valuable work without AI?" — median within 1.4–2x range
- "What fraction of the value of your current work could you have produced without AI?" — reciprocal measure
- "How many copies of yourself without AI would your team need to hire?" — median consistent with above

**Speed uplift:**
- Median: 3x (higher than value, as expected — speed is inflated by substitution into cheaper tasks)

**Temporal estimates (retrospective + forecast):**
- March 2025: 1.3x value (consistent with ~32% perceived time uplift in prior experiment)
- March 2026: 2x value
- March 2027 (forecast): 2.5x value

**Willingness-to-accept:**
- Median: would sacrifice 29% of salary to keep AI access for 1 month
- High variance; some would pay >100% of estimated wage
- Interpreted as weak evidence — employed workers aren't the party capturing value (employers are)

**Subgroup findings:**
- Higher perceived gains among: more AI usage, more AI experience, Claude Code/Codex users, startup workers
- Lower perceived gains among: METR staff (lowest of any subgroup)

**Data quality:**
- 3% of sample filtered for anomalies (logical impossibilities, extreme inconsistencies)
- Most common bad data flag: logically impossible work allocation
- Next most common: significantly declining uplift over time

### Reasons to Be Skeptical

1. METR staff (familiar with perception-reality gap) give lowest estimates
2. Early qualitative investigation of high estimates (≥10x) suggests overstatement — in 2 of 7 cases with public outputs, the "enormously more valuable work" is not externally visible
3. Prior METR RCT found developers overestimate by 40 percentage points
4. Public survey estimates have consistently exceeded field experiment estimates
5. Convenience sample with ~2% response rate — significant selection bias

### METR's Recommendations for AI Companies

1. **More careful question definition:** Value-focused questions rather than speed-focused (value is closer to what matters)
2. **Ask managers or productivity researchers** (not individual contributors) — closer to decision-makers, potentially more grounded
3. **Ensure sufficient sample size**
4. Future improvements: shorter survey (reduce fatigue), real willingness-to-accept (actually take away AI for a month), ask about employer willingness-to-accept

---

## Source 2: METR Uplift Update (February 24, 2026)

**Authors:** Joel Becker, Nate Rush, Tom Cunningham, David Rein, Khalid Mahamud
**Date:** February 24, 2026
**URL:** https://metr.org/blog/2026-02-24-uplift-update/

### Study Design

- **Original study (early 2025):** Developers paid $150/hr to work on their own open-source projects; tasks randomized to AI-allowed or AI-disallowed; time-to-completion compared
- **Original finding:** AI caused 20% slowdown for experienced developers (+2% to +39% CI)
- **Follow-up study (August 2025 onward):** 10 original developers + 47 new developers; $50/hr; broader repository coverage (smaller, greener, less mature repos)
- **Data:** 57 developers, 143 repos, 800+ tasks

### Follow-Up Results

- Original developers: estimated speedup of -18% (-38% to +9% CI)
- New developers: estimated speedup of -4% (-15% to +9% CI)
- These are likely LOWER BOUNDS on the true effect due to selection effects

### Selection Effects (The Core Problem)

1. **Recruitment/retention difficulty:** Increased share of developers won't do 50% of their work without AI, even at $50/hr for tasks of their choosing. Study systematically misses developers with most optimistic AI expectations.

2. **Task submission bias:** 30–50% of developers told METR they chose not to submit some tasks because they didn't want to do them without AI. Study systematically misses tasks with high expected AI uplift.

3. **Pay rate reduction:** $150/hr → $50/hr likely increased selection effects.

4. **Task type changes:** Developers attempt different task types with agentic AI (leaning on AI strengths) — within-study time differences may not represent value differences.

5. **Quality differences:** Quality of final work differs between AI-allowed and AI-disallowed conditions.

6. **Completion asymmetry:** Some developers don't complete AI-disallowed tasks; one developer completed no AI-disallowed tasks.

7. **Concurrent agent use:** Time-spent reporting unreliable when developers run multiple agents concurrently (they work on other tasks while waiting).

### METR's Conclusion

Based on conversations with participants, developers are likely MORE sped up from AI tools in early 2026 than in early 2025. But the selection effects make the data only very weak evidence for the size of this increase.

### METR's Future Research Directions

1. **More intensive experiments:** Shorter, intense experiments with higher pay for better compliance
2. **Observational data:** Aggregate statistics (e.g., ~4% of GitHub commits authored by Claude Code), fine-grained transcripts
3. **Questionnaires:** Careful survey questions + time-use studies
4. **Fixed-task experiments:** Give developers a fixed task (not their own) with/without AI
5. **Developer-level experiments:** Randomize at developer level (all AI or no AI) instead of task level

### Developer Quotes

- "I'm torn. I'd like to help provide updated data on this question but also I really like using AI!"
- "I found I am actually heavily biased sampling the issues... I avoid issues like AI can finish things in just 2 hours, but I have to spend 20 hours. I will feel so painful if the task is decided as AI-disallowed."
- "My head's going to explode if I try to do too much the old fashioned way because it's like trying to get across the city walking when all of a sudden I was more used to taking an Uber."

---

## The Triangulation Framework (Synthesis)

### Why No Single Method Is Sufficient

| Method | What it captures | What it misses |
|--------|-----------------|----------------|
| **Surveys** | Beliefs, perceptions, intractable questions | True productivity (overestimation), counterfactual accuracy |
| **RCTs** | Causal effect, external validity | Increasingly infeasible (selection), expensive, narrow tasks |
| **Observational/telemetry** | Real-world behavior at scale, broad task distribution | Causality, selection effects |
| **Benchmarks** | Standardized comparison, replicability | External validity (overestimate wild capabilities), narrow slice |

### The Divergence Pattern

Across METR's research program, a consistent pattern emerges:

1. **Self-reported estimates are highest** (surveys: 1.4–2x value, 3x speed)
2. **Experimental estimates are lower** (RCTs: originally negative, now uncertain due to selection)
3. **The gap widens as AI improves** (developers increasingly refuse the no-AI condition, making experiments harder)
4. **Education about the gap calibrates estimates** (METR staff give lowest estimates)

### The Implication for A-Tech

- Don't rely on self-reported productivity data alone for investment decisions
- Use telemetry (like the Faros whiplash data) to complement self-reports
- Educate teams about the perception-reality gap (METR-staff pattern) to improve self-report calibration
- Track both value and speed (speed overstates value due to task substitution)
- The selection effect is itself a signal: when developers refuse to work without AI, AI has become infrastructure — but that doesn't mean the productivity gain is as large as they report

---

*Evidence base compiled from METR blog posts (May 11, 2026 and February 24, 2026) and cross-referenced with Faros AI Engineering Report 2026 for the telemetry complement.*