# Pragmatic Engineer 2026 AI Survey — Full Extraction (Part 2)

## Source

**Title:** "AI's impact on software engineers in 2026: key trends, Part 2"
**Authors:** Gergely Orosz and Elin Nilsson
**Publication:** The Pragmatic Engineer (paid newsletter)
**Date:** May 19, 2026
**Survey size:** 900+ subscriber responses
**Series:** Third and final part of a series analyzing 2026 AI survey results

## Survey Scope

The survey asked Pragmatic Engineer subscribers about:
- AI tools they use and how they use them
- What they think of AI tools
- Tradeoffs of AI tooling (positive and negative sentiments)
- Company-level adoption challenges
- Impact on codebase quality
- Impact on less experienced engineers
- AI tooling "addiction" patterns
- Changes since 2024

---

## 1. AI Tooling Tradeoffs

### Code and Output Tradeoffs
- AI tools often mean less time spent on tedious, repetitive work
- BUT this often leads to unrealistic business expectations

### Productivity Tradeoffs
- Microsoft research (May 5, 2026) claims AI expands the pool of people who can do high-value work
- Survey found similar pattern

### Software Engineering Tradeoffs
- Some respondents spend much more time in "flow state" thanks to AI (don't have to wait for peer input, can self-unblock, fewer interruptions)
- Others report the OPPOSITE: starting more tasks in parallel causes context switching that knocks them OUT of flow state
- Positives and negatives depend on: environment, individual personality traits, where users are on the AI learning curve

---

## 2. Adopting AI at Scale Is Hard

### Key Challenges
1. **Costs:** Growing concern (covered in Part 1)
2. **Usage:** Getting people to use AI tools continuously is not always straightforward
3. **Onboarding and education:** Larger companies need support to help devs make the most of tools
4. **Reviewing AI-generated output:** Code review is a particular pain point
5. **Integrating with internal systems:** AI tools more helpful when seamlessly integrated; larger companies use in-house, deeply embedded coding agents

### The Amplifier Thesis

**Key quote:**
> "AI is an amplifier, not a fixer. Good software engineering practices get multiplied. So do the bad ones. Embedding this properly in teams is exciting and important."
> — Staff+ engineer at a large company in Europe

> "I feel like AI allows both faster prototyping and increased velocity on iterations to production software; it relies on existing best practices / project templates our team already have."
> — Solutions Architect at a small company in the USA

### Pre-Existing Culture Determines Outcomes

Teams that saw benefits from AI tools **already had:**
- **Guardrails:** Testing and automation around the codebase and deployments
- **Documentation:** Recorded architectural decisions and engineering practices
- **A quality codebase:** AI agents will replicate patterns already in a codebase

### Idiosyncratic Workflows

> "It feels like AI workflows are very idiosyncratic in that some people derive (I hate this framing, but…) 10x more productivity benefit from them than other apparently equally clever, educated, and diligent developers. It feels like finding a workflow that clicks with your own habits and heuristics is more important than finding a global optimum for everyone."
> — Senior engineer at a large company in Canada

### The Principal Disconnect

> "I use AI in what I think is probably a more sophisticated way than most of my colleagues, so there can be a disconnect between my work and theirs, which is not good news because I am 'The Principal' on the team."
> — Principal engineer at a large US company

### Tooling Chaos

> "We're still trying to figure out how to deal with tooling consistency on a team level. It's one of our biggest struggles, but possibly more due to company structure than anything else. Everyone is using different tools with little coherence. It's been rough."
> — Staff+ engineer at a 200-person business in the Middle East

### Rollback Cases

> "Since the AI boom, the quality of technical writing and reasoning from senior engineers in my org has significantly deteriorated. There's an overwhelming volume of low-quality work product that is generated entirely or in part by AI, which has made it very difficult to conduct meaningful review of RFCs or code. We've also seen costly production incidents caused by code written and/or approved by AI, and – while my employer initially bought heavily into the hype – we have now rolled back some of our AI tools to deal with the drop in quality."
> — Engineering lead at a 10,000+ person company in Europe

---

## 3. Impact on Codebase Quality

### Contributing Factors
1. **"AI slop":** More low-quality code generated — duplicated, verbose, poor abstractions
2. **Too many code reviews:** Review quality slips under volume
3. **More bugs:** Faster code output + less strict reviews = more bugs in codebases

### CTO Negative Assessment

> "A lot of tiny bugs and low code quality if you are not careful, verify carefully, and have good structure and guardrails. AI agents generate too much and repetitive code, making systems harder to maintain. Developers lose understanding of the codebase and become numb to bad architecture and bad developer experience."
> — CTO at a European startup

### Management Indifference

> "In our company, we hand AI tools to inexperienced engineers who can't distinguish good code from bad code and it's falling on deaf ears in our leadership. They only seem to care about short to mid-term cost savings."
> — Principal DevOps engineer at a large European company

### Maintenance Burden Concentration

> "'Drive by' contributions are up: many more occasional non-core-engineer contributors adding code but not sharing the maintenance burden. Contributing without adding guardrails: many engineers and most of engineering leadership are not using reasonable guardrails like tests. AI slop from folks who have nothing to do with the codebase: huge volume of slop incoming from people who don't understand the codebase, but will commit and create PRs without fully understanding what they're doing. Complexity is exploding: thanks to the above."
> — Staff engineer at a European company

> "The maintenance budget is falling upon fewer devs, while the task of refactoring bloated codebases and reducing complexity is left to those still sufficiently in touch with the codebase, thereby making the maintenance burden even worse."
> — (Summary from survey)

### The CEO Who Gets It

> "While AI has made generating code 'cheaper', the monitoring and maintenance worry me; the things that have traditionally cost the most in software. We're increasing the rate of shipping large amounts of code with less understanding and increasing the unpredictability, so how do we work the predictability back on top?"
> — CEO at a 20-person company

### Industry Pressure Dynamics

Factors driving blind AI adoption:
- Seeing actual benefits at other teams/companies
- Fear of being left behind by competitors
- Anxiety about investor interest if not adopting latest tools

This leads to:
- Top-down mandates to use AI
- Expectation of headcount reduction with smaller teams
- Management treating AI productivity gains as baseline, not bonus

> "AI is part of almost every work conversation. The entire company expects it to increase productivity and reduce the need to hire people. I keep trying to get better at using it and trying to make it more reliable so I can do more. I do worry about the quality of the work and atrophy of certain skills. It's unclear to me if those skills even matter anymore."
> — Staff engineer at a 10,000-person US company

### Red Flags for Blind Adoption

- Focus on tracking AI usage, but NOT quality of output
- Pushing for universal adoption (50%, 80%, 100% targets) blind to quality effects
- Focus on velocity without recognition of quality work
- "Move fast and break things" mantra spreading

> "I see a trend: move fast and break things, and end up breaking things too often. We have to learn to focus on testing and resiliency a lot more, as with AI-driven development we introduce more bugs than before. But the velocity gain is bigger for now."
> — Senior manager at a large, European-headquartered company

### Death of Code Review

> "We're at the death of code review. I used to do very deep code reviews where I'd take the time to understand the architecture and organization and provide feedback on maintainability and efficiency. I have no motivation in spending that time to review a giant PR where it's clear that even the original author didn't bother to do that."
> — Lead engineer at a small company

---

## 4. Less Experienced Engineers and AI

### The AI-Native Generation

> "I have never worked as a developer without AI. Writing this scares me a bit, actually, but it's the truth!"
> — Young engineer working at a startup as an intern

### The Amplifier Mirror

> "Agentic AI is a fascinating mirror. It can code as well as the user who drives it. If that user is a junior engineer, now you have a faster junior engineer. If the user is a staff engineer, now you have a faster staff engineer. What agentic AI doesn't do is magically convert a junior engineer into a staff engineer, because the user driving it still needs enough experience to know what a good solution looks like."
> — Staff engineer in the US at a large company

### The Junior Experience

> "I think AI agents are great for vibe coding or prototypes where the code quality and functionality doesn't matter that much. I think it's also useful for senior engineers who know what they're doing. For junior engineers like myself, these AI tools are stressful to use. I don't have the experience or knowledge to tell AI exactly what to do or quickly confirm its output, so I spend a lot of time on just triple checking and redoing stuff. I'm overall frustrated, but I'm trying to embrace it as we've been asked to by the company."
> — Junior engineer in Australia

### Higher Token Bills for Juniors

- Less experienced engineers use more AI tokens and rack up higher bills
- Director-level respondents noted junior engineers are in top-spender category
- Junior devs spend tokens on unproductive use cases

### The Delegation Squeeze

> "Companies need to give some breathing room to Junior engineers and help them learn and acquire knowledge using AI tools as a booster and not as a replacement."
> — Staff engineer respondent

Seniors delegating to AI instead of juniors:
> "AI allows me to have work done that I would usually delegate to a junior or pay a SaaS for; e.g., writing drafts, summarizing the news."
> — DevSecOps lead at a small company, Europe

> "I've begun to automate any repetitive task that we previously relied on juniors and offshore contractors for."
> — Engineering manager at a large company, US

> "I no longer have to delegate work by writing a very long document and briefing a junior engineer."
> — Principal engineer, large company, Europe

---

## 5. AI Tooling "Addiction"

### The Slot Machine Pattern

- Rapid feedback loops of AI-assisted development create addictive tendencies
- Noteworthy presence of "addiction lingo" in survey responses
- Using AI agents "feels like a slot machine"
- Encourages "just one more prompt"-type behavior
- Some think pricing plans are "built in a way to 'lure' them to prompt more and more"

---

## 6. Changes Since 2024

- Fewer devs are negative about AI
- But there's not all that much more positivity
- Models have become much higher quality
- Better tooling improves trust
- (Full details behind paywall)

---

## The Shadow Superpower (from related research)

Singhal (April 2026, "Why Half of Product Managers Are in Trouble"):
- Competence in the old system is the primary obstacle to adopting the new one
- The better someone mastered the old system, the harder they find reinvention
- "Your expertise in the previous model becomes the primary obstacle to adopting the next one"
- The ones most at risk are not laggards but "accomplished operators who built their identity around doing the old thing very well"
- Predicts companies will shed 30,000 people and rehire 8,000 — but the 8,000 will be entirely AI-first
- The "information mover" archetype is being eliminated; builders are in record demand

---

## Related Survey Parts

**Part 1:** "The impact of AI on software engineers in 2026: key trends. Part 1"
- Concerns about mounting AI costs
- More engineers hitting usage limits
- AI tools having uneven effects on different types of engineers
- Review fatigue and quality burden

**Part 3 (this article):** Tradeoffs, company-level adoption, codebase quality, junior engineers, addiction, changes since 2024.

---

## Key Citations

1. Orosz, G. & Nilsson, E. (May 19, 2026) — "AI's impact on software engineers in 2026: key trends, Part 2." The Pragmatic Engineer. 900+ subscriber survey.
2. Microsoft (May 5, 2026) — Research claiming AI expands pool of people who can do high-value work (Microsoft 365 Copilot chat usage).
3. Singhal, N. & Rachitsky, L. (April 25, 2026) — "Why Half of Product Managers Are in Trouble." The shadow superpower concept.
4. Orosz, G. (2026) — "The impact of AI on software engineers in 2026: key trends. Part 1." The Pragmatic Engineer.

---

## A-Tech Values Alignment

| Value | Alignment |
|---|---|
| **Open-Source AI** | Open-source AI tools with culture-aware features are inspectable and community-improvable; proprietary tools that amplify dysfunction are opaque |
| **Data Privacy** | The amplifier thesis means privacy-violating practices get multiplied too; A-Coder must be privacy-first to avoid amplifying data exposure |
| **Financial Freedom** | The junior engineer squeeze threatens career mobility; A-Tech must build tools and education paths that preserve the apprenticeship layer |
| **Practical Implementation** | The six pathologies, the culture assessment, and the five principles provide a complete implementation framework for org-level AI adoption |