# AI Review Fatigue — Evidence Base

## Primary Source

**"AI Writes Better Code. We're Getting Worse at Reviewing It."**
- Author: Patrick Hammond, Atomic Robot CTO
- Published: February 25, 2026 (atomicrobot.com/blog/ai-review-fatigue/)
- Contribution: Names "review fatigue" as the convergence of vigilance decrement, automation complacency, and context-switching costs in AI-assisted code review. Translates research from aviation, radiology, and cybersecurity into structural countermeasures for software engineering.

## Secondary Source

**"Verification Load and Fatigue with AI Coding Assistants"**
- Published: ACM (2026), DOI 10.1145/3772318.3791176
- Contribution: Controlled study isolating interface effects on verification load. N=60 participants solved three Python tasks with Inline, Chat, or Structured prompting, plus a no-AI control. Single LLM held fixed to isolate interface effects on fatigue.

## The Three-Mechanism Stack

### 1. Vigilance Decrement

- **Warm, Parasuraman & Matthews (2008):** "Vigilance requires hard mental work and is stressful." Sustained monitoring depletes attention resources.
- **Klein & Feltmate (2025):** Surveying 75 years of vigilance research, confirm performance drops after the first 30 minutes on task. Described as "one of the most robust findings in attention research."
- **Implication:** Most code review sessions exceed the 30-minute vigilance window.

### 2. Automation Complacency

- **Bainbridge (1983):** The irony of automation — the more reliable the automation, the less the human operator can contribute when it fails. The designer who tries to eliminate the human still leaves the human with the hardest tasks.
- **Parasuraman et al. (1993):** When automation was consistently reliable, operators detected only ~30% of automation errors. When the system sometimes failed visibly, detection jumped to ~75%.
- **McBride, Rogers & Fisk (2014):** Review of automation complacency research confirming the pattern.
- **Endsley (former Chief Scientist, U.S. Air Force):** The "automation conundrum" — the more automation is added and the more reliable it is, the less likely operators are to be aware of critical information and able to take over manually.
- **Implication:** The better AI gets, the worse we get at catching what it misses. The path to "AI good enough that review is unnecessary" is the most dangerous stretch.

### 3. Context Switching

- **Leroy (2009):** "Attention residue" — cognitive activity about a prior task that persists after switching. "It's like Windows staying open in our brains." People experiencing attention residue fail to notice errors and miss optimal solutions.
- **Wiehler et al. (2022, Current Biology):** Sustained high-demand cognitive work causes glutamate to accumulate in the lateral prefrontal cortex, making cognitive control literally more expensive to activate over time. This is neurochemistry, not willpower.
- **Parnin & Rugaber (2011):** Studied 10,000 recorded sessions from 86 programmers. Only 10% of sessions had coding activity begin in under a minute. In 93% of sessions, programmers navigated to other locations before editing — rebuilding the mental model they'd lost.
- **Implication:** AI-enabled workflows with parallel streams (auth, billing, infrastructure) multiply switching cost beyond traditional development.

## The Amplifier Loop

AI increases output volume → increases review volume → increases vigilance demand and context switches → degrades review quality → bugs slip through → but throughput metrics still go up. The erosion is invisible until something breaks.

## The Verification Gap

- **Perry, Srivastava, Kumar & Boneh (2023):** Developers with AI access wrote significantly less secure code than those without — yet were more likely to believe they had written secure code. Assistance increased confidence while decreasing quality.
- **Goddard, Roudsari & Wyatt (2012):** Systematic review of automation bias. Erroneous automated advice was followed at a 26% higher rate among groups using automated recommendations. Task inexperience correlated with increased automation bias errors.
- **Implication:** AI makes new domains accessible, but the verification challenge shifts into territory where you may not have the fluency to distinguish correct from plausible.

## Industry Case Studies (the cognitive mechanisms are domain-general)

### Aviation
- **Uber Tempe, Arizona (2018):** Self-driving vehicle struck and killed a pedestrian. NTSB found Uber lacked adequate mechanisms for addressing operator automation complacency. The safety driver's sole job was monitoring — complacency made the role useless.
- **FAA response:** Didn't tell pilots to pay more attention — mandated rest, checklists, and co-pilots. The solution was structural.

### Radiology
- **Drew, Vo & Wolfe (2013):** Embedded a gorilla (48× the size of a lung nodule) into CT scans. 83% of expert radiologists did not see it. Eye tracking showed over half looked directly at its location. When monitoring for one signal category, you become blind to others.

### Cybersecurity
- **Vectra AI (2023):** SOC analysts receive an average of 4,484 alerts daily. 67% go uninvestigated. 83% are false positives. Volume problem maps directly to AI code review.

## The Deeper Cost

Review isn't just QA — it's how you learn the system you're building. As Adam Toennis (principal engineer, Atomic Robot) puts it: the differentiator in AI-assisted workflow is "higher-level architectural decisions — being able to describe how data flows between all the layers and why." That knowledge comes from reviewing carefully enough to understand what was built and why. Skip the review → risk defects AND lose comprehension.

## The Recursive Automation Problem

Teams use AI to review AI-generated code — a pragmatic adaptation. But automating the review of automated output nests the human factors problem one level up. Bainbridge's irony of automation (1983) is recursive: now you're monitoring the monitor, and the same complacency dynamics apply.

## Structural Countermeasures (evidence-based)

| Countermeasure | Evidence |
|---|---|
| Breaks every 2 hours | NIOSH recommendation |
| Mandatory rest between positions | ATC regulation |
| Exposing operators to automation failures during training | Bahner et al. (2008, TU Berlin) — significantly decreased complacency, one of the few proven countermeasures |
| Slower, more deliberate procedures under time pressure | Aviation industry response to checklist-rushing accidents |
| Red-green-refactor discipline (see failure before success) | Software engineering practice aligned with Bahner et al. findings |
| Working memory ~4 slots | Cognitive science consensus — basis for WIP limits |

## Cross-References to Adjacent Skills

| Skill | Relationship |
|---|---|
| `coding-agent-decision-fatigue-mitigation` | Covers the decision-density crisis (decisions/hour). Review fatigue is the per-review manifestation of the verification burden. |
| `ai-fatigue-scale-design` | Provides the general measurement framework. Review fatigue is the developer-specific, per-session operationalization. |
| `ai-brain-fry-defense` | Covers acute overload from 4+ concurrent agents (BCG). Review fatigue is orthogonal — can occur with one agent. |
| `comprehension-debt-framework` | Covers the understanding debt. Review fatigue's "deeper cost" (eroded comprehension) is the mechanism. |
| `devex-verification-bottleneck-framework` | Covers the verification-time > writing-time inversion. Review fatigue is the human-factors dimension of that bottleneck. |
| `attention-residue-mitigation` | Covers the Leroy attention residue mechanism in general. Review fatigue is the code-review-specific application. |
| `mental-model-erosion-defense` | Covers long-term skill atrophy. Review fatigue is the acute, per-session degradation. |
| `beyond-vibe-agentic-engineering` | References review fatigue at the "Assisted" level of the autonomy spectrum. |
| `scaffolded-cognitive-friction` | Covers automation complacency in general AI interfaces. Review fatigue is the code-review-specific instance. |