# AI Explanation Ability-Cue Trap — Evidence Base

## Source

Saßmannshausen, T., Burggräf, P., Hassenzahl, M., & Sauer, C.R. (2026). "Effects of AI explanations on trust and reliance: a study in job shop scheduling." *Ergonomics*, published online March 11, 2026, 1-22. DOI: 10.1080/00140139.2026.2634115. PMID: 41811355.

**Affiliations:** International Production Engineering and Management, University of Siegen, Germany; Ubiquitous Design, University of Siegen, Germany.

**License / access:** Published in *Ergonomics* (Taylor & Francis). Abstract and plain-language summary available via PubMed (PMID 41811355); full text via Atypon.

## Study Design

- **Design:** Between-subjects online experiment in flexible job shop scheduling (FJSS).
- **N:** 253 professionals and graduate engineers.
- **Conditions:** Two AI schedulers were compared while holding the actual *recommendations identical* — isolating the effect of the explanation itself:
  1. **DRL with rationales** — a deep reinforcement learning scheduler augmented with natural-language rationales (brief explanations of why it recommended what it did).
  2. **Transparent FIFO heuristic** — a first-in-first-out scheduling rule (fully transparent, deterministic logic; no AI "explanation" needed because the rule is self-evident).
- **Task:** A five-step, path-dependent scheduling task. Each step the participant decides whether to follow the AI's recommendation or override it.
- **Key methodological strength:** Holding recommendations identical across conditions means any difference in trust or reliance is attributable to the *explanation*, not to the quality of the recommendations.

## Key Findings

### 1. Reliance increased with rationales
Participants relied more on the DRL-with-rationales scheduler than on the transparent FIFO scheduler, despite identical recommendations.

### 2. Attitudinal trust did NOT differ directly
Self-reported (attitudinal) trust did not differ significantly between the two conditions. Rationales did not make users *say* they trusted the AI more.

### 3. Trust increased through mediation of perceived ability
The pathway was: rationales → **perceived ability** ("the AI seems competent") → attitudinal trust → reliance. Trust rose *because* the rationales made the AI seem more capable, not because users understood the recommendations better.

### 4. Understanding was not a supported mediator
The data did not support a rationales → understanding → trust pathway. The explanation functioned as an ability signal, not a comprehension-building mechanism.

### 5. Task difficulty moderated the trust effect (not reliance)
The indirect trust effect (via perceived ability) was **smaller on harder tasks**. Reliance, however, was **not moderated** by task difficulty — participants relied more on the explained AI regardless of difficulty.

### 6. Domain expertise moderated the trust effect
The indirect trust effect was **larger among domain experts**. Experts were more, not less, susceptible to the ability-cue-driven trust increase.

## Abstract (verbatim, from PubMed)

"Trust calibration is critical for productive human-AI collaboration in production management, yet the impact of brief explanations on trust and reliance for black-box schedulers remains unclear. We conducted a between-subjects online experiment in flexible job shop scheduling (FJSS; N = 253 professionals and graduate engineers), comparing a deep reinforcement learning (DRL) scheduler augmented with natural-language rationales to a transparent first-in-first-out (FIFO) heuristic while holding recommendations identical to isolate explanation effects. In a five-step, path-dependent scheduling task, participants relied more on DRL with rationales, but attitudinal trust did not differ. Instead, trust increased through mediation of perceived ability. This indirect trust effect was smaller on harder tasks and larger among domain experts, whereas reliance was not moderated. Practically, short rationales function primarily as ability cues that raise adoption without necessarily deepening understanding. To maintain calibrated trust, these cues should be complemented with audience- and task-specific guidance on uncertainty and limitations."

**Keywords:** Artificial intelligence; explainable AI; human-AI collaboration; trust in AI.

## Plain-Language Summary (verbatim, from PubMed)

"Practitioner Summary: Designers of intelligent scheduling systems should consider the effects of explanations more. Brief rationales serve as ability cues but do not support understanding. This could be solved by providing detailed, context-specific explanations, noting that explanations are not only beneficial to trust but can also be harmful."

## The Ability-Cue Mechanism (Synthesis)

The study isolates a mechanism that the broader XAI literature has hinted at but not cleanly demonstrated: brief explanations raise trust and reliance by signaling the AI's *competence* (perceived ability), not by transferring *understanding* to the user. The user reads a fluent rationale, infers "this AI knows what it's doing," and follows the recommendation — without necessarily being able to verify whether the recommendation is actually correct.

This is distinct from:
- **Confidence scores** (which signal statistical certainty) — the ability cue is a *qualitative* competence signal, not a quantitative one.
- **Reasoning traces** (which show the steps) — a brief rationale is a *summary* of reasoning, not the reasoning itself, which is why it signals ability without building understanding.
- **Genuine explanations** (which are detailed, context-specific, and include uncertainty) — these are the proposed remedy, not the trap.

## Boundary Conditions — Summary Table

| Boundary condition | What moderates | Effect | Interpretation |
|---|---|---|---|
| Task difficulty | The trust effect (via ability) | Smaller on harder tasks | On hard tasks, the gap between brief rationale and real understanding is visible; ability signal weakens |
| Task difficulty | Reliance | Not moderated | Participants rely more on explained AI regardless of difficulty — the reliance effect is robust |
| Domain expertise | The trust effect (via ability) | Larger for experts | Experts parse surface plausibility fluently; fluency itself signals ability to them (fluency heuristic) |
| Attitudinal vs. behavioral trust | The measurement | Decoupled | Reliance rises without attitudinal trust rising directly — surveys will miss it |

## Connection to the Harmful-Explanation Literature

The Saßmannshausen finding joins a body of evidence that explanations can *reduce* calibration when they signal competence without delivering understanding:

- **Buçinca et al. (2021)** — "To trust or to think: cognitive forcing functions." Cognitive forcing functions (requiring the user to think before accepting) are effective only with a legible AI-human boundary; explanations can increase reliance on wrong AI outputs.
- **Bansal et al. (2019)** — "Beyond accuracy: mental models in human-AI team performance." The error boundary (knowing when the AI is right vs. wrong) is learnable only with ground truth. Without it, explanations build false confidence. This is the structurally unlearnable error boundary also central to the `twin-agent-trust-attribution` skill.
- **The fluency heuristic** (Alter & Oppenheimer, 2009) — Fluent processing is interpreted as familiarity/truth. A fluent rationale is processed easily → judged as competent → trusted. This is the cognitive mechanism underlying the expert-susceptibility finding.

The convergence: the XAI movement's most common intervention (append a short rationale) is precisely the form most likely to manufacture unearned trust, because it is fluent enough to signal ability but too shallow to build understanding.

## Relationship to A-Tech Skill Library

- **`trust-calibration-ux-pattern`** — the five-move trust calibration framework warns about vanity trust scores and unearned trust. This skill explains *why* brief explanations produce unearned trust: the ability-cue mechanism. The vanity trust score anti-pattern (trust rising with usage not accuracy) is a structural cousin of the ability-cue trap (trust rising with explanation fluency not understanding).
- **`chain-of-thought-ux-reasoning-transparency`** — reasoning traces are a deeper explanation form. When detailed, they are less susceptible to the ability-cue trap (they show the work, not just the claim). But brief CoT summaries can still fall into the trap.
- **`deferred-trust-ai-selection`** — the finding that higher SES / lower tech use predicts higher AI trust, and that fluency lowers vigilance, is a generalization of the ability-cue mechanism. The expert-susceptibility finding here is a specific instance.
- **`cognitive-surrender-defense`** — the BRACED framework (Socratic habits) counters the ability-cue trap by forcing the user to surface assumptions and seek disconfirming evidence before accepting an explained recommendation.
- **`scaffolded-cognitive-friction`** — desirable friction (comprehension checkpoints, confidence boundary flagging) is the structural counter: it inserts verification between the ability cue and the acceptance.
- **`proof-first-ux-accountability`** — evidence-before-output inverts the ability cue: instead of the AI claiming competence via a rationale, the AI shows evidence first.
- **`cognitive-fluency-trust-engine`** — the fluency-competence heuristic that underlies the ability cue. This skill names the mechanism; the fluency skill explains the cognitive root.

## Limitations of the Study

- Single domain (job shop scheduling) — generalization to other domains (coding, medical, financial) should be tested.
- Between-subjects — each participant saw only one condition; cannot measure within-person calibration change over time.
- Brief rationales only — the study did not test detailed/context-specific explanations (the proposed remedy); the harmful-explanation finding is specific to the brief form.
- Recommendations held identical — real-world recommendations vary in quality; the ability-cue trap may compound when recommendations are also actually good (trust rises for the wrong reason and is never disconfirmed).

## Citation

Saßmannshausen, T., Burggräf, P., Hassenzahl, M., & Sauer, C.R. (2026). Effects of AI explanations on trust and reliance: a study in job shop scheduling. *Ergonomics*, 1-22. DOI: 10.1080/00140139.2026.2634115. PMID: 41811355.