# Comprehension Checkpoint Protocol

A structured process ensuring developers truly understand AI-generated code before integrating it into production.

---

## Protocol Overview

The comprehension checkpoint addresses the core risk of AI-assisted development: developers accepting code they do not fully understand. This creates a knowledge debt that compounds when the original developer is unavailable for maintenance.

**Evidence base:**
- Vella & Blincoe (2026): Experience erosion correlates with uncritical AI acceptance
- Wharton research (cited in Cognitive Surrender Defense): 73% follow faulty advice when confident
- Developer surveys: Junior developers accept AI output at 2× the rate of seniors

---

## Checkpoint Triggers

**Mandatory checkpoints (cannot be bypassed):**
- AI-generated code >20 lines
- Code touching authentication, authorization, or payment flows
- Code modifying database schemas or migration scripts
- Code introducing new dependencies
- Code with >2 nested conditional blocks

**Recommended checkpoints:**
- Any AI-generated algorithm or data structure
- Code with asynchronous patterns (promises, async/await, callbacks)
- Code interacting with external APIs

---

## The Four-Question Checkpoint

Before accepting AI-generated code, the developer must answer these four questions in writing (in the PR description or review comment):

### Question 1: What does this code do?
**Requirement:** Explain in plain English, not technical jargon. If the explanation requires jargon, the developer does not understand it deeply enough.

**Example (good):**
> "This function takes a list of orders, groups them by customer, and returns the total spent per customer."

**Example (insufficient):**
> "It reduces the array using a Map accumulator."

### Question 2: What pattern or approach does it use?
**Requirement:** Name the design pattern, algorithm, or architectural approach. Connect it to known computer science or engineering concepts.

**Example:**
> "It uses the Strategy pattern to select different tax calculation rules based on jurisdiction."

### Question 3: How could this code fail?
**Requirement:** Identify at least one realistic failure mode. This demonstrates awareness of edge cases and production realities.

**Example:**
> "If the customerId is null, the grouping will create a 'null' key. If the orders list is empty, it returns an empty object rather than throwing, which callers might not expect."

### Question 4: How does it fit the existing architecture?
**Requirement:** Explain where this code lives in the architectural layers and whether it respects existing boundaries.

**Example:**
> "This is a domain service that calls the repository layer. It belongs in `services/billing/` and uses the existing `OrderRepository` interface. It does not bypass the repository layer."

---

## Integration with Code Review

### PR Template

```markdown
## AI-Generated Code Disclosure
- [ ] No AI-generated code in this PR
- [ ] AI-generated code present (below checkpoint answers)

## Comprehension Checkpoint

### Code Location
File: `src/services/billing.js`, lines 45–82

### Q1: What does this code do?
(answer here)

### Q2: What pattern does it use?
(answer here)

### Q3: How could this code fail?
(answer here)

### Q4: How does it fit the architecture?
(answer here)

### Reviewer Verification
Reviewer: @username
- [ ] Reviewer agrees the answers demonstrate comprehension
- [ ] Reviewer can explain the code independently
```

### Reviewer Responsibilities

The reviewer must:
1. Read the checkpoint answers before reviewing the code
2. Verify the answers are accurate and complete
3. Independently explain the code back to the author
4. Reject the PR if either party cannot explain the code

**Rejection is not punishment.** It signals that the code needs clarification, simplification, or decomposition before it is ready for the codebase.

---

## Tooling Support

### IDE Integration Concept

A-Coder could implement comprehension checkpoint prompts:

```
[AI generated 34 lines of code]

Before accepting:
1. Can you explain what this does in one sentence? [_____]
2. Name the pattern used: [_____]
3. Name one failure mode: [_____]
4. Which layer does this belong in? [_____]

[Accept] [Edit] [Reject] [Explain More]
```

### Git Hook (Optional)

```bash
#!/bin/bash
# .git/hooks/pre-commit

# Detect AI-generated markers
if git diff --cached | grep -q "# AI-GENERATED"; then
  echo "⚠️ AI-generated code detected. Complete comprehension checkpoint before committing."
  echo "Run: ./scripts/comprehension-checkpoint.sh"
  exit 1
fi
```

---

## Anti-Patterns to Avoid

1. **Rubber-stamp review:** Approving because "the tests pass" without understanding
2. **Answer delegation:** Junior answers checkpoint, senior reviews without reading
3. **Copy-paste explanation:** Pasting AI's own explanation back as the answer
4. **False confidence:** "I understand" without being able to explain in different words
5. **Checkpoint fatigue:** Treating checkpoints as bureaucracy rather than quality practice

---

## Measurement

Track these metrics monthly:
- Checkpoint completion rate (target: 100% for mandatory triggers)
- Average time to complete checkpoint (target: <5 minutes for simple code)
- Reviewer rejection rate due to poor checkpoint answers (target: <10%)
- Post-merge bug rate for AI-generated code with vs. without checkpoint
