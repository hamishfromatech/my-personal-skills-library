# Verification Load Interface Design — Evidence Base

## Primary Source

**"Verification Load and Fatigue with AI Coding Assistants"**
- Published: ACM (2026), DOI 10.1145/3772318.3791176
- Also available: dl.acm.org/doi/abs/10.1145/3772318.3791176
- Contribution: Controlled study isolating the effect of AI coding assistant interface on verification load and fatigue.

## Experimental Design

| Element | Detail |
|---|---|
| Participants | N = 60 |
| Tasks | 3 Python programming tasks |
| Conditions | Inline prompting, Chat prompting, Structured prompting, No-AI control |
| Model | Single LLM held fixed across all AI conditions |
| Isolation | Only the interface varies — model quality, task difficulty, and participant pool are controlled |

## The Core Finding

The interface through which a developer interacts with an AI coding assistant affects verification load and fatigue — independent of the underlying model's quality. This means:

1. **Interface is a fatigue determinant** — not just model quality or task difficulty
2. **Same model, different fatigue** — two tools using the same LLM can produce different fatigue levels
3. **Interface design choices have fatigue consequences** — inline vs. chat vs. structured is not just a UX preference

## Why This Matters

Most AI fatigue research varies the model (better model = less fatigue?) or the task (harder task = more fatigue?). This study holds both fixed and varies only the interface, establishing that interface design is an independent fatigue variable.

This has implications for:
- **Tool selection** — choosing an interface based on fatigue, not just capability
- **Tool design** — designing interfaces that minimize verification load
- **Fatigue research** — controlling for interface when studying model-level fatigue

## The Interface Conditions

### Inline Prompting
- AI suggests code directly in the editor at the cursor position
- Verification happens inline, interleaved with writing
- Lower context switch (no panel switching)
- But constant micro-interruptions (suggestions appear continuously)
- Best for: experienced users, routine code, flow-state preservation

### Chat Prompting
- AI interaction happens in a separate chat panel
- Verification requires switching between chat and code
- Higher context switch (Leroy attention residue per switch)
- But more deliberate review (separation creates a review checkpoint)
- Best for: complex tasks, learning, when deliberate review is needed

### Structured Prompting
- AI interaction follows a structured prompting protocol
- Verification is guided by the structure
- Potentially lower cognitive load (structure scaffolds verification)
- But more setup overhead (structure must be created/maintained)
- Best for: repetitive task types, team standardization, less experienced users

### No-AI Control
- Participants solve tasks without AI assistance
- Establishes the baseline verification load
- Allows calculation of the AI-induced verification burden delta

## Connection to Adjacent Skills

| Skill | Relationship |
|---|---|
| `ai-review-fatigue-mitigation` | Provides the human-factors framework (vigilance + complacency + switching). This study provides the interface-level evidence that modulates those mechanisms. |
| `ai-fatigue-scale-design` | Provides the general measurement framework. This study is the interface-specific application. |
| `coding-agent-decision-fatigue-mitigation` | Covers the decision-density crisis. Interface design modulates how decisions are presented — inline = continuous micro-decisions, chat = batched decisions. |
| `devex-verification-bottleneck-framework` | Covers the verification-time > writing-time inversion. This study shows the interface is a variable in that inversion. |
| `attention-residue-mitigation` | Covers the Leroy attention residue mechanism. Chat interfaces trigger more residue (panel switching); inline interfaces trigger less but with higher frequency. |
| `agentic-interface-consolidation` | Covers the consolidation of multiple AI interfaces. This study provides evidence for why consolidation matters — fewer interfaces = less interface-dependent fatigue. |

## Research Literacy Note

This study is an excellent example of **experimental control in AI fatigue research**:
- Single variable isolated (interface)
- Model quality held fixed
- Task difficulty held fixed
- Participant pool shared across conditions
- Baseline established (no-AI control)

This design allows causal attribution of fatigue differences to the interface, not to confounds. For Be Practical, this is a teaching example of how to research AI fatigue rigorously.