# AI Productivity: Long-Term Factors — Evidence Base

## Primary Sources

1. Chen, V., He, J., Williams, B., Valentino, J., & Talwalkar, A. (2026). *Beyond the Commit: Developer Perspectives on Productivity with AI Coding Assistants*. Open MIND, published 2026-02-03. — BNY Mellon study, 2,989 survey responses + 11 interviews.

2. Vella, A., & Blincoe, K. (2026). *The Impact of AI Coding Assistants on Software Engineering: A Longitudinal Study*. arXiv:2605.23135, May 2026. — Longitudinal mixed-methods, N=95 matched professionals, two time points six months apart.

3. Datadog (2026). *How to measure developer experience in the AI era*. — Datadog's internal framework (3,000+ engineers) adding "AI adoption and impact" as a fourth DevEx dimension.

4. Brandebusemeyer, C., Zunic, K., Zimmermann, T., Schimmer, T., & Arnrich, B. (2026). *Developers' Experience with Generative AI Beyond Productivity Assessment*. arXiv:2607.02337. — SAP mixed-methods field study, 22 developers, 4-day controlled + uncontrolled design, multimodal data.

5. Afroz, S., Feng, Z., Menezes, T., Kimura, K., Trinkenreich, B., Steinmacher, I., & Sarma, A. (2025/2026). *The Fast and Spurious: Developer Productivity with GenAI*. FSE 2026. — 415 software practitioners, SPACE framework.

## The BNY Mellon Finding

- 86% of 2,989 engineers were satisfied or very satisfied with GitHub Copilot.
- ~60% reported less than one hour of time savings per week — well below the average reported by similar-sized companies.
- Pearson correlation between satisfaction and time savings: r = 0.34 (positive but weak).
- Nearly 400 developers were very satisfied despite saving only 30 minutes/week.
- Over 100 developers saved 2+ hours/week but were neutral or dissatisfied.
- Conclusion: a single metric (satisfaction or time savings) cannot capture developer productivity with AI tools.

## The Six Factors (BNY Mellon)

From 11 semi-structured interviews across seniority levels (early career, mid-career, management):

1. **Self-sufficiency** — reduced reliance on external help; "never visit Stack Overflow now."
2. **Frustration and cognitive load** — non-deterministic outputs, verification burden, prompt sensitivity.
3. **Task completion rate** — not lines of code; criticality of work and benefit to the company.
4. **Ease of peer review** — junior developers' AI-heavy code harder to review; senior developers use AI to summarize.
5. **Technical expertise** — "If the code just works, then you just accept it" (learning risk); AI can also facilitate expertise ("look up security topics not aware of").
6. **Ownership of work** — "nothing like doing it yourself"; deep knowledge of code enables faster incident response.

Existing frameworks (DORA, SPACE) capture Factors 1–4 partially; Factors 5 and 6 are unaddressed.

## The Longitudinal Productivity-Experience Paradox (Vella & Blincoe)

- 84% reported productivity improvement at both time points (stable).
- Developer experience eroded: Negative cohort (worsened on ≥1 dimension) grew from 14% → 27%.
- Feedback loops improved significantly (+0.21, p=0.038).
- Cognitive load declined non-significantly (−0.15).
- Flow state declined non-significantly (−0.18); share rating it "worse" nearly tripled (7% → 20%).
- Correlation between change in flow state and change in productivity: r = 0.02 (zero).
- No participant who started in the Negative cohort recovered to fully Positive by Q2.
- Complete stability across all three dimensions was rare (9%).
- The DevEx framework treats its three dimensions as complementary drivers of productivity; AI assistance may reshape their relative contributions, with faster feedback compensating for increased cognitive friction.

## The Creation-to-Verification Shift

- 82% reported spending less time writing code by Q2.
- Significant shift toward verification activities (balance score V−C: 0.26 → 0.53, r=0.39, p=0.006).
- Proposed new work category: **supervisory engineering work** (directing, evaluating, correcting AI output).
- This work does not map to traditional SDLC categories and is not captured by existing productivity frameworks.

## Tool-Landscape Evolution (Vella & Blincoe)

- 82% of matched participants changed their tool combinations in 6 months.
- Average tools per engineer: 1.9 → 2.9.
- ChatGPT usage: 70% → 58%; Cursor: 16% → 29%.
- Only concern that changed significantly: **maintainability** (rose from 3% → 19% as primary concern, p=0.003).
- Engineers increasingly recognized that AI optimizes for "works now" rather than "maintains later."

## Datadog's Fourth Dimension

Datadog extended the DevEx framework (feedback loops, cognitive load, flow state) with a fourth dimension: **AI adoption and impact**.
- ~80% of PRs at Datadog are now AI-assisted.
- AI-assisted PRs have slightly lower cycle times per change but much higher concurrency overall.
- AI does not significantly speed up individual changes; it enables developers to work on more changes simultaneously.
- PR throughput exposes a stability issue: if PR velocity increases 10× but incident rate per PR is constant, total incidents also increase 10×.
- New cognitive-load proxy: **multi-agent orchestration** — the number of distinct AI agents engineers use daily, context switches, and time spent managing agents. This is now the primary source of cognitive load, displacing code-level complexity.

## SAP Field Study (Brandebusemeyer et al.)

- 445 working tasks documented over 3 days of natural work.
- AI used for 54.6% of development-heavy tasks; 88.1% of those found helpful.
- Using either in-code suggestions OR chat alone improved efficiency and reduced workload; **combining both within a single task eliminated the benefits** (comparable to no-Copilot condition).
- AI use during development-heavy tasks was associated with significantly higher cognitive load (p=0.008, d=0.34) but comparable productivity.
- When AI output was perceived as helpful, productivity was significantly higher (p=0.043, d=0.83) with comparable cognitive load.
- 73% of participants said they would adjust their GenAI use after the study.
- Developers with lower prior GenAI proficiency were significantly more likely to report a change in view (p=0.036, p=0.013).

## The "Fast and Spurious" Finding (Afroz et al., FSE 2026)

- 415 practitioners, SPACE framework.
- Frequent GenAI users reported slightly higher perceived improvements in Efficiency/Flow and Satisfaction/Well-being.
- Gains did not extend to Performance, Activity, or Communication/Collaboration.
- More than 75% of developers fell on the non-positive side of the scale across all communication/collaboration items.
- 76.3% of frequent users were not positive that GenAI helps reduce interruptions.
- Observation: GenAI tools appear to accelerate task completion rather than improve focus or attention management.
- "Spurious productivity": surface-level acceleration often accompanied by redistributed effort and hidden costs.