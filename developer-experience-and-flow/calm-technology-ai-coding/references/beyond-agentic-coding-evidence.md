# Beyond Agentic Coding — Evidence Base

## Source

Gabriella Gonzalez, "Beyond agentic coding," *Haskell for all* blog, February 7, 2026. Licensed CC BY-SA 4.0.

The author is a DevProd practitioner with over a decade of experience and a long history of open source projects and contributions. The post argues that agentic coding in its present incarnation does more harm than good to software development, and proposes calm technology as the design discipline for better AI-assisted coding.

## The Master Cue

> A good tool or interface should keep the user in a flow state as long as possible.

This is the author's personal design principle, derived from first-principles tool design rather than industry trend reaction. It is not specific to AI-assisted development but directly highlights why agentic coding misses the mark.

## Evidence That Agentic Coding Breaks Flow

### 1. The Becker Study

Screen recordings showed that idle time approximately doubled when developers used agentic coding tools. The user is placed in an idle/interruptible holding pattern — they cannot enter flow state because they must remain ready to respond to the agent.

### 2. The Shen Study

Users of agentic coding perform no better and sometimes worse when productivity is measured in terms of fixed outcomes rather than code velocity or code volume. This challenges the common metric of "lines generated" or "tasks completed" that agentic coding vendors favor.

### 3. Interview Candidate Observations

The author allows interview candidates to use agentic coding tools. Candidates who do so consistently performed worse than other candidates:
- Failed to complete the challenge
- Produced incorrect results
- Failed to match a provided golden output (which was not supposed to be the hard part)
- Sometimes did not even realize their program didn't match the golden output because they hadn't run their agentically coded solution to check correctness
- Performed worse on follow-up questions about the journey to production

This was a surprise because the expectation was that agentic coding would confer an unfair advantage. The opposite occurred.

## The Calm Technology Analysis of Chat-Based Agentic Coding

### High Attention Demand

The user has two options, both of which break flow:
1. **Sit and wait** for the agent to report back — idle, not in flow
2. **Do something else** and run the LLM semi-autonomously — but semi-autonomous sessions still prevent flow because the user must remain interruptible

### Not Pass-Through

Chat agents are a highly mediated interface to the code:
- **Indirect:** The user interacts more with the agent than with the code
- **Slow:** The user spends significant time waiting
- **Imprecise:** Natural language (English) is a "dull interface" compared to direct code manipulation

### Undermines Calm

- The user must constantly stimulate the chat to gather new information or update their understanding of the code. The chat agent does not inform the user's understanding passively or quietly.
- Chat agents are fine-tuned to maximize engagement — the opposite of calm.

## Non-LLM Calm Technology Examples in Coding

### Inlay Hints (VSCode)

IDE inlay hints sprinkle code with useful annotations (inferred type annotations, parameter names).

**Calm properties:**
- Minimize attention demands: exist on the periphery, available if interested, unobtrusive if not
- Pass-through: don't replace or substitute the code being edited; enhance the editing experience while the user stays in direct contact with the code
- Create calm: inform understanding passively. "Technology can communicate, but doesn't need to speak."

### File Tree Previews (VSCode, GitHub PR viewer)

Preview at a glance changes to the file tree.

**Calm properties:**
- Minimize attention demands: there if needed, easy to ignore or forget they exist
- Pass-through: interaction with the viewer feels direct, snappy, and precise; the representation becomes indistinguishable from the reality in the user's mind
- Create calm: passively updates in the background as changes are made; unobtrusive and not attention-grabbing

The key insight: the best calm tools are pervasive and boring things we take for granted (like light switches) that have faded so strongly into the background that we forget they exist as part of our daily workflow.

## GitHub Copilot Inline Suggestions — Partially Calm

**Calm property:** Pass-through (user interacts directly with code, suggestions are snappy, user can ignore or type through them).

**Calm violations:**
- Demand attention: default auto-trigger presents suggestions frequently; user pauses to examine output; user becomes conditioned to pause and wait (reactive, not proactive)
- Undermine calm: visually busy and intrusive; suggestions appear in center of visual focus; user must decide on the spot whether to accept or ignore before proceeding; understanding each suggestion requires focused attention

**Partial fix:** Disable automatic suggestions and require explicit trigger (Alt+\). However, this also disables the next-edit suggestions feature.

## GitHub Copilot Next-Edit Suggestions — Strongly Calm

Related follow-up edits throughout the file/project that the user can cycle between and optionally accept. Behaves like a "super-charged find and replace."

**Calm properties:**
- Minimize attention demand: cognitive load is smaller than inline suggestions because suggestions are bite-sized and easier to review
- Pass-through: user stays in close contact with the code being modified
- Create calm: presented unobtrusively; not dumped in center of attention; don't demand immediate review; exist on periphery; user can ignore or focus at leisure

## Proposed Calm AI Coding Features

### Facet-Based Project Navigation

Browse a project by a tree of semantic facets (intent-based clusters). Example: "String interpolation regression" instead of `dhall/tests/format/issue2078A.dhall`.

**Calm properties:**
- Improves the user's understanding of the project the more they use the feature (the tool fades while the mental model grows)
- Provides quick exploration by intent
- Built on real tooling, not a mock

### Automated Commit Refactor

Take an editor session, diff, or pull request and automatically split it into a series of more focused commits that are easier to review.

**Key value:** This is a case where AI *reduces* human review labor (most agentic coding tools create more review labor). Some prior art exists but the area is nascent.

### File Lens

**Focus on…**: Specify an interest; only related files and lines are shown; others hidden/collapsed/folded. Zen mode for a feature domain.

**Edit as…**: Edit a file as if it were a different programming language or file format. Edit Haskell "as Python"; the AI back-propagates changes to Haskell. Edit a CLI parser "as YAML"; modify the simplified YAML to add options.

**Calm properties:**
- Amplify the best of humanity (user's understanding of a familiar format) and the best of technology (AI translation between representations)
- Keep the user in contact with the problem domain while abstracting the implementation syntax

## Implications for A-Tech

1. **A-Coder differentiation:** The dominant agentic coding tools (Cursor, Copilot Agent, Claude Code) are chat-centric. A calm-technology-first IDE is a genuine market differentiator aligned with growing developer fatigue toward attention-demanding AI.

2. **The "one tool" thesis reinforced:** Calm technology aligns with the existing agentic-interface-consolidation skill — one calm, well-integrated tool beats ten attention-demanding ones.

3. **Flow state as measurable outcome:** The Becker study's idle-time-doubling finding connects to the unified-devex-measurement-stack-2026 skill's flow state metrics. Calm technology provides the design framework; the measurement stack provides the instrumentation.

4. **Open-source opportunity:** Facet-based navigation, automated commit refactor, and file lens are all buildable as open-source IDE extensions. Builder's Club could produce these as community artifacts.

5. **The boring-tool celebration:** Calm technology's insight that the best tools are boring and taken-for-granted suggests a radical evaluation criterion: the most successful A-Coder AI features will be the ones users forget they're using.