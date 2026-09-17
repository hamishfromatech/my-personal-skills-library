# Daily Research Report — 2026-08-18

## Executive Summary

Today's research cycle identified **4 novel findings** across three research domains, resulting in **4 new skills created** (plus 4 reference documents). The findings span the first production-scale characterization of AI coding-agent workloads (GitHub Copilot, 13.5M sessions), the first empirical study of personalized vs generic coding-agent skills, the first large-scale agentic AI adoption analysis from OpenAI Codex, and a real-world reality check on LLM-personalized advertising through Meta's ad platform. All findings represent genuinely new empirical contributions not previously in the skill library.

---

## Research Phase Findings

### 1. Developer Experience & Flow

**Novel Finding: Agentic Coding Production Characterization (Liu et al., UIUC/Microsoft Azure Research, arXiv:2608.00101, August 2026)**

The first production-scale characterization of AI coding-agent workloads, using sampled GitHub Copilot traces from June 2026 (3.2M users, 13M sessions, 761M LLM calls, 95T tokens). This is the largest empirical study of real-world coding agent behavior to date.

- **1:1 LLM↔Tool coupling**: 87% of LLM calls are agent-initiated; near 1:1 ratio of LLM calls to tool invocations. Agentic coding is structurally distinct from chatbot serving — entire agent execution chains must be scheduled, not individual requests.
- **Session-structured KV cache**: 90% cache hit within turns, dropping to 55% at turn boundaries and 8% after model switches. KV cache is a session-aware schedulable resource.
- **Context compaction**: 7.8% of sessions, 44% of tokens; drops 72.8% of prompt tokens, resets cache as severely as a model switch (median -66.1% hit rate).
- **Tool failures amplify compute**: 9% of turns → 4× compute via autonomous retry loops with growing context windows.
- **Five user archetypes**: Readers (41.7%), Coders (30.4%), Terminal users (11%), Deep-loop users (9.2%), Chat-only users (7.6%). 50× token range across archetypes → uniform resource policies are suboptimal.
- **Six workflow archetypes**: Deep-loop read (30.5%), LLM-only (20.2%), Multi-cycle edit (19%), Multi-cycle other (13.2%), Deep-loop w/failures (9.1%), Deep-loop run (8.1%).
- **Idle-time predictor**: LightGBM survival-curve predictor (2MB) achieves ROC-AUC 0.73 for >60s idle, captures 86-90% of total idle time. Turn boundaries are the most actionable reclamation signal.
- **Key insight**: Serving infrastructure designed for chat (independent, short-lived, stateless requests) is structurally mismatched to coding-agent workloads (tight sequential dependencies, session-structured cache, bimodal idle periods).
- **Source**: arXiv:2608.00101, August 2026
- **Skill created**: `developer-experience-and-flow/agentic-coding-production-characterization`

**Novel Finding: Personalized Coding Agent Skills — Generic Beats Personalized (Huang, Du & Lan, UMass Amherst/OpenRefinery.ai, arXiv:2608.10319, August 2026)**

The first empirical study of whether personalized (developer-specific) coding-agent skills improve performance, using 206 real-world sessions from 13 developers with a reproducible replay framework.

- **Counterintuitive finding**: Personalized skills provide only limited and inconsistent improvements (+0.97, p=.399, not significant). Generic skills pooled across developers consistently outperform (+3.78, 50.95% win rate).
- **Threshold effect**: Personalized skills become effective only when a developer has ≥6 relevant historical sessions (+10.17 vs baseline, +5.67 vs generic). Below that, generic is better.
- **Skills change behavior**: All skill conditions produce MORE execution (more turns, tokens, tool calls, validation) — not less interaction. Skills encourage systematic validation (test command groups 0.56→0.97, successful validation 43.1%→58.9%).
- **Simulator effectiveness**: LLM-based developer simulator achieved 89.47% semantic consistency with real developer follow-ups (59.65% exact match).
- **Skill content**: Generic skills have more rules (25 vs 14.15 avg) and more commit rules (16% vs 7.6%). 64.7% of personalized rules are lexically unique to a single developer.
- **Key insight**: Broadly transferable procedural knowledge (generic skills) is more robust than developer-specific preference signals when per-developer data is sparse. Personalization becomes viable only with rich interaction histories.
- **Source**: arXiv:2608.10319, August 2026
- **Skill created**: `developer-experience-and-flow/personalized-coding-agent-skills`

### 2. AI Agents & Workflows

**Novel Finding: Agentic AI Adoption — Evidence from Codex (Johnston, Holtz, Martin et al., OpenAI/Columbia/Wharton/Duke, arXiv:2606.26959, June 2026)**

The first large-scale analysis of agentic AI adoption patterns using OpenAI Codex usage data across individual, organizational, and internal OpenAI populations.

- **Rapid but uneven shift**: 5× weekly active user growth in H1 2026. Codex share of output tokens: Individual 16.5%, Organizational 63.3%, OpenAI workers 99.8% (essentially replaced ChatGPT for work).
- **Delegated production, not consultation**: Users ask Codex to do work (debug, refactor, validate, configure, draft, analyze), not just provide advice. Contrasts with conversational AI where "asking" dominated.
- **Anchored in software but broadening**: Largest share is software work, but deepest adoption extends to research, planning, communication, data analysis, recruiting, sales. Within OpenAI, legal/recruiting went from ~0% to 75% Codex share in ~3 months.
- **Intensive users organize around parallel workflows**: >10% manage 3+ concurrent agents weekly; 28.6% of OpenAI workers manage 5+ concurrently. P99 OpenAI employee runs 71 hours of agent turns per day.
- **Task complexity growing 10×**: Users sending ≥8-hour tasks rose from 2.1% (Dec 2025) to 25.6% (May 2026). First turn of thread >2× more likely to be the most complex.
- **Skill systematization**: 26.6% of active users invoke skills (up from 5.4% in March). Custom skills most valuable in high-context organizational environments. Skills = SKILL.md format (same as A-Tech skill spec).
- **Key insight**: Agentic AI is not just a more capable chatbot — it's a different mode of use requiring new metrics (task complexity, runtime, concurrency, workflow reuse) and organizational complements (file access, management expectations, review processes).
- **Source**: arXiv:2606.26959, June 2026
- **Skill created**: `ai-agents-and-workflows/agentic-ai-adoption-codex-evidence`

### 3. Marketing & Content

**Novel Finding: LLM Personalized Ads Fail in Real-World Delivery (El Fraihi et al., Inria/CNRS, ICWSM 2026)**

The first real-world evaluation of LLM-generated personalized ads deployed through Meta's advertising system, testing 3 LLMs (GPT-4o, Gemini 1.5 Pro, LLaMA 3.1) × 4 demographics × 3 personalization strategies.

- **No significant engagement improvement**: Personalized messages did NOT significantly improve user engagement vs non-personalized alternatives. For some demographics, specific strategies reduced engagement.
- **Survey-behavior gap**: Survey-based assessments of ad appeal diverged from observed behavioral outcomes. Users rate personalized ads as more appealing in surveys but don't click them more in the wild.
- **Algorithmic delivery shift**: LLM personalization cues shifted ad delivery toward intended audience by up to 8% without explicit targeting — but bounded by platform's own relevance predictions.
- **Contrast with non-ad persuasion**: In non-advertising contexts (health, prosocial, product info delivered directly, not through ad platforms), LLM personality-tailored persuasion CAN work when content is semantically anchored (Xu, Zhou & Zhao, Frontiers in Psychiatry, Jan 2026; 16/18 conditions significant with anchoring). The difference: ad platforms add an algorithmic mediation layer.
- **Key insight**: Controlled-setting LLM persuasion gains don't transfer to algorithmically mediated ad platforms. Always validate with behavioral field data, not just surveys. The platform is a co-variable.
- **Source**: ICWSM 2026, Proceedings of the International AAAI Conference on Web and Social Media, Vol 20(1), pp 723-737
- **Skill created**: `marketing-and-content/llm-personalized-ads-reality-check`

### Supporting Context: Neuromarketing Research (2026)

- **AI-Enhanced Neuromarketing** (Bucea-Manea-Țoniș et al., 2026; N=416, PLS-SEM): Knowledge→Application β=0.726; Application→Social Media β=0.633. AI amplifies through real-time processing, predictive analytics, automated personalization.
- **Consumer Impulsivity & Neuromarketing** (Nagpal et al., Frontiers in Psychology, March 2026; N=609, PLS-SEM): Emotional appeals (β=0.469) and cognitive processing (β=0.378) dominate; scarcity/urgency (β=0.043) and endorsement (β=0.045) NOT significant. Consumer traits moderate efficacy→impulsivity. Affective + cognitively reinforced mechanisms, not scarcity/endorsement.
- **Marketing Digital Humans** (Pei et al., Advances in Psychological Science, Feb 2026): fMRI-based dual-trust (cognitive + affective) framework for marketing digital humans. CNN+LSTM predictive model from neural, behavioral, and consumption data.

---

## Synthesis & Cross-References

### Theme 1: Real-World vs Controlled-Setting Divergence
A recurring pattern across today's findings: controlled-setting advantages don't reliably transfer to real-world deployment.
- LLM personalized ads work in surveys but not on Meta (El Fraihi et al.)
- Personalized coding skills work in theory but generic beats them in practice (Huang et al.)
- SWE-bench benchmarks don't capture production coding-agent behavior (Liu et al.)
- Agentic AI adoption is uneven because organizational complements, not model capability, are the binding constraint (Johnston et al.)

### Theme 2: Agent Autonomy and Workflow Structure
- 87% of coding-agent LLM calls are agent-initiated (Liu et al.)
- Intensive Codex users manage 5+ concurrent agents (Johnston et al.)
- Skills change agent behavior toward more validation, not less interaction (Huang et al.)
- OpenAI workers at P99 run 71 hours of agent turns per day (Johnston et al.)

### Theme 3: Measurement Evolution
- Active users/chats/messages become less informative for agentic AI (Johnston et al.)
- Need: task complexity, runtime, concurrency, workflow reuse, production output
- Survey metrics diverge from behavioral metrics for ads (El Fraihi et al.)
- SWE-bench must be supplemented with production traces (Liu et al.)
- Coding-agent skills need replay-based evaluation, not just benchmark scores (Huang et al.)

### Existing Skills Updated
None updated today — all 4 findings required new skills.

### Existing Skills Cross-Referenced
- `developer-experience-and-flow/cli-agentic-coding-adoption-impact` — CLI adoption findings complement production characterization
- `developer-experience-and-flow/agentic-coding-returns-to-expertise` — Expertise findings complement skill personalization
- `developer-experience-and-flow/coding-agent-misalignment-large-scale` — Misalignment taxonomy complements production characterization
- `ai-agents-and-workflows/agent-protocol-stack-2026` — MCP/agent protocols complement adoption patterns
- `marketing-and-content/algorithmic-influence-predictive-ai-limitations` — Algorithmic influence limitations complement ad reality check
- `marketing-and-content/neuromarketing-consumer-impulsivity-trait-moderation` — Consumer impulsivity findings support ad reality check context

---

## Skills Created Today

### NEW (2026-08-18): Agentic Coding Production Characterization (`developer-experience-and-flow/agentic-coding-production-characterization/`)
- **SKILL.md** — Applies production-scale characterization of GitHub Copilot coding-agent workloads. Based on Liu et al. (UIUC/Microsoft Azure Research, arXiv:2608.00101, August 2026). 13.5M sessions, 3.2M users, 761M LLM calls. Seven structural properties: 1:1 LLM↔tool coupling (87% agent-initiated), session-structured KV cache (90%→55% at turn boundaries), model switch cache destruction (8% hit), context compaction cost (7.8% sessions, 44% tokens, -66% cache), turn-boundary idle signal (4.1 min container), tool failure compute amplification (9% turns → 4× compute), heterogeneous users (50× token range). Five user archetypes (Readers 41.7%, Coders 30.4%, Terminal 11%, Deep-loop 9.2%, Chat-only 7.6%). Six workflow archetypes. Idle-time predictor (LightGBM, ROC-AUC 0.73, captures 86-90% idle). A-Tech alignment: open-source (applies to Aider, Cline, OpenHands), data privacy (internal telemetry), financial freedom (86-90% idle capture → cost savings), practical implementation (13.5M sessions).
- **references/agentic-coding-production-evidence-base.md** — Full evidence base: dataset, per-session/turn statistics, 7 key findings with metrics, user archetypes table, workflow archetypes table, LLM/tool characteristics, idle-time predictor specification, serving design implications, limitations.

### NEW (2026-08-18): Personalized Coding Agent Skills (`developer-experience-and-flow/personalized-coding-agent-skills/`)
- **SKILL.md** — Applies empirical evidence on personalized vs generic coding-agent skills. Based on Huang, Du & Lan (UMass Amherst/OpenRefinery.ai, arXiv:2608.10319, August 2026). 206 sessions, 13 developers, 5 seeds. Personalized skills +0.97 (p=.399, not significant); generic +3.78 (50.95% win rate). Threshold: personalized effective only with ≥6 relevant historical sessions (+10.17 vs baseline). Two-stage skill generation (bootstrap + evidence-grounded refinement). Replay evaluation with LLM developer simulator (89.47% semantic consistency). Skills increase validation (43.1%→58.9% successful validation), not reduce interaction. Generic skills have more rules (25 vs 14.15) and commit rules (16% vs 7.6%). A-Tech alignment: open-source (any agent with skill support), data privacy (interaction traces only), financial freedom (generic reduces infrastructure cost), practical implementation (reproducible replay, 206 sessions, 5 seeds).
- **references/personalized-skills-evidence-base.md** — Full evidence base: experimental conditions, quantitative results, relevant-history impact analysis, simulator effectiveness, interaction/execution metrics, skill content statistics, developer-level variation, pipeline detail.

### NEW (2026-08-18): Agentic AI Adoption: Evidence from Codex (`ai-agents-and-workflows/agentic-ai-adoption-codex-evidence/`)
- **SKILL.md** — Applies large-scale agentic AI adoption evidence from OpenAI Codex. Based on Johnston, Holtz, Martin et al. (OpenAI/Columbia/Wharton/Duke, arXiv:2606.26959, June 2026). 5× WAU growth H1 2026. Codex token share: Individual 16.5%, Org 63.3%, OpenAI 99.8%. Four stylized facts: rapid but uneven shift, delegated production, software-anchored but broadening, parallel workflow organization. Task complexity 10× growth (≥8hr tasks 2.1%→25.6%). Concurrency: 28.6% of OpenAI workers manage 5+ agents. Skill use 26.6% (5× growth in 3 months). Skills = SKILL.md format. Workforce restructuring implications. A-Tech alignment: open-source AI (tool-agnostic patterns), data privacy (privacy-protecting pipeline), financial freedom (adoption curve planning), practical implementation (millions of users, validated classifiers).
- **references/codex-adoption-evidence-base.md** — Full evidence base: adoption metrics by population, persona classification, job function adoption, seniority, OpenAI internal timeline, task taxonomy, complexity growth, concurrency, long-running agents, skill use by source, Codex skills/plugins architecture, four stylized facts, workforce implications.

### NEW (2026-08-18): LLM Personalized Ads: Reality Check (`marketing-and-content/llm-personalized-ads-reality-check/`)
- **SKILL.md** — Applies real-world evidence that LLM-personalized ads don't improve engagement on Meta. Based on El Fraihi et al. (Inria/CNRS, ICWSM 2026). 3 LLMs × 4 demographics × 3 strategies. No significant engagement improvement. Survey-behavior gap. Algorithmic delivery shift ≤8% (bounded by platform). Three-dimensional evaluation framework (engagement, perceived appeal, platform behavior). Contrast with non-ad persuasion (semantic anchoring works when delivered directly). Practical recommendations: don't extrapolate from lab studies, measure behavior not just perception, expect modest algorithmic effects. A-Tech alignment: open-source (any LLM including LLaMA 3.1), data privacy (real platform data), financial freedom (prevents overinvestment), practical implementation (real Meta deployment).
- **references/llm-personalized-ads-evidence-base.md** — Full evidence base: experimental design, three findings, related LLM persuasion research (controlled vs mixed findings), semantic anchoring context, neuromarketing research context, why real-world fails where lab succeeds, evaluation framework.

---

## Methodology

- Scanned latest research across agentic coding, developer experience, AI monetization, neuromarketing, and AI adoption
- Cross-referenced with existing skill library (400+ skills) to identify genuinely novel findings
- Prioritized findings with strong empirical methods (production-scale data, RCTs, validated classifiers)
- Focused on A-Tech values alignment: open-source AI, data privacy, financial freedom, practical implementation
- Created skills following Agent Skills Specification (YAML frontmatter + markdown, references/ subdirectory)
- All reference documents contain full evidence bases for deeper engagement