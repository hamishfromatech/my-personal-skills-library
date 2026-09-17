# Iterative Mindset Method (IMM) — Evidence Base

This reference document contains the detailed empirical evidence underlying the Iterative Mindset Method (IMM) skill. It is organized by study, with full methodological detail, statistical results, and interpretation. For the applied framework, see `SKILL.md` in the parent directory.

---

## Study 1: Health Domain — Iterative Mindset, Health Habits, and Weight Loss

**Source:** Bobinet, K., Burnette, J.L., Becker, W. et al. (2026). "Motivation and successful goal pursuit: an iterative mindset predicts habits, weight loss, and work productivity." *Current Psychology*, 45, 755. DOI: [10.1007/s12144-026-09318-9](https://doi.org/10.1007/s12144-026-09318-9)

### Sample
- **N = 370** US adults
- **Age range:** 30-65 years
- **Recruitment:** Online sample
- **Domain:** Health behavior change (weight loss context)

### Measures

| Measure | Description | Reliability |
|---|---|---|
| **IMI-Weight** (Iterative Mindset Inventory — Weight) | 3-factor scale assessing the three IMM components (assess, iterate, practice) in the weight-loss context | α = 0.80 |
| **SRBAI** (Self-Report Behavioral Automaticity Index) | Subscale of the Self-Report Habit Index, measuring health habit automaticity | α = 0.97 |
| **Weight-loss success** | Binary self-report measure (achieved/not achieved weight-loss goal) | Single item |

### Confirmatory Factor Analysis (CFA)

- **3-factor structure confirmed** for the IMI-Weight scale (assess, iterate, practice)
- **2 items removed** from the initial item pool to achieve acceptable model fit
- The three-factor structure supports the theoretical distinction between the three IMM components — they are related but not redundant constructs

### Correlation Results

| Variable Pair | r | p |
|---|---|---|
| Iterative mindset ↔ Health habit automaticity | **0.438** | < .001 |
| Iterative mindset ↔ Weight loss success | **0.256** | < .001 |
| Health habit automaticity ↔ Weight loss success | Significant | (mediator) |

**Interpretation:**
- The iterative mindset has a **moderate-to-strong** correlation with health habit automaticity (r = 0.438), indicating that individuals with a stronger iterative mindset develop more automatic health habits.
- The correlation with weight loss success is weaker but significant (r = 0.256), consistent with the mediation hypothesis: the mindset's effect on weight loss operates *through* habit automaticity, not directly.
- The moderate (not large) correlation between mindset and weight loss is expected — weight loss is influenced by many factors beyond habits (metabolism, environment, genetics, social context).

### Mediation Analysis

**Method:** PROCESS Model 4 (Hayes), testing the indirect effect of iterative mindset on weight loss success through health habit automaticity.

| Effect | Estimate | 95% CI | Interpretation |
|---|---|---|---|
| **Indirect effect** (mindset → automaticity → weight loss) | **0.337** | **[0.186, 0.517]** | CI excludes zero → **mediation confirmed** |
| Direct effect (mindset → weight loss, bypassing automaticity) | Not significant | — | Full mediation through habit automaticity |

**Interpretation:** The iterative mindset does not directly produce weight loss. It produces health habit automaticity, which in turn produces weight loss. This is the empirical validation of the mediation pathway that is the core theoretical claim of IMM. Any intervention that builds iterative mindset without also building habit automaticity will not produce goal success — the automaticity is the active ingredient.

### The Practice Factor Anomaly

A notable and theoretically important finding:

| Factor | Correlation with Habit Automaticity | Correlation with Weight Loss |
|---|---|---|
| Assess | Significant | Significant |
| Iterate | Significant | Significant |
| **Practice** | **p = .505 (NOT significant)** | **p < .05 (significant)** |

**The anomaly:** In the health domain, the practice factor did *not* correlate with habit automaticity, but *did* correlate with weight loss.

**Theoretical explanation:** Weight-loss behaviors (dietary choices, exercise, portion control) are **highly cue-dependent and context-unstable**. Unlike work habits (which occur in a relatively stable office environment), health behaviors are triggered by variable social, emotional, temporal, and environmental cues. This context instability means:

- Raw repetition (practice) in unstable contexts does not reliably build automaticity, because the cue-behavior association is inconsistent (different contexts, different cues each time).
- The **assess** and **iterate** factors compensate by adapting the practice to changing contexts — adjusting the approach when the environment changes, so that some version of the behavior continues even when the original cue is absent.
- Practice correlates with weight loss directly (not through automaticity) because repeated dietary/exercise behaviors produce physiological effects even when they haven't become automatic habits.

**Practical implication:** For context-unstable behaviors (health, diet, exercise in variable environments), the **iterate** component is more important than the **practice** component for building automaticity. Product designers should emphasize adaptive iteration over raw repetition for these behaviors. For context-stable behaviors (work routines in a fixed office environment), practice may be sufficient — consistent with Study 2's results where the full 3-factor model fit well.

---

## Study 2: Work Domain — Iterative Mindset, Work Habits, and Productivity

**Source:** Bobinet, K., Burnette, J.L., Becker, W. et al. (2026). Same publication as Study 1.

### Sample
- **N = 915** full-time US workers
- **Age range:** 30+ years
- **Recruitment:** Online sample
- **Domain:** Workplace productivity

### Measures

| Measure | Description | Reliability |
|---|---|---|
| **IMI-Work** (Iterative Mindset Inventory — Work) | 3-factor scale assessing IMM components in the work context | α = 0.79 |
| **SRBAI** (Self-Report Behavioral Automaticity Index) | Measuring work habit automaticity | α = 0.96 |
| **HWQ** (Health and Work Questionnaire) | Productivity measure | α = 0.93 |

### Confirmatory Factor Analysis (CFA)

**Good model fit:**
| Fit Index | Value | Interpretation |
|---|---|---|
| **CFI** | **0.961** | > 0.95 = excellent fit |
| **TLI** | **0.950** | > 0.95 = excellent fit |
| **RMSEA** | **0.062** | < 0.08 = good fit |

**Interpretation:** The 3-factor structure (assess, iterate, practice) fits the work-domain data well, confirming the construct validity of the IMM framework in a second, independent domain with a larger sample. Unlike Study 1 (where 2 items were removed), the work-domain scale achieved good fit, suggesting the IMI-Work is a cleaner measurement instrument — possibly because work behaviors are more contextually stable, making the three components more distinctly measurable.

### Correlation Results

| Variable Pair | r | p |
|---|---|---|
| Iterative mindset ↔ Work habit automaticity | **0.329** | < .001 |
| Iterative mindset ↔ Work productivity | **0.388** | < .001 |
| Work habit automaticity ↔ Work productivity | Significant | (mediator) |

**Interpretation:**
- The iterative mindset correlates moderately with work habit automaticity (r = 0.329) — slightly weaker than in the health domain (r = 0.438), possibly because work habits are influenced by more external constraints (deadlines, managers, team dependencies) that reduce the variance attributable to mindset.
- The correlation with work productivity is moderate (r = 0.388) — stronger than the mindset-to-weight-loss correlation (r = 0.256), likely because productivity is more directly behavior-dependent than weight loss (which has physiological moderators).
- The mediation pathway holds: mindset → automaticity → productivity.

### Mediation Analysis

**Method:** PROCESS Model 4 (Hayes), testing the indirect effect of iterative mindset on work productivity through work habit automaticity.

| Effect | Estimate | 95% CI | Interpretation |
|---|---|---|---|
| **Indirect effect** (mindset → automaticity → productivity) | **0.372** | **[0.2797, 0.4754]** | CI excludes zero → **mediation confirmed** |
| Direct effect (mindset → productivity, bypassing automaticity) | Evaluated | — | Mediation pathway is the primary mechanism |

**Interpretation:** The mediation pathway is confirmed in the work domain, generalizing the IMM framework beyond health. The indirect effect (0.372) is slightly larger than in the health domain (0.337), suggesting that the mindset→automaticity→success pathway may be somewhat stronger for work productivity than for weight loss — consistent with the idea that productivity is more behavior-dependent (fewer physiological moderators).

### Cross-Domain Comparison

| Metric | Study 1 (Health) | Study 2 (Work) |
|---|---|---|
| N | 370 | 915 |
| Mindset ↔ Automaticity r | 0.438 | 0.329 |
| Mindset ↔ Goal Success r | 0.256 | 0.388 |
| Indirect effect | 0.337 [0.186, 0.517] | 0.372 [0.2797, 0.4754] |
| IMI scale α | 0.80 | 0.79 |
| SRBAI α | 0.97 | 0.96 |
| CFI | (2 items removed) | 0.961 |
| TLI | — | 0.950 |
| RMSEA | — | 0.062 |
| Likert scale | 5-point | 7-point |

**Key cross-domain finding:** The mediation pathway (mindset → automaticity → success) is robust across both domains, but the relative strength of the mindset→automaticity link is stronger in health (r=0.438) while the mindset→success link is stronger in work (r=0.388). This suggests that in health, more of the mindset's effect is "captured" by habit automaticity (health behaviors are more habit-dependent), while in work, the mindset has additional direct effects on productivity beyond automaticity (e.g., through better problem-solving, adaptability, stress management).

---

## Randomized Controlled Trial: Fresh Tri App + National DPP

**Source:** Leichter, J.W., Cannon, M.J. et al. (2026). "An iterative mindset approach as an adjunct to the national diabetes prevention program." *BMC Digital Health*, 4, 13. DOI: [10.1186/s44247-026-00247-y](https://doi.org/10.1186/s44247-026-00247-y)

### Trial Design

| Parameter | Detail |
|---|---|
| **Design** | Randomized controlled trial (RCT) |
| **N** | 364 participants |
| **Sites** | 33 sites |
| **Nesting structure** | 3-level nested design: participants → classes → sites |
| **Duration** | February 2021 – October 2022 |
| **Population** | Communities with above-average CCVI (COVID Community Vulnerability Index) — i.e., populations with elevated vulnerability factors |
| **Intervention** | Fresh Tri app (IMM-based) as an adjunct to the standard National Diabetes Prevention Program (DPP) Lifestyle Change Program (LCP) curriculum |
| **Control** | Standard DPP LCP curriculum only |
| **Primary outcomes** | Retention (6-month, 12-month) and weight loss (12-month) |

### Retention Outcomes

| Timepoint | Fresh Tri + DPP | DPP Only | Absolute Difference | Odds Ratio (OR) | p-value |
|---|---|---|---|---|---|
| **6 months** | **79.7%** | 67.1% | **+12.6%** | **2.06** | **.01** |
| **12 months** | **78.3%** | 53.1% | **+25.2%** | **3.14** | **.03** |

**Interpretation:**
- At 6 months, the Fresh Tri group retained ~80% of participants vs ~67% in the standard DPP — a 12.6 percentage-point advantage and more than double the odds of retention (OR = 2.06).
- At 12 months, the effect **widened**: the Fresh Tri group retained ~78% vs ~53% in the standard DPP — a 25.2 percentage-point advantage and **triple** the odds of retention (OR = 3.14).
- The widening effect over time is consistent with the IMM theory: as the standard DPP group encounters setbacks (inevitable in a 12-month behavior change program), the lateral habenula's motivation-loss pathway activates, and participants drop out. The Fresh Tri group, equipped with the "assess" reframe, neutralizes these setbacks and remains in-effort.
- Retention is the **primary bottleneck** for DPP effectiveness — you cannot lose weight if you drop out of the program. The retention gain is therefore the mechanism through which the weight-loss gain operates.

### Weight Loss Outcomes (12 months)

| Outcome | Fresh Tri + DPP | DPP Only | Absolute Difference | Odds Ratio (OR) | p-value |
|---|---|---|---|---|---|
| **Achieved weight-loss goal** | **41.3%** | 30.6% | **+10.7%** | **1.72** | **.05** |

**Interpretation:**
- At 12 months, 41.3% of the Fresh Tri group achieved their weight-loss goal vs 30.6% of the standard DPP group — a 10.7 percentage-point advantage and 72% higher odds (OR = 1.72).
- The p-value (.05) is at the conventional significance threshold, indicating a modest but meaningful effect. Given that the retention effect (which is the mechanism) is much stronger and clearly significant, the weight-loss effect being at p = .05 is consistent with the mediation model: the intervention's primary effect is on retention (keeping people in the program), and weight loss follows as a downstream consequence.
- This is the real-world validation of the mediation pathway from Studies 1 & 2: IMM keeps people in-effort (iterate) long enough for the habit to form (automaticity), which produces the goal outcome (weight loss).

### Why the Retention Effect Is the More Important Finding

The retention effect (OR = 2.06 at 6mo, OR = 3.14 at 12mo) is larger and more statistically robust than the weight-loss effect (OR = 1.72 at 12mo). This is not a weakness — it is the theoretically predicted pattern:

1. The IMM directly targets the **failure point** (setback → motivation loss → dropout). Its primary effect is therefore on **retention**, not on the physiological outcome (weight loss).
2. Weight loss is a **distal outcome** — it depends on retention (proximal) plus dietary adherence, exercise, metabolism, and many other factors. The intervention only affects weight loss indirectly, through retention.
3. The fact that the weight-loss effect is significant despite these additional moderators validates the full causal chain: IMM → retention → habit formation → weight loss.

**For product design:** This means the primary success metric for an IMM-based intervention should be **retention/engagement persistence**, not the distal goal outcome. If retention is sustained, the goal outcomes follow (as the mediation model predicts). Optimizing for the goal outcome directly (e.g., weight lost) without addressing the retention bottleneck is the standard approach that IMM is designed to replace.

---

## The Lateral Habenula Neuroscience

The IMM's "assess" component is grounded in the neuroscience of the lateral habenula. This section provides the detailed neuroscience foundation.

### The Lateral Habenula: The Brain's Disappointment Processor

The lateral habenula (LHb) is a small structure in the epithalamus that has emerged as a central node in the neuroscience of motivation, disappointment, and avoidance learning.

### Key Neuroscience References

#### Lawson et al. (2014)

- Demonstrated that the lateral habenula **encodes disappointment** — specifically, it activates in response to outcomes that are **worse than expected** (negative reward prediction errors).
- LHb activity **predicts subsequent disengagement** — individuals (and animals) who show stronger LHb activation after a disappointing outcome are more likely to abandon the behavior.
- The LHb's response to disappointment is not just correlational — it is **causally involved** in the motivation loss. Inhibiting the LHb reduces the motivation-drop following negative outcomes.
- This is the neural mechanism that IMM's "assess" component targets: by reframing setbacks as neutral data (not "worse than expected failure"), the LHb is not activated, and the motivation-drop is prevented.

#### Hikosaka (2010)

- Comprehensive review establishing the lateral habenula as a **key node connecting motivation to reinforcement learning**.
- The LHb receives inputs from forebrain structures involved in value assessment and sends outputs to midbrain dopamine and serotonin systems.
- **Dopamine modulation:** The LHb projects to the ventral tegmental area (VTA) and substantia nigra pars compacta (SNc), where it **inhibits dopamine neurons**. When the LHb activates (disappointment), dopamine release decreases — producing the subjective experience of motivation loss, anhedonia, and the "why bother" feeling.
- **Serotonin modulation:** The LHb also influences the dorsal raphe nucleus (serotonergic system), contributing to the mood drop that accompanies perceived failure. This dual dopamine-serotonin modulation explains why failure feels both demotivating (dopamine) and emotionally low (serotonin).
- **Connection to reinforcement learning:** By signaling worse-than-expected outcomes, the LHb teaches the brain to avoid behaviors that produce disappointment. This is a useful evolutionary function — avoid fruitless efforts — but it becomes maladaptive in habit formation, where temporary setbacks are a normal part of the process, not a signal to abandon the behavior.

### The Motivation-Loss Pathway (Detailed)

The standard behavior-change failure sequence, mapped to the neuroscience:

1. **User attempts behavior change** with a goal-setting mindset (high expectation)
2. **Inevitable setback occurs** (missed day, slip, underperformance)
3. **Cognitive appraisal: "I failed"** — the outcome is appraised as worse-than-expected (negative reward prediction error)
4. **Lateral habenula activates** — processes the disappointment signal
5. **Dopamine downregulation** (via VTA/SNc inhibition) — motivation drops
6. **Serotonin modulation** (via dorsal raphe) — mood drops
7. **Behavioral disengagement** — the user abandons the effort or reduces engagement
8. **Habit never reaches automaticity** — insufficient repetitions accumulated
9. **Goal not achieved** — the mediation pathway is broken at the automaticity stage

**IMM intervention point: Step 3.** By reframing the setback as neutral learning data ("What did I learn?"), the outcome is not appraised as worse-than-expected — it is appraised as information. The lateral habenula is not activated. Steps 4-7 do not occur. The user remains in-effort (Step 8 continues), and the habit has the opportunity to reach automaticity (Step 9 is achievable).

### Connection to Habit Formation and Reinforcement Learning

The LHb's role in reinforcement learning creates a paradox for habit formation:

- **Reinforcement learning** (the brain's mechanism for learning from outcomes) uses reward prediction errors to update behavior. Positive errors (better than expected) strengthen behavior; negative errors (worse than expected) weaken behavior — via the LHb.
- **Habit formation** requires many repetitions, including imperfect ones. But the reinforcement learning system treats each imperfect repetition as a negative prediction error, weakening the behavior via the LHb.
- **The IMM resolves this paradox** by altering the *appraisal* of the prediction error. If a missed day is appraised as "failure" (negative error), the LHb weakens motivation. If the same missed day is appraised as "data" (neutral), the LHb is not engaged, and the motivation is preserved for the next iteration.

This is why the "assess" component is placed first in the IMM cycle — it is the gatekeeper that determines whether the setback triggers the motivation-loss pathway or becomes a learning signal that feeds the next iteration.

---

## Comparison with Learning Goal Orientation (LGO)

A critical construct-validity question: is IMM just another name for Learning Goal Orientation (LGO, derived from Dweck's work on achievement goals)? The empirical evidence says they are related but distinct.

### Empirical Distinction

- **Correlation between IMM and LGO: r = 0.392** — moderate overlap, confirming shared variance but not identity. If they were the same construct, the correlation would be near r = 0.85+; at r = 0.392, they share only ~15% of variance.
- The remaining ~85% of variance is what makes IMM a distinct and additive framework.

### Conceptual Comparison

| Dimension | Learning Goal Orientation (LGO) | Iterative Mindset Method (IMM) |
|---|---|---|
| **Core focus** | Orientation toward learning and mastery | Method for surviving setbacks and reaching habit automaticity |
| **Relationship to failure** | Reframes failure as learning opportunity | **Neutralizes failure perception** to prevent LHb activation (goes beyond cognitive reframing to target the neurochemical motivation crash) |
| **Practice component** | Not included | Explicit component — accumulating repetitions for neuroplasticity |
| **Iteration component** | Not explicit — learning from experience is emergent | Explicit, structured try-and-tweak strategy that keeps individuals in-effort |
| **Neuroscience grounding** | Cognitive/motivational theory | Lateral habenula, dopamine/serotonin modulation, reinforcement learning |
| **Outcome pathway** | LGO → performance (direct) | IMM → habit automaticity → goal success (mediated) |
| **Domain specificity** | General achievement contexts | Validated in health (weight loss) and work (productivity) domains with domain-specific scales |

### Practical Distinction

- **LGO is a mindset; IMM is a method.** LGO answers "how do I think about challenges?" IMM answers "what do I do when I fail, and how do I keep going until the habit forms?"
- **LGO does not address habit formation.** LGO is about learning and performance, not about the neurological transition from effortful to automatic behavior. IMM explicitly targets the habit automaticity stage that mediates goal success.
- **LGO does not address the lateral habenula.** LGO's cognitive reframing ("failure is learning") may or may not be sufficient to prevent the LHb's motivation-loss pathway. IMM's "assess" component is specifically designed to neutralize the emotional appraisal that triggers the LHb — going beyond cognitive reframing to emotional neutralization.
- **They are complementary, not competing.** A user with high LGO will find IMM natural; a user with low LGO may need IMM's structured method to compensate for their default performance-orientation. IMM can be viewed as the *operationalization* of LGO for behavior-change contexts, with the added components of practice and lateral-habenula-targeted emotional neutralization.

---

## Limitations

### Studies 1 & 2 (Correlational)

1. **Correlational, not causal.** The mediation pathway (mindset → automaticity → success) is statistically supported via cross-sectional data and PROCESS Model 4, but cannot establish causation. Reverse causation is plausible (people who successfully lose weight may develop a more iterative mindset as a result, not a cause). The RCT (Study 3) provides causal evidence for the *intervention*, but not for the specific mediation pathway.

2. **Online samples.** Both studies used online convenience samples of US adults. Online samples may differ from clinical or workplace populations in motivation, digital literacy, and self-selection bias. The effect sizes may not generalize to populations who did not self-select into an online survey.

3. **Self-report measures.** All measures (IMI, SRBAI, HWQ, weight-loss success) are self-reported, introducing:
   - **Common-method variance:** correlations may be inflated by shared method variance
   - **Social desirability bias:** participants may overreport iterative mindset and habit automaticity
   - **Recall bias:** self-reported habit automaticity may not match behavioral observation

4. **Single-item weight-loss measure.** Study 1's weight-loss success was measured with a binary single item (achieved/not achieved). This limits precision, reduces statistical power, and may underestimate effects. A continuous measure (e.g., % body weight lost) would provide more granular data.

5. **Likert scale differences.** Study 1 used a 5-point Likert scale; Study 2 used a 7-point Likert scale for the IMI measure. This prevents direct scale-score comparison across domains. While standardized correlations and CFA fit indices are comparable, the raw scores cannot be pooled or directly compared.

6. **Practice factor anomaly is post-hoc.** The theoretical explanation for why the practice factor did not correlate with health habit automaticity (cue-dependency and context instability of health behaviors) is plausible and consistent with habit theory, but it is a post-hoc interpretation. It requires direct empirical testing — ideally, a study that manipulates context stability and measures whether practice correlates with automaticity differently in stable vs. unstable contexts.

### Study 3 (RCT)

7. **CCVI above-average communities.** The RCT was conducted in communities with above-average COVID Community Vulnerability Index. This population may differ from the general DPP-eligible population in ways that affect generalizability (e.g., higher stress, fewer resources, different social support structures).

8. **Weight-loss outcome at p = .05.** The weight-loss effect (OR = 1.72, p = .05) is at the conventional significance threshold. While the retention effect is robust (p = .01, p = .03), the weight-loss effect is marginal and could be a false positive at conventional alpha levels. However, given that the retention effect (the mechanism) is clearly significant, and the weight-loss effect is in the predicted direction with a meaningful effect size, the marginal p-value is more likely a power issue than a true null.

9. **App vs. method confound.** The RCT tested the Fresh Tri *app* as an adjunct to DPP, not the IMM *method* in isolation. The retention and weight-loss effects could be partially attributable to the app's UX, notifications, or other non-IMM features, rather than the IMM methodology specifically. Isolating the IMM method from the app delivery vehicle would require a different design (e.g., IMM training without the app, or a different app implementing the same IMM method).

---

## Cross-References to Existing Skills

- **`rapid-habit-transition-switch`** — The Johns Hopkins discovery that habit formation is a sudden phase transition (not gradual strengthening). IMM is complementary: the switch model explains *when* habits form; IMM explains *how to remain in-effort* through the setback-filled pre-switch period.
- **`habit-formation-neuroscience-2026`** — The 7-principle neuroscience framework (complexity-matched timelines, cue design, friction reduction, identity alignment, accountability, stress management, unremarkable regularity). IMM provides the *mindset layer* that makes these principles workable when users encounter setbacks. The neuroscience defines what to engineer; IMM defines how to survive the engineering process.
- **`habit-degradation-strategies-intensive-longitudinal`** — IMM's "assess" component is directly relevant to preventing habit degradation: reframing lapses as data (not failure) is the mechanism that prevents a lapse from cascading into a full relapse via the LHb motivation-loss pathway.
- **`behavior-change-synthesis-2026`** — The integrated behavior-change synthesis (TTM, TPB, PMT + habit mechanisms). IMM is the mindset-level intervention that sustains the intentional process (TTM action/maintenance stages) long enough for the automatic process (habit formation) to take over. Without IMM, the intentional process collapses at the first major setback before the automatic process can form.
- **`ai-motivational-interviewing-scale`** — AI-delivered motivational interviewing (Change Talk, Decisional Balance, Direct Persuasion). The MI study's key finding (motivation ≠ behavior change) aligns with IMM's mediation model: motivation (which MI increases) does not directly produce behavior change; habit automaticity (which IMM targets) does. AI-MI could serve as the conversational delivery mechanism for IMM's "assess" component, and IMM explains *why* the MI motivation gain doesn't directly produce behavior change (because automaticity, not motivation, is the mediator).

---

## Citations

1. **Bobinet, K., Burnette, J.L., Becker, W. et al. (2026).** "Motivation and successful goal pursuit: an iterative mindset predicts habits, weight loss, and work productivity." *Current Psychology*, 45, 755. DOI: [10.1007/s12144-026-09318-9](https://doi.org/10.1007/s12144-026-09318-9)

2. **Leichter, J.W., Cannon, M.J. et al. (2026).** "An iterative mindset approach as an adjunct to the national diabetes prevention program." *BMC Digital Health*, 4, 13. DOI: [10.1186/s44247-026-00247-y](https://doi.org/10.1186/s44247-026-00247-y)

3. **Lawson, M.A., Whiskeyman, S., Miller, A.D., Mars, R.B., & Husain, M. (2014).** (Lateral habenula encodes disappointment / worse-than-expected outcomes and predicts disengagement.) — Neuroscience foundation for the "assess" component.

4. **Hikosaka, O. (2010).** "The habenula: from stress evasion to value-based decision-making." *Nature Reviews Neuroscience*, 11, 503-513. — Comprehensive review establishing the lateral habenula as a key node connecting motivation to reinforcement learning via dopamine and serotonin modulation.

5. **Hayes, A.F.** PROCESS Model 4 — Statistical method used for mediation analysis in Studies 1 & 2 (indirect effect estimation with bootstrap confidence intervals).

6. **Verplanken, B., & Orbell, S. (2003).** Self-Report Habit Index (SRHI) — The SRBAI (Self-Report Behavioral Automaticity Index) used in both studies is a subscale of the SRHI.

7. **Dweck, C.S., & Leggett, E.L. (1988).** Achievement goal theory (Learning Goal Orientation vs. Performance Goal Orientation) — The construct used for the discriminant validity comparison with IMM.

8. **Centers for Disease Control and Prevention (CDC).** National Diabetes Prevention Program (National DPP) Lifestyle Change Program (LCP) — The standard curriculum to which the Fresh Tri app was added as an adjunct in the RCT.

9. **Surgo Ventures.** COVID Community Vulnerability Index (CCVI) — The vulnerability index used to select study communities with above-average vulnerability for the RCT.