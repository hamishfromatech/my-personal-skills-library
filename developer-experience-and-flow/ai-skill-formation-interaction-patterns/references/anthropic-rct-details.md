# Anthropic RCT: AI Impact on Skill Formation — Detailed Reference

## Source

Shen, J. H. & Tamkin, A. (January 2026). "How AI Impacts Skill Formation." Anthropic. arXiv:2601.20245. Study conducted as part of the Anthropic Safety Fellows Program.

Published as "How AI assistance impacts the formation of coding skills" on anthropic.com/research.

## Study Design

### Participants
- 52 software engineers (mostly junior)
- Inclusion criteria: >1 year Python experience, code in Python at least weekly, have tried AI coding assistance, never used the Trio library
- Recruited via crowd-working platform; paid $150 flat rate
- Balanced across: years of coding experience, Python experience, asyncio usage, Python frequency, async familiarity score

### Task
- Learn Trio, a Python asynchronous programming library unfamiliar to all participants
- Two coding tasks (timer printing seconds while other functions run; record retrieval with error handling using memory channels)
- 35-minute time limit for both tasks
- Designed to mimic self-guided tutorial learning of a new library

### Conditions
- **Treatment (AI):** Access to AI assistant (GPT-4o, prompted as intelligent coding assistant) with access to participant's code; can produce complete correct code when prompted
- **Control (No AI):** Web search and instructions only; no AI assistance

### Evaluation
- 14-question quiz, 27 points total
- Question types: Debugging, Code Reading, Conceptual (Code Writing excluded to reduce syntax noise)
- Covers 7 core Trio concepts: async/await, starting Trio functions, error handling, coroutines, memory channels, nursery context managers, sequential vs concurrent execution
- Pre-registered grading rubric and analysis plan (OSF: https://osf.io/pk6a5)
- Designed using item response theory to ensure question quality

### Pilot Studies (4 conducted)
- Pilot A (n=39): 35% non-compliance in no-AI condition (participants used AI anyway)
- Pilot B (n=107): 25% non-compliance despite explicit warnings
- Pilot C (n=20): Local item dependence in quiz (participants guessed answers from other questions)
- Pilot D (n=20): Python syntax delays confounded results; reduced to 2 tasks; added syntax hints

## Results

### Main Study (n=52)
- **Quiz score:** AI group 50% vs No-AI group 67% (17% difference, ~2 grade points)
- **Effect size:** Cohen's d=0.738, p=0.01
- **Task completion time:** AI group ~2 minutes faster; NOT statistically significant
- **Controlling for warm-up task time:** Effect remains significant (d=0.725, p=0.016)
- **Across all experience levels:** No-AI group scored higher on average (effect not driven by any experience subgroup)
- **Question type breakdown:** Largest gap in debugging; smallest gap in code reading
- **Self-reported learning:** Control group reported higher learning (7-point scale); both groups reported high enjoyment
- **Task difficulty:** AI group found task easier; both found quiz similarly challenging

### Why No Significant Speed Gain
- Some AI participants spent up to 11 minutes (30% of total time) composing queries
- Some asked up to 15 queries
- Some spent up to 6 minutes composing a single query
- Median completion time in AI condition: 19 minutes
- The AI delegation subgroup (n=4, ~20% of AI group) finished much faster (19.5 min vs 23 min control), but this speed didn't extend to the full group

### Error Analysis
| Condition | Total errors (median) | Task 1 errors | Task 2 errors |
|-----------|----------------------|---------------|---------------|
| AI | 1.0 (0-3) | 0.0 (0-2) | 0.0 (0-1) |
| No AI | 3.0 (2-5) | 2.0 (0.5-3) | 2.0 (0.5-2) |

- 12 participants completed both tasks without errors; only 2 were in the control group
- No-AI group encountered more Trio-specific errors (TypeError, RuntimeWarning) that directly mapped to tested concepts
- NameError and AttributeError (typos) were common to both groups and not Trio-specific

### Code Adoption Pattern
- Direct paste (n=9): Fastest completion
- Manual copying (n=9): Similar speed to no-AI control
- Hybrid (n=4): Similar to manual copying
- Own code with clarification only (n=4): Relatively fast, high proficiency
- No notable quiz score difference between paste vs manual copy groups — cognitive effort matters more than raw time spent

## Qualitative Analysis Methodology

- Manually annotated screen recordings of 51/52 participants
- Events coded: Task Start, AI Interaction, AI Query, Websearch, Paste (Direct), Code Copying, Error, Interface Error, Task Completion, Task Submission
- Query types categorized: Explanation (79), Generation (51), Debugging (9), Capabilities (4), Appreciation (4)
- 21/25 AI participants asked explanation questions (high engagement)
- 16/25 used AI to generate code; 4 used only code generation with no other questions
- 3 of 8 lowest-scoring participants asked for code generation without explanations

## Participant Feedback (Selected)

### AI Group
- "By using the AI assistant, I feel like I got lazy. I didn't read the Trio library intro and code examples as closely as I would have otherwise."
- "I would not have minded going into a little more depth with the assistant to really understand... I feel like I got a pretty good overview but there are still a lot of gaps in my understanding."
- "I wish I'd taken the time to understand the explanations a bit more!"
- "I'm slow. I think the time limit made me act in a way that wasn't representative to my normal workflow, particularly in the proportion of time spent building mental models vs. obtaining code progressions."

### No-AI Group
- "The programming tasks were very fun and did a good job of helping me understand how Trio works despite never having used it before."
- "It was fun to learn about asynchronous programming, which I had not encountered before."

## Independent Corroboration: University of Maribor Study

Jošt, Taneski, and Karakatič (2024). University of Maribor. Applied Sciences.
- 10-week experiment, 32 undergraduate students learning React
- Significant negative correlations between LLM use for code generation/debugging and final grades
- LLM use for explanations showed no significant negative impact
- Authors concluded: explanation-focused LLM use "might not hinder, and could potentially aid, student performance"

This independent academic study, conducted in a different context (semester-long course vs. one-hour experiment, students vs. professionals, React vs. Trio), found the same pattern: the interaction mode (explanation vs. generation) determines learning outcome.

## Limitations (Acknowledged by Authors)

- Single task, chat-based interface (lower bound for cognitive offloading; agentic tools would be worse)
- Short-term measurement (quiz immediately after task; longitudinal skill development unknown)
- Crowd-worker participants (not identical incentive structure to real employment)
- No prompting skill measurement (self-reported only)
- No human-assistance counterfactual
- Small sample for qualitative pattern clusters

## Key Implications for Tool Designers

1. **The default interaction mode matters.** If "generate code" is the path of least resistance, most users will take it and lose learning. If "explain" is the default, learning is preserved.
2. **Error encounters are features, not bugs, during learning.** Tools that eliminate all errors during skill acquisition remove the learning mechanism.
3. **The comprehension checkpoint is the critical difference** between Generation-Then-Comprehension (high-scoring) and AI Delegation (low-scoring) — the only difference is the follow-up questions.
4. **Agentic coding tools will have a MORE pronounced negative effect** than chat-based assistants, because they require even less human participation. The authors explicitly state this.
5. **Debugging skill is the most at-risk capability** — and it's the most critical for AI code oversight.

*Reference: Shen, J.H. & Tamkin, A. (2026). "How AI Impacts Skill Formation." arXiv:2601.20245. Anthropic. Jošt, B., Taneski, J. & Karakatič, J. (2024). University of Maribor, Applied Sciences.*