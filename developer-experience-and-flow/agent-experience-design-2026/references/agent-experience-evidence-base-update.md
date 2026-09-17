# Agent Experience Design 2026 — Evidence Base (Update)

## New Sources (Since 2026-07-13 Version)

### GitKraken Code Flow (June 2026)
- **Source**: SD Times article on GitKraken Code Flow launch
- **Framework**: describes how work flows between developers, coding agents, repositories, reviews, PRs, production
- **Products**: Kepler (ADE, agent-first), GitKraken Desktop 12 (Git mode), GitLens 18 (IDE agentic)
- **Key insight**: AI has multiplied the volume of code flowing through existing failure points — the review that never gets picked up is now three reviews; the merge conflict is now five; the branch drift happens every sprint
- **Three audiences**: coding agent (frontline), developer (orchestrator), engineering leader (wants 50% faster)
- **Preview user pattern**: start in Kepler (spin up 12 agents), move to GitLens (refactor), move to GitKraken Desktop (resolve conflicts)

### Harness Engineering (Tamal Sen, Theta One, June 2026)
- **Source**: "Blueprint of a Software Development IDE in the Agentic Era" (Theta One Insider, Medium)
- **Seven elements**: LLM Model, Context, Tools/MCPs, Workflow, Feedback Surface, Backpressure, Human-as-API
- **Stop Hook**: intercepts agent exit, checks completion criteria (tests green, coverage above threshold, type checks clean), reinjects if unmet
- **Reward hacking**: human approval before test case updates prevents agents from modifying tests to pass rather than fixing code
- **Software as communication agreement**: software enables communication and carries the agreement between consumer and provider
- **Teleportability**: desktop-to-cloud session handoff (start on laptop, hand to cloud agent overnight, pick up in morning)
- **Multi-agent orchestration**: planner agent coordinates specialized sub-agents; task-adaptive model routing (downgrade to cheaper model for well-scoped subtasks)
- **Loop engineering**: designing the right escalation points; agent explains what it tried and why it is uncertain before asking for help

### Copilot vs Agentic IDE vs Agentic DevOps (Nowakowski, Monterail, July 2026)
- **Source**: "The 2026 Developer Infrastructure" (Monterail blog)
- **Copilot**: completes code at keystroke; single function/file; in-session; inline suggestion; every suggestion needs accept/reject
- **Agentic IDE**: acts on CI signal/failing test/log event/ticket; cross-file/cross-repo; self-corrected PR; review gate before merge
- **Agentic DevOps**: connects observability directly to code generation; test failure → root-cause hypothesis → patch → re-run → PR; needs human only at review
- **Signal loop**: organizations that get most out of agentic infrastructure have cleanest signal loop between pipeline, tests, logs + clear gates — not most capable agent
- **What breaks first**: sparse/unstructured logging — agents need legible signals; noisy logs + flaky tests produce unreliable root-cause hypotheses
- **Implementation readiness**: structured logging, test coverage above threshold, defined merge gates + rollback, accountability model for agent PRs, centralized agent policy layer (prevent agent sprawl)
- **Gating**: merge gates, staged rollouts, policy-as-code checks; audit trail logging what agent changed and why; rollback as fast for agent-authored as human-authored; permission scoping

### Anthropic 2026 Agentic Coding Trends Report
- **Source**: Anthropic official report (resources.anthropic.com)
- **Eight trends across three categories**: Foundation (SDLC changes), Capability (multi-agent teams, long-running agents, human oversight scales, new surfaces), Impact (productivity economics, non-technical use cases, dual-use security)
- **Key stat**: engineers use AI in ~60% of work; can "fully delegate" only 0-20% of tasks
- **Productivity pattern**: net decrease in time per task, much larger net increase in output volume; ~27% of AI-assisted work = tasks that wouldn't have been done otherwise
- **Case studies**: Augment Code (4-8 month project in 2 weeks), Fountain (50% faster screening, 40% quicker onboarding, 2x conversions), Rakuten (vLLM feature in 7 hours, 99.9% accuracy), TELUS (13,000 AI solutions, 30% faster, 500,000 hours saved), CRED (doubled execution speed), Zapier (89% AI adoption, 800+ agents), Anthropic legal team (2-3 days to 24 hours)
- **Multi-agent**: Fountain Copilot as central orchestrator coordinating specialized sub-agents; one logistics customer staffed fulfillment center in <72 hours (was 1+ week)
- **Long-running**: Rakuten Claude Code — 7 hours autonomous on 12.5M-line codebase
- **Priorities for 2026**: (1) multi-agent coordination, (2) scale human-agent oversight via AI review, (3) extend beyond engineering to domain experts, (4) embed security from earliest stages

### Agent Experience (AX) — GitHub Copilot App (Alto, June 2026)
- **Source**: mer.vin blog (Valentina Alto walkthrough)
- **UX vs Agent UX vs AX**: UX optimizes interactions; Agent UX optimizes conversations; AX optimizes collaboration over time
- **Six AX shifts**: artifact-as-tracked-work-item, plan-before-act, persistent-workspace, parallel-coordination, continuous-delivery-path, durable-trail
- **AX primitives**: My Work (control center), git worktrees (isolation), canvases (bidirectional work surfaces), cloud/local sandboxes (bounded action), Agent Merge (accountability), session modes (Interactive/Plan/Autopilot), rubber duck agent (adversarial critique), Memory++/chronicle (temporal continuity), partner agent apps (ecosystem)
- **Chat vs canvas**: chat for instruction/ambiguity; canvases where intent becomes inspectable work; long chat fails once agent runs for hours
- **AX design checklist**: can I see what happened while away? are decisions auditable? does context persist? is responsibility clear? can I run parallel workstreams? does review scale with agent output?
- **Microsoft Agent UX principles**: Space (connecting not collapsing; accessible yet invisible), Time (Past: history beyond states; Now: nudging more than notifying; Future: adapting and evolving), Core (embrace uncertainty; transparency, control, consistency)
- **GitHub metrics**: ~1.4 billion commits/month (nearly 2× YoY); ~2 billion Actions minutes/week

### Verification Bottleneck (Mugrage, Thoughtworks, June 2026)
- **Source**: "Is developer experience dead?" (Thoughtworks)
- **Primary friction**: not producing code, it's verifying it; time spent reviewing code has eclipsed time spent writing it
- **Three flow-killers**: verification fatigue (reading harder than writing; 500 lines in 10 seconds → human must trace logic), "vibe coding" hangover (surface velocity masks debt/drift/risk), context-switching noise (prompt → wait → inspect → correct → prompt jolt)
- **Supervisory engineering**: senior engineer's value no longer measured by writing algorithms; now orchestrating agents
- **DevEx transformation**: not dead, but traditional frameworks obsolete; measuring commits/dep-frequency/lines-of-code meaningless in agentic world
- **Key practices**: machine-readable intent (structured specs), agentic testing layers (adversarial agents hunt edge cases before human review), cognitive guardrails (line-level AI attribution, semantic diffs, dashboards showing why agent decided)
- **Quote**: "If we want to rescue developer satisfaction and maintain true system quality, the focus of DevEx must shift from protecting the mechanical flow of typing to protecting the strategic flow of architecture and our ability to make decisions about it."

### Agent Experience is the New Developer Experience (Moore, Builder.io, June 2026)
- **Source**: builder.io/blog/agent-experience
- **Seven core tenets**: context-as-onboarding (minimal/transparent/tested), environment-as-prompt (deterministic environment for non-deterministic LLM), no-handoffs-without-verification (spend tokens before reviewer attention), deterministic-safety (sandboxing/scoped credentials/permission gates), model-routing-as-boring-infrastructure, codebase-as-source-of-truth (progressive disclosure for machines), agents-as-cross-functional-glue
- **Context discipline**: minimal (global context stays thin), transparent (reviewer can audit which rule shaped the work), tested (team skills self-explanatory enough agent invokes at right time)
- **Environment**: if agent can't compile, run dev server, seed database, or hit local API, output is not trustworthy; agent will route around failure, change wrong file, ship guess with polite commit message
- **Safety**: "Make sure not to mess with the database!" doesn't help — prompts are easily bypassed; safety must be structural (sandboxing, scoped credentials, file/network limits, separate dev/prod data, env-var approval gates, human-in-loop for high-risk)
- **Golden rule**: LLMs should do the glue work. People should do the interesting work.

### State of AI Coding Tools 2026 (Kingy AI, July 2026)
- **Source**: kingy.ai/blog/state-of-ai-coding-tools-2026
- **Seven lanes**: AI IDEs (Cursor), platform-native (GitHub Copilot), cross-surface (OpenAI Codex, Claude Code), browser app builders (Replit, Lovable, Bolt, v0), code review/governance (Qodo, Copilot review, Sourcegraph, Tabnine), open-source (Aider, Cline, Continue, OpenHands), context/orchestration (Sourcegraph, MCP servers)
- **Workflow stack**: Intent → Context → Agent work → Validation → Delivery → Learning loop
- **Agent Maturity Matrix**: Demo → Repo → Validation → Team → Automation → Governance layers
- **Key insight**: the best buyer question is not "which agent is smartest?" but "which tool can repeatedly finish safe, reviewable work in our workflow?"
- **Pricing traps**: seat vs. model usage vs. credits vs. enterprise controls vs. bring-your-own-key
- **2026 signals**: Cursor iOS beta (June 29), GitHub Copilot Opus 4.8 fast mode (June 29), Continue acquired by Cursor (repo read-only), Windsurf → Devin Desktop (June 2026)