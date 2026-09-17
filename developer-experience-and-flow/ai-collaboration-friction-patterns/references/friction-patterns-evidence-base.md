# AI Collaboration Friction Patterns — Evidence Base

## Source

Rahul Garg (Principal Engineer, Thoughtworks), published on MartinFowler.com Feb 24 – May 5, 2026:
- "Patterns for Reducing Friction in AI-Assisted Development" (overview, Feb 24, 2026)
- "Knowledge Priming" (Feb 24, 2026)
- "Design-First Collaboration" (Mar 3, 2026)
- "Context Anchoring" (Mar 17, 2026)
- "Encoding Team Standards" (Mar 31, 2026)
- "Feedback Flywheel" (Apr 8, 2026)
- "Lattice — Operationalizing AI Collaboration" (LinkedIn, May 5, 2026)

Martin Fowler Fragments note (May 5, 2026): "Over the last couple of months Rahul Garg published a series of posts here on how to reduce the friction in AI-assisted programming."

URLs:
- https://martinfowler.com/articles/reduce-friction-ai/
- https://www.linkedin.com/pulse/lattice-operationalizing-ai-collaboration-rahul-garg-wh9ic

---

## 1. The Frustration Loop

The typical interaction pattern:

> A developer asks the AI to create a service. The AI responds quickly with syntactically correct code that follows common patterns from its training data. However, it uses Express.js when the project uses Fastify. It places files in utils/ when the convention is lib/services/. It generates class-based code when the codebase is functional. Each correction requires another generation cycle, and with each cycle, the time advantage erodes.

**The loop:** Generate → Review → "Not quite right" → Regenerate → Review → "Still wrong" → Give up

**Why it persists:** AI assistants draw from training data — "the average of the internet" — rather than code that fits a specific team's architecture and conventions.

**Common symptoms:**
- AI generates solutions that don't align with existing architecture
- Developers spend significant time on post-generation editing
- Context established early in a conversation is lost as the session lengthens
- Quality varies depending on which team member is prompting

---

## 2. The Speed Trap

Teams track metrics like time to first output or lines of code generated. These are easy to capture and show immediate results, but they obscure the actual cost.

> If an AI generates 200 lines of code in seconds, but a developer spends 30 minutes reviewing, debugging, and refactoring to fit team patterns, the net productivity gain is questionable. The work has shifted from writing to fixing, but the total effort may not have decreased.

**Collaboration-quality metrics (the correction):**

| Misleading Metric | More Useful Alternative |
|---|---|
| Time to first output | First-pass acceptance rate |
| Lines of code generated | Iteration cycles per task |
| Tasks completed | Post-merge rework required |
| Generation speed | Review burden compared to manual writing |

For teams tracking DORA metrics, first-pass acceptance serves as a leading indicator for change failure rate. Code that requires extensive correction before being usable is a signal of misalignment, and misaligned code that ships becomes technical debt.

---

## 3. The Tool-to-Teammate Shift

> AI assistants are like junior developers with infinite energy but zero context.

The reframe: stop treating AI as a tool and start treating it as a teammate — a distinction that sounds semantic but has practical implications.

**The parallel to pair programming:**

| Human Pair Programming | AI Collaboration Equivalent |
|---|---|
| "Let me show the docs first" | Sharing architectural context before requesting code |
| "Let's sketch this on the whiteboard" | Structured design discussion before implementation |
| "Here's how reviews work here" | Encoding standards into reusable prompts or commands |
| "Let me update the doc with the decision" | Persisting decisions so they survive session boundaries |
| "What did that teach us?" | Systematically capturing what worked and what didn't |

AI needs: **Onboarding** (context about the codebase before contributing), **Whiteboarding** (structured design discussion before implementation), **Guardrails** (standards and quality checks consistently applied).

---

## 4. The Five Patterns (detailed)

### Pattern 1 — Knowledge Priming
- Human parallel: Onboarding a new hire
- Action: Before asking AI to generate code, share curated context about the project — tech stack with version numbers, directory structure, naming conventions, examples of existing patterns. This is manual RAG (Retrieval-Augmented Generation) — filling the context window with high-value, project-specific information that overrides generic training data.
- Failure mode addressed: AI defaults to generic patterns because it lacks project-specific context.

### Pattern 2 — Design-First Collaboration
- Human parallel: Whiteboarding before coding
- Action: Walk through progressive levels of design — capabilities, components, interactions, contracts, and only then implementation. Mirrors the whiteboarding that precedes coding in effective human collaboration.
- Failure mode addressed: AI jumps to implementation before understanding requirements, producing code that solves the wrong problem.

### Pattern 3 — Context Anchoring
- Human parallel: Updating the doc with decisions
- Action: Maintain a living document that captures decisions, constraints, and current state as a feature evolves. This document serves as external memory, anchoring context that would otherwise be lost as conversations grow longer or sessions end.
- Failure mode addressed: AI forgets decisions made earlier in long conversations, leading to contradictions and inconsistency.

### Pattern 4 — Encoding Team Standards
- Human parallel: What a senior developer does instinctively
- Action: Make tacit knowledge explicit. When defaults exist as artifacts rather than intuition, they apply consistently regardless of who is prompting.
- Failure mode addressed: AI generates technically correct code that violates team expectations, requiring extensive rework.

### Pattern 5 — Feedback Flywheel
- Human parallel: Retrospectives and continuous learning
- Action: Systematically harvest learnings from AI interactions to improve the other four patterns. When a prompt works particularly well, capture it. When AI consistently misunderstands something, add it to the priming context. When a failure pattern emerges, add a guardrail.
- Failure mode addressed: Teams make the same mistakes repeatedly without building institutional knowledge about effective AI collaboration.

---

## 5. The Shared Mental Model

These patterns work together to create a shared mental model between humans and AI:

- Same vocabulary (through priming)
- Same architecture vision (through design-first discussion)
- Same quality standards (through codified commands)
- Same decision history (through anchored documentation)

The cognitive load shifts from constant vigilance and correction to expressing intent and refining output. This is the difference between struggling with a tool and collaborating with a capable partner.

Garg's hypothesis: these patterns, when applied consistently, could yield higher first-pass acceptance rates, fewer iteration cycles per task, and less post-merge rework. He notes these are not yet validated findings — a proposed framework based on reasoning about why friction occurs. Early experiments in his own work have been encouraging.

---

## 6. Trade-offs and Limitations

- Creating and maintaining priming documents requires effort.
- Design-first conversations take longer than immediately requesting code.
- The patterns require discipline to sustain.
- For simple, one-off tasks, the overhead may not be justified. The investment pays off primarily for non-trivial work, especially work that spans multiple sessions or involves team coordination.
- Learning curve: teams accustomed to treating AI as a search engine or autocomplete system may find the transition to structured collaboration unfamiliar. The shift requires deliberate practice.

---

## 7. Lattice — Operationalizing AI Collaboration

### The Operationalization Gap

> Most collaboration patterns do not fail the first time someone tries them. They usually fail a few weeks later.

A team reads the Knowledge Priming article, creates a priming document, and six weeks later the document is stale. Another team tries Design-First for a sprint, then reverts to "just generate the code" when a deadline tightens. A senior engineer crafts careful instructions; her teammates, who did not read it, still prompt generically.

The knowledge exists on the team. The discipline does not persist. Understanding a practice and consistently applying it are different things.

> Teams do not rely on developers remembering style rules: they encode them as .eslintrc, CI pipelines, infrastructure-as-code. The series argued to "treat context as infrastructure, not habit."

### What Lattice Is

Lattice is an open-source framework of composable AI skills. The skills are markdown files. There is no separate runtime, no build step, no SaaS dependency. Think ".eslintrc for AI collaboration, but covering architecture, design methodology, domain modeling, and security rather than just code style."

- Tool-agnostic: works with Claude Code, Cursor, or any tool that supports a skills or instructions mechanism
- Skills are markdown, portable by nature
- MIT-licensed, built to be adopted, forked, and customized
- Primarily focused on backend development (expanding to other verticals)

### The Three-Tier Composability Model

**Atoms** — independent, single-principle skills. Each atom teaches one engineering discipline: clean code, architecture, domain-driven design, secure coding, test quality. An atom is self-contained; it carries its own guardrails, its own validation checklist, its own anti-pattern scan. Any atom can be used standalone.

Four structural atoms: knowledge-priming (loads project identity), design-first (five progressive design levels), context-anchoring (per-feature living documents), collaborative-judgment (surface uncertainty as structured options instead of guessing silently).

**Molecules** — multi-step workflows that compose atoms. The design-blueprint molecule composes knowledge-priming, context-anchoring, design-first, architecture, and domain-driven design into a full design workflow. The code-forge molecule composes knowledge-priming, context-anchoring, clean-code, architecture, domain-driven design, secure-coding, and test-quality into an implementation workflow. Molecules reference atoms; they never duplicate atom content.

**Refiners** — how teams enrich atoms so molecules behave the way the team wants. Each refiner runs a guided interview and produces a standards document in `.lattice/standards/`. When an atom is invoked, it checks for this document and adapts its defaults. The refiner does not change the atom's skill file. It produces project-specific configuration that the atom consults when the skill is used.

> The construct, put simply: atoms are independent skills, molecules compose them into workflows, refiners enrich them so the composed workflows reflect a particular team's standards.

### Three Design Principles

1. **Skills over prompts.** Skills live in the repo, change through pull requests, and apply the same way for everyone. Prompts do not.
2. **Composability over monoliths.** Small skills compose into workflows; update one atom and every molecule that uses it benefits.
3. **Living context over static config.** Review feedback, refiners, and `.lattice/` turn the framework into institutional memory that accrues each cycle, not a one-time setup.

### The Feature Lifecycle

1. `lattice-init` once per project — suggest refiners, seed `.lattice/`
2. `design-blueprint` — load context, walk design levels, persist an approved blueprint before code
3. `code-forge` — implement against that blueprint and prior learnings, with atoms doing checklist verification
4. `review` — independent pass, severity-ordered findings, insights written to `.lattice/learnings/`
5. The next feature's `code-forge` reads those learnings so the project does not repeat the same mistakes

> That is the same cadence I would want with a human pair: orient, design, implement, review, carry the lesson forward.

Non-greenfield work uses the same spine: refactor-safely and bug-fix converge on review, still feeding the living context layer.

### The Compounding Effect

After a few cycles, atoms are enforcing your standards, refined by review history and re-run refiners, not generic internet defaults. `code-forge` benefits from what review already caught because the learnings loop is automatic.

The packaged skills stay stable; what deepens each cycle is `.lattice/`: standards, learnings, health signals, and per-feature context. That is when the Feedback Flywheel stops being a workshop idea and becomes how the team actually works.

### Installation

```
# In Claude Code:
/plugins marketplace add techygarg/lattice
/plugins install lattice
/reload-plugins

# Or to a skills directory directly:
git clone https://github.com/techygarg/lattice.git
cd lattice
./tools/install.sh /path/to/your/skills/folder
```

First steps: run `lattice-init`, run the knowledge-priming refiner, then try `design-blueprint` → `code-forge` → `review`.

The repo includes a sample at `sample/` — a realistic .NET 8 User Service spec.

### Customization

- **Overlay mode:** document only what differs from defaults (most teams should use this)
- **Override mode:** replaces an atom's defaults entirely when your philosophy diverges

---

## 8. Cross-Reference Mapping to Existing A-Tech Skills

| This Skill's Concept | Related Existing Skill | Relationship |
|---|---|---|
| Tool-to-teammate shift (pair-programming level) | `ai-employee-agent-team-management` | Same shift at team-management level; this is the pair-programming level |
| Beyond the frustration loop | `beyond-vibe-agentic-engineering` | This provides the collaboration scaffolding that makes the maturity arc sustainable |
| Spec-before-code / Design-First | `spec-driven-development-framework` | Design-First is the human-AI-pairing expression of spec-driven development |
| Knowledge Priming (manual RAG) | `context-engineering` / `context-engineering-production-practice` | Knowledge Priming is manual RAG; context-engineering is the systematic discipline |
| Context Anchoring (living docs) | `context-engineering` | Anchoring is living-context management at the feature level |
| Encoding Team Standards | `devex-verification-bottleneck-framework` (3 D's) | Operationalizes the Design and Documentation D's |
| Shared mental model | `cognitive-offloading-ladder` (cognitive debt) | The shared mental model is the active mitigation of cognitive debt |
| First-pass acceptance rate metric | `developer-experience-flow-state` | Feeds the feedback-loop and flow-state measurement framework |
| AI as junior dev with zero context | `agent-friendly-api-documentation-2026` | Reciprocal — making codebase consumable for AI |
| Proactive intervention vs. pre-established alignment | `proactive-ai-intervention-timing` | Alignment reduces the need for interruption |