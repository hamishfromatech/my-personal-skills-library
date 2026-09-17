# Decision Fatigue Evidence Base

This file holds source extracts supporting the `coding-agent-decision-fatigue-mitigation` skill. All material is synthesized; no content is copied verbatim beyond short factual quotes attributed to their speakers.

## Source 1 — Stack Overflow Blog / Smartsheet (May 21, 2026)

**Title:** "Coding agents are giving everyone decision fatigue"
**Author:** Ryan Donovan (Staff, Stack Overflow)
**Primary research source:** Smartsheet CPTO Pratima Arora interview + Smartsheet internal research; supporting interviews with Carol Lee, PhD (Intuit) and Fitz Nowlan (VP AI & Architecture, Smartbear); reference to Cat Wu (head of product, Claude Code, Anthropic).

### Key findings

- **Work densification:** Smartsheet research found automation intensity for enterprise users grew 55% year-over-year, and overall activity increased 46%. The workday has not grown; it has become denser with work as automations produce more without alleviating the need for humans to decide on what "good" is.
- **80% edited:** 80% of AI-generated content is edited before finalization. Those edits come from gathering context about the code (since no one wrote the original), producing a judgement.
- **Code is cheap, review less so:** One engineer producing 7x the code of her team was a superstar — but the other six people spent the majority of their time reviewing her code rather than writing their own.
- **Review requires broad context:** The best code reviews look at a change in the context of the larger system, requiring the reviewer to hold and understand that context. The gatekeeper pressure is real (Carol Lee: "If I mess up this review, that was the gatekeeper of this code. And if I mess it up, that might be my fault.")
- **Legacy code aversion:** When engineers take over an old codebase they want to rewrite it — it is easier to start from scratch and understand it than to look at code and judge where errors happened because you didn't write it.
- **Every builder is a decider:** Smartsheet defines the builder as anyone who understands a customer problem, has an idea, and can prototype and ship software quickly. Core skills: understanding context and making judgement calls.
- **Senior compensation:** "We see most of our senior people loading a lot more in context and then making smaller changes. They end up working on the most complex piece, hence it's a lot more context for them versus the lines of code."
- **Judgement degradation:** One effect of decision fatigue is sloppier decisions. Arora referenced Cat Wu (Anthropic) describing the source-code-leak as the result of human error — "even with human judgment, sometimes errors can happen because we can get a little sloppy in some pockets."
- **End-to-end judgement:** Judgement should become a validation of overall outcomes, not spot-checking specific features. "Our judgement moves to much more higher order problems as we automate some of these lower order things."
- **Two gates:** Big judgement gates at the beginning (requirements, guardrails, specs, allowed dependencies) and the end (success/failure modes, security, dependability). Fitz Nowlan: "Think in terms of intent, functionality, and requirements... as dev velocity increases 10x, QA velocity must also increase 10x, and the only way to fight fire is with fire."
- **Smartsheet in practice:** The entire chain — PMs, designers, engineers — becomes builders. Design system integrated in Claude and Cursor; designers prototype front-end code, engineers review and check in. Designers not yet allowed to fully check in code; future state once the process matures.
- **SDLC reconfiguration:** "We're trying to align the tooling and align the systems between the teams to make it better." The new focus of DevEx happens after the code is generated. Systems were set up for the old way where AI was not an everyday thing.

### Source quality notes

- This is a trade-press synthesis based on a named executive (CPTO of Smartsheet) and her company's internal research, plus named expert interviews. The underlying Smartsheet research figures (55% automation intensity, 46% activity, 80% edited) are presented as enterprise-user research but the methodology is not published in a peer-reviewed form. Treat as credible industry signal, not controlled study.
- The decision-fatigue mechanism itself (quality degrades with successive decisions) traces to Baumeister et al. ego-depletion research, which has had replication controversies. The qualitative observation — late-day sloppier decisions under agent load — is widely reported but the precise mechanism is debated.

## Source 2 — Warped Visions (Bruce Alderson, 2025/2026)

**Title:** "The hidden cost of AI-assisted development: cognitive fatigue"
**URL:** https://warpedvisions.org/blog/2025/hitting-the-wall-at-ai-speed/

### Key findings (practitioner essay)

- **New fatigue type:** Traditional programming fatigue comes from wrestling with implementation details (syntax, obscure errors, repetitive work). AI eliminates much of this friction but replaces it with decision fatigue at the design level.
- **Decision branching:** When you can prototype three approaches in the time it used to take to implement one, you make architectural decisions constantly — service? library? script? error handling? data persistence? Each decision branches into more decisions, and the AI is ready to implement immediately.
- **Bottleneck shift:** From "can I build this?" to "should I build this, and how?" — a much higher cognitive load that accumulates faster than expected.
- **Wall-hitting speed:** AI-assisted development hits fundamental design decisions immediately because implementation is fast. You face questions about data models, API design, system boundaries before you've had time to think them through.
- **Architectural flatness:** When humans write code, architectural thinking happens during implementation (small adjustments, refactor-as-you-go). AI doesn't communicate that thinking in its output. The result is code that works but feels architecturally flat — you do more explicit architectural thinking to compensate.
- **The review problem:** Volume of code to review has exploded, but the harder challenge is that you can't interrogate AI's reasoning after the fact. When a human makes an odd choice you can ask why; when AI does, the reasoning is buried in a larger set of changes. Querying the agent produces sycophantic apologizing, not explanation. You review the what without access to the why.
- **Testing discipline:** If AI can't or won't test its own code (usually won't unless explicitly asked), you're flying blind on correctness. Speed advantage disappears debugging subtle issues proper tests would have caught.
- **Adaptation:** Take deliberate breaks between major design shifts (clear your own mental context); use AI as a thinking partner for design exploration, not just implementation (what's missing? what's been done? what are the tradeoffs?); front-load architectural thinking.

### Source quality notes

- This is a single-practitioner reflective essay (first-person, three months of AI-assisted development). Not a study, not controlled, not generalizable in a statistical sense. Valuable as a high-fidelity qualitative description of the mechanism the Smartsheet research quantifies. Author is Bruce Alderson (@robotpony), a developer.

## Cross-references to existing skills

- **ai-brain-fry-defense** — covers the acute overload from managing *too many concurrent agents* (BCG: 4+ agents → 33% more decision fatigue). This skill covers the *density* problem — the decision-stream itself — which is orthogonal to agent count. A developer with one agent making 80 decisions/hour can be fatigued without ever hitting the 4-agent threshold.
- **devex-verification-bottleneck-framework** — covers the verification-time > writing-time inversion and the measurement stack (DX Core 4, DSat). This skill adds the decision-density dimension and the two-gate model that the verification framework's "mitigation practices" section gestures at.
- **mental-model-erosion-defense** — covers skill atrophy from autocomplete dependency. This skill covers the acute-per-session fatigue, not the long-term skill degradation.
- **comprehension-debt-framework** — covers the understanding debt from AI code. This skill's "review-blind-spot" (what without why) is the per-review manifestation of that debt.
- **dora-ai-attribution-developer-experience-2026** — notes that senior engineers should pair with juniors to review AI-generated architectural decisions. This skill's "senior-engineer compensation pattern" (load context, make small changes) is the positive form of that observation.