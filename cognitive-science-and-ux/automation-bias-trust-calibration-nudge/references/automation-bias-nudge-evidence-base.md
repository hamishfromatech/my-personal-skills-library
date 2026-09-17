# Automation-Bias Trust Calibration Nudge — Evidence Base

## Source

Qazi, A., Ali, S., Khawaja, F. S., Akhtar, M. N., Sheikh, R., & Alizai, S. A. (2026). "Mitigating automation bias in physician-LLM diagnostic reasoning: A randomized controlled trial of a dual-component behavioral nudge." *medRxiv* (preprint). ClinicalTrials.gov registration: NCT07328815.

**Affiliation:** Lahore University of Management Sciences (LUMS), Lahore, Pakistan.

**Registration:** ClinicalTrials.gov NCT07328815 (preregistered randomized controlled trial).

**Access:** medRxiv preprint; ClinicalTrials.gov registry entry.

## Study Design

- **Design:** Preregistered, parallel-group randomized controlled trial (RCT).
- **N:** 72 AI-trained physicians (all had completed prior AI literacy training — see the NEJM AI prior-study reference below).
- **Cases:** 432 total diagnostic cases (6 clinical vignettes per physician, 3 of the 6 deliberately seeded with errors).
- **Arms:**
  1. **Control** — physicians consulted ChatGPT with no behavioral nudge (standard AI consultation).
  2. **Treatment** — physicians consulted ChatGPT with the dual-component trust-calibration nudge active in the consultation interface.
- **Randomization:** Physicians randomized to control vs. treatment; all physicians evaluated the same 6 vignettes (3 with deliberate errors).
- **Inter-rater reliability:** Krippendorff's α = 0.89 (outcome scoring — substantial-to-near-perfect agreement; well above the 0.80 acceptability threshold).

## The Dual-Component Nudge Design

The nudge combines two behavioral-science cues, each targeting a distinct automation-bias mechanism:

### Component 1: Anchoring Cue (Baseline Expectation Calibration)

- **What it does:** Before the physician begins the AI consultation, the interface reports ChatGPT's benchmark diagnostic accuracy on standardized medical reasoning tasks (a published, external accuracy figure — not a self-assessment).
- **Mechanism:** Anchoring-and-adjustment. The physician enters the consultation with a calibrated baseline expectation of what the model can and cannot do, rather than an open-ended (and typically inflated) prior. This prevents the default "the AI is an oracle" anchor that automation bias feeds on.
- **What it is NOT:** Not a per-output confidence score (which is the model's self-assessment and subject to the self-assessment critique). It is a one-time, external, benchmark-anchored expectation-setter.

### Component 2: Ensemble Selective-Attention Cue (Traffic-Light Reliability Signal)

- **What it does:** For each recommendation the AI produces, an ensemble of three LLMs from **distinct model families** independently generates a confidence rating. The ensemble disagreement is mapped to a traffic-light signal:
  - 🟢 **Green:** all three models agree the recommendation is high-confidence → low scrutiny needed.
  - 🟠 **Orange:** partial disagreement → moderate scrutiny recommended.
  - 🔴 **Red:** substantial disagreement across models → high scrutiny recommended; the recommendation is unreliable.
- **The three model families:** Claude Sonnet 4.5 (Anthropic), Gemini 2.5 Pro Thinking (Google), GPT-4o (OpenAI) — deliberately chosen from distinct labs/families to ensure architectural diversity in the ensemble.
- **Mechanism:** Selective attention. The traffic-light signal directs the physician's scarce scrutiny to the cases where the recommendation is least reliable — the disagreement cases — while preserving low-friction, fast processing of routine (green) recommendations. This is the key efficiency property: scrutiny is selective, not blanket.
- **What it is NOT:** Not any single system's self-reported confidence score. The reliability signal is model-agnostic — it emerges from the *disagreement across independent systems*, not from one system's internal probability estimate. This is the core innovation relative to conventional AI confidence displays.

## Ensemble Methodology

### Why Three Distinct Model Families

The ensemble uses Claude Sonnet 4.5, Gemini 2.5 Pro Thinking, and GPT-4o — three models from three different labs (Anthropic, Google, OpenAI). The rationale:

1. **Architectural diversity:** models from different families have different training data, architectures, and post-training procedures, so their errors are less correlated than same-family models. Disagreement is therefore a more informative signal.
2. **Correlated-error avoidance:** if the ensemble used three models from the same family (e.g., GPT-4o, GPT-4o-mini, GPT-4.1), they would share systematic blind spots — agreement would not mean the recommendation is correct, and disagreement would be rare even when the answer is wrong. Distinct families maximize the independence of the error signals.
3. **Model-agnostic reliability:** the signal does not depend on trusting any one system. If Claude, Gemini, and GPT all independently agree a recommendation is high-confidence, that is a stronger signal than any one of them saying so alone — and if they disagree, no single system's self-assessment can override the cross-system divergence.

### Traffic-Light Mapping

| Ensemble state | Signal | Scrutiny level | Rationale |
|---|---|---|---|
| All three agree: high confidence | 🟢 Green | Low (fast-track) | Cross-family agreement is a strong reliability signal; routine processing preserved |
| Partial agreement / one dissenter | 🟠 Orange | Moderate | Mixed signal warrants attention but is not necessarily wrong |
| Substantial disagreement | 🔴 Red | High (scrutinize) | Cross-family disagreement is a model-agnostic unreliability signal independent of any single system |

### Why This Beats Conventional Confidence Scores

Conventional AI confidence scores are a single system's self-assessment — the same system that produced the recommendation is telling you how confident it is. This is vulnerable to:
- **Self-confirmation bias:** the system is assessing its own output.
- **Correlated miscalibration:** the confidence estimate and the recommendation share the same failure modes.
- **Opaque calibration:** the user cannot tell whether "85% confident" is calibrated or vanity (the ability-cue-trap problem — see `ai-explanation-ability-cue-trap`).

The ensemble-disagreement signal sidesteps all three: the reliability assessment comes from *independent* systems that did not produce the recommendation, so their disagreement is a genuine out-of-sample signal. This is the model-agnostic property that makes it robust.

## Primary and Secondary Outcomes

### Primary Outcome: Diagnostic Reasoning Score

| Metric | Control | Treatment | Difference | 95% CI | P-value |
|---|---|---|---|---|---|
| Diagnostic reasoning score | — | — | **+7.6 pp** | 1.4 – 13.9 | **0.016** |

The treatment group scored 7.6 percentage points higher on the composite diagnostic reasoning measure. The effect is statistically significant and the confidence interval excludes zero.

### Secondary Outcome: Top-Choice Diagnosis Accuracy

| Metric | Control | Treatment | Difference | 95% CI | P-value |
|---|---|---|---|---|---|
| Top-choice diagnosis accuracy | — | — | **+10.9 pp** | 1.7 – 20.0 | **0.020** |

The treatment group's top-choice (primary) diagnosis was correct 10.9 percentage points more often. This is the clinically most important outcome — getting the primary diagnosis right.

### Effect-Size Interpretation

Both effects are moderate in absolute terms (7.6pp, 10.9pp) but substantial in context: the intervention is a lightweight interface nudge (a pre-consultation anchor + a traffic-light signal), not a training program or a workflow overhaul. A ~8-11pp improvement on a low-friction, deployable interface cue is a high effect-per-dollar intervention.

## Subgroup Analyses

### By Years of Practice

| Subgroup | Effect (pp) | P-value | Detected? |
|---|---|---|---|
| ≤8 years practice | **+11.9** | **0.003** | ✅ Yes |
| >8 years practice | not significant | n.s. | ❌ No |

The benefit is concentrated among early- and mid-career physicians. The effect among experienced physicians was not detected (and the ≤8 years effect is ~50% larger than the overall effect).

**Interpretation:** Less-experienced physicians are more susceptible to automation bias (they have a weaker independent baseline to anchor against the AI's recommendation), so the nudge has more to correct. Experienced physicians may already have calibrated trust (or are not consulting the AI as heavily), leaving less for the nudge to shift.

### By Gender

| Subgroup | Effect (pp) | P-value | Detected? |
|---|---|---|---|
| Female physicians | **+12.1** | **0.001** | ✅ Yes |
| Male physicians | not significant | n.s. | ❌ No |

The benefit was detected among female physicians but not among male physicians. The female-physician effect (~12.1pp) is the largest subgroup effect in the study.

**Interpretation:** The authors do not commit to a single causal story for the gender effect. Candidate mechanisms (for investigation, not conclusions): differential AI-consultation patterns, differential responsiveness to calibration cues, or differential baseline automation-bias susceptibility. The gender effect is a finding that warrants replication and mechanism work, not a design prescription.

### By Prior LLM Experience

All 72 physicians had completed prior AI literacy training (per the NEJM AI study referenced below). The nudge still produced a significant effect — meaning **prior AI literacy training alone was insufficient to prevent automation bias.** The interface-level nudge added measurable benefit on top of training. This is the key policy finding: training is necessary but not sufficient; the interface must carry calibration cues too.

## Time-on-Task Analysis

The study measured time-on-task per case. The selective-attention design (scrutiny focused only on red/orange cases, green cases fast-tracked) is the mechanism that makes the nudge scalable rather than a blanket-scrutiny tax:

- The nudge does not impose a blanket verification burden. Green cases are processed at low friction.
- The extra scrutiny is concentrated on the disagreement (red/orange) cases — exactly where it is most valuable.
- This preserves the throughput advantage of AI consultation while inserting scrutiny precisely where it matters. Without this selectivity, a "always scrutinize everything" intervention would destroy the productivity benefit and produce algorithmic aversion (see `algorithmic-aversion-defense`); the traffic-light design avoids that failure mode.

## Mechanism Interpretation

### Selective System 2 Engagement

The traffic-light signal triggers System 2 (analytic, effortful) reasoning *selectively* — on disagreement cases — while allowing System 1 (fast, automatic) processing of routine (green) recommendations. This is the cognitive-efficiency mechanism:

- **Green:** System 1 processing — the cross-family agreement is a strong enough reliability signal that the physician can accept with low scrutiny. This preserves the AI's throughput benefit.
- **Red/Orange:** System 2 engagement — the cross-family disagreement is an unreliability signal that prompts the physician to slow down, reason independently, and verify. This is where the diagnostic accuracy gains come from.

Without selectivity (blanket scrutiny), the nudge would either (a) fail to scale (too much friction → algorithmic aversion) or (b) dilute attention across all cases (no concentration on the cases that matter). The traffic-light design concentrates System 2 engagement on exactly the cases where the AI is most likely to be wrong.

### Anchoring as Prior Calibration

The anchoring cue (Component 1) works by setting the physician's *prior* on the AI's competence before the consultation begins. This counteracts the default inflated prior ("the AI is an oracle") that drives automation bias. The mechanism is anchoring-and-adjustment: with a calibrated prior, subsequent AI recommendations are processed with appropriate skepticism rather than default acceptance.

### The Two Components Are Complementary

- The anchoring cue calibrates the *global* expectation (how good is this AI in general?).
- The ensemble cue calibrates the *local* expectation (how reliable is this specific recommendation?).

Together they address both the baseline-trust problem (anchoring) and the per-recommendation-reliability problem (ensemble selective attention). Neither alone is sufficient: anchoring without the per-recommendation signal leaves the physician unable to tell which recommendations to scrutinize; the ensemble signal without the anchor leaves the baseline expectation uncalibrated.

## Policy Implications

### Build Calibration Cues Into the AI Consultation Interface — Not Training Only

The headline policy finding: **prior AI literacy training was insufficient.** All 72 physicians had been AI-trained (per the prior NEJM AI study), yet automation bias persisted in the control group and the interface nudge still produced a significant improvement. The implication:

1. **Training is necessary but not sufficient.** It raises awareness but does not, by itself, produce calibrated trust at the moment of consultation.
2. **The interface must carry calibration cues.** The anchoring cue and the ensemble traffic-light signal are interface-level interventions — they are present at the moment the physician consults the AI, which is exactly when automation bias operates. A training that happened weeks ago cannot compete with a confident recommendation in front of you right now.
3. **Interface cues are more scalable than training.** They deploy to every consultation without requiring per-physician retraining; they update as models update.

This is the core policy argument: **do not rely on training alone to mitigate automation bias — build the calibration cue into the AI consultation interface.**

### The Selective-Attention Design Is the Scalability Key

The traffic-light design is not "always verify everything" (which would destroy throughput and cause algorithmic aversion). It is "verify the cases the ensemble flags as unreliable." This selective-scrutiny design is what makes the nudge deployable at scale without sacrificing the AI's productivity benefit. Blanket-verification interventions fail because they make the AI useless by making every consultation a verification exercise; the selective design preserves the throughput advantage.

### Limitations of the Single-Model Self-Assessment

The study's ensemble approach (three distinct model families) is a direct response to the limitation of conventional AI confidence scores: a single system assessing its own output is self-referential and shares the recommendation's failure modes. The model-agnostic ensemble-disagreement signal is the innovation that makes the reliability assessment trustworthy. Policy implication: **calibration signals should come from independent systems, not the system that produced the recommendation.**

## Limitations

- **Single domain (clinical diagnosis):** the trial is in physician-LLM diagnostic reasoning. Generalization to other high-stakes domains (legal, engineering, financial) is plausible but untested.
- **Preprint (medRxiv):** the study is a preprint and has not yet completed peer review. The findings should be treated as provisional pending peer-review validation and replication.
- **N=72 physicians:** a moderate sample; the subgroup analyses (years of practice, gender) are necessarily exploratory and should be treated as hypothesis-generating, not confirmatory. The gender subgroup effect in particular needs replication.
- **Single model consulted (ChatGPT):** the physicians consulted ChatGPT; the ensemble (Claude/Gemini/GPT) provided only the reliability signal. The architecture extends to other consultation models, but this specific trial used ChatGPT as the consulted system.
- **6 vignettes per physician, 3 with deliberate errors:** the error-seeding rate (50%) is higher than real-world AI error rates, which may amplify the nudge's measured effect relative to field deployment. Real-world error rates vary by domain.
- **Short-term, single-session measurement:** the trial measures the nudge's effect within a consultation session, not whether the calibrated-trust habit persists over time. Whether physicians internalize the selective-scrutiny habit (and maintain it without the interface cue) is an open question for longitudinal follow-up.
- **K-α = 0.89 is strong but not perfect:** inter-rater reliability is high, but residual scoring disagreement exists.
- **Ensemble cost:** the three-model ensemble triples the inference cost of the reliability signal. For green cases this may be acceptable (low scrutiny downstream); for high-throughput settings the cost-benefit tradeoff needs evaluation.

## Cross-References to Existing Skills

| Cross-referenced skill | What it contributes | Connection |
|---|---|---|
| `cognitive-science-and-ux/trust-calibration-ux-pattern` | The five-move trust calibration framework (per-domain track records, supervised→autonomous progression, trust repair, under-trust as failure) | This skill provides the *empirical evidence* that an interface-level calibration cue works; the trust-calibration pattern provides the broader design framework. The anchoring cue is a specific instance of "show the track record"; the ensemble signal is a specific instance of "tie trust to performance not usage." |
| `cognitive-science-and-ux/ai-explanation-ability-cue-trap` | The mechanism behind automation bias (brief rationales raise reliance via perceived ability, NOT understanding) | This skill's ensemble-disagreement signal is the direct counter to the ability-cue trap: instead of a single system's fluent self-assessment (the ability cue), the reliability signal comes from *independent* systems whose disagreement cannot be manufactured by fluency. The model-agnostic property is the structural fix for the self-assessment critique. |
| `cognitive-science-and-ux/cognitive-surrender-defense` | Preventing the erosion of independent thinking when users over-trust confident AI | The nudge is a structural cognitive-surrender defense: the traffic-light signal forces System 2 engagement on disagreement cases, preventing the default acceptance (surrender) that automation bias produces. The BRACED framework (disconfirming evidence, counter-hypothesis) is the manual version; the ensemble signal is the automated version. |
| `cognitive-science-and-ux/ai-adoption-calibration` | The bias-aversion spectrum and the verifiability taxonomy | This skill provides the interface-level intervention that the adoption-calibration framework calls for: the traffic-light signal is a verifiability aid that concentrates scrutiny on the cases the user cannot fast-verify. The ≤8-years subgroup effect is the adoption-calibration finding that less-experienced users (weaker independent baseline) benefit most from calibration cues. |
| `privacy-and-trust/deferred-trust-ai-selection` | The compensatory trust mechanism (distrust in humans redirects to AI) and the fluency-driven over-reliance risk | The ensemble signal counters the fluency-driven over-reliance chain: when the consulted system's fluency would otherwise manufacture trust (the deferred-trust risk), the cross-family disagreement provides an independent reliability signal that fluency cannot manufacture. |

## A-Tech Alignment

| Pillar | Alignment |
|---|---|
| **Open-source AI** | The ensemble uses open-weight-accessible model families (Claude, Gemini, GPT — all available via API; the architecture extends to fully open-weight models like Llama, Mistral, DeepSeek). The cross-family disagreement signal is stronger with architecturally diverse models — open-weight diversity is an asset, not a liability. An open-source ensemble (e.g., Llama + Mistral + DeepSeek + Qwen) would preserve the model-agnostic property while being fully open-weight. |
| **Data privacy** | The ensemble reliability signal can be computed without the recommendation content leaving the local environment (the three models rate confidence locally; only the traffic-light signal is displayed). No patient data needs to be transmitted to a third-party ensemble service. The architecture is privacy-preserving by construction. |
| **Financial freedom** | The nudge is a low-cost, high-leverage intervention (interface cue, not a training program). The effect-per-dollar is high: a ~8-11pp diagnostic accuracy improvement from a lightweight interface signal. The ensemble inference cost is the main expense; for high-stakes domains (clinical, legal, engineering) the cost-benefit is strongly favorable. |
| **Practical implementation** | Preregistered RCT (NCT07328815), moderate sample (72 physicians, 432 cases), clear primary/secondary outcomes, high inter-rater reliability (α=0.89), subgroup analyses with clear policy implications. The intervention is deployable as an interface feature, not a training program — scalable by design. |

## Related Prior Study

The NEJM AI study referenced by the authors: prior AI literacy training of the same physician cohort. The finding that all 72 AI-trained physicians still showed automation bias in the control arm (and the nudge still produced a significant effect on top of training) is the empirical basis for the policy argument that training alone is insufficient. (Full citation to be confirmed upon peer-review publication of the referenced NEJM AI study.)

## Citation

Qazi, A., Ali, S., Khawaja, F. S., Akhtar, M. N., Sheikh, R., & Alizai, S. A. (2026). Mitigating automation bias in physician-LLM diagnostic reasoning: A randomized controlled trial of a dual-component behavioral nudge. *medRxiv* (preprint). ClinicalTrials.gov NCT07328815.

## Date Researched

2026-08-18 | Daily Research Process | A-Tech Research Division