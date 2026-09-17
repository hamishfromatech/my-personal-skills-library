# Agentic Cognitive Engagement Decline — Evidence Base

## Primary Source
**Catalan, C. R., Dizon, L. M., Monderin, P. N., & Kuang, E. (2026).** "'I'm Not Reading All of That': Understanding Software Engineers' Level of Cognitive Engagement with Agentic Coding Assistants." (CHI Workshop, 2026)

### Study Design
- **4 software engineers** with varying experience (less than 1 year to 10+ years)
- **Tool:** Cline (open-source agentic coding assistant)
- **Task:** Code generation task from DevGPT dataset (Excel file processing script)
- **Framework:** Bloom's Taxonomy (Remember → Understand → Analyze → Evaluate)
- **Measurement:** Self-report survey immediately after task + behavioral observation + thematic analysis

### Key Findings

#### Finding 1: Cognitive Engagement Declines as Task Progresses
- **Planning phase:** All participants actively engaged — comprehending prompt, verifying files, reading ACA's plan, answering clarifying questions (germane cognitive load)
- **Execution phase:** Engagement decreased; participants looked away from screen; only returned when ACA provided results. P4: "I'm not reading all of that." P2, P3 prompted for next steps despite unprocessed information on screen. Text-only communication amplified extraneous cognitive load.
- **Evaluation phase:** Minimal cognitive resources; participants checked output not process. "It generated my desired output" (P1), "I trust Cline" (P2), "It worked" (P3). Greedy allocation strategy: lowest-effort checks that confirm task completion; deeper reasoning only when surface-level validation fails.

#### Finding 2: Happy-Path Focus
- **Recall:** None correctly answered how many functions the generated script had
- **Understand:** Only half could summarize the first function
- **Analyze:** Only half felt confident about edge-case handling
- **Evaluate:** All evaluated only the happy path; none reviewed the underlying process

#### Risk Amplification
- LLMs remain prone to hallucinations (Gao et al., 2022) and algorithmic biases (Mbalaka, 2023)
- Poisoning and backdoor attacks (Aghakhani et al., 2024; Schuster et al., 2021) could inject malicious code that happy-path evaluation would miss
- "Participants' evaluations may have been too narrow in scope, potentially overlooking unexpected or edge-case scenarios"

## Supporting Research

### Cognitive Forcing Functions
- **Buçinca et al. (2021, CSCW):** Cognitive forcing functions reduce overreliance on AI in AI-assisted decision-making
- **Ghosh et al. (2026, arXiv:2601.18033):** Experimental comparison of cognitive forcing functions for execution plans in AI-assisted writing — effects on trust, overreliance, and perceived critical thinking
- **Park et al. (2019, CSCW):** A slow algorithm improves users' assessments of the algorithm's accuracy — "slowing down" the AI's decision greatly increases user accuracy
- **Kuang et al. (2024, CHI):** In usability testing, AI suggestions shown only after user critically analyzed the demonstration → improved perception of efficiency and trust

### Multimodal Communication Research
- **Liew et al. (2025, Human Behavior and Emerging Technologies):** Multiple AI voice technologies in learning systems → significantly lower perceived cognitive load, improved retention and recall
- **Beege & Schneider (2023):** Emotional design of pedagogical agents — enthusiasm and model-observer similarity
- **Liew et al. (2020, Information and Learning Sciences):** Speaker's voice enthusiasm affects social cue, cognitive load, and transfer in multimedia learning
- **Wang et al. (2006, IUI):** Two talking heads better than one in e-learning

### Cognitive Load Theory
- **Sweller et al. (1998):** Three types of cognitive load — intrinsic, extraneous, germane
- **Krieglstein et al. (2022):** Systematic meta-analysis of reliability and validity of subjective cognitive load questionnaires
- **Mutlu-Bayraktar et al. (2019):** Cognitive load in multimedia learning environments — systematic review

### Dual-Process Theory
- **Kahneman (2011):** System 1 (fast, intuitive) vs System 2 (slow, analytical)
- **Wason & Evans (1974):** Dual processes in reasoning
- Participants employed System 1 shortcuts (analyzing the happy path) to minimize cognitive resources; designing systems that encourage System 2 thinking is a long-standing challenge

## Implications for A-Tech

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | Cline is open-source; the study notes this "affords us the flexibility to implement prototypes that encourage active user engagement for future work" |
| **Practical implementation** | Cognitive forcing designs are concrete, implementable interventions (slow-down, delay, multimodal) with evidence of effectiveness |
| **Developer experience** | Addresses the deepest layer of DevEx — cognitive engagement — not just surface productivity metrics |
| **Data privacy** | Shallow evaluation of agent output is a security risk (missed backdoors/poisoning); deeper engagement is a privacy-adjacent safety measure |

## Future Research Directions
- Recruit more participants and add more open-ended tasks that reward cognitive engagement
- Use eye-tracking software to determine points of attention or lack thereof
- Deeper understanding of cognitive engagement beyond self-report surveys