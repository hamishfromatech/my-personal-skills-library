# Evidence Base: Junior-Senior Agency Allocation

## Source
Feng, Yun, Wang (Independent Researcher, ETH Zurich, ETH Zurich). "From Junior to Senior: Allocating Agency and Navigating Professional Growth in Agentic AI-Mediated Software Engineering." arXiv:2602.00496v2, February 11, 2026.

## Study Design

### Three-Phase Qualitative Study
- 20 professional software engineers total: 10 juniors (≤1 year full-time), 10 seniors (≥5 years + ≥1 year advanced role)
- Convenience and snowball sampling via LinkedIn and referrals
- ETH Zürich Ethics Committee approved (Project 25 ETHICS-191)

### Phase 1: Senior Tacit Knowledge Elicitation (5 seniors)
- 60-minute semi-structured interviews + ACTA (Applied Cognitive Task Analysis)
- ACTA framework: Task Diagram, Knowledge Audit, Simulation
- Decomposed domain-relevant scenarios into steps; identified cognitively demanding judgments
- Delphi-inspired consensus-building: anonymized task descriptions circulated for ranking
- Final task: React debugging with three bugs (service worker routing, nested useEffect, window.href.location)
- Selected because it "requires juniors to apply tacit knowledge of React effects and service workers in a realistic debugging scenario"

### Phase 2: Junior AI-Assisted Debugging (10 juniors)
- 65-minute interviews with Cursor (Agent and Ask modes)
- 4 female, 6 male, aged 22-24, US-based
- React-based admin panel with three bugs (no console errors)
- No think-aloud protocol required for natural workflow
- Postmortem written without AI assistance (after first participant used Cursor, procedure adjusted)
- NASA-TLX and SMEQ workload scales
- Semi-structured interviews on over-reliance, learning, professional/emotional implications

### Phase 3: Senior Artifact Review (5 different seniors)
- 60-minute interviews reviewing anonymized junior artifacts
- All male, aged 31-55, US/Canada-based
- Each senior reviewed two juniors' artifacts (code, postmortem, prompt history, interaction statistics)
- Live cognitive walkthroughs
- Assessed code quality, reasoning processes, accurate/erroneous assumptions

### Participant Demographics

**Phase 1 Seniors (S1-S5):**
- S1: 9 yrs total / 1.5 sr, Big Tech/Cloud & AI, Sr Software Engineer
- S2: 12 yrs / 6 sr, FinTech, Lead Software Engineer
- S3: 5 yrs / 3 sr, Design Tools, Senior Applied Scientist
- S4: 13 yrs / 2 sr, DevTools startup, Senior Software Engineer
- S5: 13 yrs / 4 sr, Enterprise SaaS, Lead Developer

**Phase 2 Juniors (J1-J10):**
- All ≤1 year full-time experience
- Company types: FinServ startup, FinTech, Enterprise SaaS, Big Tech, HealthTech, FinMedia, Investment, Payments, Consulting
- AI tool usage: Cursor multi/day (most), Copilot multi/day, Gemini reviewer

**Phase 3 Seniors (S6-S10):**
- S6: 25 yrs / sr, FinTech, Lead Software Engineer
- S7: 13 yrs / 8 sr, Social/Consumer, Staff Software Engineer
- S8: 18 yrs / sr, Big Tech/Cloud & AI, Staff Software Engineer
- S9: 26 yrs / sr, Insurance/Health, Senior Software Developer
- S10: 8 yrs / 6 sr, FinTech, Senior Software Engineer

## Key Findings

### RQ1: Agency Allocation

**Company rules preconfigure AI agency boundaries:**
- Formal restrictions: approved internal tools, prohibited non-company AI, data protection
- Some require specific tools (Cursor); others have no clear policies
- "Use AI now" top-down push: "subtle loss of agency... delivered matter-of-factly rather than as a choice" (S8, S5, S4, S1, J9, J7)
- Tacit role capabilities limit AI use: infrastructure conventions, tooling, history

**High familiarity — seniors maintain control:**
- Detailed delegation: scope the change, delegate small units, insist on minimal reviewable diffs
- "I'm usually already going in with the idea... it might not survive implementation, that's fine." (S10)
- Seed context: feed exemplar code, ask model to match patterns
- Joy-based delegation: "If you don't like writing tests, throw that to the AI" (S2)

**High familiarity — juniors use constraint-based approaches:**
- "I wouldn't use an AI to do something that I don't think I could actually do." (J6)
- Break work into sub-steps: "absolutely sure of what's going into your code before you approve it" (J9)
- Scope files first, code known fix by themselves when clear
- Crosschecking routine (Google, second model, coworkers, exemplar code)
- "it started spiraling... I just stopped it. The fix was a three-line change" (J6)

**Low familiarity — seniors separate design from code generation:**
- Sensemaking: understand code-to-interaction mapping, pros/cons, diagrams, templates
- "can you show me the code path" (S4) — confirm expectations, don't ask open-endedly
- AI for library syntax recall, small snippets, JWT generation
- Guardrail prompt: add tests indicating changes are harmless

**Low familiarity — juniors oscillate:**
- Over-reliance: "when you're coding with agents, it's like you're just free-falling... I could see myself easily pressing... spamming the agent button" (J10)
- "I trust Cursor more than I trust themself until they gain context" (J3)
- Outage dependence: "Copilot was down... I feel like I've become a lot more reliant on AI" (J2)
- Defensive resistance: reject all agent suggestions, refuse giant diffs, ask other engineers
- Time shapes control: "if I had a lot more time" they wouldn't use AI (J2, J7, J5, J9)

### RQ2: Professional Growth

**Seniors developed foundational instincts pre-AI:**
- "The closest thing you had to AI autocomplete was just IntelliSense" (S3)
- Trial-and-error: "you have to take down the database in production so you know not to do it" (S2)
- Mental map building: "get an intuition of how all these pieces connect" (S7)
- Scoping failures, insufficient observability as felt red flags
- "I can just look at something... and go 'this isn't gonna work very well'" (S2)

**Seniors frame AI as one tool:**
- "always there, but never in charge" (S6)
- "a faster, more interactive Google... and a rubber duck" (S6)
- "create messes faster so they can learn from them" (S6)
- "the AI's not handling the high level problem solving" (S10)
- "I feel like junior engineers just have no intuition... I am routinely pinged... with zero follow-ups on a speculation" (S7)
- "much of junior work can be replaced with AI" (S8)

**Juniors: speed gains but ownership struggles:**
- "the whole time I've been... a software engineer, there has been AI... I don't really have a point of comparison" (J4)
- Imposter syndrome: "It has my name on it, but I have no idea why it works" (J8)
- "Cursor did the work... I just tried to find the problem" (J3)
- "like a fraud" (J9, about hackathon Cursor use)
- "it's kind of like if someone gave you like all the answers to test" (J2)
- Accomplishment when involved in design; diminished when even design delegated: "doesn't feel good when you finish something really quickly... you're like, I just clicked on AI" (J8)

**Confidence changes (P2 task):**
- "Confidence in ability to code without AI": Pre-Task 3.90±0.88 → Post-Task 3.30±0.82 (Cohen's d=0.71)
- 6 of 10 decreased; all three initial level-5 raters decreased
- "Confidence in evaluating AI output": converged toward level 4 (7 of 10 post-task vs 2 pre-task)

**What makes a senior:**
- Senior diffs smaller and safer: "the smaller your changes are, the less likely it's going to lead to issues" (S5)
- Juniors own low-ambiguity refactors; seniors reshape systems
- Big-picture thinking: how one change affects another, understanding how elements fit
- Anti-pattern: juniors shipping large PRs
- "coding is the easiest part" (S1); seniors set team direction
- Non-code skills increasingly important: "soft-skills" over "hard-skills" in AI age

**AI for learning:**
- Seniors: AI as always-on mentor — "anytime I need to understand something, I just go back and forth with Claude" (S4)
- "don't take it at face value. Do your research" (S2)
- "did you read the whole answer before you copied the code... we didn't do that. We won't do that now either" (S7)
- "at no point can you hand over your expertise. You're just handing over the workload" (S4)

**Juniors navigate learning tension:**
- "learn by doing" — copy-pasting not helpful (J2, J5, J6, J9)
- Shifted from YouTube crash courses to prompting for "really small example" (J1)
- Ask AI to explain reasoning: "why it chose something" (J8, J9)
- Tech-debt concern: "this is where I need to just learn as many things as possible" (J7)
- "critical thinking muscle" atrophying (J1, J3, J8, J9)
- While agent buffers: "what I do is go to Instagram" (J3)

### RQ3: Mentorship

**AI as accessible fallback mentor:**
- "can help show them some patterns which juniors are not aware of" (S7)
- AI replaced question-answering from seniors (S4)
- "He trusts ChatGPT to teach me the same way that he would teach me" (J10, recalling mentor)
- Juniors ping teammates less; AI for basic questions (J1, J2)

**When AI cannot mentor:**
- Seniors as critical guardrails against AI context blindness
- "AI can confidently lead a junior to a solution that they are not looking for" (S1, S6)
- Senior mentorship-by-questioning: ask juniors why they chose approach, what they expect next
- "Today's models tend to agree uncritically and contribute to loss of critical thinking" (S10)
- Code review fatigue: "AI can generate tons of code... beyond the point where a senior engineer might implicitly trust" (S4)

**Juniors recognize AI limitations:**
- "when the reason for a decision is sitting in someone's head, I can't expect to get that answer from an AI" (J7)
- "the original author of a piece of code should be able to explain it... way better and faster" (J4)
- AI code reviewer: "doesn't catch things, and in terms of more complex design, it will turn up a lot of false positives" (J9)
- "I enjoy talking to people more than I like reading a big wall of text" (J7)

### RQ4: AI Records and Mentorship

**Senior prompt review feedback:**
- Start with understanding: S7 favored J4's Ask mode to understand first; S8 wanted juniors to explain and defend agent's proposed change
- "prompting spiral": "after the fifth try, the issue's still not fixed" (S8 on J10)
- Tighten prompts: merge early prompts, add detail (S8 on J9)
- Small testable batches: S10 praised J8's small prompts; S7 liked J4's clean reviewable diff
- Proper tool use: pair AI with grep, debuggers, logs — "know the tools available to you" (S10)
- Constant vigilance: "read the diffs meticulously to see if AI changed anything unexpected" (S7 on J4)

**Seniors infer competence from prompts:**
- S6: "I think they were feeling overwhelmed with the codebase" (from "What does handleDateRangeChange do?")
- S10: J6's natural-language prompting hinted at React familiarity vs J8's smaller technical prompts
- S7: J4 "seemed pretty comfortable with agent mode... did pay attention to what fixes were being made"

**Prompt histories as mentorship windows:**
- S10, S4, S9: prompt histories give window into junior thinking without live pairing
- S6, S1: prefer pairing and co-using AI
- S10: prompt review valuable for sharing effective prompt examples
- S8: company tracks prompts in shared Google Doc due to poor tool history
- S7: likened prompt scrutiny to questioning Stack Overflow use (disagreed)
- S7: "accept, accept, accept is a very different thing versus generating all that content and then not actually reading it" (on timing data)

## The Three Evolving Practices

### 1. Preserving Individual Agency
Company-wide and personal practices for using AI tools:
- Incremental changes
- Interrupting and verifying outputs
- Three non-negotiables: interruptibility/override, legible provenance with verification, small test-bounded diffs
- "To remain autonomous is to be the author of one's reasons, not merely the approver of outputs"
- AI-generated code is still your own code; you must be accountable
- Authorship without understanding undermined fulfillment; shaping intent and using AI as labor restored achievement

### 2. Evolving the Mentorship Pipeline
- Senior role: "Socratic guides and organizational anchors"
- Shift from answering coding questions (AI does this) to asking right questions that develop junior judgment
- Provide irreplaceable context about organizational dynamics and unwritten rules
- Junior growth: "earning judgment through deliberate restraint"
- Develop intuition for when not to delegate, when to trust instincts, when to seek human guidance
- Pipeline: from gradual responsibility increase → "immediate accountability with guardrails"
- Juniors engage with production immediately but with accountability mechanisms like prompt reviews
- "AI as another abstraction layer, similar to the transition from assembly to high-level languages"

### 3. Prompt & Code Reviews (PCRs)
- Accountability preserves agency
- When engineers document and defend prompting strategies, they remain authors of reasoning even when AI generates code
- Components: (1) problem framing and expected outcomes, (2) key prompts and interaction modes, (3) brief human-written rationale
- Selective, lightweight artifacts focusing on critical decision points
- Juniors self-curate 2-3 key prompts; automation filters sensitive/exploratory content
- Even brief 12-minute senior reviews identified over-reliance patterns, suggested better prompting, caught incorrect AI suggestions accepted

## Cross-Domain Connections

- **Assistant-to-Agent Brownfield Onboarding** (Appelt & Glauben): 61.7% faster but shift to passive supervision — aligns with junior over-reliance pattern
- **Cognitive Engagement Decline** (Catalan et al.): Engagement drops across task phases — aligns with "I'm Not Reading All of That" pattern
- **Comprehension Debt Framework** (multiple): Agent users score lower on comprehension — directly connects to junior imposter syndrome
- **Agentic Coding Returns to Expertise** (Hitzig et al.): Domain expertise drives success — supports senior "foundational instincts" finding
- **SE Agent Building Practice** (Lyu et al.): Comprehension debt and productivity metrics breakdown — connects to junior ownership struggles

## A-Tech Alignment

- **Open-source AI**: Findings apply to open-source agents (Cline, OpenCode); on-device models enable privacy-preserving agent use for sensitive codebases
- **Data privacy**: Company policies preconfiguring agency (Layer 1) align with data sovereignty; local-first AI tools preserve both agency and privacy
- **Financial freedom**: 61.7% time savings from agent use reduces onboarding cost; PCR process is lightweight enough for solo founders mentoring early hires
- **Practical implementation**: PCR process immediately implementable; interaction pattern taxonomy provides self-assessment tool; agency allocation framework is actionable for team leads

## Limitations
- Scenario-based design; juniors and seniors completed different tasks
- Small sample (20 participants), concentrated in USA/Canada
- Snapshot in time (summer 2025); rapidly evolving field
- Product-type manipulation (dress vs T-shirt) may differ in hedonic value, formality, femininity
- Self-report bias; social desirability may affect agency reporting
- Cross-cultural differences unexplored