# Agent Experience Design 2026 — Evidence Base

## Primary sources

1. Moore, A. (2026). "Agent experience is the new developer experience." Builder.io Blog, June 8, 2026. (Seven core tenets of AX, environment-as-prompt, deterministic safety, model routing as infrastructure, cross-functional glue)
2. Alto, V. (2026). "Introducing Agent Experience (AX) — Designing Long-Running AI Agent Collaboration." Medium. (AX primitives from GitHub Copilot app: My Work, git worktrees, canvases, sandboxes, Agent Merge, session modes, rubber duck, Memory++/chronicle, partner agent apps; UX/Agent UX/AX distinction)
3. Mugrage, K. (2026). "Is developer experience dead?" Thoughtworks, June 23, 2026. (Verification bottleneck, flow-killers, supervisory engineering, cognitive guardrails)
4. Nowakowski, M. (2026). "The 2026 Developer Infrastructure: Moving from Copilot Tooling to the Agentic IDE and Agentic DevOps." Monterail, July 7, 2026. (Copilot vs agentic IDE, agentic DevOps, human-in-the-loop gating, implementation readiness checklist)

## Secondary sources (2026 agentic IDE ecosystem)

- AskEntity/Matrix (2026). Multi-agent IDE where every tab is a task, every task is a complete story. Tree-parallel execution, living memory, cache engineering, two-phase lifecycle, cross-project communication.
- Shofer.dev (2026, June). Open-source AI coding agent for VS Code with declarative multi-agent workflows (.slang DSL), live agent visualization, codebase memory, hard cost caps, git worktree support, kernel-level sandboxing.
- AgenticFlowX (2026). VS Code workflow layer for AI coding. Chat-first, Code/Explore/Spec modes, actionable markdown previews, project canvas, repo-owned project memory.
- CMolG/heliox-ide (2026). Visual-first agentic IDE with E2E snapshot validation. Spatial canvas, snapshot-verified agents, provider-agnostic, market system (flows/roles/modifiers).

## The seven core tenets (Moore 2026)

### Tenet 1 — Context is onboarding
- Agent onboarding happens at the start of every task, not once per quarter.
- Temptation is to over-document. Left unchecked: graveyard of stale AGENTS.md rules, conflicting definitions, hidden tool instructions.
- Good context: minimal (global stays thin, points to code), transparent (reviewer can audit which rule shaped work), tested (skills self-explanatory, invoked at right time).
- Agent rules/skills/context must be team discipline, not a pile of experiments.

### Tenet 2 — The environment is part of the prompt
- LLMs non-deterministic; execution environment must be aggressively deterministic.
- Dependency versions, local scripts, env-var shapes, seed data, auth setups, local services determine what agent can observe and correct.
- "I couldn't run the tests locally, but this should work" is a massive DX red flag for humans. When an agent does it, we hope for the best.
- Agent that can't compile, run dev server, seed DB, or hit local API → output not trustworthy. Agent routes around failure, changes wrong file, ships guess with polite commit message.

### Tenet 3 — No handoffs without verification
- Agents shouldn't stop at generating code; they need to prove work.
- Trading friction of writing code for exhausting cognitive load of reviewing it.
- If you spend 30 minutes manually QA-ing an agentic PR, the agent didn't save time — it shifted labor.
- Agent's completed task should present evidence: tests run, screenshots captured, browser flows checked, logs inspected, accessibility trees reviewed, edge cases explored.
- Typechecks, unit tests, linting are the bones. But product work fails where compilers are blind: responsive layouts, loading states, broken flows that technically compile.
- Tools: Chrome DevTools MCP, Playwright, automatic branch previews.
- **Spend tokens before spending reviewer attention.** Tokens cheap, unlimited, 24/7. Senior developer focus precious, expensive, burns out.
- Handoff should be easy to act on: visual note on preview branch, failed check sent back into agent's execution loop for self-correction.

### Tenet 4 — Safety needs to be deterministic
- Good DX made dangerous actions hard. Good AX must make dangerous actions impossible.
- "Make sure not to mess with the database!" doesn't help. Prompts easily bypassed.
- Safety must be structural: sandboxing, scoped credentials, file/network limits, separate dev/prod data, env-var approval gates, human-in-the-loop for high-risk.
- When agent drops a database: "Why did your system allow for that? Why did the agent have that access?"
- Non-technical teammates (PM, designer, marketer) using agents shouldn't inherit root access. Sandbox must be the absolute security boundary.

### Tenet 5 — Model routing as boring infrastructure
- Too much focus on weekly frontier-model horse race. Real questions: Who can access agents? Against what systems? With what context? Under what review process? Through which model route? With what audit trail?
- Cheap, fast models: low-risk summarization, triage, classification, scaffolding, routine review.
- Deterministic syntax validation: linters, typecheckers, test runners.
- Expensive reasoning models: harder multi-file judgment work.
- Governance inside the agent path: admins see usage/costs, reviewers see execution evidence, teams can switch providers without rewriting application logic.

### Tenet 6 — Design systems and code architecture are the agent's best source of truth
- Codebase must be the most accurate record of how the product works.
- If docs say one thing, components do another, Storybook three versions behind → agent synthesizes confusion into elegant-looking garbage.
- Design for machines: deep modules with thin public interfaces, typed APIs, predictable routing, clean directory structures.
- Progressive disclosure for machines: lower cognitive load → lower token usage → fewer logic errors.
- Design systems: the more codebase forces agent to reuse human-made components, tokens, accessibility patterns, the less output becomes generic sludge. Agent shouldn't invent a custom button when team's button is right there.

### Tenet 7 — Agents become cross-functional glue
- When agents make V1 cheap to generate, they don't solve V2, V3, V4.
- Without shared workspace: developer becomes human router copying visual feedback from Slack, translating to agent's prompt interface, running workspace locally, managing branch.
- Best AX moves from pre-code abstractions to shared iteration around live product surfaces.
- Interactive preview deployments + role-aware controls: designer, marketer, PM talk to agent directly inside safe bounded preview. Test responsive states, iterate on copy, tweak layouts on branch preview. Developers retain architecture, safety, system integration ownership.
- Developer no longer copy-paste bottleneck; they are platform engineer who owns system design.
- **Golden rule:** LLMs should do the glue work. People should do the interesting work.

## AX primitives (Alto 2026 — GitHub Copilot app)

| Primitive | Role in AX |
|---|---|
| My Work | Control centre: active sessions, issues, PRs, background automations in one view |
| Git worktrees | Isolation: each session gets own branch copy — parallel agents without collision |
| Canvases | Bidirectional work surfaces: plan, PR, terminal, browser, deployment state — agents update, humans steer on same surface |
| Cloud/local sandboxes | Bounded action: ephemeral Linux in cloud or restricted local env; enterprise policy enforcement |
| Agent Merge | Accountability to ship: monitors CI, reviewers, failing checks; configurable automation to green, address feedback, or merge |
| Session modes | Autonomy dial: Interactive (suggest, wait), Plan (plan first, execute after approval), Autopilot (write, test, iterate without waiting) — change mid-session |
| Rubber duck agent | Adversarial critique: separate model reviews plan, implementation, or tests |
| Memory++ / /chronicle | Temporal continuity: context across app, CLI, VS Code, and GitHub.com sessions |
| Partner agent apps | Ecosystem surface: LaunchDarkly, PagerDuty, Miro, Sonar, others assignable from GitHub |

Six AX shifts in the GitHub Copilot app (loyalty-points feature walkthrough):
1. Artifact start — session opens from an issue in My Work, not blank prompt; context loads automatically; Plan mode starts by default.
2. Plan before act — agent proposes implementation plan; developer reviews and edits before any code changes.
3. Persistent workspace — isolated git worktree or cloud sandbox with own branch; agent researches, edits, runs tests and linters.
4. Parallel coordination — multiple isolated sessions; switch tasks while agents run.
5. Continuous delivery path — preview locally, open PR, inspect CI and review activity, spawn session from PR for follow-up.
6. Durable trail — session history, saved quick chats, /chronicle summaries across app and CLI — intent, execution, review remain queryable.

## UX, Agent UX, AX distinction (Alto 2026)

| Layer | Software role | Design question | Optimises |
|---|---|---|---|
| UX | Reactive application | Can I use this product? | Friction on screens and workflows |
| Agent UX | Intelligent collaborator in chat surface | Can I effectively interact with this agent? | Transparency, control, multimodal triggers, trust in one session |
| AX | Persistent participant in operating model | Can I effectively work with this agent over time? | Legibility, auditability, context persistence, clear responsibility |

Microsoft Agent UX principles (Space · Time · Core) mapped to AX:
- Space: Connecting not collapsing; accessible yet occasionally invisible.
- Time · Past: History beyond states (session logs, canvases, /chronicle). Time · Now: Nudging more than notifying (plan approval gates, rubber duck). Time · Future: Adapting and evolving (scheduled cloud automations, voice dictation, cross-device pickup).
- Core: Embrace uncertainty, establish trust (visible reasoning, configurable autopilot). Transparency, control, consistency (sandbox policies, merge conditions, skills and MCP extensions).

## The verification bottleneck (Mugrage 2026)

- Primary friction in 2026 isn't producing code; it's verifying it.
- Global code production exploded; delivery timelines haven't shortened because review time eclipsed writing time.
- Three flow-killers:
  1. **Verification fatigue:** reading code harder than writing it. Agent generates 500 lines in 10 seconds → human must trace logic for subtle bugs, security vulnerabilities.
  2. **"Vibe coding" hangover:** surface velocity masks technical debt, architectural drift, compliance risks. When "vibe" breaks, debugging agonizing.
  3. **Context-switching noise:** human flow requires continuity. Agentic workflows transactional (prompt → wait → inspect → correct → prompt). Developer jolted from deep problem-solving into adversarial review.
- Mitigation:
  - **Machine-readable intent:** living docs, spec-driven development agents parse cleanly without assumptions.
  - **Agentic testing layers:** adversarial agent architectures hunt edge cases, security flaws (OWASP Top 10 for Agentic Applications), architectural drift before human review.
  - **Cognitive guardrails:** line-level AI attribution, semantic diffs, visual dashboards showing why agent made a decision.
- "If we want to rescue developer satisfaction and maintain true system quality, the focus of DevEx must shift from protecting the mechanical flow of typing to protecting the strategic flow of architecture and our ability to make decisions about it."
- DevEx isn't dead; it's transforming. Measuring commits, deployment frequency, lines of code in an agentic world is meaningless.
- AI coding agents are the most powerful tools we have ever built, but they are still just tools. Ultimate responsibility for system integrity, security, user empathy sits with the human engineer.

## Copilot vs agentic IDE vs agentic DevOps (Nowakowski 2026)

| Dimension | Copilot | Agentic IDE |
|---|---|---|
| Trigger | Keystroke or comment in editor | CI signal, failing test, log event, ticket |
| Scope of action | Single function or file, in-session | Cross-file, cross-repo, spans the pipeline |
| Output | Inline suggestion or autocomplete | Self-corrected pull request |
| Human touchpoint | Every suggestion (accept/reject) | Review gate before merge |

Agentic DevOps workflow: test fails → log parsed → root-cause hypothesis generated → patch proposed → suite re-runs → PR opens if patch resolves failure. Requires clean observability data; noisy logs and flaky tests produce false root-cause hypotheses.

Human-in-the-loop gating: merge gates, staged rollouts, policy-as-code checks. Three things must hold under compliance: audit trail (what changed and why), rollback process (as fast as human-authored), permission scoping (which repos/environments/data agent can touch).

Implementation readiness checklist:
1. Structured logging across services — agents need legible signals.
2. Test suite coverage above threshold — prevents false root-cause hypotheses.
3. Defined merge gates and rollback process — enterprise-safe autonomy requires checkpoint.
4. Accountability model for agent-authored PRs — compliance requires clear owner.
5. Centralized agent policy layer — prevents agent sprawl across repos.

Key takeaways:
- Agentic IDE acts on signals from CI/CD and logs directly; copilot only acts on what human types.
- Cycle time drops mainly because review work shifts from writing fixes to judging proposed ones.
- Agentic DevOps depends on clean observability data; noisy logs and flaky tests produce unreliable root-cause hypotheses.
- Gating is what makes autonomous deployment acceptable to compliance, more than raw agent capability.
- Agent sprawl across uncoordinated repos is a bigger operational risk than any single agent making a bad suggestion.

## Agentic IDE ecosystem (2026)

- **Matrix (AskEntity):** Multi-agent IDE. Task tree (not file tree). Tree-parallel execution (5 agents on isolated git worktrees). Living memory (closed tasks forkable). Cache engineering (frozen tools, byte-identical JSONL). Two-phase commit (agent decides → daemon commits). Self-bootstrapping.
- **Shofer:** Open-source (Apache 2.0), VS Code. Declarative Slang DSL for multi-agent workflows (statically analyzable, deadlocks caught before run). Live agent visualization (topology/sequence/swimlane). Codebase memory (persistent read-only Assistant Agent). Hard cost caps (per-session USD). Git worktree support with kernel-level (Landlock/bwrap) sandboxing.
- **AgenticFlowX:** VS Code extension. Chat-first, Code/Explore/Spec modes. AFX Viewer (actionable markdown). Repo-owned project memory (specs, designs, tasks, journals, ADRs, notes). Experimental canvas (JSON Canvas). @see CodeLens linking code to specs.
- **Heliox IDE:** Visual-first agentic IDE (Electron). Spatial canvas (Figma-inspired). E2E snapshot validation (Playwright + Core Web Vitals + pixelmatch). Market system (flows/roles/modifiers as composable Markdown prompts). Auto-correction up to 3 times before reverting on regression.

## Cross-references to adjacent skills

| Adjacent skill | Relationship |
|---|---|
| `devex-verification-bottleneck-framework` | Verification bottleneck measurement (the 3 D's). AX is the design response to the bottleneck this framework measures. |
| `ai-review-fatigue-mitigation` | Review fatigue mitigation. AX's "no handoffs without verification" tenet is the design pattern; this skill is the fatigue mechanism + countermeasures. |
| `agent-friendly-api-documentation-2026` | Agent-readable documentation. This is the context layer of AX (Tenet 1: context is onboarding). |
| `coding-agent-decision-fatigue-mitigation` | Decision fatigue in coding agents. AX's "model routing as boring infrastructure" reduces decision fatigue. |
| `harness-engineering-ai-agents-2026` | Harness engineering. AX is the experience layer; harness engineering is the infrastructure layer underneath. |
| `ai-collaboration-friction-patterns` | AI collaboration friction patterns. AX is the design discipline that addresses these friction patterns. |
| `outer-loop-harness-framework` | Outer-loop harness. AX's session modes and Agent Merge are the experience layer on top of the harness loop. |
| `beyond-vibe-agentic-engineering` | Beyond-vibe agentic engineering. AX's "no handoffs without verification" tenet is the design response to vibe-coding. |

## Novelty confirmation

The existing `agent-experience-design-2026` skill (created 2026-07-13 per README) covers AX at a high level. This updated skill extends it with:
1. The seven core tenets from Builder.io (Moore 2026) — the previous version did not enumerate these.
2. The AX primitives from the GitHub Copilot app (Alto 2026) — My Work, worktrees, canvases, sandboxes, Agent Merge, session modes, rubber duck, Memory++/chronicle, partner agent apps.
3. The UX/Agent UX/AX three-layer distinction — the previous version did not distinguish these.
4. The verification-bottleneck analysis from Thoughtworks (Mugrage 2026) — flow-killers, supervisory engineering, cognitive guardrails.
5. The copilot-vs-agentic-IDE-vs-agentic-DevOps distinction from Monterail (Nowakowski 2026) — implementation readiness checklist, gating requirements.
6. The 2026 agentic IDE ecosystem (Matrix, Shofer, AgenticFlowX, Heliox) — concrete implementations of AX principles.

The previous skill was a high-level concept; this version is the full design discipline with tenets, primitives, evidence, and ecosystem.