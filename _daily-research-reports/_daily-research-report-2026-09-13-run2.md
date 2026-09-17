# A-Tech Daily Research Report — 2026-09-13 (Cycle 25, Run 2)

**Scope:** Open-source AI models & business models · Behavioral psychology & nudging · Neuromarketing/consumer neuroscience · Privacy-first AI · Developer experience · Community & growth · Financial freedom · Agent payments
**Method:** Five web sweeps across the standing domains (Sept 12–14, 2026 window: OSS-tool funding, consent-gated product launches, PII-middleware releases, leader-side AI-code surveys, DP/FL papers, neuromarketing preprints) → targeted grep dedup against the on-disk library → 5 new standalone SKILL.md folders across 4 category directories + 1 in-place refresh → this report → README cycle-25 run-2 index update
**Deliverables:** 5 new SKILL.md folders + 1 refresh + README run-2 index + this report
**Coordination note:** library state surveyed before writes; cycle-25 run-1 (Ant AMP/KYA, Agent-Payment Record Gap, Alipay Skill Pay, Gander + china-open-source-llm-arr-tracker refresh) was fully on disk before this run's creation. All five selections passed targeted greps against the live library before creation, and each new skill names its nearest neighbors in Pairs-with.

---

## 1. Research Summary

This run continues cycle 25 on the same calendar day (run-1 completed earlier on Sept 13 local). The Sept 12–14 window produced one dominant theme — **the agent-runtime and consent layers moved from protocols to working open products, and both monetize things that are not the model** — plus a leader-side DevEx dataset that reframes the verification bottleneck, and a behavioral-science field experiment that formalizes "inform without demanding." Five selections passed grep dedup; the headline clusters:

1. **The agent fleet got its runtime, and a solo founder got paid for it.** Herdr (Can Celik, Rust, Apache-2.0; launched March 2026) is the first terminal multiplexer purpose-built for coding agents — sessions classified into working/blocked/done/idle/unknown, surfaced as an attention queue in a sidebar, with multi-machine federation (local + SSH servers in one status surface). 36,000+ GitHub stars and 780K downloads in ~4 months, 1,000+ community plugins, and a **$6M seed (Bessemer, announced ~Sept 10) with Y Combinator, e2vc, Tobi Lütke, and Cloudflare CTO Dane Knecht** — the strongest recent solo-OSS funding datapoint in the library, and the attention-queue sibling to the library's ambient-inbox (Pizza Bot) and voice (Gander) lanes. Omarchy v4 "Quattro" integrated it (DHH contributing patches).
2. **Consent-based data-for-compute crossed into consumer coding agents.** Bolt Forge (research preview Sept 14) is the first product where users trade anonymized build sessions (secrets stripped, DPA-governed, Arcee AI as training partner) for up to **50× usage allocation** on open models (GLM 5.3 Flash default; Kimi K3 and DeepSeek v4 Pro as premium experimental options) scoring ~91% of the top paid model on Bolt's own Build Index (92.2 vs 101.0). The "allocation instead of credits" pricing plus consent-first corpus training makes this the first consumer-scale instantiation of the data-for-compute barter pattern — folded as a CYCLE-25-RUN-2 ADDENDUM into `china-open-source-llm-arr-tracker` (fifth mechanism lane: **consent-corpus barter**).
3. **The leader-side verification picture inverted the developer-side one.** New Relic × Hanover "2026 State of AI Coding" (N=200 leaders, June 2026): **94% rate AI code HIGHER quality at review time, yet 78% report more production incidents, 86% more senior firefighting, and 82% hit ≥1 production failure tied to AI code in six months.** Both pictures are accurate — review-time quality is readability, production performance is runtime behavior. The loop: close the 62% blind-trust gap, budget the operational tax explicitly, map the four failure modes (~3-in-10 orgs each: integration 31%, compliance 30%, data integrity 29%, security 28%) to detection patterns, move telemetry into the prompt (78% of teams already do), and treat the observability platform as the system of record.
4. **The mask-before-call privacy pattern got two reference implementations.** Septum (MIT) and ElyAgent (Elastic License v2) formalize PII masking before any LLM call with approval gates, zone architecture (air-gapped modules never importing core), 17 regulation packs (GDPR/KVKK/HIPAA...), structural HITL on 30+ tool categories, and zero-knowledge vaults. This is the middle path between fully-local agents (Kryzz AI, KIRA, Sutra — also Sept 2026) and raw cloud agents.
5. **Façade success got its diagnosis loop.** ElyAgent v2.2 + KIRA Superapp independently converge on the same runtime discipline: record whether the agent *actually* did what it claimed after each autonomous run, flag "reported success, no real effect," diagnose, and surface a reviewable fix proposal. The model is the planner; the runtime owns execution, permissions, and proof — the general-runtime sibling to the library's agent-payment-record-gap skill.

### Fresh Signals (5 new skills)

| # | Skill | Category | Source | Why it matters now |
|---|---|---|---|---|
| 1 | Herdr Agent-Fleet Terminal Multiplexer | ai-agents-and-workflows | Herdr GitHub; heise; Lookonchain/Fundz Sept 10–13, 2026 | The ATTENTION-QUEUE pattern: terminal as agent-fleet supervisor (five states, three detection mechanisms, multi-machine federation); the strongest solo-OSS funding datapoint ($6M seed on 36K stars) |
| 2 | Bolt Forge Consent-Training Corpus | financial-freedom-and-wealth | Bolt.new launch post Sept 14, 2026 | The CONSENT-CORPUS-BARTER pattern: anonymized sessions for 50× allocation, DPA-governed, weights publish back; open models at ~91% of paid-model quality in a consumer agent |
| 3 | Agentic Verification & Observability Loop | developer-experience-and-flow | New Relic × Hanover, June 2026 (N=200) | The LEADER-SIDE trust inversion: 94% higher review quality vs 78% more incidents — runtime evidence becomes the comprehension signal; telemetry-in-the-prompt |
| 4 | Sovereign PII-Masking Middleware | privacy-and-trust | Septum (MIT) + ElyAgent (ELv2), Sept 2026 | The MASK-BEFORE-CALL pattern with approval gates, zone architecture, 17 regulation packs, and structural HITL — "your data never leaves, your AI still works" |
| 5 | Façade-Success Diagnosis Loop | ai-agents-and-workflows | ElyAgent v2.2 + KIRA Superapp, Sept 2026 | The EVIDENCE-BASED-COMPLETION pattern: runtime witnesses tool execution, flags "reported success, no real effect," measures real vs declared success rate |

---

## 2. Deduplication & Novelty Assessment

All five selections passed targeted greps against the live library:

- **Herdr** — grep for "Herdr|agent multiplexer|terminal multiplexer coding agent" across the full library: **zero matches**. The attention-queue pattern for agent fleets is uncaptured. **Novel.**
- **Bolt Forge / consent-corpus barter** — grep for "Bolt Forge|data-for-compute|consent-based training corpus": only `prompt-data-barter-pricing-muse-spark` (Meta's per-token market price for prompt data, cycle 18). The **consumer-product, consent-gated, weights-publish-back** form is uncaptured. **Novel** (with tracker addendum capturing the mechanism-lane delta).
- **New Relic leader-side data** — grep for "94%|86%|82% production failure|observability loop": the library holds the developer-side (Sonar 96% trust gap) and the maintainability RCT (`echoes-of-ai-maintainability-rct`), but no skill holds the leader-side inversion (94% higher review quality vs 78% more incidents). **Novel.**
- **Septum/ElyAgent mask-before-call** — grep for "Septum|Presidio|PII masking before LLM|de-anonymization middleware": **zero matches**. **Novel.**
- **Façade-success diagnosis loop** — grep for "façade|facade success|ElyAgent|KIRA": **zero matches**. The runtime-witness pattern is uncaptured as a skill. **Novel** (sibling to `agent-payment-record-gap`, cross-linked).

### Already tracked (no action)
- Mozilla State of Open Source AI (held, cycle 16; harness-layer thesis re-confirmed in sweep, no new mechanism)
- JetBrains 90%/Claude Code 39%/OpenCode 7%-42%-mindshare (held, cycle 23 run 1 + cycle 16 run-2 refresh — re-confirmed, no above-bar delta)
- Temporal State of Development 2026 (held, cycle 14)
- GitKraken proof gap (held, cycle 21)
- Anthropic 2026 Agentic Coding Trends (held in substance across SDLC/adoption skills)
- Moonshot $2B ARR / 30% rev-share / distillation allegations (held, cycle 24)
- Kimi 300B daily tokens / OpenRouter dip analysis (held by ARR tracker; no new mechanism)
- DP-DyLoRA (Samsung; flagged below-bar in cycle-25 run 1 — promotion trigger still stands: second independent implementation or production DP-FL deployment)
- XCal-FL (held, cycle 21 run 2)
- Component-aware α-split DP (held, cycle 24 run 2)
- Split-LLM obfuscation (held, cycle 23 run 1)
- FGLGuard (held, cycle 22 run 1)
- NeuMa hybrid GFT scheme (Brain Informatics 2025) — the multimodal EEG+ET consumer-choice decoding entry point; the library's neuromarketing stack (three-layer discipline, PRISMA meta-analysis, EEG+ET pipelines) already holds the method family at the calibration bar; no new mechanism beyond the GFT feature choice
- Prescription-error information intervention (arXiv:2609.09673, held as flagged in cycle-24 run-1's honest sweep; the 8.6% error reduction via non-mandatory information is a healthcare-domain case of the library's non-mandatory-information pattern — below standalone bar)
- Influenza-vaccination nudge, Thailand taxpayer RCT, France Travail aspiration study, Swiss true-cost food-policy conjoint (behavioral window items held or below bar)
- EV-charging incentive/scarcity experiment (infrastructure-scarcity context; the incentive-design pattern is held by nudge-complementarity family)
- Kryzz AI, KIRA Superapp, Sutra (local-first OSS agents; captured as context in the PII-middleware and façade-success skills rather than standalone — the patterns are held)
- Herdr's multi-machine management (Sept 7) — folded into the Herdr skill itself

### Below-bar candidates (flagged, not created)
- **NeuMa-42 multimodal consumer-choice decoding** (Kalaganis/Georgiadis et al., Brain Informatics 12:23, 2025): strong method entry point (GFT + gaze fusion, κ≈0.35) but the library holds the three-layer neuromarketing discipline, the PRISMA meta-analysis (moderate, context-dependent effects), and multiple EEG+ET pipelines — no new mechanism beyond graph signal processing as a feature choice. Flag as a methods footnote candidate for a future A-Tech neuro-methods piece.
- **ataLSTM neuromarketing recommender** (Yang et al., Sci Rep, Aug 31 2026; 92.42% EEG-driven like/dislike, 14-channel EMOTIV) — already held as `marketing-and-content/ataLSTM-eeg-recommender-attention` (cycle 21 run 3); the "recommender-system framing" is the same paper's contribution, already captured.
- **Prescription-error information intervention** (arXiv:2609.09673, Sept 9): 2.81M prescriptions, 1,700 physicians, 8.6% DDI-error reduction with ~$4.8M annualized savings — strong field-experiment craft, but it is a domain case of "non-mandatory information beats mandatory alerts," a mechanism the library's WhatsApp-backfire skill already frames (and its complement). Flagged as a candidate for a healthcare-flavored A-Tech case study, not a new mechanism.
- **EV-charging incentive/scarcity experiment** (Garg et al., JEEM 2026): incentives + environmental messages + perceived-scarce infrastructure — good scarcity-design datapoint; held by the library's nudge-complementarity/incentive-design family.
- **Zenodo EEG+CV purchase-intent preprints** (May 2026; 88.3% / 89.2% accuracy claims): unreviewed preprints with vendor-style accuracy claims and no code — below the library's small-N/review bar; flagged.
- **Herdr's plugin marketplace curation status**: flagged inside the Herdr skill as an honesty caveat rather than a standalone skill.

## 3. Skill Library State After This Run

- New (5): `ai-agents-and-workflows/herdr-agent-fleet-terminal-multiplexer/` · `ai-agents-and-workflows/facade-success-diagnosis-loop/` · `financial-freedom-and-wealth/bolt-forge-consent-training-corpus/` · `developer-experience-and-flow/agentic-verification-observability-loop/` · `privacy-and-trust/sovereign-pii-masking-middleware/`
- Refreshed in place (1): `monetization-and-revenue/china-open-source-llm-arr-tracker/` (CYCLE-25-RUN-2 ADDENDUM: consent-corpus barter as the fifth mechanism lane + open-weight consumer distribution via Bolt Forge's open-model lineup; watch items: allocation survival past Oct 14, per-token barter convergence)
- All SKILL.md files follow the Agent Skills spec: YAML frontmatter (name + description with use-when/NOT-for triggers), under 500 lines (verified: 53–54 lines each), Overview → When to Use → Core Process/Workflow → Key Evidence → Pairs-with cross-links → A-Tech alignment → Honesty Caveats → sources.

## 4. Watch Items for the Next Run

1. **Forge allocation survival** — whether the 50× allocation and the Arcee AI training run (first run October 2026) produce published model weights and any usage/adoption metrics after the Oct 14 preview close; whether per-token data barter (Muse Spark) and consent-corpus barter converge.
2. **Herdr hiring & monetization** — whether the $6M seed converts to a sustainable business model (sponsors + partnerships) without closing the Apache-2.0 core; whether the plugin marketplace gets curation; multi-machine adoption metrics.
3. **PII-middleware adoption** — whether Septum/ElyAgent get production deployments or CVEs; whether a second regulation-pack wave (more jurisdictions) or an enterprise-grade fork emerges; watch the Elastic-License-vs-GPL boundary cases.
4. **Façade-success benchmarking** — whether either project publishes measured façade-success rates or a shared benchmark for the tool-result loop; whether the pattern gets adopted by a mainstream agent runtime (two or more = pattern graduation).
5. **Leader-side verification data triangulation** — whether a second independent leader-side survey confirms the 94%-vs-78% inversion; watch for a DORA-style framework absorbing the operational-tax framing.
6. **Neuromarketing methods** — whether the NeuMa/GFT hybrid scheme gets an independent replication with ≥40 participants + HD-EEG (the authors' own confirmatory guidance).
7. **EU CRA first agent-scope enforcement action** — carried from run 1; still the signal that turns the record-gap discipline into compliance urgency.
8. **Anthropic S-1** (reported October 2026) — carried from run 1; public-market look at frontier-lab gross margins remains the largest repricing event on the calendar.

## 5. Method Notes

- Search window: Sept 12–14, 2026 publications plus standing sweeps (open-weight releases, agent-payment protocols, monetization/ARR, behavioral/nudge, neuromarketing, privacy/FL, DevEx surveys, agent payments, OSS funding).
- Dedup method: targeted grep against category directories and the full library for every candidate's distinctive identifiers (product names, paper IDs, benchmark figures) before creation; every new skill names its nearest-neighbor existing skills in Pairs-with to prevent future duplication.
- Cycle numbering: one cycle per calendar day (Pacific/Auckland local date 2026-09-13); this is cycle 25, run 2. Run 1 (Ant AMP/KYA, record gap, Skill Pay, Gander + ARR-tracker refresh) completed earlier the same calendar day.
- Report file naming follows the established `_daily-research-report-YYYY-MM-DD[-runN].md` convention.

---

*Report compiled by A-Tech Research Division — Cycle 25, Run 2 — 2026-09-13 (Pacific/Auckland)*