# Anthropic Internal Research Findings: How AI Is Transforming Work at Anthropic

## Study Details

- **Date:** December 2, 2025
- **Authors:** Saffron Huang, Bryan Seethor, Esin Durmus, Kunal Handa, Miles McCain, Michael Stern, Deep Ganguli
- **Method:** 132 Anthropic engineers/researchers surveyed; 53 in-depth qualitative interviews; 200,000 internal Claude Code transcripts analyzed (Feb & Aug 2025) using privacy-preserving analysis tool
- **Models in use at time of study:** Claude Sonnet 4 and Claude Opus 4

## Key Quantitative Findings

### Usage and Productivity

| Metric | 12 months ago | Now | Change |
|---|---|---|---|
| Claude usage (% of daily work) | 28% | 59% | +2x |
| Self-reported productivity boost | +20% | +50% | +2.5x |
| Merged PRs per engineer per day | — | — | +67% (corroborating metric) |
| Power users (>100% productivity boost) | — | 14% | — |

### The Output-Volume Pattern

Across almost all task categories:
- **Time spent:** Net decrease (small)
- **Output volume:** Net increase (large)

This suggests AI enables productivity primarily through greater output volume, not by doing the same work faster.

**The time-saved bimodal distribution:** Some people spend significantly more time on Claude-assisted tasks. Reasons:
- More debugging and cleanup of Claude's code ("vibe code myself into a corner")
- Cognitive overhead for understanding code they didn't write
- Persisting on tasks they previously would have abandoned
- More thorough testing and exploration in new codebases

### The 27% New-Work Ratio

27% of Claude-assisted work wouldn't have been done without AI:
- Scaling projects
- Nice-to-have tools (interactive dashboards)
- Documentation and testing (useful but tedious)
- Exploratory work not cost-effective manually
- Papercut fixes (8.6% of Claude Code tasks): minor quality-of-life improvements like refactoring for maintainability, terminal shortcuts

### Delegation Patterns

- **"Fully delegate" ratio:** More than half can fully delegate only 0-20% of work
- **Tasks delegated:** Easily verifiable, low-stakes, well-defined, repetitive, outside expertise
- **Tasks kept:** High-level design, strategic thinking, organizational context, "taste"
- **Cold start problem:** Intrinsic knowledge about team codebase that Claude doesn't have — the biggest delegation blocker

### Claude Code Usage Trends (Feb → Aug 2025)

| Metric | 6 months ago | Now | Change |
|---|---|---|---|
| Average task complexity (1-5 scale) | 3.2 | 3.8 | +0.6 |
| Max consecutive tool calls without human input | 9.8 | 21.2 | +116% |
| Human turns per transcript | 6.2 | 4.1 | -33% |
| Feature implementation usage | 14.3% | 36.9% | +22.6pp |
| Code design/planning usage | 1.0% | 9.9% | +8.9pp |

## Key Qualitative Findings

### The Trust Progression (Google Maps Analogy)

1. **Phase 1:** Use AI only for unknown domains (Google Maps for routes you don't know)
2. **Phase 2:** Use AI for mostly-known domains, need help with the last mile
3. **Phase 3:** Use AI for everything, even daily commute. Trust it considered all options.

**Risk:** In Phase 3, you stop developing the judgment needed to know when the AI is wrong.

### Skill Transformations

**New capabilities:**
- Becoming "more full-stack" — backend engineers building UIs, researchers creating visualizations
- Faster learning and iteration: "couple week process" becomes "couple hour working session"
- Reduced "activation energy" defeats procrastination
- Parallel exploration: running many Claude instances, each testing a different approach

**Less hands-on practice:**
- Skills atrophying as more is delegated
- Collateral learning lost (the incidental understanding built during manual problem-solving)
- Tool expertise eroding (relying on AI to tell you how to use new tools)
- Skill scaffolding removed (AI skips the easy-instance struggle that builds toward hard ones)

**The paradox of supervision:** Effectively using AI requires supervision, and supervising AI requires the very coding skills that atrophy from AI overuse.

### Changing Social Dynamics

- Claude is now the first stop for questions that used to go to colleagues
- 80-90% of questions go to Claude; the last 20% (crucial, complex) go to humans
- About half report unchanged team collaboration
- Others report less interaction, reduced mentorship opportunities
- "More junior people don't come to me with questions as often" (senior engineer)

### Career Uncertainty

- Engineers see themselves as "managers of AI agents"
- Work shifted "70%+ to being a code reviewer/reviser rather than a net-new code writer"
- Short-term optimism, long-term uncertainty: "I feel optimistic in the short term but in the long term I think AI will end up doing everything"
- "It kind of feels like I'm coming to work every day to put myself out of a job"

### The Craft Question

Engineers diverge sharply on whether they miss hands-on coding:
- **Loss:** "It's the end of an era for me — I've been programming for 25 years"
- **Acceptance:** "There are certainly some parts I miss, but overall I'm so much more productive I'll gladly give it up"
- **Reframing:** "I thought I really enjoyed writing code, and instead I actually just enjoy what I get out of writing code"
- **New preference:** Iterating with Claude is more fun because you can be more picky with feedback than with humans

## Team-Specific Patterns

| Team | Primary Claude Code Use | Pattern |
|---|---|---|
| Pre-training | Building new features (54.6%) | Running extra experiments |
| Alignment & Safety | Front-end development (7.5%) | Data visualizations |
| Post-training | Front-end development (7.4%) | Data visualizations |
| Security | Code understanding (48.9%) | Analyzing security implications |
| Non-technical | Debugging (51.5%), Data science (12.7%) | Bridging technical knowledge gaps |

**Pattern:** Teams use Claude to augment their core expertise while expanding into adjacent domains. Everyone becomes more "full-stack."

## Limitations (Acknowledged by Authors)

- Convenience and purposive sampling, possible selection bias toward engaged users
- Social desirability bias (responses not anonymous; Anthropic employees assessing Anthropic product)
- Recency bias (12-month recall)
- Productivity is hard to measure; self-reports should be taken with a grain of salt
- METR study found experienced developers overestimated productivity boost from AI on familiar codebases
- Claude Code analysis uses proportionate sampling — measures relative, not absolute, changes
- Study conducted August 2025; models have advanced since

## Source

Huang, S., Seethor, B., Durmus, E., Handa, K., McCain, M., Stern, M., & Ganguli, D. (December 2, 2025). "How AI Is Transforming Work at Anthropic." Anthropic Societal Impacts research. https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic

Also referenced in the 2026 Agentic Coding Trends Report (Anthropic, January 2026), which cites the 60% usage / 0-20% fully-delegatable finding and the 27% new-work ratio.