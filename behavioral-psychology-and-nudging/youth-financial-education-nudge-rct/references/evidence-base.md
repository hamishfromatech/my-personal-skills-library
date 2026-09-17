# Youth Financial Education Nudge RCT — Evidence Base

## Primary Source

**Levy, Howard & Lukas (2026).** Large-scale preregistered field experiment testing behavioral nudges to increase youth participation in voluntary financial skills training.

**Institutions:** Stanford University, University of Virginia, University of St. Andrews

**Date:** January 31, 2026

**Design:** Preregistered, large-scale natural field experiment

**Scale:** Approximately 425,000 children and their parents

**Context:** Mobile app featuring 25 financial-skills questions (voluntary financial skills training)

---

## Experimental Design

### Factorial Structure

The study employed a **3 × 10 (parent × child) factorial design**, yielding 30 experimental cells:

**Parent dimension (3 levels):**
- Control (no parent nudge)
- Parent nudge arm 1
- Parent nudge arm 2

**Child dimension (10 levels):**
- Control (no child nudge)
- 9 distinct child nudge types spanning four psychological categories:
  - **Identity priming** — messages framing the child as financially savvy or responsible
  - **Future payoff** — messages emphasizing long-term benefits of financial skills
  - **Attention** — messages raising the salience of the available training
  - **Endorsement** — messages leveraging social endorsement or authority signals

### Randomization

Participants (children and their parents) were randomly assigned across all 30 cells with balanced assignment. The factorial design allows independent estimation of:
- Main effect of parent nudges
- Main effect of child nudges (and pairwise comparison of all 9 child nudge types)
- Interaction effect of parent × child nudges

### Preregistration

The study was preregistered, meaning the hypotheses, analysis plan, and outcome measures were specified before data collection or analysis. This is a critical evidence-quality marker: the null findings (no parent effect, no content difference, no persistence, no downstream behavior) are credible precisely because they were not the result of selective reporting or post-hoc analysis.

### Outcome Measures

- **Primary outcome:** Training uptake (whether the child engaged with the financial skills training in the app)
- **Secondary outcomes:**
  - Engagement timing (when the child engaged, for difference-in-differences analysis)
  - Downstream savings behavior (whether the child subsequently saved money)
- **Subgroup analyses:** Age and gender

### Context

The intervention was delivered within a mobile app environment featuring 25 financial-skills questions. The training was voluntary — children were not required to engage. This makes the baseline uptake rate (2.70%) a meaningful benchmark for voluntary engagement in financial education.

---

## Results

### Result 1: Child-Directed Nudges Increased Training Uptake

- **Control (no child nudge):** 2.70% training uptake
- **Child nudge (pooled across all 9 types):** 3.13% training uptake
- **Average treatment effect (ATE):** 0.43 percentage points
- **Statistical significance:** p < 0.001
- **Relative increase:** 16%

**Interpretation:** Nudging the child directly produced a statistically significant, meaningful relative increase in voluntary training uptake. The absolute effect (0.43pp) is small, consistent with the broader nudge literature showing small absolute effects in the financial domain (Mertens et al. 2022: d = 0.25 for finance, the lowest of all domains tested). The 16% relative increase is a more encouraging framing, but it is built on a low baseline — a reminder that "relative percentage increase" can overstate the practical magnitude when the base rate is low.

### Result 2: Parent-Directed Nudges Had NO Detectable Effect on Child Outcomes

- Parent-directed nudges produced no statistically detectable effect on child training uptake.
- The parent nudge was designed to influence the child's behavior indirectly (through the parent's encouragement, monitoring, or facilitation).

**Interpretation:** This is one of the most practically important null findings in the study. It challenges the common assumption that gatekeeper nudges (parents, managers, admins) are an effective lever for changing end-user behavior. In this context, the child held the action lever (opening the app and engaging with the training), and the parent nudge could not move it — possibly because the parent did not check the nudge channel, lacked leverage, or the parent-child dynamic around financial education was not responsive to a single message.

### Result 3: Nudging Both Parents and Children Offered NO Advantage Over Nudging Children Alone

- **Interaction term (parent × child):** Effectively zero
- **Statistical significance:** p = 0.92

**Interpretation:** The combination of parent and child nudges was no more effective than the child nudge alone. This is a strong null on the interaction (p = 0.92 is far from any conventional significance threshold). The practical implication: do not spend resources on gatekeeper nudges if the end-user nudge is already deployed and the gatekeeper nudge has not shown an independent effect. The additive-cost model (nudge both) does not produce additive benefits.

### Result 4: All 9 Child Nudge Types Outperformed Control, but NONE Was Superior to Another

- All 9 child nudge types (identity priming, future payoff, attention, endorsement categories) significantly outperformed the no-message control.
- **Pairwise comparisons across all 9 nudge types:** All pairwise p > 0.75
- No nudge type was significantly superior to any other.

**Interpretation:** This is the "sending a message matters more than optimizing its content" finding. The dominant effect is the act of outreach — any well-formed message sent through the child's active channel increases engagement. The psychological framing of the message (identity, future payoff, attention, endorsement) does not produce a detectable marginal benefit. This challenges the common practice of investing heavily in message-copy A/B testing for engagement nudges. The evidence says: pick any reasonable message and ship it; spend the optimization budget on higher-leverage decisions (targeting, channel, timing, and post-engagement design).

The p > 0.75 threshold across all 36 pairwise comparisons is particularly striking. With approximately 425,000 participants, the study had substantial power to detect even small differences between nudge types. The fact that none emerged suggests that in this context, message content is not a meaningful lever — the decision to engage is driven by the salience of the opportunity, not by how the opportunity is framed.

### Result 5: Effects Were Homogeneous Across Age and Gender Subgroups

- No significant heterogeneity in treatment effects across age subgroups
- No significant heterogeneity in treatment effects across gender subgroups

**Interpretation:** The nudge worked equally well (in relative terms) regardless of the child's age or gender. This is informative for deployment decisions: there is no evidence that a segmented nudge strategy (different messages for different age/gender groups) would outperform a uniform strategy. However, this does not mean that baseline engagement rates were the same across subgroups — only that the treatment effect (the lift from the nudge) was homogeneous.

This finding contrasts with studies where a theory-identified latent moderator (e.g., loss aversion in Bauer et al. 2026) drives real heterogeneity. The lesson: when theory identifies a moderator, test for heterogeneity and consider targeting; when no theory-predicted moderator exists (as with simple attention/identity nudges), expect homogeneity and save the segmentation cost.

### Result 6: Difference-in-Differences — Nudge-Induced Engagement Did NOT Persist Over Time

- Difference-in-differences analysis revealed that the nudge-induced engagement was a **timing shift**, not a durable change.
- Nudge recipients engaged earlier than non-recipients, but cumulative engagement over the longer observation window did not differ significantly.

**Interpretation:** The nudge accelerated engagement (people who would have engaged eventually engaged sooner) rather than creating new engagement (people who would never have engaged decided to engage). This is the "pure attention" pathway described in the `nudge-persistence-technology-adoption` skill: a one-time message nudge raises salience at the moment of delivery but does not create a durable artifact (technology adoption) or a cue-response loop (habit formation). The effect vanishes once the attention boost fades.

**Design implication:** If the goal is cumulative engagement (not just timely engagement), a single nudge is insufficient. The intervention needs:
- **Repeated nudges** (but beware of habituation and annoyance)
- **Structural re-engagement** built into the product (progress tracking, streaks, checkpoints, social accountability — see `zeigarnik-onboarding-architecture` and `behavioral-design-practical-playbooks`)
- **A curriculum that creates its own re-engagement pull** (compelling narrative, progressive mastery, unlocked content)

### Result 7: NO Downstream Effects on Savings Behavior

- Despite increasing training uptake, the nudges produced no detectable downstream effect on savings behavior.
- Getting children into the financial skills training did not translate into changed financial behavior (saving money).

**Interpretation:** This is the most sobering finding and the strongest argument for combining nudges with boosts. The intervention successfully moved the engagement metric (training uptake) but failed to move the behavior metric (savings). The gap between engagement and behavior change is real and was not closed by the training alone.

This finding has three possible explanations:
1. **The training was insufficient to change behavior.** Twenty-five questions in a mobile app may not build the competence needed to change savings behavior. Financial literacy interventions often show weak effects on behavior (the nudge-effectiveness-reality-check skill notes finance is the lowest-receptivity domain, d = 0.25).
2. **The behavior measurement window was too short.** Savings behavior may change with a longer lag than the study's observation window captured.
3. **The training-to-behavior link requires more than knowledge.** Changing savings behavior likely requires structural support (auto-save defaults, commitment devices), incentives, or a more intensive competence-building intervention (a boost) — not just exposure to financial-skills questions.

**Design implication:** Measure the downstream behavior, not just the engagement metric. An engagement lift that does not move the behavior metric is a partial result. If the goal is behavior change, the intervention must include more than an engagement nudge. Pair the nudge with a boost (competence-building) and a structural change (default enrollment, auto-save, commitment device). See `boosting-empowering-behavior-change` and `nudge-persistence-technology-adoption`.

---

## Implications

### For Nudge Design

1. **Target the end-user, not the gatekeeper.** When the end-user holds the action lever, a direct nudge is more effective than a gatekeeper nudge. Nudging both is no better than nudging the end-user alone.

2. **Ship the message; do not over-optimize the copy.** The act of sending a well-formed message through the end-user's active channel is the dominant effect. Message-content optimization (identity priming vs future payoff vs attention vs endorsement) yields no detectable marginal benefit. Redirect optimization budgets to targeting, channel, timing, and post-engagement design.

3. **Expect a timing shift, not durable engagement.** A one-time message nudge accelerates engagement but does not create lasting engagement. Design for persistence with structural re-engagement, not repeated one-time nudges.

4. **Engagement is not behavior change.** Getting people into training does not mean they will change their behavior. The intervention must include competence-building (boosts) and structural support (defaults, commitment devices) to close the engagement-to-behavior gap.

5. **Expect homogeneous effects when no theory-predicted moderator exists.** Do not over-segment. Save the segmentation budget for contexts where theory identifies a meaningful moderator.

### For Experimental Design

1. **Use factorial designs to test multiple dimensions efficiently.** The 3 × 10 design tested 30 cells in a single experiment, enabling independent estimation of parent effects, child effects, and their interaction.

2. **Pre-register to make null findings credible.** The study's null findings (no parent effect, no content difference, no persistence, no downstream behavior) are credible because the study was preregistered. Without preregistration, null findings are always suspect (did the authors search for a significant result and fail to find one?).

3. **Power for interaction tests.** The p = 0.92 interaction null required sufficient power to rule out a meaningful interaction. Underpowered factorial experiments will produce inconclusive interaction tests rather than credible nulls.

4. **Report all pairwise comparisons.** The "all nudge types are equivalent" finding rests on 36 pairwise comparisons all exceeding p > 0.75. This is only convincing when all comparisons are reported, not just the ones that were significant.

### For Financial Education Policy

1. **Nudges can increase participation in voluntary financial education, but the effect is small.** A 16% relative increase on a 2.70% baseline means 2.70% → 3.13% — an additional 0.43 children per 100 engaged. Scaling to a population of 1 million children, this is an additional 4,300 children engaged. Meaningful at scale, but modest in absolute terms.

2. **Financial education participation does not automatically produce financial behavior change.** This is consistent with the broader literature on financial literacy interventions, which often show weak effects on behavior. The implication is not that financial education is worthless, but that it must be paired with structural and competence-building interventions to produce behavior change.

3. **Parent engagement is not a reliable lever for child financial behavior.** The parent nudge null finding challenges the common policy assumption that engaging parents is an effective pathway to changing child financial behavior.

---

## Limitations

1. **Single context (mobile app, 25 questions).** The findings may not generalize to other financial education contexts (classroom-based, gamified, longer-duration curricula). The 25-question format may be too brief to build the competence needed for behavior change, which could explain the null downstream effect.

2. **Downstream behavior measurement.** The savings-behavior measurement may have been limited by window length, measurement granularity, or the specific savings behavior tracked. A longer follow-up window or a different behavior metric might show effects not detected here.

3. **Parent nudge channel and design.** The parent nudge's null effect may reflect the specific channel or message design used, not the futility of parent nudges in general. A different parent nudge (different channel, different content, different timing) might produce an effect. The study's finding is specific to the parent nudge as implemented, not to all possible parent nudges.

4. **Homogeneity may be underpowered for small subgroup differences.** While the study was large (N ≈ 425,000), the homogeneity finding across age and gender rules out moderate-to-large heterogeneity but may not rule out small subgroup differences that could matter in practice.

5. **Voluntary engagement context.** The training was voluntary, and the baseline uptake was low (2.70%). Nudges may behave differently in mandatory or higher-baseline contexts. The "content does not matter" finding, in particular, may be specific to low-baseline voluntary contexts where the primary barrier is awareness, not motivation.

6. **The 0.43pp absolute effect is small.** While the 16% relative increase is encouraging, the absolute effect (0.43 percentage points) is modest. In contexts where the baseline is higher, the same relative effect would produce a larger absolute lift, but the generalizability of the 16% relative figure is not established.

---

## A-Tech Alignment

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | The factorial experimental design template is reproducible with open-source tools (R, Python, OSF preregistration). The findings can be validated and extended by A-Tech's open-source community. |
| **Data privacy** | The study used app-based behavioral data (engagement, savings) without invasive personal data collection. The design respects user privacy while measuring behavioral outcomes. |
| **Financial freedom** | This is the most directly relevant evidence base for Be Practical's financial freedom mission. It tells A-Tech what nudges can and cannot do to increase participation in financial education, and it sets honest expectations about whether participation will produce behavior change. |
| **Practical implementation** | The findings are immediately actionable: (1) nudge the learner directly, (2) do not over-invest in message copy, (3) build structural re-engagement, (4) pair engagement nudges with competence-building boosts, (5) measure downstream behavior, not just engagement. The effect-size benchmark (16% relative lift on a ~2.7% baseline) gives product teams a realistic target. |

### Be Practical Two-Stage Intervention Model

Based on the Levy et al. findings, Be Practical should be designed as a two-stage intervention:

**Stage 1 — Engagement (the nudge):**
- Send a direct, simple nudge to the learner via their active channel
- Any well-formed message works (identity, future payoff, attention, endorsement — all equivalent)
- Do not over-engineer the copy
- Target: 10-20% relative lift in voluntary module engagement
- Do NOT target the learner's spouse, manager, or parent — the end-user holds the action lever

**Stage 2 — Behavior change (the boost + structure):**
- The curriculum itself must be a competence-building boost (see `boosting-empowering-behavior-change`)
- Provide structural tools: calculators, auto-enrollment templates, implementation intentions, commitment devices (see `nudge-persistence-technology-adoption`)
- Build re-engagement into the curriculum: progress tracking, streaks, checkpoints, social accountability (see `zeigarnik-onboarding-architecture`, `behavioral-design-practical-playbooks`)
- Measure downstream financial behavior (savings rate, debt reduction, investment actions), not just module completion
- The Levy et al. finding tells us Stage 1 alone does not produce Stage 2 — the boost and the structure are necessary, not optional

### Effect-Size Calibration for A-Tech Product Teams

| Metric | Levy et al. Benchmark | A-Tech Target |
|---|---|---|
| Voluntary engagement baseline | 2.70% | Measure your own baseline; expect 2-5% for voluntary financial content |
| Nudge lift (relative) | 16% | 10-20% relative lift is a good outcome; 50%+ is likely inflated |
| Nudge lift (absolute) | 0.43pp | Small in absolute terms; meaningful only at scale |
| Persistence | Timing shift only | Do not expect persistence from a one-time nudge; build structural re-engagement |
| Downstream behavior | No effect | Do not expect engagement to produce behavior change without a boost + structure |

---

## Cross-Reference Network

This skill connects to the following existing A-Tech skills:

- **`nudge-persistence-technology-adoption`** — The timing-shift finding is a direct instance of the "pure attention" pathway. Use the persistence framework to design a follow-on intervention that produces durable change (technology adoption or habit formation).
- **`nudge-effectiveness-reality-check`** — The 16% relative lift is consistent with the bias-corrected nudge effect-size range. Finance is the lowest-receptivity domain (d = 0.25). Use this skill to calibrate expectations.
- **`nudge-second-order-meta-analysis-publication-bias`** — This study meets the evidence-quality standard (preregistered, high-powered) that the meta-analysis skill recommends. Its null findings are credible.
- **`boosting-empowering-behavior-change`** — The "no downstream behavior change" finding is the core argument for pairing nudges with boosts. The nudge gets learners in; the boost builds the competence that makes learning translate to action.
- **`boosts-vs-nudges-public-preference`** — When communicating the Be Practical strategy, frame the nudge (invitation) and the boost (curriculum) as complementary.
- **`scaled-behavioral-measurement-targeting`** — Levy et al. found homogeneous effects (no age/gender heterogeneity). This contrasts with Bauer et al.'s finding that a theory-identified moderator (loss aversion) drove real heterogeneity. The lesson: test for heterogeneity when theory identifies a moderator; expect homogeneity when it does not.
- **`behavioral-design-practical-playbooks`** — The playbook design principles (implementation intentions, progressive commitment, social proof, variable rewards) are the Stage 2 boost that Levy et al. says is necessary.
- **`zeigarnik-onboarding-architecture`** — The Zeigarnik-based re-engagement design is a structural solution to the persistence gap Levy et al. identified.
- **`hyperbolic-discounting-reversal`** — The "future payoff" nudge type was no more effective than simple attention nudges, suggesting future-payoff framing alone is insufficient to overcome present bias.
- **`llm-iterative-personalized-nudging`** — The Li et al. study found large personalization effects in electricity (low-friction); Levy et al. found no content-optimization benefit in financial education. The contrast suggests personalization matters more in some domains than others.
- **`gap-framework-advanced-applied-behavioral-science`** — The factorial design template and the two-stage nudge + boost model fit within the GAP framework's General Tools (diagnosis) and Algorithms (intervention design) components.

## Novelty Confirmation

- "youth financial education" — no matches in `/home/user/.skills`
- "Levy" — no matches in `/home/user/.skills`
- "425,000" — no matches in `/home/user/.skills`
- "parent nudge" / "gatekeeper nudge" — no matches as a targeting decision framework
- "sending a message matters more than optimizing its content" — no matches
- The combination of (a) the targeting decision framework (nudge the child, not the parent), (b) the message-content optimization guidance (content does not matter), (c) the persistence finding (timing shift, not durable change), (d) the downstream-behavior null (engagement ≠ behavior change), and (e) the factorial experimental design template as a single decision-support skill is novel in the A-Tech skill library.