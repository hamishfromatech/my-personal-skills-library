# AI Code Rot Research Compilation

Academic and industry evidence on the quality and sustainability of AI-generated code.

---

## Key Studies

### SSRN — "From Stateless Generation to Layered Software Decay in AI-Assisted Software Engineering" (2026)
- **Finding:** AI-generated code creates "layered software decay" — each generation layer adds entropy without architectural awareness
- **Mechanism:** Statelessness means each completion is independent of codebase history
- **Implication:** Codebases accumulate inconsistent patterns, duplicated logic, and architectural drift at accelerated rates
- **Citation:** SSRN abstract_id=6655438

### Softwareseni — "Almost Right But Not Quite" (2026)
- **Finding:** 62% of AI-generated code contains design flaws or vulnerabilities
- **Context:** Teams feel 20% faster but are actually accumulating technical debt faster than manual coding
- **Key insight:** The productivity paradox — perceived speed masks quality degradation

### DX / getdx.com — "Code Rot and Productivity" (2026)
- **Finding:** AI-generated code increases duplication by 8× and reduces code reuse
- **Mechanism:** AI prefers generating new code over referencing existing patterns
- **Hidden cost:** Duplication creates cascading maintenance burden as requirements change

### Vella & Blincoe — "The Impact of AI Coding Assistants on Software Engineering: A Longitudinal Study" (arXiv:2605.23135, May 2026)
- **Finding:** Creation-to-verification shift in developer work patterns
- **Productivity-experience paradox:** 84% productivity stable, 14% → 27% experience erosion over time
- **Flow state erosion:** Continuous switching between generation and verification breaks deep work

### Reddit r/ExperiencedDevs — DeveloperWeek 2026 Synthesis
- **Finding:** Off-the-shelf AI models don't understand internal architecture, generating locally correct but globally incoherent code
- **Developer quote:** "It writes code that compiles but doesn't belong"
- **Pattern:** Junior developers accept AI output at higher rates, accelerating debt accumulation

### Pragmatic Coders — "What Is Technical Debt?" (2026)
- **Finding:** "Messy codebases produce more wrong code and a new layer of AI-generated spaghetti"
- **Term:** "Vibe coding" — generating code without architectural intent
- **Implication:** Technical debt is now generated faster than it can be manually refactored

---

## Mechanisms of AI Code Rot

1. **Stateless generation:** No memory of codebase architecture, conventions, or evolution
2. **Local optimization:** Each completion optimized for immediate task, not global coherence
3. **Pattern blindness:** AI misses subtle architectural patterns that human developers recognize
4. **Utility re-implementation:** Generating functions that already exist in the codebase
5. **Error handling omission:** AI-generated code often lacks production-grade error handling
6. **Test gaps:** Generated tests cover happy paths, miss edge cases and failure modes
7. **Dependency drift:** Generated code may use deprecated or conflicting dependency versions

---

## Mitigation Evidence

### Positive Patterns from Literature
- **Project-specific context loading:** Reduces duplication by 40–60% (anecdotal from engineering blogs)
- **Comprehension checkpoints:** Teams requiring explanation of AI code before merge show 30% lower defect rates
- **Architecture fitness functions:** Automated architectural governance catches 70% of AI-generated structural violations
- **Pair programming with AI:** Human+AI pairs produce code with lower rot than solo AI usage

---

## A-Tech Implications

The evidence supports A-Tech's core thesis: AI tools must be wrapped in human-centered workflows that preserve architectural intent and comprehension. Raw AI generation without governance creates a debt treadmill that eventually negates productivity gains.
