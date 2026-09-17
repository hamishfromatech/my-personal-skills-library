# Progressive Disclosure Patterns for Developer Tools

## Pattern 1: The Layered README
**Context**: Open-source repository landing page
**Structure**:
```
Layer 1 (Promise): One-sentence description + 30-second GIF demo
Layer 2 (Principle): Architecture overview (50 words max)
Layer 3 (Implementation): Quick start (3 commands to "hello world")
Layer 4 (Extension): Link to "How it works" deep-dive doc
Layer 5 (Contribution): Link to architecture decision records and contribution guide
```
**Example**: A-Coder README
- Promise: "A-Coder runs autonomous AI agents in your IDE with zero data retention."
- Principle: "Morph Fast Apply guarantees deterministic code application."
- Implementation: `npm install -g a-coder`, `a-coder init`, `a-coder run hello-agent`
- Extension: "See Architecture.md for how Morph Fast Apply handles multi-file diffs."
- Contribution: "Found an edge case? See CONTRIBUTING.md."

## Pattern 2: The Tutorial Cliffhanger
**Context**: Documentation tutorials and walkthroughs
**Structure**: Each section ends with a solved problem and a natural next question.
**Example**:
- Section 1: "You built an agent that writes a function. But how does it know *which* function to write?"
- Section 2: Reveals context parsing. Ends with: "But what if the context spans 10 files?"
- Section 3: Reveals multi-file context windows. Ends with: "But what if you need to run this offline?"
- Section 4: Reveals local model integration.

**Rule**: The question must be *genuine* — the next section must answer it completely.

## Pattern 3: The Configurable Onboarding Wizard
**Context**: First-run IDE or tool setup
**Structure**: 5 steps, each revealing one layer and creating appetite for the next.
**A-Coder Example**:
1. "Choose your coding identity" (language/framework)
2. "Set your AI personality" (tone, verbosity)
3. "Connect your world" (import keybindings)
4. "Your first plugin" (enable one community plugin)
5. "Claim ownership" (show local data directory)

**Critical**: Each step must show immediate value. No "we'll explain this later."

## Pattern 4: The Contextual Whisper
**Context**: In-product discovery of advanced features
**Structure**: Small, dismissible hints in contextually relevant locations.
**Example**:
- Status bar after 3 days: "There's a plugin for auto-generating commit messages."
- Terminal footer after first error: "Did you know there's a debug mode that shows agent reasoning?"
- Settings panel: "You've customized 12 settings. 3 more unlock expert mode."

**Rule**: Must be dismissible without penalty. Must not interrupt flow.

## Pattern 5: The Chapter Sequel Tease
**Context**: Book and playbook chapter endings
**Structure**: Close the chapter's scar, open a specific next gap.
**Be Practical Example**:
- Chapter 4 ends: "You now have a local model running on your machine. But running a model is only half the battle — how do you make it *useful* in your actual workflow? In Chapter 5, we'll build the bridge between 'local AI' and 'productive AI.'"

**Anti-pattern**: "Coming soon: Chapter 5" — never reference content that doesn't exist.

## Pattern 6: The Tiered Community Reveal
**Context**: Community knowledge sharing with participation gates
**Structure**: Information disclosure tied to contribution level, not payment.
**Builder's Club Example**:
- Public: "Members are building autonomous agents."
- Free member: "Here's the architecture pattern."
- Contributor: "Here's the repo and failure log."
- Core member: "Here's the multi-agent orchestration fork."
- Recognized builder: "Here's how to publish a community standard."

**Critical distinction**: This is "earned disclosure" (through contribution), not "paywalled content." Aligns with open-source and financial freedom values.

## Pattern 7: The Skill Tree Discovery Map
**Context**: Community learning and competence visualization
**Structure**: Visual tree where nodes create visible knowledge gaps with transparent unlock paths.
**Implementation**:
- Grayed nodes with evocative names: "Edge-First Inference," "Zero-Shot Tool Calling"
- Hover reveals prerequisite path
- Click prerequisite shows specific action: "Complete Playbook 4 + share one build"
- No hidden requirements. No surprise gates.

## Pattern 8: The Reverse Documentation
**Context**: Complex feature documentation where readers are experts
**Structure**: Start with the full implementation, then progressively disclose *why* simpler approaches fail.
**Example**: "Here's the complete Morph Fast Apply algorithm. [Full code block]. Now, here's what happens if you skip step 3... [failure mode]. And here's what happens if you skip step 7... [catastrophic failure]. This is why each step exists."

**Best for**: Advanced users who want to understand edge cases before adopting.

## Anti-Patterns

| Pattern | Why It Fails | Developer Reaction |
|---------|--------------|-------------------|
| Clickbait headlines | Promise exceeds delivery | Immediate distrust |
| Email gates for basic docs | Extracts before delivering | Fork and self-host |
| Forced tutorial sequences | Removes autonomy | Skip or uninstall |
| Infinite scroll docs | Exploits completion compulsion | Abandon; seek alternatives |
| Fake scarcity | Detected instantly | Public call-out |
| Competence shaming | Excludes beginners | Community backlash |

## Implementation Checklist

For each progressive disclosure flow, verify:
- [ ] Layer 1 is visible within 2 seconds
- [ ] Each layer provides standalone value
- [ ] The gap between layers is small enough to feel closable
- [ ] The gap between layers is large enough to feel meaningful
- [ ] Users can exit at any layer without loss
- [ ] The final layer connects to contribution or ownership
- [ ] All gaps close completely — no abandoned promises
