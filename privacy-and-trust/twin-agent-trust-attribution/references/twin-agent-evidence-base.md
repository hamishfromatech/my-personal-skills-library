# Twin Agent Trust Attribution — Evidence Base

## Primary Source

### Andersson, H. & Elmqvist, N. (May 19, 2026) — "From Role to Person: Trust Calibration Challenges in Twin Agents"

Aarhus University, Denmark. arXiv:2605.19838v1 [cs.HC]. Presented at AutomationXP26 Workshop, 2026 CHI Conference on Human Factors in Computing Systems, Barcelona, Spain.

## Key Definitions

**Twin agent:** A social AI agent grounded in the communicative and epistemic profile of a specific real individual, constructed to represent rather than replace them. A professional representation reflecting how someone thinks, communicates, and engages with colleagues. Active in contexts where the real person is absent. Should never impersonate that person or commit actions on their behalf.

**Digital twin (for contrast):** Computational replicas of physical systems used for simulation and monitoring in engineering. Strive for maximum replication fidelity; passive models queried by other systems.

**The "twin" framing:** Twins share origin and close resemblance but are clearly not the same entity. A twin agent is not a copy or clone; it is a related but separate presence. The gap between them is where the trust problem lives.

## The Threefold Attribution Ambiguity

When a colleague's twin agent tells you something, you are receiving what is framed as your colleague's knowledge and perspective. The trust you extend is not trust in a model; it is trust in a person, mediated by a model. When a twin agent's output causes doubt, the colleague faces three simultaneous explanations:

1. **Schema gap:** The agent's representation of the person is incomplete or inaccurate.
2. **Epistemic gap:** The user simply does not know the person's actual view on this matter.
3. **Model artifact:** The output reflects an LLM failure such as hallucination, sycophancy, or drift.

The schema gap and epistemic gap may feel identical (both surface as uncertainty about what the person thinks) but originate in different places: one in the agent's model of the person, the other in the colleague's own knowledge of them. No reliable attribution path exists from the outside.

## Why Cognitive Forcing Functions Fail

Cognitive forcing functions (Buçinca et al. 2021) address overreliance effectively when a clear boundary exists between the AI and the human decision-maker. Twin agents dissolve that boundary.

- Existing interventions assume a socially neutral relationship between user and system.
- Twin agents introduce a pre-existing relationship: the colleague is navigating a relationship with a real person, not evaluating a model.
- The social cost of expressing doubt is itself a variable.
- Interventions that work between a user and an anonymous AI system may be ineffective or counterproductive when doubt implicates a real colleague.

## The Structurally Unlearnable Error Boundary

Bansal et al. (2019): effective human-AI teaming depends on humans forming an accurate mental model of the AI's error boundary (conditions under which it fails). That model is learnable only when ground truth feedback is available.

Twin agents break this assumption:
- The relevant "error" is not a task failure with a verifiable outcome but a misrepresentation of a person.
- Colleagues have no independent access to the real person's views to calibrate against.
- Every interaction with the agent is simultaneously data about the agent AND data about the user.
- The error boundary is structurally unlearnable.

## The Research Agenda (Three Directions)

### Direction 1: Interface design — layered transparency
- Knowing a message came from a twin agent is insufficient for calibration if the recipient cannot tell whether the agent is synthesizing an inferred view or relaying something the person actually authored.
- A finer transparency layer distinguishing **reconstructed inference** from **direct relay** gives colleagues a more actionable basis for trust than a generic disclosure badge.

### Direction 2: Attribution legibility — epistemic provenance
- When a twin agent's output is traceable to a specific artifact authored by the represented person (design document, stated position, prior communication), surfacing that source transforms the schema gap from invisible to inspectable.
- A twin agent that can say "this reflects what she wrote in the project brief" is meaningfully different from one that cannot.
- Epistemic provenance is a design resource, not merely a logging concern.

### Direction 3: Relational dimension — new class of intervention
- Interventions must operate at the relational level: making visible when an agent is operating beyond the range of its documented representation.
- Creating low-cost pathways for colleagues to surface unresolved uncertainty back to the real person.
- The goal: calibrated relational trust that accounts for epistemic uncertainty while preserving the social value that makes twin agents worth deploying.

## Related Work

- **Park et al. (2023):** Generative agents — interactive simulacra of human behavior. LLM agents sustain contextually consistent, socially believable behavior over extended interactions. Agents represent fictional characters, not specific real individuals.
- **Park et al. (2024):** Generative agent simulations of 1,000 people. Agents trained on interviews with real people can replicate their attitudes and behaviors with measurable accuracy. Establishes feasibility of person-specific simulation at scale.
- **Shaikh et al. (2025):** GUM — Creating general user models from computer use. Rich, confidence-weighted models of specific individuals from passive observation of behavioral traces (screen recordings, communications). Detailed person-models inferred without explicit elicitation.
- **Cheng et al. (2025):** Conversational agents on your behalf during multitasking. Examines shared autonomy and appropriate delegation in voice communication.
- **Hu et al. (2026):** When your boss is an AI bot. AI systems replicating managerial personas. Design workshops found higher fidelity may unsettle rather than reassure.
- **Gani (2025):** Klarna's AI CEO picks up customer hotline. Early organizational deployment of persona-representing agents.
- **The Simile Team (2026):** The simulation company. Commercial translation of person-specific simulation research.
- **Nass & Moon (2000):** Computers Are Social Actors (CASA). People automatically apply social heuristics to machines regardless of awareness.
- **Jakesch et al. (2023):** Human heuristics for AI-generated language are flawed. People cannot reliably detect AI-generated text; well-calibrated systems produce text perceived as more natural than human-written output.
- **Buçinca et al. (2021):** Cognitive forcing functions can reduce overreliance in AI-assisted decision-making. Only effective when legible boundary exists between AI and human.
- **Bansal et al. (2019):** Beyond accuracy — the role of mental models in human-AI team performance. Effective teaming depends on accurate mental model of AI's error boundary; learnable only with ground truth feedback.

## Motivation

Expertise is increasingly decoupled from physical presence but remains coupled to availability. While distributed work has normalized asynchronous collaboration, the knowledge, judgment, and perspective of a specific person still require that person to be reachable. Twin agents are a response to that constraint. The potential: fundamentally changing how expertise travels through organizations, making knowledge and judgment available in ways current tools cannot approximate. Twin agents are an emerging reality that demands HCI attention before the design space closes around assumptions that have not yet been examined.

## The Represented Person's Perspective

The paper notes that twin agents also raise important questions about agency, consent, and identity rights of the person being modeled. What it means for a professional identity to be represented, approximated, and potentially distorted without one's continuous involvement is a distinct and equally pressing problem. The paper treats the represented person's perspective as a companion research question.

## Source Bibliography

1. Andersson, H. & Elmqvist, N. (2026) — "From Role to Person: Trust Calibration Challenges in Twin Agents." arXiv:2605.19838v1. Aarhus University.
2. Park, J.S. et al. (2023) — Generative agents: interactive simulacra of human behavior. UIST.
3. Park, J.S. et al. (2024) — Generative agent simulations of 1,000 people.
4. Shaikh, O. et al. (2025) — Creating general user models from computer use. UIST.
5. Cheng, Y.F. et al. (2025) — Conversational agents on your behalf. CHI.
6. Hu, Q. et al. (2026) — When your boss is an AI bot. CHI.
7. Gani, A.S. (2025) — Klarna's AI CEO. Bloomberg Tech In Depth.
8. The Simile Team (2026) — The simulation company.
9. Nass, C. & Moon, Y. (2000) — Machines and mindlessness: social responses to computers. Journal of Social Issues 56(1).
10. Jakesch, M. et al. (2023) — Human heuristics for AI-generated language are flawed. PNAS 120(11).
11. Buçinca, Z. et al. (2021) — To trust or to think: cognitive forcing functions. PACMHCI.
12. Bansal, G. et al. (2019) — Beyond accuracy: mental models in human-AI team performance. HCOMP.
13. Ehsan, U. & Riedl, M.O. (2020) — Human-centered explainable AI: reflective sociotechnical approach. SafeAI Workshop.