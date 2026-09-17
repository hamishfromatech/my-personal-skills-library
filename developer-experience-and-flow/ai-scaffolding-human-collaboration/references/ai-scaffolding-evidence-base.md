# AI Scaffolding for Human Collaboration — Evidence Base

## Source

Farach, F., Cambon, B., Tankelevitch, L., Hsueh, J., & Janssen, C. (Microsoft Corporation), March 2026, arXiv:2604.08678. Field experiment with 388 employees at Gap Inc. (Fortune 500 retailer) testing two scaffolding interventions for human-AI collaboration.

---

## 1. Study Design

**Setting:** Gap Inc., a Fortune 500 retailer — an organizational (not lab) field experiment, which is a strength for ecological validity and a limitation for control.

**Participants:** 388 employees, randomized into 194 pairs.

**Task structure:**
- **Task A — pair-level document task.** Pairs produced a document collaboratively. This is where the behavioral scaffold (requiring joint AI use within pairs) was applied and where its effects on pair-level output quality and production were measured.
- **Task B — individual-level document task.** Each participant produced a document individually. This is where the cognitive scaffold's effect on individual top-quality work was measured (OR = 2.07, p = .022, exploratory binary model).

The ordering (Task A pair → Task B individual) is the basis of the carry-over analysis — Task B outcomes may be influenced by Task A exposure, and the authors interpret the cognitive scaffold's belief-change signal partly as recovery from Task A carry-over.

**Interventions tested:**
1. **Behavioral scaffolding** — the "Create-Out-Loud" collaborative protocol (detailed in §3).
2. **Cognitive scaffolding** — partnership training using the *AI Mindset* curriculum by Conor Grennan (detailed in §4).
3. **Control** — baseline AI use without either scaffold.

---

## 2. The Behavioral vs. Cognitive Scaffolding Distinction

This is the core conceptual contribution and the reason the skill exists. The two scaffold types operate on different layers of the human-AI interaction:

### Behavioral scaffolding

- **What it structures:** *how* humans interact with AI — the procedure, the turn-taking, the coordination protocol.
- **Mechanism:** external protocol that participants follow. The behavior change comes from compliance with an explicit rule ("use the AI together, voice each step"), not from a changed understanding of the AI.
- **Dependency:** high. Requires compliance, reliable infrastructure, and a task where the coordinated procedure adds value. Fails when any of these are absent — and the failure mode is *worse than baseline* because participants incur the protocol's overhead without realizing its benefits.
- **Analogy:** a choreography. Everyone must know the steps and the floor must be stable. One person forgetting the steps or a slippery floor makes the whole dance worse than freestyle.

### Cognitive scaffolding

- **What it reshapes:** *mental models* of AI — how participants conceive of what the AI is and how they should engage with it.
- **Mechanism:** internal reframe that changes default interaction. The behavior change comes from a shifted mental model ("AI is a smart intern, so I iterate and critique"), not from a prescribed procedure.
- **Dependency:** lower. Does not require real-time compliance monitoring or infrastructure reliability — the reframe persists as a default even when conditions vary. The dependency is on the reframe *sticking* and on participants being able to evaluate AI output (the "intern" metaphor fails if the supervisor can't judge the intern's work).
- **Analogy:** a mindset shift. Once you see the AI as an intern, you naturally iterate — no one has to tell you to, because nobody accepts a first intern draft as final.

**Why the distinction matters:** the two are not interchangeable, and choosing the wrong one can produce *worse* outcomes than no scaffold at all. The study's behavioral scaffold reduced quality (d = 0.81 in the harmful direction) — a large negative effect — while the cognitive scaffold trended positive on top-quality individual work. Treating "scaffolding" as a single category, as much of the practitioner literature does, obscures this critical asymmetry.

---

## 3. The "Create-Out-Loud" Collaborative Protocol (Behavioral Scaffold)

The behavioral intervention was a structured collaborative protocol requiring joint AI use within pairs:

- **Joint AI use mandate.** Both members of a pair must engage the AI *together* — not parallel individual use, not one-person-uses-while-the-other-watches. The AI functions as a third participant in a three-way collaboration. The mandate is the load-bearing element: it forbids the natural fallback of "let me just prompt it myself and we'll compare."
- **Verbalized coordination ("create out loud").** Each step of AI use is spoken aloud: what we're about to ask the AI, why we're asking it that way, what we expect to get back, what we actually got, and whether it's usable. The protocol externalizes reasoning that would otherwise be internal and silent.
- **Structured turn-taking.** The protocol prescribes who initiates the AI query, who reviews the output, and how the pair converges on accepting or revising — rather than letting coordination emerge ad hoc. This is designed to ensure both perspectives are integrated into the AI-assisted output.

**Intended benefit:** for tasks requiring cross-perspective integration, forcing pairs to coordinate around the AI should surface and integrate both perspectives, producing output neither would have produced alone.

**Actual outcome (the cautionary finding):**
- **Lower document quality:** b = −4.96, p < .001, d = 0.81 — a large effect in the *harmful* direction. Coordinated pairs produced worse documents than non-coordinated pairs.
- **Substantially lower document production:** OR = 0.12 — the odds of producing a document at all were roughly an eighth of baseline. The protocol's overhead caused many pairs to fail to complete within the session.
- **Coordination costs exceeded collaboration benefits.** Every voiced step, every joint review, every turn-taking handoff is overhead that solo AI use avoids. The study's task apparently did not have enough perspective divergence for the integration benefit to offset this overhead — and when the AI infrastructure was unreliable, the overhead compounded because tool failures disrupted the coordinated procedure as well as the work.

**When it could still help (per the framework, not yet replicated):** cross-perspective integration tasks where perspectives genuinely diverge enough that coordination adds unique value, *and* infrastructure is reliable, *and* compliance is high. The study's task may not have met the first condition strongly enough; the framework conditions (§5) specify what would need to hold.

---

## 4. Partnership Training and the "Smart Intern" Metaphor (Cognitive Scaffold)

The cognitive intervention was **partnership training** built on the *AI Mindset* curriculum by Conor Grennan.

**Core reframe — "smart intern":**
- Treat the AI as a capable but junior collaborator — smart, eager, fast — who nonetheless lacks your context, your judgment, and your accountability.
- The intern produces *drafts* you must review, refine, and redirect — not deliverables you can accept as-is.
- The intern can be wrong. The intern needs context. The intern's output is a *starting point*.

**Why this metaphor works (when it works):**
- It counteracts two common but unhelpful mental models: the **oracle** model (AI is authoritative, accept its output) which produces over-reliance, and the **autocomplete** model (AI finishes my sentence, accept and move on) which produces under-engagement.
- The intern model naturally induces **iterative dialogue**: nobody accepts a first intern draft as final, so the reframe shifts the default interaction from "ask once, accept" to "iterate, critique, redirect" — without prescribing a procedure. The behavior change is emergent from the mental model, which is why it doesn't carry the behavioral scaffold's coordination overhead.
- It sets realistic expectations: the AI is useful but fallible, and the human retains judgment and accountability — which supports appropriate trust calibration rather than either over-reliance or avoidance.

**When the metaphor fails (the cognitive-load condition):**
- The "intern" metaphor requires the supervisor to be able to *judge the intern's work*. A novice who can't evaluate AI output quality gets no benefit from the reframe — they lack the supervisory skill the metaphor assumes. The framework specifies cognitive scaffolding helps only "when [the] shift toward dialogue doesn't impose excessive cognitive load," and evaluating an intern you can't assess is exactly that excessive load.
- It can collide with entrenched existing models. If participants are anchored on "AI is an oracle," the intern reframe may not stick, leaving them with two competing models and no clear default.
- It assumes conversational iteration is practical for the task. Time-pressured or high-volume contexts where one-shot querying is the norm don't accommodate the dialogue shift well.

**Partnership training curriculum (AI Mindset by Conor Grennan):** a training program designed to instill the partnership mental model rather than teach a procedure. The distinction from the behavioral scaffold is structural: the training changes *how you think about the AI*, not *what steps you follow when using it*. This is why its effects can persist without compliance monitoring — the reframe, once internalized, is the new default.

---

## 5. The Framework for Scaffold Effectiveness

The study's most transferable contribution is the conditional framework specifying *when* each scaffold type helps vs. harms. This is the decision logic the SKILL.md workflow operationalizes.

### When behavioral scaffolding helps

Behavioral scaffolds help when **all three** conditions hold:

1. **Compliance is high.** Participants actually follow the protocol. Partial compliance is the worst case — participants incur coordination overhead without realizing the coordination benefit, because protocols that depend on joint action only pay off when everyone acts together.
2. **Infrastructure is reliable.** The AI tool is accessible, fast, and doesn't fail during the coordinated session. Infrastructure failure compounds with coordination overhead: when the tool fails mid-protocol, it disrupts the coordinated procedure as well as the work, and the protocol has no fallback.
3. **The task requires cross-perspective integration.** The perspectives that the protocol forces together genuinely diverge enough that integration adds value the individuals couldn't produce alone. If perspectives are similar, coordination is pure overhead.

### When behavioral scaffolding harms

- When *any* of the three conditions above fails — and the study's task/infrastructure/compliance profile failed on infrastructure reliability and possibly on cross-perspective divergence.
- The harm is not "no effect" but *worse than baseline*: coordination overhead is paid regardless, but the benefit requires all three conditions. Failure compounds because the protocol is a fixed cost that only pays off under specific conditions.

### When cognitive scaffolding helps

Cognitive scaffolds help when **all three** conditions hold:

1. **The task benefits from iterative refinement.** A single-shot interaction underutilizes the AI; iterating on drafts produces better output. If the task is genuinely one-shot (e.g., a lookup), the reframe adds nothing.
2. **The default interaction underutilizes AI capabilities.** Participants are treating the AI as a search engine or autocomplete when it could function as a thought partner. The reframe unlocks capabilities they weren't using.
3. **The shift toward dialogue doesn't impose excessive cognitive load.** Participants can evaluate AI output quality (the "supervisor" can judge the "intern's" work) and conversational iteration is practical for the task context.

### When cognitive scaffolding is null or weak

- When the default interaction already uses the AI well (no underutilization to correct — the reframe adds nothing).
- When participants can't evaluate output (cognitive load from the dialogue shift is excessive).
- Under ceiling effects: if most participants already score at the top (the study's 68% at 20/20), there is little variance for the reframe to move, and continuous ITT will be null even if the scaffold helps the remaining tail.

---

## 6. Key Results Table

| Outcome | Scaffold | Result | Interpretation |
|---|---|---|---|
| Document quality (Task A, pair) | Behavioral | b = −4.96, p < .001, d = 0.81 | Large *harmful* effect; coordinated pairs produced worse documents |
| Document production (Task A, pair) | Behavioral | OR = 0.12 | Substantially lower odds of producing a document at all; protocol overhead blocked completion |
| Top-quality individual work (Task B) | Cognitive | OR = 2.07, p = .022 (exploratory binary) | Higher odds of top-quality output; exploratory, not pre-registered |
| Top-quality individual work (Task B) | Cognitive | p = .223 (continuous ITT) | Null in pre-registered continuous model; likely attenuated by ceiling effect (68% scored 20/20) |
| Belief change — Exploration & Experimentation | Cognitive | BH-adjusted p = .013 | Greater positive belief change; interpreted as recovery from carry-over, not durable training effect |
| Belief change (ANCOVA) | Cognitive | p = .223 (null) | ANCOVA null supports the carry-over interpretation over a standalone effect |

**Reading the table:** the behavioral scaffold's negative results are the robust, pre-registered findings. The cognitive scaffold's positive results are *exploratory* (binary model, belief change) and must be read against the null continuous ITT and ANCOVA. The framework (§5) is the durable, generalizable contribution; the specific effect sizes are provisional pending replication.

---

## 7. Robustness and Sensitivity Analyses

### Lee trimming bounds for attrition

Attrition was addressed using **Lee trimming bounds** — a non-parametric approach that bounds the treatment effect under worst-case attrition assumptions (the attrition is either all from the best outcomes or all from the worst). The bounds provide a range within which the true effect must lie regardless of why participants dropped out.

**Implication for interpretation:** the point estimates (b = −4.96, OR = 0.12, OR = 2.07) should be read against the Lee bounds, not as precise values. If the behavioral harm holds even under the favorable bound, the finding is robust to attrition; if the cognitive benefit only holds at the point estimate but not under the conservative bound, it is attrition-sensitive. The SKILL.md treats the behavioral harm as robust and the cognitive benefit as provisional, consistent with how the authors frame the robustness.

### Word-count sensitivity analysis

Results were **sensitive to document length**. The behavioral arm produced shorter documents, and the quality gap may partly reflect length rather than per-word quality. This raises a mechanistic question: did the "Create-Out-Loud" protocol reduce quality *directly* (less time for substance because time went to coordination), or *indirectly* (shorter documents score lower because they have less content)?

**Implication:** the behavioral harm is real either way (shorter, fewer documents is itself a harm), but the *mechanism* matters for the framework. If the harm is purely length-mediated, then a protocol that preserved length might avoid the quality hit — but the production collapse (OR = 0.12) suggests the protocol's overhead is the upstream cause, and length is a symptom, not an independent mediator. Treat the length sensitivity as a caveat on the effect-size precision, not a challenge to the directional finding.

### AM/PM session confound

Session timing (morning vs. afternoon) was not fully balanced across conditions, creating a potential confound: differences attributed to the scaffold could partly reflect time-of-day effects (fatigue, attention) or task-order effects rather than the intervention itself.

**Implication:** treat the effect sizes as provisional. The *direction* of the behavioral harm is consistent with the framework's mechanism (coordination cost > benefit under unreliable infrastructure), so the confound is unlikely to reverse the qualitative conclusion — but the magnitude could be inflated or deflated. Replication with balanced scheduling is needed for precise estimates.

---

## 8. Carry-Over Effects

Task A (pair-level) preceded Task B (individual-level), creating a carry-over pathway: Task B outcomes may be influenced by Task A exposure, including which scaffold was applied in Task A.

**The cognitive scaffold's belief-change signal as carry-over recovery:**
- The partnership-training group showed greater positive belief change on Exploration & Experimentation (BH-adjusted p = .013), which could be read as "the training worked."
- But the ANCOVA result was null (p = .223), and the authors interpret the belief-change signal as **recovery from carry-over** — i.e., Task A (under the behavioral scaffold or control) may have *depressed* beliefs, and the cognitive training in the Task B window helped participants *recover* from that depression rather than independently boosting beliefs above baseline.
- This is a more cautious and mechanistically coherent reading: the training counteracted a negative spillover rather than producing a durable positive shift. The implication for practice is that partnership training may be most valuable as a *corrective* intervention (after a demoralizing collaborative experience) rather than as a standalone inoculation.

**Implication for the SKILL.md:** the cognitive scaffold's belief-change finding is treated as exploratory and carry-over-contaminated, not as established. The positive quality result (OR = 2.07) is also exploratory (binary model) and read against the null continuous ITT. The framework conditions (§5) are the durable takeaway; the cognitive scaffold's specific positive effects are promising but unconfirmed.

---

## 9. Limitations

- **Single organization.** Gap Inc. is one Fortune 500 retailer; organizational culture, task type (retail-context documents), and workforce composition limit generalization to coding, design, engineering, or other domains.
- **Single task type.** The documents produced may not represent the range of tasks where scaffolds matter. The framework's conditions (§5) are the authors' generalization beyond their specific task, but they are theory-derived, not yet empirically validated across task types.
- **Ceiling effects.** 68% of participants scored 20/20 on the individual task, compressing variance and likely attenuating the continuous ITT for the cognitive scaffold. The binary top-quality result isolates the tail where variance remained, but it is exploratory.
- **AM/PM and order confounds.** Unbalanced session timing and fixed task order (A then B) confound the intervention effects with time-of-day and carry-over.
- **Attrition.** Addressed with Lee trimming bounds, but the point estimates should be read against the bounds, not as precise values.
- **Word-count sensitivity.** The behavioral quality effect is partly length-mediated; the mechanism (overhead → shorter documents → lower scores) vs. (overhead → lower per-word quality) is not fully disentangled.
- **Carry-over.** Task B outcomes are contaminated by Task A exposure; the cognitive scaffold's belief-change signal is best read as carry-over recovery, not a standalone effect.
- **Exploratory vs. pre-registered.** The cognitive scaffold's positive results (binary quality, belief change) are exploratory; the pre-registered continuous ITT was null. The behavioral scaffold's negative results are pre-registered and robust.

**Net reading:** the behavioral scaffold's *harm* is the robust, generalizable finding (direction consistent with the framework mechanism, pre-registered, robust to attrition bounds). The cognitive scaffold's *benefit* is promising but provisional (exploratory, ceiling-attenuated, carry-over-contaminated). The *framework* — the conditional logic for when each scaffold helps vs. harms — is the most transferable contribution and the basis for the SKILL.md workflow.

---

## 10. Cross-References to Existing Skills

| This skill's concept | Related skill | Relationship |
|---|---|---|
| Behavioral scaffold = structured interaction protocol | `ai-collaboration-friction-patterns` | Garg's five patterns (Knowledge Priming, Design-First, Context Anchoring, Encoding Standards, Feedback Flywheel) and the Lattice framework are *behavioral* scaffolds — structured protocols for human-AI collaboration. This skill supplies the evidence that such protocols can backfire when coordination costs exceed benefits or infrastructure is unreliable. Garg's patterns are lighter-weight (per-task, not mandatory-pair) and may avoid the worst of the "Create-Out-Loud" failure, but the same gating conditions apply: compliance, infrastructure, cross-perspective value. |
| "Smart intern" reframe → iterative dialogue | `prompt-wait-evaluate-flow-collapse` | The cognitive scaffold's shift to iterative dialogue can either mitigate the prompt-wait-evaluate loop (by making each exchange part of a deliberate refinement rather than a slot-machine pull) or worsen it (if the dialogue shift adds cognitive load on top of the loop's interruptions). The framework's "excessive cognitive load" condition is the discriminator. |
| Behavioral coordination overhead compounds flow fragmentation | `surge-flow-state-successor` | The "Create-Out-Loud" protocol adds verbalized coordination to an already-interrupting prompt-wait-evaluate loop, compounding the surge-state fragmentation. Cognitive scaffolding that shifts defaults without adding per-step overhead is the less flow-disruptive choice. |
| Behavioral vs. cognitive choice for AI partnership design | `agent-experience-ax-devex-evolution` | AX designs the *agent-side* environment (context, tools, verification, safety). This skill designs the *human-side* intervention. The two are complementary: a well-designed agent environment reduces the infrastructure-reliability condition that gates behavioral scaffolding, potentially making behavioral scaffolds viable where they would otherwise fail. |
| Cognitive scaffold supports exploration capacity | `developer-ai-ambidexterity-shift` | The "smart intern" reframe induces iterative refinement, which is an exploration behavior. Behavioral mandates that reduce production (OR = 0.12) can undermine the exploration capacity this skill describes. The cognitive scaffold is the ambidexterity-compatible choice; the behavioral scaffold, when it fails, is ambidexterity-hostile. |
| Coordination cost as a friction pattern | `ai-collaboration-friction-patterns` | The "frustration loop" and "speed trap" in Garg's framework are individual-level frictions; the "Create-Out-Loud" failure is a *coordination-level* friction — a different and under-documented class that this skill adds. |

---

## 11. A-Tech Alignment

| A-Tech value | Alignment |
|---|---|
| **Open-source AI** | The study is vendor-neutral (Microsoft researchers studying a retailer using general AI tools). The scaffold framework applies regardless of model provider; the cognitive "smart intern" reframe works with any open-weight model. A-Tech can teach the framework without vendor lock-in. |
| **Data privacy** | The field experiment used organizational document tasks; no sensitive personal data is implicated in the framework itself. The cognitive scaffold's "iterate and critique" default supports privacy-preserving local AI use (you review before accepting), whereas behavioral mandates requiring shared AI sessions may pressure cloud-tool adoption. |
| **Financial freedom** | Choosing the right scaffold type avoids the cost of failed interventions — the behavioral scaffold's harm (lower quality, lower production) is a direct productivity loss. The framework helps organizations avoid spending on coordination mandates that backfire. |
| **Practical implementation** | The decision matrix (SKILL.md §5) is directly actionable: assess task type, infrastructure, compliance, and cognitive load, then choose. The "Create-Out-Loud backfired" case is a concrete cautionary example organizations can use to pressure-test their own intervention plans. |
| **Developer sovereignty** | The cognitive scaffold preserves developer agency — the reframe changes how you *choose* to interact, rather than mandating a procedure. The behavioral scaffold, by contrast, overrides individual discretion with a protocol, which is part of why its failure is so costly (you lose autonomy *and* quality). The framework's default-toward-cognitive heuristic aligns with developer sovereignty. |

---

## Novelty confirmation

The behavioral-vs-cognitive scaffolding distinction, with empirical evidence that a behavioral scaffold can produce *worse* output than baseline (d = 0.81 harm, OR = 0.12 production collapse), is not represented in the existing skills corpus. Adjacent skills (`ai-collaboration-friction-patterns`, `prompt-wait-evaluate-flow-collapse`, `agent-experience-ax-devex-evolution`) address interaction quality, flow disruption, and agent-environment design respectively, but none names the scaffold-type distinction or supplies the conditional framework for when each type helps vs. harms. This skill adds the human-side intervention layer and the coordination-cost failure mode that those skills do not cover.