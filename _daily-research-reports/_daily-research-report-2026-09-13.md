# A-Tech Daily Research Report — 2026-09-13 (Cycle 25, Run 1)

**Scope:** Open-source AI models & business models · Behavioral psychology & nudging · Neuromarketing/consumer neuroscience · Privacy-first AI · Developer experience · Community & growth · Financial freedom · Agent payments
**Method:** Five web sweeps across the standing domains (Sept 6–13, 2026 window: open-weight releases, agent-payment protocol events, consumer-neuro publications, DP/FL papers, developer-survey waves, agent-economy coverage) → targeted grep dedup against the on-disk library → 4 new standalone SKILL.md folders across 3 category directories + 1 in-place refresh → this report → README cycle-25 run-1 index update
**Deliverables:** 4 new SKILL.md folders + 1 refresh + README run-1 index + this report
**Coordination note:** library state surveyed before writes; cycle-24 run-2 (SoL-Pi, OpenAI intern telemetry, Pizza Bot, UnifoLM-WLA, TranslatePsy, α-split DP) was fully on disk before this run. All four selections passed targeted greps against the live library before creation, and each new skill names its nearest neighbors in Pairs-with. The behavioral/neuro sweep returned only below-bar candidates this window — see §2.

---

## 1. Research Summary

The Sept 6–13 window produced one dominant theme — **the agent-payment stack moved from protocol competition to infrastructure consolidation, and the accountability layer underneath it is missing** — plus one open voice-agent architecture event and the first consumer-scale skill-monetization rail. Four selections passed grep dedup; the headline clusters:

1. **The wallet rails opened.** Ant International open-sourced AMP (Sept 10) — the mobile-wallet agent-payment protocol now on GitHub with SDKs and docs, rolling out across Alipay+ (10 wallets, 1.5B accounts in Phase I; 50+ wallets, 150M merchants, 2B accounts network-wide) with 7 acquirers including Adyen, Fiserv, Worldline. The same week, Ant + Mastercard + Visa began a KYA interoperability collaboration via MAS's BuildFin.ai — the first cross-network agent-identity interoperability effort, with the "register once, recognized everywhere" portability promise (Yang: "If an agent registers with Ant, they don't need to register again with Visa, Mastercard"). This fills the mobile-wallet lane in the protocol map the library holds (`ap2-mpp-x402-protocol-stack-2026`): x402 = HTTP settlement, AP2 = mandates, Agent Pay = card rails, AMP = wallets.
2. **The record gap is now a regulatory deadline.** The Sept 9 AgentRisk analysis — built on a 2.68M-agent index with 10.4M hash-chained behavioral records — makes the sharpest articulation yet of the accountability gap under 205M settled x402 transactions (~$53M): every protocol proves authorization; none records what the agent did between mandate and payment. The four failure bases are documented (transcript-editing per OpenAI's post-mortem; the unlogged MCP layer; GitSpawn's CLI-agent execution flaws; the $2,400 unbounded-spend case). The timer: EU CRA Article 14 went live Sept 11, 2026 — 24-hour exploited-vulnerability reporting with agents, MCP servers, and inference endpoints explicitly in scope, at €15M/2.5%-of-turnover penalties. You cannot file a 24-hour report on an agent you have no independent record of. Stop Rogue AI Act (introduced Sept 3, bipartisan) and OpenAI's own standards call complete the three-jurisdiction pressure.
3. **Skills became a billing unit.** Alipay's AI Collect upgrade (Sept 11) adds Vibe Pay, **Skill Pay**, and Machine Pay plus an AI Wallet Agent — the first consumer-scale platform to productize per-skill monetization of agent capabilities inside an existing wallet network. The design template (package the capability → match the rail → wallet-agent governance) with the open-rail comparison (MCP payment tools, x402 per-call) and the standing caveat that spend caps are mandates with nothing underneath witnessing behavior.
4. **The voice lane got its open reference implementation.** Tencent Hunyuan's Gander (Sept 9): 9B, Apache-2.0, fully local — Cerebellum-Brain split with a streaming interruptible front half (Full-Duplex-Bench v3 100% turn-taking) and a pluggable back brain (default Codex). The voice-lane sibling to Pizza Bot's ambient inbox: both assume the human is not continuously attending.
5. **China ARR calibration firmed up (refresh, not new skill).** Moonshot's $300M (June) → $1B (August) → $2B year-end target re-confirmed (Bloomberg/TechCrunch Sept 11–12), with OpenRouter ~300B tokens/day on K3 and the Anthropic distillation allegation (~300K routed requests, >23M collected responses) now load-bearing context. Peer prints unchanged (Z.ai $1.6B, MiniMax $800M). Folded as a CYCLE-25 ADDENDUM into `china-open-source-llm-arr-tracker`.

All four new selections passed targeted grep dedup against the live library (details in §2).

### Fresh Signals (4 new skills)

| # | Skill | Category | Source | Why it matters now |
|---|---|---|---|---|
| 1 | Ant AMP Open-Source + KYA Interoperability | ai-agents-and-workflows | Ant/Business Wire Sept 9–10, 2026; CNBC; Cryptonomist | The OPEN-WALLET-RAIL pattern: mobile-wallet lane of agent payments open-sourced at 2B-account scale; KYA interoperability as the first cross-network agent-identity effort (MAS/SAFR-anchored) |
| 2 | Agent-Payment Record Gap | ai-agents-and-workflows | AgentRisk/DEV Sept 9, 2026; x402 205M milestone; EU CRA Article 14 live Sept 11 | The MANDATE-RECORD-DECOUPLING pattern: authorization proves permission, nothing records behavior; three jurisdictions now demand an artifact mandates cannot supply; the four-record-layer-property checklist |
| 3 | Alipay Skill Pay Monetization | monetization-and-revenue | Alipay/Tech in Asia Sept 11, 2026 | The SKILL-AS-BILLING-UNIT pattern: first consumer-scale platform making packaged agent capabilities a first-class revenue unit (Vibe/Skill/Machine Pay + AI Wallet Agent) |
| 4 | Gander Open Voice-Agent Architecture | ai-agents-and-workflows | Tencent Hunyuan release Sept 9, 2026 via AI/TLDR | The CEREBELLUM-BRAIN-SPLIT pattern: open 9B Apache-2.0 artifact decoupling interruptible streaming speech from pluggable long-horizon reasoning — the voice lane of ambient-agent UX |

*Note: one refresh also landed this run — see the cycle-25 run-1 index entry in README.md: `china-open-source-llm-arr-tracker` (CYCLE-25 ADDENDUM: Moonshot velocity re-confirmation + OpenRouter K3 token volume + distillation-allegation context).*

---

## 2. Deduplication & Novelty Assessment

- **Ant AMP / KYA interoperability** — grep for "KYA|Know-Your-Agent|Agent Connect|Agentic Mobile Protocol|AMP" matched the card-network skill (`agent-pay-card-network-integration`, which holds Mastercard Agent Pay tokenization + the KYA four-pillar framework from June 2026) and protocol-map skills, but the AMP **open-sourcing** and the **cross-network interoperability collaboration** are uncaptured events. **Novel**, scoped to the wallet-lane protocol + interoperability event; cross-linked to the card-network skill.
- **Agent-Payment Record Gap** — grep for "behavioral record|mandate file|record layer" returned only unrelated SaaS system-of-record hits and the CXM framework; no agent-payment accountability skill exists. The x402 family holds settlement security (free-riding attack surface, production checklist); this is the layer beneath — **Novel**.
- **Alipay Skill Pay** — grep for "Skill Pay|Vibe Pay|AI Collect|AI Wallet Agent" returned zero matches. **Novel.**
- **Gander** — grep for "Gander|Cerebellum|Hunyuan voice|omni-interaction" returned zero matches; no speech/voice-agent skill exists in the library. **Novel** as the open voice-architecture event.
- **Already tracked (no action):** Moonshot $2B ARR (held; folded as tracker addendum), K2 Horizon fleet (held, cycle 19 — re-confirmed in sweep, no new mechanism), Cohere North Small Translate (held, cycle 23 run 1), Nex-N2.5-Max open weights (held in the tracker addendum), MiniCPM5-2B quiet drop (held, cycle 22 run 1), JetBrains agent-adoption Q2-2026 (held, cycle 23 run 1 — the Temporal State of Development data is also held, cycle 14), Sonar State of Code (held), State of AI 2026 survey (held), Anthropic Agentic Coding Trends report (held in substance across the SDLC/adoption skills), fMRI neuromarketing SLR (held, cycle 20), consumer-neurophysiological moderators (held, cycle 20 — same Nagpal et al. paper re-surfaced), NeuroPack packaging study (held, flagged below bar in cycle 23 run 2 — the Feb 2026 Amfiteatru publication is the same study at journal print), eco-label EEG/eye-tracking study (below the cycle-16 small-N bar, N=13, exploratory — flagged with testable H1/H2 worth tracking), gaming-consumer neuromarketing SLR (Frontiers in Neuroergonomics Sept 10 — an extension of the gaming-retention evidence base; the personalized-difficulty-adjustment finding is held by the freemium/retention family; below bar as a synthesis without a new mechanism), CDAM purchase-decision framework (conceptual, untested — flagged), neuro-AI packaging convergence (same NeuroPack family), XCal-FL + DP-FedAdamW + component-aware α-split (all held by the DP-FL family), modality-decoupled FedMVLA (a new mechanism — modality-sliced FL for VLA robots — but below the create-bar in this window; flagged), split-LLM obfuscation + gradient decoy (held, cycle 23), heterogeneous multi-LLM federated inference (held, cycle 21).
- **Below-bar candidates (flagged, not created):** DP-DyLoRA (Samsung, ICCL 2026-ish; dynamic-rank DP-LoRA — novel mechanism vs the DP-PEFT family but the library holds five stronger DP-FL entries; promote if a second independent implementation or production deployment lands); the x402 volume/valuation bubble analysis (Verda Ventures — the rails-right/prices-first thesis is held in substance by `x402-demand-reality-check-2026` and `agentic-token-metrics-openrouter`; the 95%-of-volume-over-$1 shift is a tracker-worthy datapoint, folded as a watch note); the EU CRA compliance angle beyond agents (held by `eu-cyber-resilience-act-compliance-2026`; the agent-scope reading is folded into the record-gap skill); Alipay's Abao end-to-end assistant (a distribution datapoint for the Skill Pay skill's watch item).

## 3. Skill Library State After This Run

- New (4): `ai-agents-and-workflows/ant-amp-kya-interoperability/` · `ai-agents-and-workflows/agent-payment-record-gap/` · `ai-agents-and-workflows/gander-open-voice-agent-architecture/` · `monetization-and-revenue/alipay-skill-pay-monetization/`
- Refreshed in place (1): `monetization-and-revenue/china-open-source-llm-arr-tracker/` (CYCLE-25 ADDENDUM: velocity re-confirmation + token-volume + provenance context)
- All SKILL.md files follow the Agent Skills spec: YAML frontmatter (name + description with use-when/NOT-for triggers), under 500 lines (verified: 48–55 lines each), Overview → When to Use → Core Process/Workflow → Key Evidence → Pairs-with cross-links → A-Tech alignment → Honesty Caveats → sources.

## 4. Watch Items for the Next Run

1. **AMP Phase I execution** — whether the 10-wallet/7-acquirer rollout produces published transaction metrics; whether the KYA interoperability collaboration publishes a technical spec or pilot scope (the standards-work-vs-product test).
2. **The record layer's first adopter** — whether any payment platform or MCP registry ships a neutral behavioral-record layer; whether CRA enforcement guidance names agent infrastructure explicitly; the first public agent-payment incident and its liability allocation.
3. **Skill Pay mechanics** — published pricing/fee schedule and whether Western platforms (Stripe/OpenAI/Visa) match with skill-level billing units; whether the AI Wallet Agent ships.
4. **Gander adoption** — independent benchmarking, whether the Cerebellum-Brain pattern gets ported into mainstream voice-agent frameworks (two or more = pattern graduation), whether back brains other than Codex gain share (the open-stack test).
5. **Moonshot ARR print** — whether the $2B year-end target holds, the 30% rev-share negotiations with Microsoft/Amazon/Google, and the HK IPO timing (~$50B target); the Anthropic-distillation resolution.
6. **DP-DyLoRA promotion trigger** — second independent implementation or production DP-FL deployment using dynamic-rank allocation.
7. **Anthropic S-1** (reportedly October 2026) — carried from cycle 24; the public-market look at frontier-lab gross margins remains the largest repricing event on the calendar.
8. **EU CRA first agent-scope enforcement action** — the signal that turns the record-gap analysis from design discipline into compliance urgency.

## 5. Method Notes

- Search window: Sept 6–13, 2026 publications plus standing sweeps (open-weight releases, agent-payment protocols, monetization/ARR, behavioral/nudge, neuromarketing, privacy/FL, DevEx surveys, agent payments, OSS funding).
- Dedup method: targeted grep against category directories and the full library for every candidate's distinctive identifiers (protocol names, company/product names, arXiv IDs, benchmark figures) before creation; every new skill names its nearest-neighbor existing skills in Pairs-with to prevent future duplication.
- Cycle numbering: one cycle per calendar day (Pacific/Auckland local date 2026-09-13); this is cycle 25, run 1. Previous run (cycle 24, run 2) completed on the 2026-09-12 local calendar.
- Report file naming follows the established `_daily-research-report-YYYY-MM-DD[-runN].md` convention.

---

*Report compiled by A-Tech Research Division — Cycle 25, Run 1 — 2026-09-13 (Pacific/Auckland)*