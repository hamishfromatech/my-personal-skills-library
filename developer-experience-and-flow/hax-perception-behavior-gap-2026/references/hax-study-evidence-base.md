# HAX Study — Full Evidence Base

## Citation & Design

JetBrains Human-AI Experience (HAX) team, presented at ICSE 2026 (Rio de Janeiro). Blog: "Understanding AI's Impact on Developer Workflows" (Katie Fraser, Agnia Sergeyuk, April 2026).

- **Telemetry**: anonymized usage logs from IntelliJ IDEA, PyCharm, PhpStorm, WebStorm. Filter: devices active in both October 2022 (ChatGPT release) and October 2024. Groups: 400 AI users (≥1 JetBrains AI Assistant interaction/month, Apr–Oct 2024) vs 400 non-users (never in study period). **151,904,543 logged events.**
- **Survey**: 62 professional developers, 5-point change scales on the same workflow dimensions + open-ended examples.
- **Interviews**: semi-structured follow-ups on day-to-day AI use, trust decisions, fragmentation.
- Rationale (from prior research): developers spend ~70% of time on comprehension; ~1/3 of time double-checking/editing Copilot suggestions; ~1/5 of initially-accepted code later deleted, ~7% heavily rewritten.

## Event Proxies

| Dimension | Proxy event |
|---|---|
| Productivity | Typed characters (count only, not content) |
| Code quality | Debugging-session starts |
| Code editing | Delete/undo actions |
| Code reuse | External paste events (paste without prior in-IDE copy) |
| Context switching | IDE window activations |

## Results Detail

### 1. Productivity — ALIGNED
- AI users: +~600 typed characters/month trend. Non-users: +~75/month. Gap widened over two years.
- Survey: >80% report slight/significant productivity increase; 2 slight/significant decrease. >50% say coding time decreased; ~15% increased.
- Interview: "When I get stuck on naming or documentation, I immediately turn to AI, and it really helps."

### 2. Code quality — DIVERGED
- Telemetry: no significant change in debugging starts for AI users; slight decrease for non-users. Lines sit close together across two years.
- Survey: ~47% say quality slightly/significantly increased; ~10% decreased. Readability: 43.5% up, 6.5% down, 50% no change.
- Interview: "I triple-check it, and even then, I still feel a bit uneasy."
- Interpretation: quality perception is not quality behavior. Feelings of quality ≠ evidence of quality.

### 3. Code editing — INVERTED (the headline)
- Telemetry: AI users +~100 deletions/month (statistically significant). Non-users: +~7/month. ~14× divergence.
- Survey: 50% report NO perceived change in editing behavior; ~40% slight/significant increase; ~7% decrease.
- Interview: "AI is like a second pair of eyes, offering pair programming benefits without social pressure – especially helpful for neurodivergent people. It's not always watching, but I can call on it for code review and feedback when needed." (system architect, 15+ years)
- Interpretation: curation workload — accepting, reworking, deciding what stays — grew massively and invisibly.

### 4. Code reuse — roughly ALIGNED (flat)
- Telemetry: AI users higher external-paste baseline; no large change over time for either group.
- Survey: ~33% increase, ~20% decrease, 44% no change.
- Interview: "For me, it's better to take responsibility for what I did myself rather than adopt a third-party solution." (15+ years)
- Surprise vs expectation: AI didn't become a dominant reuse channel in this window.

### 5. Context switching — DIVERGED
- Telemetry: AI users +~6 IDE activations/month; non-users −7/month.
- Survey: ~25% increase, ~20% decrease, ~50% no change.
- Interview: "I stopped switching contexts, saving a few seconds every time I would have googled something."
- Interpretation: in-IDE AI trades google-hops for dialogue management — a different fragmentation pattern, not fewer interruptions. Interacting with AI adds cognitive overhead as developers alternate between writing, interpreting suggestions, and managing the dialogue.

## Prior-Research Anchors (cited within)

- Developers perceived productivity increases with Copilot "despite the data showing otherwise."
- Separate study: developers believed completion time improved 20%; actually 19% slower.
- ~70% of developer time is comprehension (reading, navigating, reviewing).

## Meta-Lesson

> "AI coding assistants are quietly reshaping developer workflows in ways that otherwise can go unnoticed... combining methods matters: it reveals the gap between what feels different and what actually changes in day-to-day behavior." — "If you're building or adopting AI tools, the takeaway is simple: don't just ask whether people like them. You should look closely at what they are actually doing!"

## Companion Context (same research wave)

- **JetBrains AI Pulse survey (Jan 2026, 10,000+ devs, 8 languages)**: 90% of devs use ≥1 AI tool at work; 74% adopted specialized AI dev tools. Copilot: 76% awareness, 29% work adoption (growth stalled; 40% in 5,000+ employee companies). Cursor 69% aware / 18% at work; Claude Code 57% aware / 18% at work (6× from ~3% a year earlier; 24% US/Canada; highest loyalty: CSAT 91%, NPS 54). Codex 27% aware / 3% at work (pre-desktop-app). Google Antigravity 6% adoption. ChatGPT-for-coding 28%. "Product excellence now outweighs ecosystem lock-in."
- **Digital Applied Q1 2026 (2,847 devs, 320 orgs)**: Claude Code 28% primary (+7 QoQ) overtakes Cursor 24%; Copilot 17% primary but 58% any-use (−4). **Reviewing overtook writing: 11.4 hrs/week reviewing AI-generated code vs 9.8 writing** (review fatigue flagged in 37% of write-ins). Productivity plateau after 180 days (+34%→+37%). Token cost volatility top pain point (42%, +11 QoQ), prompt injection 31%. Agencies: 81% adoption vs 64% in-house, but 37% lower seat spend. Migration: 51% of switchers landed on Claude Code; only 4% reverse-migrated away from it.
- **GitKraken State of AI (554 devs, published July 2026)**: 96.4% adoption; 84% feel more productive but **only 20% of orgs measure it specifically**; 39% no measurement at all; 33% self-reports only → **72% running on belief, not numbers** ("the proof gap"). Agentic shift: 7.6% → 28% primary-agent delegation in 9 months. 34% run agents all workday. Agent-native teams: 62% "much more productive" vs 28% assistive. Enterprises run agents hardest (47% all-day) and measure most (no-measurement 47%→19% at scale). Four moves: baseline before scaling; maturity ladder (assistive→emerging→agent-native→agent-optimized); compare tools/models on quality+cost; instrument the agents.
- **Sonar State of Code 2026 (1,149 devs)**: 96% don't fully trust AI code correctness; only 48% always check before committing; 61% "looks correct but isn't reliable"; toil didn't shrink, it changed flavor (heavy AI users: toil = technical debt management 44% + correcting AI code 25% vs non-heavy users: legacy debugging 34%); 57% worry about sensitive-data exposure; BYOAI: 35% of top-10 tool use via personal accounts (ChatGPT 52% personal).
- **Halkwinds 2026 (758 orgs)**: 76% org-wide AI assistant deployment but only 34% can attribute audited delivery-metric change; 66% adopted AI before baselining; 63% say AI ROI is the hardest tooling ROI ever measured; DevEx now a named planning line item at 68% (from 29% three years ago).

## Synthesis: The 2026 Consensus

Across five independent studies (JetBrains HAX + AI Pulse, Digital Applied, GitKraken, Sonar, Halkwinds): adoption is settled, feeling is universal, proof is rare, and the invisible costs are specifically **curation/editing load** (HAX editing inversion; Digital Applied review 11.4 hrs; Sonar toil-shift) and **unmeasured spend** (GitKraken 72%; Halkwinds 66%/63%). The HAX study supplies the methodological foundation: self-reports diverge from behavior on exactly the dimensions that matter for ROI (editing, quality, attention), so measurement programs must instrument behavior, not sentiment.
