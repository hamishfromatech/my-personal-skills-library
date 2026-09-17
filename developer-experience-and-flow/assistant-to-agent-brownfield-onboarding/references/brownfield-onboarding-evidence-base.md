# Assistant-to-Agent Brownfield Onboarding — Evidence Base

## Source
Appelt, Carmen Laura Janina & Glauben, Adrian (2026). "From Assistants to Agents: Exploring Efficiency and Human Agency in AI-Supported Programming." PACIS 2026 Proceedings, Paper 13. Technical University Darmstadt.

## Study Design

- **Participants:** 24 developers
- **Task:** Brownfield onboarding (working with an existing codebase)
- **Conditions:**
  - GitHub Copilot Ask (IDE-integrated assistant, conversational help)
  - GitHub Copilot Agent (LLM-based agent, autonomous code editing)
- **Measurement framework:** SPACE (productivity, perceived workload, interaction patterns, prompt behavior)
- **Publication:** PACIS 2026 (Pacific Asia Conference on Information Systems)

## Headline Results

| Metric | Result |
|--------|--------|
| Task completion time | -61.7% (agent is 61.7% faster) |
| NASA-TLX workload | -57.4% (agent reduces workload by 57.4%) |
| Code correctness | No significant improvement |
| Interaction pattern | Shift from active collaboration to passive supervision |

## The Core Finding: Active Collaboration → Passive Supervision

The interaction data revealed the most consequential pattern:

**With Copilot Ask (assistant):**
- Developers actively write code
- Use AI for suggestions, explanations, syntax help
- Remain in the loop: review, adapt, integrate
- Active collaboration pattern

**With Copilot Agent (agent):**
- Developers prompt and supervise
- Agent edits code directly
- Developer reviews the agent's output rather than writing code
- Shift to passive supervision pattern

**Why this matters:**
- Passive supervision reduces cognitive load (explains the 57.4% workload reduction)
- But passive supervision reduces developer engagement with the code
- Over time, this risks skill erosion: developers who supervise but do not write lose deep comprehension
- The productivity gain is real; the agency cost is real

## The Productivity-Agency Paradox

The study captures a paradox that is central to the 2026 agentic coding discourse:

1. **Productivity:** Agents deliver massive, measurable productivity gains (61.7% faster)
2. **Workload:** Agents dramatically reduce perceived workload (57.4% lower NASA-TLX)
3. **Correctness:** Code correctness does NOT improve — the agent is faster, not better
4. **Agency:** The human role shifts from creator to supervisor — raising over-reliance and skill-erosion concerns

This aligns with multiple existing findings in the skill ecosystem:
- **(Im)Paired Programming** (Balepour et al.): Agents improve productivity but harm comprehension (28% lower comprehension, p<0.002, d>0.80)
- **Cognitive engagement decline** (Catalan et al. CHI 2026): Engagement declines across planning→execution→evaluation phases
- **Comprehension debt** (multiple studies): Agent users score lower on code comprehension despite preferring agents

## SPACE Framework Application

| SPACE Dimension | Copilot Ask (Assistant) | Copilot Agent (Agent) |
|-----------------|-------------------------|----------------------|
| Satisfaction & well-being | Lower (more effort) | Higher (less effort) |
| Performance | Slower | 61.7% faster |
| Activity | Active coding | Passive supervision |
| Communication & collaboration | Active AI dialogue | Supervisory prompts |
| Efficiency & flow | Moderate | Higher (less interruption) |

## Brownfield vs Greenfield Distinction

This study is specifically about **brownfield onboarding** — working with an existing, unfamiliar codebase.

**Greenfield (new projects):**
- Developers build mental models from scratch
- Agents can scaffold without harming comprehension
- The mental model is co-constructed with the agent

**Brownfield (existing code):**
- Developers must understand existing code
- Agents that write code for them may prevent mental model formation
- The mental model must be reverse-engineered from agent output

**The risk for new hires:**
- New hires using agents for brownfield onboarding complete tasks faster
- But they may not develop deep codebase understanding
- This creates a comprehension debt that surfaces later during debugging or extension

## Interaction Pattern Taxonomy

| Pattern | Description | Comprehension Risk |
|---------|-------------|-------------------|
| Active collaboration | Developer writes, AI suggests | Low |
| Guided generation | Developer specifies, AI implements | Medium |
| Supervised generation | Developer prompts, agent implements, developer reviews | High |
| Passive supervision | Developer prompts, agent implements, developer accepts | Very high |
| Full autonomy | Developer delegates, agent completes without review | Extreme |

The study found that Copilot Agent use shifted developers rightward on this taxonomy — from "Active collaboration" toward "Supervised generation" or "Passive supervision."

## Comparison with Adjacent Research

### (Im)Paired Programming (Balepour et al., 2026)
- 54 students, agent vs chatbot comparison
- Agent users: 28% lower comprehension (p<0.002, d>0.80)
- Low-effort interaction (copy+paste, auto-accept) linked to lower comprehension
- Users prefer agents despite weaker understanding

**Relationship:** Balepour measures comprehension; Appelt & Glauben measure the interaction pattern shift that causes it. Together they explain WHY agents harm comprehension: the shift to passive supervision.

### Cognitive Engagement Decline (Catalan et al., CHI 2026)
- 4 software engineers, Cline agent
- Cognitive engagement declines across planning→execution→evaluation
- "I'm not reading all of that" — information overload during execution
- Focus on output evaluation rather than process evaluation

**Relationship:** Catalan explains the mechanism (engagement decline within a task); Appelt & Glauben explain the systemic pattern (shift from collaboration to supervision across tasks).

### Conversational Programming (Tang et al., 2026)
- 74,998 messages, 11,579 sessions, 1,300 repos
- Progressive specification, not upfront task description
- Cognitive work redistribution to AI

**Relationship:** Tang shows the collaborative pattern with assistants; Appelt & Glauben show how that pattern changes with agents.

### Three-Phase Evaluation (Strubel, 2026)
- Partial AI-assisted (Copilot) → AI-exclusive (Copilot) → AI-exclusive (AWS Kiro)
- Higher AI autonomy = reduced effort, improved adherence, lower workload
- Developer frustration increased modestly
- Tooling architecture influences outcomes independently of AI autonomy level

**Relationship:** Strubel confirms the productivity-workload tradeoff across increasing autonomy; Appelt & Glauben add the brownfield onboarding context and the interaction pattern analysis.

## A-Tech Values Alignment

- **Open-source AI:** The study uses GitHub Copilot (proprietary), but the findings apply to open-source agents (Cline, OpenCode) and open-weight models. The interaction patterns are tool-agnostic.
- **Data privacy:** Brownfield onboarding involves existing codebases — agent use must respect data privacy (on-device models like Muse Glimmer, Qwen3.8-27B enable privacy-preserving agent use)
- **Financial freedom:** The 61.7% time savings directly reduces onboarding cost for new hires; open-source agents (Cline) eliminate API costs
- **Practical implementation:** The guardrails (progressive autonomy, comprehension checkpoints, alternating agent/assistant use) are immediately implementable

## A-Tech Applications

### A-Coder
- Implement progressive agent autonomy for new-hire onboarding
- Comprehension checkpoints after agent-assisted tasks
- Alternate between agent mode (boilerplate, tests) and assistant mode (core logic)

### Be Practical
- Teach the productivity-agency tradeoff in the agentic coding curriculum
- The interaction pattern taxonomy as a self-assessment tool for developers
- The "no-agent session" protocol for skill maintenance

### Builder's Club
- Community guideline: share agent prompts AND comprehension self-assessments
- Mentorship pattern: experienced developers review agent output with new hires
- The brownfield-onboarding guardrails as a community onboarding protocol

## Composes With
- `agentic-cognitive-engagement-decline` — the within-task mechanism (engagement decline across phases)
- `coding-agent-comprehension-harm` — the comprehension measurement (28% lower comprehension)
- `ai-collaboration-friction-patterns` — interaction pattern taxonomy
- `cognitive-offloading-ladder` — the capability-retention framework
- `ai-adoption-calibration` — the four-phase onboarding sequence (verifiable/low-stakes → unverifiable/high-stakes)
- `developer-ai-ambidexterity-shift` — the role elevation from implementer to decision-maker