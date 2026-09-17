# Review-Overtakes-Writing — Full Evidence Base

## Primary Source

Digital Applied, "AI Coding Tool Adoption 2026: Developer Survey Results" (n=2,847 developers across 320 agencies and in-house engineering teams; fieldwork Jan 10–Mar 28, 2026; 20 tools evaluated; 20.3% qualified response rate from ~14,000 invited; weighted by role and team size).

### Workflow Time Table (medians)

| Workflow | Median hrs/week | YoY change | "Largest sink" share |
|---|---|---|---|
| Reviewing AI-generated code | **11.4** | +31% | 38% |
| Writing new code with AI | 9.8 | +8% | 29% |
| Debugging with AI assistance | 6.1 | +14% | 17% |
| Refactoring existing code | 4.7 | +22% | 10% |
| Writing documentation/tests | 3.3 | +18% | 6% |

- "Reviewing overtook writing as the single largest AI-assisted time sink in Q1 2026 — a reversal from our Q4 2024 survey when writing held a four-hour lead."
- Heavy agentic-tool users: review hours climb to 14–16/week while writing stays flat or drops modestly.
- Hidden cost: "review fatigue as an underreported productivity drag. When the AI produces more code than a developer can meaningfully review, teams either merge under-reviewed work or queue PRs indefinitely. Both failure modes surfaced in 37% of long-form responses."

### Adoption Context (same survey)

- Primary tool: Claude Code 28% (+7 QoQ, first time ahead), Cursor 24%, Copilot 17% (−4; still 58% any-use), Codex 11%.
- Role splits: backend Claude Code 34%; frontend Cursor 31%; DevOps Copilot 28%; data/ML Copilot 25% + Gemini 18% (workflow fit beats benchmarks).
- NPS: Claude Code +58, Cursor +51, Copilot +14.
- Productivity: +34% at 60 days, +37% at 180 days (plateau); gains concentrate in boilerplate (78%), tests (64%), unfamiliar languages (59%); losses in architecture (18%) and production incident debugging (21%).
- Pain points: token cost volatility 42% (+11 QoQ, top), prompt injection 31% (+9), onboarding friction 27%, model reliability 24% (−7 — tools stabilized), review burden 22% (+6).
- Agencies vs in-house: 81% vs 64% adoption; 2.4 vs 3.1 tools; $63 vs $100 seat spend; formal AI policy 34% vs 58%.
- Predictions: async agents to 15–18% primary share by Q3; IDE-native consolidation; pricing experiments accelerate (hybrid seat+usage, capped enterprise agreements, flat-rate volatility-hedge tiers).

## Corroborating Sources

### Sonar State of Code 2026 (n=1,149; fieldwork Oct 2025)
- 42% of committed code is AI-generated/assisted (predicted 65% by 2027; 6% in 2023). 72% of AI-tryers use daily.
- Verification bottleneck framing: 96% don't fully trust functional correctness; only 48% always check before committing; 61% "looks correct but isn't reliable"; 61% "requires a lot of effort to get good code."
- 95% spend some effort reviewing; 59% rate effort moderate/substantial; **38% say reviewing AI code requires MORE effort than reviewing human code** (27% less).
- Trust gap skill ranking: reviewing/validating AI code for quality+security 47% > prompting 42%.
- Toil: 75% believe AI reduces toil, but toil share of week unchanged (~24%); heavy users' toil = technical debt 44%, correcting AI code 25% (vs 15%/25% for light users); light users' toil = legacy debugging 34%.
- 88% report ≥1 negative AI impact on tech debt (looks-correct-but-isn't 53%, unnecessary/duplicative 40%); 93% report ≥1 positive (docs 57%, tests/debugging 53%).
- Static analysis: 70% use; 57% apply to AI code; expected value growth 60%→68% over two years. Non-SonarQube-users 80% more likely to report AI-linked outage increases.
- Security concerns: 57% sensitive-data exposure; 47% new/subtle vulnerabilities; 44% severe vulnerabilities; only 28% use agents for security patching (the gap between concern and use).
- Experience gap: juniors 40% productivity lift vs seniors 32%; juniors 66% "looks correct but isn't" vs seniors 48%; juniors 40% review-harder vs seniors 29%; juniors more worried about deskilling (50% vs 41%).
- SMB vs enterprise: SMBs 39% productivity (vs 34%) but more negative impacts (67% looks-correct vs 56%) and more AI-correction toil (28% vs 17%); enterprises more rigorous (39% compliance vs 28%) and higher code-quality/maintainability gains.
- BYOAI: 35% of top-10 tool usage via personal accounts; ChatGPT 52% personal; Perplexity 63% personal; Copilot/Amazon Q ~17% personal (top-down rollouts).

### GitKraken State of AI in Engineering (n=554; published July 2026)
- 96.4% adoption; 84% feel more productive (43% "much more"); <5% slower.
- **The proof gap**: 84% feel faster but only 20% of orgs measure specifically; 39% no measurement; 33% self-reports → 72% on belief.
- Agentic shift: 7.6% (Sept 2025) → 28% (June 2026) primary delegation; 2/3 run agents at least sometimes; 34% all workday (47% enterprise; 2% around the clock).
- Maturity ladder: assistive 28% "much more productive" → emerging 44% → agent-native 62%.
- Maturity and measurement move together: agent-native 34% track metrics vs 10% assistive; no-measurement 54%→28%.
- Tool choice as maturity signal: Codex/Cursor users ~45% parallel agents; Copilot/ChatGPT users lowest; Claude Code between. Claude Code leads small (63%)/mid (57%); Copilot leads enterprise (74% vs 56%) — procurement ≠ agentic reality.
- Four moves: baseline before scale; maturity model; compare tools/models on quality+cost; instrument the agents (cost, cycle time, review burden, durability).

### Halkwinds Software Engineering Productivity Benchmark 2026 (758 orgs; published Aug 8, 2026)
- 76% org-wide AI assistant deployment (41% in 2024) but only 34% attribute audited delivery-metric change.
- 22% Elite/High on all four DORA metrics; 31% have documented metrics→business-outcome bridge.
- 66% adopted AI before establishing pre-adoption baselines; 63% say AI ROI is the hardest tooling ROI measured in five years; renewals driven by sentiment.
- 68% treat DevEx as named planning line item (from 29% three years ago).
- Cited third-party anchors: GitHub/Microsoft 55% task speedup (narrow task); METR 2025 RCT: experienced OSS devs ~19% slower on their own repos despite believing +20–24%; GitClear churn/duplication rise correlated with AI usage.
- Recommendations: enterprise (DORA/SPACE baselines; route by task type; fund platform engineering as headcount discipline; board-visible metrics bridge; AI-code governance policy; monitor churn/duplication) and SME (one assistant + one review tool; lightweight DORA; golden-path templates; recurring short sentiment survey; **explicit review-time budgets for AI-generated changes**).

### JetBrains HAX (ICSE 2026) + AI Pulse
- The perception-behavior divergence foundation (see companion skill): AI users +100 deletions/month vs +7 non-users while reporting no editing change; quality perception up while debugging behavior flat; context switching up while perceived down.
- AI Pulse (10,000+ devs): 90% use AI at work; 74% specialized tools; Claude Code 6× YoY work adoption, CSAT 91% NPS 54; "product excellence now outweighs ecosystem lock-in."

## Cognitive-Science Synthesis

1. **Flow inversion**: writing-flow (challenge-skill balance, immediate feedback, intrinsic reward) is displaced by review-shaped days (interrupt-driven, deficit-framed, fragmented). The scarce commodity moves from generation flow to sustained judgment blocks.
2. **Where learning happens**: the review bottleneck is now the primary site of skill formation — but only diagnostic review teaches; approving review atrophies both reviewer and author. Juniors at the bottleneck without expert scaffolding reproduce the ETH agency-allocation risks (over-reliance / defensive resistance).
3. **Verification as the scarce skill**: Sonar's 47% ranking + GitKraken's instrument-the-agents + Halkwinds' review-time budgets converge: the industry's binding constraint is now review throughput and quality, not authoring speed.
4. **The measurement implication**: because self-report diverges from behavior (HAX), review burden must be instrumented (queue depth, latency, rework-after-review), not surveyed.

## Content Angles

- "Your developers now spend more time reviewing AI code than writing it — here's what that changes"
- The review-debt ledger: how under-reviewed AI code becomes next year's technical debt (GitClear churn data)
- Diagnostic vs approving review: turning the bottleneck into the best learning loop in software
- The verification stack: deterministic first-pass + human semantic review + agent self-verification (ties to verifiability-driven-automation skills in the library)
