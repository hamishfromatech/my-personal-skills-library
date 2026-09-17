# Flow Collapse Evidence Base

## Primary sources

### The prompt-wait-evaluate loop (Dargo, July 2026)

The core mechanism: a typical AI-assisted coding session is prompt → wait → evaluate → re-prompt. This is the opposite of flow because all three of Csikszentmihalyi's preconditions collapse at once:

- **Goals no longer clear at each moment.** When you type code yourself, you know what the next line should do. When you write a prompt, you describe what you want at a high level but have no moment-to-moment clarity about what the machine will produce. You are waiting for a surprise.
- **Feedback no longer immediate.** Instead of the tight type-compile-see loop, you get a chunk of output after a delay — seconds for chat, minutes to tens of minutes for agentic tools running in the background.
- **Challenge no longer matches skill in the right way.** Writing the prompt is often too easy; evaluating the output is often too hard (reviewing code you didn't write, built on assumptions you didn't make). The difficulty is in the wrong place.

The loop is insidious because it looks exactly like work — you are in your IDE, typing, code is appearing, commits are going in. But inside your head you are context-switching. Every prompt is a handoff; every response is an interruption; every evaluation is a cold start.

### Junk flow (Howard, AI Engineer Melbourne keynote, 2026)

Jeremy Howard pointed out that Csikszentmihalyi, the psychologist who gave us flow, also described a counterfeit cousin: *junk flow* or *dark flow*. It looks like flow but is the opposite of good for you — you get hooked on a superficial version that feels like flow at first, then slowly turns into something you are addicted to rather than something that helps you grow. Casinos manufacture this through an illusion of control.

- **Real flow:** A genuine challenge stretches your skills and you grow from it.
- **Junk flow:** You keep pulling the lever, chasing the next good result. The dopamine is in the anticipation, not the payoff.

Prompting an AI can feel like pulling that lever — sometimes it nails it, sometimes it hands you nonsense — but you keep going even when you know you should stop and do it by hand.

### The 39-percentage-point perception gap (cognitive load inversion research)

In a study of experienced developers:
- Participants predicted AI tools would make them 24% faster.
- After completing the tasks, they still believed they had been 20% faster.
- The measured reality: they were 19% slower.
- The perception gap is 39 percentage points — and it compounds with every sprint, every code review, every feature shipped.

This is the *cognitive load inversion*: AI tools offload the cheap cognitive work (writing syntactically correct code, boilerplate, function names) while generating a harder class of cognitive work (continuous evaluation of uncertain outputs). You didn't eliminate cognitive effort; you automated the easy half and handed yourself the hard half.

### Interruption recovery cost (Parnin & Rugaber, 2011)

Based on 10,000 recorded sessions of 86 programmers and a survey of 414 more:
- Only 1 in 10 interruptions let the programmer resume coding within a minute.
- 93% of sessions involved significant navigation (opening files, scrolling, searching) before editing resumed — they were rebuilding where they left off, not finding it.
- Resuming is not about finding the right file; it is about reconstructing the mental context: goals, plan, invariants, constraints already considered and dismissed.

The prompt-response cycle forces this reconstruction every single time.

### Attention-span decline (Gloria Mark, UC Irvine)

- After a single interruption it takes an average of 23 minutes 15 seconds to return to the same level of focus (not to restart the task — to reach the same depth of engagement).
- Average time on a single screen before switching dropped from 2.5 minutes (2004) to 47 seconds (2023, *Attention Span*).
- ~44% of all interruptions are self-generated. The AI prompt cycle is the latest and most productive-looking form of self-interruption.
- "We don't have work days. We have work minutes that last all day."

### The productivity-experience paradox (Vella & Blincoe, arXiv:2605.23135, May 2026)

Longitudinal study of professional software engineers over six months:
- 84% reported AI improving productivity at both timepoints — remarkably stable.
- The negative-experience cohort nearly doubled, from 14% to 27%.
- Only 37% of those who started fully positive were still fully positive by the end.
- The negative cohort was sticky — not one person who started in it climbed back to fully positive; the best managed was neutral.
- Flow state took the steepest fall: the share rating it worse nearly tripled (7% → 20%), the biggest move of the three DevEx dimensions.
- Cognitive load got worse gently (6% → 9%).
- Feedback loops improved (the only dimension that did).
- The correlation between change in flow state and change in productivity was 0.02 — about as close to zero as it gets. Experience and productivity appear to stop moving together once AI enters the picture.

Possible connection: AI hands you a response every few seconds, but each response is a context switch (read it, judge it, accept or fix it, prompt again). Feedback loops improve while flow state deteriorates — both true at once. As AI took over more of the writing, what made engineers feel productive changed: at the start it went with being absorbed in a hard problem (flow); by the end it went with how fast the code came back (feedback speed).

### Internal vs. external goods (MacIntyre, via Nicholas Gruen)

- **Internal goods** of a practice: the skill, the mastery, the satisfaction you can only get by doing the thing itself.
- **External goods:** money, status, output.

MacIntyre warned that external goods have a habit of crowding out internal ones. With AI, the external good (productivity) holds steady or climbs; the internal goods (flow, mastery, the joy of the craft) erode. AI is brilliant for external goods and potentially corrosive for internal ones. How a developer feels about AI correlates with which goods they are chasing — if external, it is a superpower; if internal, it can feel like a loss.

### "The AI vampire" (Steve Yegge)

The tool that makes you far more productive while draining you dry. When experience keeps eroding but output doesn't, nothing forces the problem to the surface — the dashboards look fine but the people aren't. The hidden strain surfaces as burnout and turnover.

## Adjacent skills cross-referenced

- `developer-experience-and-flow/supervisory-engineering-work` — the creation-to-verification shift and the three-component model of supervisory labour.
- `developer-experience-and-flow/ai-productivity-output-volume-paradox` — AI increases output volume, not time savings per task; the 27% new-work ratio.
- `developer-experience-and-flow/ai-review-fatigue-mitigation` — the convergence of vigilance decrement, automation complacency, and context-switching costs.
- `developer-experience-and-flow/verification-load-interface-design` — how the interface (inline/chat/structured) shapes verification load independent of the model.
- `developer-experience-and-flow/ai-fatigue-scale-design` — the five-dimension AI fatigue measurement instrument.
- `developer-experience-and-flow/calm-technology-ai-coding` — calm-technology principles applied to AI coding assistants.
- `cognitive-science-and-ux/spec-driven-cognitive-partnership` — interaction patterns that preserve cognitive engagement.
- `developer-experience-and-flow/developer-experience-flow-state` — foundational flow-state engineering for coding tools.

## Novelty confirmation

Grep across the existing skills corpus confirms no prior skill names the prompt-wait-evaluate loop as a distinct flow-collapse mechanism, integrates the junk-flow concept, or provides the mode-separation protocol. The `supervisory-engineering-work` skill documents the productivity-experience paradox but frames it as a role shift; this skill provides the *interaction-pattern mechanism* (the loop itself) and the behavioural + structural defence.