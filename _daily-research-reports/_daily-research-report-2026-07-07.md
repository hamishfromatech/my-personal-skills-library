# A-Tech Daily Research Report — 2026-07-07

**Date:** July 7, 2026
**Researcher:** A-Tech Research Division
**Focus Areas:** MCP economy and dual-identity product design, AI productivity output-volume paradox, boosts-vs-nudges public perception, user trust in AI (12-country study)

---

## Executive Summary

Today's research cycle identified four significant developments warranting new skill creation, spanning all four of A-Tech's primary research domains: AI agents/workflows, developer experience, behavioral psychology, and privacy/trust.

1. **MCP Dual-Identity Problem** (Shaili Guru, AI Product Management Guru, June 5, 2026) — The most clear articulation of the product-design consequence of MCP adoption: when a SaaS product exposes an MCP server, it gains a second consumer (the AI agent) with radically different traffic patterns (10-100x load), identity needs, and pricing implications. The dual-identity problem is the 2026 version of the mobile-first fork. This skill extends the existing MCP monetization cluster with the upstream product-design and identity-model dimension.

2. **AI Productivity: The Output-Volume Paradox** (Anthropic internal research, December 2025; 132 engineers, 53 interviews, 200,000 transcripts) — The most detailed internal study of how AI actually changes developer work. The key finding: AI increases productivity primarily through greater output volume (more features, more fixes, more experiments), not through time savings per task. 27% of AI-assisted work wouldn't have been done otherwise. The productivity-experience paradox shows output rising while experience declines. The paradox of supervision reveals that the skills needed to oversee AI are the same skills that atrophy from AI use. This skill adds the output-volume productivity mechanism to the DevEx measurement cluster.

3. **Boosts vs. Nudges: Public Preference** (Paunov & Grüne-Yanoff, Energy Policy, February 2026) — Empirical evidence that public preference between boosts and nudges is shaped by perceived effectiveness and criticism, not just underlying theory. Boosts are structurally disadvantaged by the perception that they require more effort (B = −0.568, p = .007, OR = 0.57). This skill adds the perception/communication dimension to the boosting skill cluster — the "how to communicate" layer that was previously missing.

4. **User Trust in AI and Major Tech Companies: 12-Country Study** (Behaviour & Information Technology, 2026) — Cross-country empirical validation of the multidimensional trust construct (privacy, transparency, competence, benevolence) and the trust gap between AI (technology) and major tech (organizations). Design and transparency are the key trust levers. The behavioral consequences of trust (engagement, data sharing, retention) create the trust-engagement-revenue flywheel. This skill adds the cross-country empirical foundation and design/transparency intervention patterns to the trust-design skill cluster.

---

## Research Phase Summary

### AI Agents & Workflows: MCP Dual-Identity Problem

**Key finding:** Shaili Guru's analysis (June 5, 2026) provides the clearest articulation of the product-design consequence of MCP server adoption. When a SaaS product exposes an MCP server, it gains a second consumer: the AI agent acting on behalf of a human user.

**The traffic asymmetry:** A human user makes 50-200 actions per day (session-paced). An agent using Salesforce via MCP might make 500 tool calls in 10 seconds while "researching a lead" before doing anything visible. Same infrastructure, same database queries, same compute, same egress — 10x to 100x the load.

**The dual-identity problem:** The product now has two consumers with different traffic patterns, different cost profiles, and different rate-limit needs. The SaaS product's pricing, identity model, and audit logs probably assume only the first one (human). Key design questions:
1. Does the product treat agent actions as the user's actions, or as separate?
2. How is cost and usage attributed between human and agent?
3. Can the system tell, from a single API call, whether a human or agent triggered it?
4. What happens when one agent's actions affect other users' rate limits?

**Three pricing models emerging:**
- **Per-call (tool invocations):** xpay charges search $0.01, analyze $0.05, generate $0.10 per call. Nevermined offers sub-cent micropayments at $0.001/transaction. Trap: rewards vendor for agent inefficiency.
- **Re-wrapped seats:** Microsoft Copilot $30/user/mo, Salesforce Agentforce $125-$150/user/mo. The seat is still anchored to a person. Doesn't fix unit economics.
- **Outcome-based:** Intercom Fin $0.99/resolution, Zendesk $1.50-$2.00/resolution. Most disruptive but defining/arbitrating the outcome is hard.

**The MCP gateway layer:** Being built by Kong, Tyk, MintMCP, Arcade, TrueFoundry, Microsoft (AI Gateway in Foundry). Handles authentication, rate limiting, cost attribution, audit logging, agent identity. The gateway is where the measurement data lives — per-agent, per-tool, per-workflow cost. CISO tools exist; CFO tools don't. This is the next big category for AI PMs.

**The mobile-first analogy:** This is the same foundational design choice as when mobile apps appeared. Some treated mobile as a thin wrapper. Some redesigned around mobile-first. The ones who redesigned won. Agent identity is the 2026 version of that fork.

**Novel vs. incremental:** NOVEL as a product-design and identity-model skill. The existing skill library has `mcp-server-monetization-2026` (four pricing models), `mcp-gateway-monetization` (gateway aggregation platforms), and `agent-protocol-stack-2026` (full protocol stack). None address the dual-identity product-design problem, the traffic asymmetry quantification, the per-agent rate-limit architecture, the four design questions, or the mobile-first transition analogy. This skill is the upstream product-design layer that the monetization skills depend on.

**A-Tech alignment:** A-Coder should expose an MCP server with separate agent identity tokens (NHI), per-agent rate limits, and cost attribution dashboards. Builder's Club marketplace should use the full identity stack. The agent cost attribution toolkit is an open-source opportunity.

---

### Developer Experience: AI Productivity Output-Volume Paradox

**Key finding:** Anthropic's internal research (December 2, 2025) studied 132 engineers, conducted 53 in-depth interviews, and analyzed 200,000 Claude Code transcripts. The central insight: AI increases productivity primarily through greater output volume, not by doing the same work faster.

**The output-volume pattern:** Across almost all task categories, there is a small net decrease in time spent but a large net increase in output volume. Engineers report slightly less time per task category but considerably more output — more features shipped, more bugs fixed, more experiments run.

**The 27% new-work ratio:** 27% of AI-assisted work consists of tasks that wouldn't have been done without AI — scaling projects, nice-to-have tools (interactive dashboards), exploratory work, papercut fixes (8.6% of Claude Code tasks). This is the hidden value that time-per-task metrics miss entirely.

**The productivity-experience paradox:** Output can rise while developer experience declines. The creation-to-verification shift (from writing code to reviewing/directing AI code) disrupts flow state — the most affected dimension (54% → 76% of negative-experience cohort across two time points in the longitudinal data).

**The paradox of supervision:** Effectively using AI requires supervision, and supervising AI requires the very coding skills that atrophy from AI overuse. Engineers describe a trust progression (Google Maps analogy) — but in Phase 3 (trust AI for everything), you stop developing the judgment needed to know when the AI is wrong.

**Claude Code usage trends (Feb → Aug 2025):**
- Task complexity: 3.2 → 3.8 (on 1-5 scale)
- Max consecutive tool calls without human input: 9.8 → 21.2 (+116%)
- Human turns per transcript: 6.2 → 4.1 (-33%)
- Feature implementation usage: 14.3% → 36.9%
- Code design/planning usage: 1.0% → 9.9%

**Novel vs. incremental:** NOVEL as a productivity-mechanism and measurement skill. The existing skill library has `supervisory-engineering-work` (the creation-to-verification shift and supervisory labor category), `ai-engineering-culture-amplifier` (organizational dynamics), `cognitive-load-reduction-for-ide` (cognitive load), and `unified-devex-measurement-stack-2026` (5-layer measurement stack). None provide the output-volume-not-time finding, the 27% new-work ratio, the paradox of supervision, the Google Maps trust progression, or the metric design framework that captures both output and experience dimensions. This skill is the productivity-measurement mechanism that the DevEx cluster was missing.

**A-Tech alignment:** A-Coder needs output-volume dashboards alongside time-per-task metrics, new-work ratio tracking, "manual mode" for deliberate practice, and skill-atrophy early warning signals. Be Practical needs modules on "Why AI Makes You More Productive (But Not How You Think)" and "The Paradox of Supervision."

---

### Behavioral Psychology: Boosts vs. Nudges Public Preference

**Key finding:** Paunov & Grüne-Yanoff (Energy Policy, February 2026, vol. 209, 114953) demonstrate that public preference between boosts and nudges is shaped by perceived effectiveness and criticism — not just by the underlying theory.

**The boost disadvantage:** Among criticism variables, higher perceived time/effort costs reduced the odds of choosing a boost (vs. a nudge): B = −0.568, p = .007, OR = 0.57. For every unit increase in perceived time/effort cost, the odds of choosing a boost decrease by 43%.

**The perception trap:** Boosts are theoretically superior for durable behavior change (they build lasting competence), but they lose the perception battle on effort (boosts require teaching a skill; nudges just change the default), speed (nudges appear faster; boosts take time), and convenience (nudges are done to the user; boosts require participation). The very features that make boosts superior (durability, transferability, agency-building) are the features that make them harder to "sell" to the public.

**The criticism asymmetry:** Currently, nudges face more criticism (the 2026 reckoning — Chater & Loewenstein's "It's On You", the replicability crisis) than boosts. This creates a window for boosting-first strategies — but only if the effort perception is managed.

**The three-stage hybrid:** Nudge to start (overcome the effort/speed perception barrier) → Boost to sustain (build durable competence) → Self-nudge to maintain (user becomes their own choice architect). This captures the strengths of both.

**Novel vs. incremental:** NOVEL as a perception/communication skill. The existing skill library has `boosting-empowering-behavior-change` (the six boost categories — the WHAT), `nudging-reckoning-precision-future` (the 2026 reckoning — the WHY), and `nudge-theory-choice-architecture` (implementing specific nudges). None address the public-perception dimension (the HOW-TO-COMMUNICATE), the boost disadvantage on perceived effort, the three-stage hybrid strategy, or the communication reframe (effort as investment, speed as sustainability, participation as empowerment). This skill is the perception layer that the boosting cluster was missing.

**A-Tech alignment:** A-Tech's boosting-first approach is theoretically superior but faces the perception disadvantage this study identifies. The response: use the three-stage hybrid (nudge to onboard, boost to build, self-nudge to sustain), reframe communication (effort as investment, speed as sustainability, participation as empowerment), and defend with the durability evidence when challenged with "just nudge them."

---

### Privacy & Trust: User Trust in AI 12-Country Study

**Key finding:** A 2026 twelve-country study (Behaviour & Information Technology, DOI: 10.1080/0144929X.2026.2619648) provides cross-country empirical validation of the multidimensional trust construct and the trust gap between AI and major tech companies.

**The trust construct:** Trust in AI is multidimensional — shaped by privacy protection, transparency, competence, and benevolence. These map closely to the 4-pillar trust model (Ability, Benevolence, Integrity, Predictability) in the existing `trust-design` skill.

**The trust gap:** Users distinguish between trust in AI (the technology — its capability, accuracy, competence) and trust in major tech companies (the organizations deploying AI — their motives, data practices, benevolence). Users may trust the technology's capability while distrusting the company's motives. This gap is the market opening for privacy-first, open-source AI companies.

**Design and transparency as trust levers:** The study highlights two controllable trust drivers:
- **Design:** User-centered design prioritizing welfare over engagement, honest interfaces, privacy-led UX, verifiable behavior
- **Transparency:** Reasoning visibility (show WHY, not just WHAT), data handling disclosure, capability honesty, decision explainability

**Behavioral consequences:** Trust in AI results in meaningful behavioral changes — users who trust AI interact more, share more data, and engage more deeply. Distrust leads to disengagement, data withholding, and workaround behavior. This creates the trust-engagement-revenue flywheel: privacy-first design → trust → engagement → retention → revenue → reinvestment in privacy-first design.

**Australia-specific context:** A 47-country study (Agile Insights, 2025) found only 36% of Australians are willing to trust AI, despite half using it regularly, and 78% worry about negative outcomes. A-Tech's home market has both high AI usage and high AI distrust — ideal conditions for a trust-first product.

**Novel vs. incremental:** NOVEL as an empirical cross-country trust study. The existing skill library has `trust-design` (the 4-pillar framework), `privacy-first-ai-pipeline-defense` (architecture patterns), `privacy-first-competitive-differentiator` (competitive positioning), and `cognitive-fluency-trust-engine` (cognitive fluency as trust driver). None provide the 12-country empirical validation, the trust gap between AI and major tech, the design/transparency as trust levers finding, the behavioral consequences of trust, or the trust-engagement-revenue flywheel mechanism. This skill adds the empirical foundation and the design intervention patterns.

**A-Tech alignment:** Privacy-first design is the core trust driver. Open-source AI logic is a transparency trust signal. The trust gap (trust the AI's competence + trust the company's benevolence) is the strategic opening for A-Tech against big-tech alternatives. The trust-engagement-revenue flywheel means privacy is not just an ethical choice but a revenue strategy.

---

## Synthesis Phase: Cross-Reference with Existing Skills

### New Skills Created (4)

| # | Skill | Category | Novelty | Key Source |
|---|---|---|---|---|
| 51 | mcp-dual-identity-problem | ai-agents-and-workflows | NOVEL (product-design + identity model) | Shaili Guru, AI PM Guru, June 2026 |
| 52 | ai-productivity-output-volume-paradox | developer-experience-and-flow | NOVEL (output-volume productivity mechanism) | Anthropic internal research, Dec 2025 |
| 53 | boosts-vs-nudges-public-preference | behavioral-psychology-and-nudging | NOVEL (perception/communication dimension) | Paunov & Grüne-Yanoff, Energy Policy, Feb 2026 |
| 54 | user-trust-ai-major-tech-2026 | privacy-and-trust | NOVEL (12-country empirical trust study) | Behaviour & Information Technology, 2026 |

### Skills Cross-Referenced (not modified)

**MCP Dual-Identity Problem:**
- `mcp-server-monetization-2026` — Four pricing models (this skill adds the dual-identity design layer upstream)
- `mcp-gateway-monetization` — Gateway aggregation platforms (this skill adds the product-design and identity model)
- `agent-protocol-stack-2026` — Full protocol stack (this skill is the product-design consequence)
- `agentic-commerce-pricing-consolidation-2026` — Outcome-based pricing market validation (this skill explains why per-seat breaks)
- `hybrid-ai-pricing-architecture` — Hybrid pricing implementation (this skill explains the agent-traffic driver)
- `agent-reputation-identity-framework` — Agent identity and reputation (this skill extends to MCP dual-identity)
- `agentic-trust-security-protocols-2026` — NHI security (this skill adds the product-design dimension)

**AI Productivity Output-Volume Paradox:**
- `supervisory-engineering-work` — Creation-to-verification shift (this skill adds the output-volume mechanism and paradox of supervision)
- `ai-engineering-culture-amplifier` — Organizational dynamics (this skill is the individual-level measurement layer)
- `cognitive-load-reduction-for-ide` — Cognitive load reduction (this skill explains why load increases despite time savings)
- `the-80-percent-problem` — Invisible 20% (this skill explains the volume increase that masks the quality gap)
- `comprehension-debt-framework` — Comprehension debt (this skill explains the supervisory cost)
- `unified-devex-measurement-stack-2026` — 5-layer DevEx stack (this skill adds output-volume and new-work-ratio metrics)
- `ai-brain-fry-defense` — Multi-agent cognitive overload (this skill explains why output rises while experience declines)
- `agentic-coding-trends-2026` — Anthropic's 2026 trends (this skill provides the internal research data)

**Boosts vs. Nudges Public Preference:**
- `boosting-empowering-behavior-change` — Six boost categories (this skill adds the perception/communication dimension)
- `nudging-reckoning-precision-future` — The 2026 reckoning (this skill adds the empirical public-preference data)
- `nudge-theory-choice-architecture` — Implementing nudges (this skill explains when to use them as hybrid first stage)
- `nudge-disclosure-transparency-effectiveness` — Nudge transparency (this skill adds the perception-of-effectiveness finding)
- `digital-nudging-ethical-persuasion` — Ethical nudging (this skill explains the hybrid nudge-then-boost approach)

**User Trust in AI 12-Country Study:**
- `trust-design` — 4-pillar trust framework (this skill adds the cross-country empirical validation)
- `privacy-first-ai-pipeline-defense` — Privacy-first architecture (this skill adds the trust-building dimension)
- `privacy-first-competitive-differentiator` — Privacy as differentiator (this skill adds the trust mechanism)
- `cognitive-fluency-trust-engine` — Cognitive fluency as trust driver (this skill adds the transparency and design levers)
- `machine-mediated-market-strategy` — Machine-mediated markets (this skill adds the trust signal dimension)
- `anticipatory-privacy-design` — Anticipatory privacy (this skill adds the trust consequence)

---

## Skill Creation Phase Summary

### Skill 51: MCP Dual-Identity Problem
- **Path:** `ai-agents-and-workflows/mcp-dual-identity-problem/`
- **SKILL.md:** 8,424 bytes. Covers the traffic asymmetry (10-100x), the four design questions, the three pricing models with real numbers, the MCP gateway layer, the mobile-first transition analogy, the 18-month PM roadmap, and the A-Tech application matrix. Cross-references to 7 existing skills.
- **references/dual-identity-design-framework.md:** 7,160 bytes. The full design questionnaire (identity model, traffic asymmetry, rate-limit architecture, cost attribution, audit trail), the three pricing models with rates, the MCP gateway platform comparison, and the mobile-first transition timeline.

### Skill 52: AI Productivity: The Output-Volume Paradox
- **Path:** `developer-experience-and-flow/ai-productivity-output-volume-paradox/`
- **SKILL.md:** 10,459 bytes. Covers the output-volume-not-time finding, the 27% new-work ratio, the productivity-experience paradox, the paradox of supervision, the Google Maps trust progression, the 8-metric dashboard, 6 interventions for experience decline, and the A-Tech application matrix. Cross-references to 8 existing skills.
- **references/anthropic-internal-research-findings.md:** 7,038 bytes. The detailed study data: usage (28%→59%), productivity (+20%→+50%), output-volume pattern, 27% new-work ratio, delegation patterns, Claude Code trends, team-specific patterns, skill transformations, social dynamics, career uncertainty, and the craft question.

### Skill 53: Boosts vs. Nudges: Public Preference
- **Path:** `behavioral-psychology-and-nudging/boosts-vs-nudges-public-preference/`
- **SKILL.md:** 8,967 bytes. Covers the Paunov & Grüne-Yanoff finding, the boost disadvantage (perceived effort/speed/convenience), the three-stage hybrid strategy (nudge to start, boost to sustain, self-nudge to maintain), the communication reframe, the context-dependent intervention choice, and the A-Tech application matrix. Cross-references to 5 existing skills.
- **references/paunov-grune-yanoff-2026-findings.md:** 5,769 bytes. The study methodology, statistical results, the perception trap, the criticism asymmetry, and the implications for behavioral policy design.

### Skill 54: User Trust in AI and Major Tech Companies: 12-Country Study
- **Path:** `privacy-and-trust/user-trust-ai-major-tech-2026/`
- **SKILL.md:** 7,120 bytes. Covers the multidimensional trust construct, the trust gap between AI and major tech, design and transparency as trust levers, the behavioral consequences of trust, the trust-engagement-revenue flywheel, four design intervention patterns, and the A-Tech application matrix. Cross-references to 6 existing skills.
- **references/trust-study-design-implications.md:** 6,693 bytes. The trust dimensions, trust gap analysis, four design intervention patterns, the cross-cultural dimension, the Australia-specific finding (36% trust, 78% worry), and the trust-engagement-revenue flywheel mechanism.

---

## A-Tech Values Alignment

| Skill | Open-Source AI | Data Privacy | Financial Freedom | Practical Implementation |
|---|---|---|---|---|
| MCP Dual-Identity Problem | ☑ Open MCP standards; agent identity as open infrastructure | ☑ Per-agent rate limits protect user data; local-first agent identity | ☑ Hybrid pricing captures agent traffic value; outcome-based for high-value | ☑ 5-step design process, gateway architecture, 18-month PM roadmap |
| AI Productivity Output-Volume Paradox | ☑ Open-source DevEx dashboard template | ☑ Local behavioral signals for experience tracking | ☑ 27% new-work ratio = hidden ROI; output volume drives revenue per engineer | ☑ 8-metric dashboard, 6 interventions, Google Maps trust progression |
| Boosts vs. Nudges Public Preference | ☑ Open-source boost library; self-nudging = user-owned architecture | ☑ No hidden tracking; boosts are transparent skill-building | ☑ Durable competence = sustainable wealth behaviors; financial literacy pays for life | ☑ 3-stage hybrid strategy, communication reframe, context-dependent choice |
| User Trust in AI 12-Country Study | ☑ Open-source AI logic = transparency trust signal | ☑ Privacy-first design is the core trust driver | ☑ Trust-engagement-revenue flywheel; trust drives retention and premium pricing | ☑ 4 trust dimensions, 4 design intervention patterns, trust gap as market opportunity |

---

## Cross-Skill Connections

The four new skills form two coherent narratives across A-Tech's domains:

**Narrative 1: The Agent Economy (Skills 51 + 52)**
- MCP Dual-Identity Problem explains the product-design consequence of agent adoption (two personas, traffic asymmetry, identity model)
- AI Productivity Output-Volume Paradox explains the human-side consequence (output rises, experience can decline, supervision paradox)
- Together: when products serve agents AND humans (51), the productivity measurement must capture both output volume and experience (52)

**Narrative 2: Trust and Perception (Skills 53 + 54)**
- Boosts vs. Nudges Public Preference explains how public perception shapes behavioral intervention adoption (the effort perception disadvantage of boosts)
- User Trust in AI 12-Country Study explains how trust shapes AI product adoption (the privacy-transparency-competence-benevolence construct)
- Together: the success of both behavioral interventions (53) and AI products (54) depends on perception and trust, not just theoretical superiority. A-Tech's privacy-first, boosting-first approach is theoretically superior but must actively manage perception and build trust to win adoption.

The new skills also connect to the broader skill library:
- The **MCP dual-identity problem** extends the MCP monetization cluster into product design and creates the foundation for the agent FinOps category.
- The **output-volume paradox** provides the measurement mechanism that the DevEx cluster (unified-devex-measurement-stack-2026, supervisory-engineering-work, ai-engineering-culture-amplifier) needs to capture AI's actual productivity impact.
- The **boosts-vs-nudges perception** completes the boosting skill triad (WHAT: boosting-empowering-behavior-change, WHY: nudging-reckoning-precision-future, HOW-TO-COMMUNICATE: boosts-vs-nudges-public-preference).
- The **trust study** provides the empirical foundation that the trust-design cluster (trust-design, privacy-first-competitive-differentiator, cognitive-fluency-trust-engine) needs to justify the privacy-first investment.

---

## Research Sources

551. **NEW:** Shaili Guru (June 5, 2026) — "The MCP Economy Is Already Here. Have you noticed it yet?" AI Product Management Guru (Substack). Dual-identity problem, traffic asymmetry (10-100x), three pricing models, MCP gateway layer (Kong, Tyk, MintMCP, Arcade, TrueFoundry, Microsoft), xpay rates, Intercom Fin ($0.99/resolution), Zendesk ($1.50-$2.00/resolution), Nevermined ($0.001/transaction), mobile-first analogy, 18-month PM roadmap. References Deloitte TMT Predictions 2026, Palo Alto Prisma AIRS (NHI framing).
552. **NEW:** Huang, S., Seethor, B., Durmus, E., Handa, K., McCain, M., Stern, M., & Ganguli, D. (December 2, 2025) — "How AI Is Transforming Work at Anthropic." Anthropic Societal Impacts research. 132 engineers, 53 interviews, 200,000 transcripts. Usage 28%→59%, productivity +20%→+50%, output-volume-not-time pattern, 27% new-work ratio, 0-20% fully delegatable, paradox of supervision, Google Maps trust progression. METR study caveat on self-reported productivity overestimation.
553. **NEW:** Paunov, Y. & Grüne-Yanoff, T. (February 2026) — "Boosts vs nudges: perceived effectiveness and criticism shape preferences for sustainable behavioural policies." Energy Policy, 209, 114953. Perceived effectiveness drives preference; perceived time/effort cost reduces boost preference (B = −0.568, p = .007, OR = 0.57); criticism reduces preference for criticized type.
554. **NEW:** "User trust in AI and major tech companies in twelve countries." (2026) Behaviour & Information Technology. Taylor & Francis. DOI: 10.1080/0144929X.2026.2619648. Multidimensional trust (privacy, transparency, competence, benevolence), trust gap between AI and major tech, design and transparency as trust levers, behavioral consequences of trust.
555. **CONTEXT:** Anthropic (January 2026) — "2026 Agentic Coding Trends Report." 8 trends across foundation, capability, impact. Already captured in `agentic-coding-trends-2026`. Cross-referenced for the 60% usage / 0-20% fully-delegatable / 27% new-work data that originates from the December 2025 internal research.
556. **CONTEXT:** Agile Insights (2025) — "AI Trust in 2025: What Australians think." 47-country study. 36% Australian trust willingness, 78% worry about negative outcomes. Supporting context for the 12-country trust study.

---

## Next Research Priorities

1. **Agent identity implementation pilot:** Design A-Coder's MCP server with separate agent identity tokens (NHI) distinct from human accounts. Implement per-agent rate limits and cost attribution dashboards. Test the hybrid pricing model (seat + usage + outcome).
2. **Output-volume dashboard prototype:** Build the 8-metric dashboard (output volume, new-work ratio, papercut fix rate, task complexity trajectory, autonomy progression, experience dimensions, full-delegation ratio, skill atrophy signals) for A-Coder.
3. **Boost perception A/B test:** Test the communication reframe (effort as investment, speed as sustainability, participation as empowerment) in Be Practical module descriptions. Measure whether the reframe increases boost module completion rates.
4. **Trust scorecard implementation:** Build the 4-dimension trust scorecard (privacy, transparency, competence, benevolence) for A-Coder. Instrument trust signals and track the trust-engagement-revenue flywheel.
5. **MCP gateway evaluation:** Evaluate Kong, Tyk, MintMCP, Arcade, TrueFoundry, and Microsoft AI Gateway for A-Tech's MCP infrastructure needs. The agent FinOps gap (CISO tools exist, CFO tools don't) is an open-source opportunity.

---

*Report compiled by A-Tech Research Division | July 7, 2026*