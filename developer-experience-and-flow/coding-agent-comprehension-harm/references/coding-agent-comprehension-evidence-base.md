# Coding Agent Comprehension Harm — Evidence Base

## Primary Source

**Balepur, N., Baumler, J., Chen, X., Choi, Y., Rudinger, R., & Boyd-Graber, J. (2026).** "Coding Agents Can Hurt Comprehension: An Experimental Study." University of Maryland / New York University / Carnegie Mellon University, 2026.

---

## 1. Study Design

### Participants
- **N = 54** computer science students (undergraduate and graduate)
- Between-subjects design: participants randomly assigned to one of two AI assistance conditions
- Variation in prior coding background (measured via pre-task survey)

### Task
- Build a website implementing **"zic-zac-zoe"** — a variation of tic-tac-toe on a 3×3 grid with custom rules
- Task chosen to be tractable in a single session but complex enough to require meaningful code structure (HTML/CSS/JS)

### Two AI Conditions

| Condition | Tool | Behavior |
|---|---|---|
| **Agent (direct-edit)** | **Aider** (open-source AI pair programmer) | AI directly edits files in the repository; user reviews and accepts/rejects diffs |
| **Chatbot (guide)** | Generic chatbot (e.g., ChatGPT-style interface) | Provides high-level code snippets only; user must integrate the code themselves |

### Extension Task (No AI)
- After the initial build, all participants completed an **extension task without any AI assistance**
- Designed to measure whether users could modify and build upon the code they (or the AI) wrote
- Tests the dependency hypothesis: if the agent produced better initial code but the user understood it less, does the scaffolding advantage survive without the agent?

---

## 2. Comprehension Metrics

### Question Types (Bloom's Taxonomy Aligned)

| Level | Question Type | What It Measures |
|---|---|---|
| **Remember / Recall** | "What does function X do?" | Factual recall of code components |
| **Understand / Reasoning** | "If you changed parameter Y, what would happen to behavior Z?" | Causal reasoning about code behavior |

- Comprehension score = aggregate of recall + reasoning questions
- Questions specifically about the code the participant (or their AI) produced — not generic programming knowledge
- Scored on a 0–1 normalized scale

### Key Property
Initial task accuracy barely predicts comprehension (adjusted R² moves from 0.40 to 0.42 when added as a predictor). Completion and comprehension are **independent axes** — a tool can excel at one and fail at the other.

---

## 3. Key Results

### 3.1 Comprehension (Primary Finding)

| Metric | Agent Group | Chatbot Group | Effect |
|---|---|---|---|
| Comprehension score (overall) | Lower | Higher | **28% lower, p < 0.002, Cohen's d > 0.80** |
| Recall questions | Lower | Higher | Significant gap |
| Reasoning questions | Lower | Higher | Significant gap |

**Interpretation:** The comprehension gap is large (d > 0.80 is a "large" effect size) and consistent across question types. Agent users built working code but understood substantially less of what was built.

### 3.2 Initial Task Accuracy

| Metric | Agent Group | Chatbot Group | Effect |
|---|---|---|---|
| Initial task accuracy | Higher | Lower | Agent produces better scaffolding (d ≈ 1.4 / 1.2 on sub-metrics) |

The agent does what it's advertised to do: it produces higher-quality, more complete initial code. The problem is not the output quality — it's what happens to the user's understanding when the AI does the writing.

### 3.3 Extension Task Accuracy

| Metric | Agent Group | Chatbot Group | Effect |
|---|---|---|---|
| Extension task accuracy (no AI) | Statistically similar | Statistically similar | Not significant |

**Why "similar" is misleading — the extension-task trap:**
- Agent users had **higher-quality initial code** (positive mediator for extension accuracy)
- Agent users had **lower comprehension** (negative mediator for extension accuracy)
- The two opposing pathways **cancel out**, producing statistically similar extension accuracy
- Path mediation confirms: the indirect effect through comprehension is significant and **opposite in sign** to the effect through code quality
- **Implication:** The agent has traded understanding for scaffolding. The user is now dependent on the agent to extend the very code the agent produced. "Similar" extension performance hides a structural dependency.

### 3.4 Background Coding Skill

| Finding | Detail |
|---|---|
| Background skill predicts comprehension **regardless** of group | In both agent and chatbot conditions, higher background skill → higher comprehension |
| Agent use equalizes initial task accuracy across skill levels | Novices match experts on the first task when using the agent |
| Extension accuracy predicted by background skill **only in chatbot group** | In the agent group, the agent's scaffolding compensates for the user's missing comprehension, masking who actually understands the code |

**Implication:** Traditional coding skill remains valuable even with agents. Tools and curricula that let foundational skill atrophy create a hidden dependency.

---

## 4. Interaction Strategy Analysis

### 4.1 Prompt Types

| Prompt Strategy | Comprehension Impact |
|---|---|
| **Copy + paste task criteria verbatim** (low-effort) | Lowest comprehension |
| **Add code syntax / technical context to prompts** (high-effort) | Highest comprehension |

Low-effort prompting is a detectable, classifiable signal — the content and structure of the prompt itself reveals whether the user is engaging cognitively with the problem.

### 4.2 Review Behaviors

| Review Behavior | Comprehension Score |
|---|---|
| **Auto-accept edits** (bulk accept without reading) | 0.615 |
| **Review each file individually** (per-file review) | 0.777 |

Per-file review does not fully reach chatbot comprehension levels, but it closes most of the gap. This makes review UX a high-leverage design lever — making per-file review the default path rather than bulk accept recovers most of the comprehension harm without forcing users back to manual coding.

---

## 5. Code Readability Analysis

Comprehension correlates with code readability, not just who wrote it. Lower comprehension is linked to:

| Readability Metric | Relationship to Comprehension |
|---|---|
| Lines of code (volume) | More lines → lower comprehension |
| Comment proportion | More comments (counter-intuitively) → lower comprehension — verbosity can obscure rather than clarify |
| Halstead volume | Higher volume → lower comprehension |
| Entropy | Higher entropy → lower comprehension |

**Implication:** Concise, readable agent output helps comprehension; bloated output harms it. Readability is a trainable signal: agents can be optimized to produce fewer lines, lower volume, and concise comments. Guard against reward-hacking (e.g., collapsing code into unreadable one-liners to minimize LOC).

---

## 6. Regression and Path Mediation

### Regression Model
- Comprehension predicted by: group (agent vs chatbot), background skill, interaction strategy, code readability
- Adding initial task accuracy to the model barely changes R² (0.40 → 0.42), confirming completion and comprehension are independent

### Path Mediation (Extension Task)

```
Group (Agent vs Chatbot)
  ├──→ Code Quality (positive path) → Extension Accuracy (+)
  └──→ Comprehension (negative path) → Extension Accuracy (−)
```

- The indirect effect through comprehension is significant and **opposite in sign** to the effect through code quality
- This is why extension accuracy looks "similar" across groups: two opposing forces cancel
- The mediation model is the strongest evidence that the agent trades understanding for scaffolding

---

## 7. User Perception vs Measured Understanding

### The Preference Paradox

Users self-report weaker understanding yet still prefer the agent:

| Self-Report Item | Agent | Chatbot |
|---|---|---|
| Helpfulness | 4.7 | 3.3 |
| Mental effort (lower = easier) | 1.9 | 3.7 |
| "I understand how my code works" | 3.3 | 4.2 |
| Code feels like my own | 2.5 | 3.7 |
| Prefer this tool | 4.2 | 3.9 |
| Prefer switching between both | 4.6 | 4.6 |

**Key insight:** Users will not self-correct. Subjective preference actively misleads — the tool that feels best (fast, low-effort, helpful) is the one that harms understanding most. Self-reported "I understand how my code works" is lower for agent users (3.3 vs 4.2), showing users have some awareness, but this does not change their preference. This is why comprehension must be an explicit, measured design target rather than left to user choice.

---

## 8. Design Takeaways for Coding Agent Developers

1. **Measure both axes.** Track comprehension (recall + reasoning, Bloom-aligned) alongside task completion. A benchmark that measures only completion is blind to the harm.
2. **Classify prompt effort and intervene.** Detect copy-paste / low-effort prompts; nudge users to add technical context before generating. Build low-effort prompt classifiers and refusal/re-request behaviors.
3. **Make per-file review the default.** Replace bulk auto-accept with a per-file review path; even partial review recovers most of the comprehension gap (0.777 vs 0.615).
4. **Optimize code readability, not just correctness.** Use LOC, Halstead volume, entropy, and comment proportion as generation signals. Fewer lines and concise comments help; guard against unreadable minimalism.
5. **Route between agent and user implementation.** Predict when the AI vs the user should write a given piece of code, balancing productivity and understanding. Users prefer having the option to switch (hybrid preference 4.6/5) — design for hybrid, not all-or-nothing.
6. **Promote active engagement during review.** Current diff/summary review never fully replaces writing code for comprehension. Design interactions that force cognitive engagement rather than passive acceptance.

---

## 9. Limitations

- **Student population:** N=54 CS students; may not fully generalize to professional developers with more experience or domain expertise (though background skill was controlled)
- **Single task:** One website-building task (zic-zac-zoe); comprehension effects may vary by task complexity, domain, or codebase size
- **Short-term measurement:** Comprehension measured immediately after the task; long-term retention and cumulative comprehension debt not assessed
- **Two-condition design:** Only agent (Aider) vs chatbot; does not test intermediate modes (e.g., agent with forced review prompts, agent with cognitive forcing functions)
- **Self-report reliance for preference:** Subjective ratings subject to social desirability and post-hoc rationalization, though the comprehension gap is objectively measured
- **Tool-specific effects:** Aider is one specific agent; results may vary with other agents (Cline, Cursor Agent, Claude Code) with different interaction models
- **Extension task without AI:** Tests dependency in one direction (can you extend without the agent?); does not test whether agent users catch up on comprehension over longer collaboration periods

---

## 10. Cross-References to Existing A-Tech Skills

| Skill | Relationship |
|---|---|
| `agentic-cognitive-engagement-decline` | Engagement declines across planning→execution→evaluation (Catalan et al., CHI 2026, 4 engineers + Cline). This skill's review and routing levers are the interaction-design complement to that finding's cognitive-forcing designs. |
| `comprehension-debt-framework` | The accumulating gap between code and understanding (Ahmad, EASE 2026, 207 students, 621 diaries). This skill provides the **controlled experimental evidence** for that debt — four accumulation patterns observed qualitatively, confirmed quantitatively here. |
| `mental-model-erosion-defense` | Context-rich vs shallow AI interaction. This skill's "guide vs direct-edit" lever is the interaction-type version of that distinction — direct-edit erodes mental models; guide preserves them. |
| `ai-skill-formation-interaction-patterns` | Six interaction patterns (three cognitive-engagement, three cognitive-offloading). This skill's low-effort prompting / auto-accept findings map onto the offloading patterns; high-effort prompting maps onto engagement patterns. |
| `ikea-effect-digital-ownership` | Self-reported "code feels like my own" drops under agents (2.5 vs 3.7). The ownership-comprehension link — users who feel less ownership also understand less. |
| `agent-oversight-work-heuristics` | Post-hoc review heuristics. This skill shows why post-hoc review alone is insufficient for comprehension — per-file review helps but does not fully close the gap. |

---

## 11. A-Tech Alignment

| A-Tech Value | Alignment |
|---|---|
| **Open-source AI** | Applies directly to open-source agents (Aider, Cline, OpenCode). Comprehension matters most when code is public and collaborative — open contributions that no one understands are unsustainable. The study itself uses Aider, an open-source tool. |
| **Data privacy** | Comprehension enables air-gapped and confidential work without agent dependency. A developer who understands the code can work offline or in restricted environments; a developer who depends on the agent cannot. |
| **Financial freedom** | Comprehension debt is technical debt — it costs money when teams cannot maintain or extend agent-generated code without the agent. The extension-task trap shows the cost: the agent's scaffolding advantage evaporates without the agent, leaving teams dependent. |
| **Practical implementation** | 54-participant controlled experiment with validated comprehension metrics (Bloom-aligned), regression analysis, and path mediation. Concrete, implementable design levers: prompt classification, per-file review UX, readability signals, routing logic. |

---

## 12. Citation

Balepur, N., Baumler, J., Chen, X., Choi, Y., Rudinger, R., & Boyd-Graber, J. (2026). "Coding Agents Can Hurt Comprehension: An Experimental Study." University of Maryland / New York University / Carnegie Mellon University.