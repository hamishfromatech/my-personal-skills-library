# A-Tech Daily Research Report — 2026-08-31 (Cycle 14b, second run)

**Scope**: Neuro-marketing & behavioral psychology · AI revenue models & open-source monetization · Developer experience · Privacy-first design
**Method**: Web scan across the four domains → hard deduplication against existing skills (including a re-verification pass against the full on-disk library, not just the README index) → 4 new skills created, 6 duplicate candidates correctly rejected → index updated
**Deliverables**: 4 new SKILL.md folders + README cycle-14b entries + this report

---

## 1. Scan & Research Summary

### What was checked for duplication (and deliberately NOT re-created)

This cycle's most important quality event: **six strong candidates were created, then deleted after re-verification against the on-disk library** — the README index is incomplete relative to disk, so index-only dedup is unsafe. Rejected as duplicates of existing skills:

| Candidate (this cycle) | Superseded by (existing skill) | Verdict |
|---|---|---|
| GitHub Copilot production-scale workload (3.2M users, 13M sessions, 95T tokens) | `developer-experience-and-flow/agentic-coding-production-characterization` — already covers the identical Liu et al. Azure/UIUC paper (cycle-13 report also lists it in its skip list) | Deleted |
| KDD SE 3.0 early-adoption study (25,264 PRs, 2,361 repos) as a standalone DevEx skill | Kept, but **refocused to community-and-growth** and crossed with Koren et al. — the DevEx-side facts are better housed with the maintainer-economics argument than as another adoption stat | Reframed |
| Claude Code leak / Amazon code-safety-reset / Linux-Rust tagging convergence | `community-and-growth/ai-agent-open-source-governance` (already covers the policy landscape) + `vibe-coding-state-of-art-review` (already covers the leak, clean-room crisis, Amazon reset, kernel/Rust policies in depth) | Deleted |
| Scarcity/endorsement nulls (Frontiers S-O-R study, N=609) | `neuromarketing-sor-trait-moderation-model` + `neuromarketing-consumer-impulsivity-trait-moderation` — both already cover the same study including the scarcity/endorsement insignificance finding | Deleted |
| Neuromarketing-AI CXM framework + vignette ethics (Topcugil & Hiziroglu, Future Business Journal) | `marketing-and-content/neuromarketing-ai-cxm-integration-framework` (same paper, same framework and vignettes) | Deleted |
| CodeStruct AST action space | `developer-experience-and-flow/codestruct-ast-action-space` (created in cycle 6, report verified) | Deleted |

### Fresh signals identified (this cycle's 4 skills)

| Signal | Source | Why it matters now |
|---|---|---|
| Project-level agentic adoption is broad but shallow; oversight is single-human; Koren et al. model shows welfare loss via engagement collapse | Raida & Hou (KDD SE 3.0, Aug 9 2026) + Koren/Békés/Hinz/Lohmann (arXiv:2601.15494) | First rigorous pairing of adoption data with the maintainer-compensation mechanism; the funding-layer answer to two existing skills |
| Open-weight licenses now regulate business type and scale, not just commercial use | Kimi K3 clause, Qwen3.8-Max "AI Work Assistant", Qwen3.8-Flash-Next no-threshold license, MiniMax M2.7→M3 and H3 geography (Reuters Aug 7/26; 36Kr Aug 27) | A new licensing axis distinct from the metered-revenue-share skill; includes the litigation-shield use of geography |
| Topology-aware DP for FL: per-client noise allocation with degeneracy guarantee | Fulcrum (Rangwala, Sinnott & Buyya, U. Melbourne, GPL-3.0 codebase) | Fills the spatial axis of the DP-FL stack; adoptable unconditionally; comes with its own attack (TADI) |
| Language migration as escape-routes-become-toll-roads | JetBrains Research language-migration analysis (2025 State of Dev Ecosystem) | Durable strategic frame for language choice; gains a new twist from agentic rewriting economics |

---

## 2. Skills Created (4)

### 2.1 Agentic OSS Economics 2026
**Category**: `community-and-growth/agentic-oss-economics-2026/`
**Evidence**: 25,264 agentic PRs / 2,361 repos: median 1–2 agentic PRs per repo per quarter; small projects (1–5 contributors) adopt hardest (mean 50.2 PRs/repo); single-human oversight in 78.9% (1 reviewer + 1 committer, same person); multi-human only 11.3%; 1% exceed 36 PRs/participant. Koren et al.: demand shock up, engagement compensation down → welfare falls despite productivity gains (curl bug-bounty closure as instance).
**Skill value**: Three-regime adoption diagnostic; oversight-bottleneck quantification; engagement-compensation diagnostic; five-channel compensation redesign (managed hosting, enterprise controls, foundation funding, metered license, sponsored triage); five-part oversight protocol against bus-factor-1.
**A-Tech fit**: extends `open-source-maintainer-ai-burden` and `oss-public-knowledge-erosion` from load/erosion into maintainer *economics*; supplies the evidence base `ai-agent-open-source-governance` lacked.
**Files**: `SKILL.md`

### 2.2 License Axis: Business Type and Scale
**Category**: `monetization-and-revenue/license-axis-business-type/`
**Evidence**: Six-pattern 2026 landscape: unmodified MIT (DeepSeek V4-Pro, GLM-5.3-Flash) / conditional-free (MiniMax M3, >$20M product revenue) / business-type carve-out (Kimi K3: MaaS + >$20M group revenue, internal use exempt) / category carve-out (Qwen3.8-Max: "AI Work Assistant" naming Qoder & QwenWork, >$50M, siblings Apache) / no-threshold category license (Qwen3.8-Flash-Next, Qwen Community License 1.0) / geographic exclusion (MiniMax H3: US/EU/UK/KR — litigation shield).
**Skill value**: New-axis locator table; substitution test (MIT substitute + ecosystem gravity) as the viability gate; flagship-restriction pattern; five-signal reading table; precedents (MongoDB/Elastic/HashiCorp; Unreal Engine threshold).
**A-Tech fit**: complements `metered-open-license-revenue-share-2026` (the meter's economics) with the who-and-what axis; legible-carve-out position for open values.
**Files**: `SKILL.md`

### 2.3 Fulcrum Topology-Aware DP
**Category**: `privacy-and-trust/fulcrum-topology-aware-dp/`
**Evidence**: Additive per-client MI bound (mechanism term + prior-coupling term ℓ°ᵢ); allocation σ*²ᵢ = a/(K★ − ℓ°ᵢ); degenerates to uniform on symmetric topologies; TADI attack with four channel ablations; privacy-bound gaps up to 0.871 nats (ER) / 1.193 nats (BA); up to 26.6% relative improvement on Fed-ISIC2019; TOST equivalence (no utility cost). Three leverage proxies: SBM group size, graph degree, dataset-size influence.
**Skill value**: Spatial axis of the DP-FL stack (vs temporal-axis `adadp-fedsec-*`, engineering-path `dptrainer-*`, wire-axis ZK/secure-aggregation); run-the-attack-first workflow; unconditional-adoptability guarantee; adoption decision test.
**A-Tech fit**: open GPL-3.0 replication package; closed-form math (no training-cost penalty); honest caveats (preprint, author-run, passive adversary, classification-only).
**Files**: `SKILL.md`

### 2.4 IDE Escape Routes Toll Roads
**Category**: `developer-experience-and-flow/ide-escape-route-toll-roads/`
**Evidence**: JetBrains Research language-migration mapping (2025 State of Developer Ecosystem): project requirements dominate stated switches; retention governed by accumulated tolls (hiring, tooling, muscle memory, certifications) rather than technical merit.
**Skill value**: Three-driver classification; incumbent toll-road audit (each friction seeds a competitor's migration product; captives become detractors); build-or-toll trade-off; polyglot portfolio discipline; agentic twist — agents compress craft tolls, organizational tolls persist.
**A-Tech fit**: escape-route preservation is an open-source values argument (multi-vendor, permissive implementations); pairs with `oss-license-trap-fork-cycle` and `comprehension-debt-framework`.
**Files**: `SKILL.md`

---

## 3. A-Tech Values Alignment Matrix

| Skill | Open-Source AI | Data Privacy | Financial Freedom | Practical Implementation |
|---|---|---|---|---|
| Agentic OSS Economics 2026 | ☑ direct commons defense; evidence for contribution policy | ☑ works only on public PR metadata | ☑ maintainer compensation redesign; prevents burnout-driven project death | ☑ diagnostics + compensation table + oversight protocol |
| License Axis Business Type | ☑ keeps "open weights" legible; MIT-substitution discipline | ☑ geographic carve-outs read as risk-offloading, not privacy design | ☑ license risk as model-choice input; MIT fallback mapping | ☑ six-pattern table + substitution test + flagship pattern |
| Fulcrum Topology-Aware DP | ☑ GPL-3.0 codebase, attack + defence + docs | ☑ formal per-client MI bound; weakest clients protected to common K★ | ☑ degeneracy = zero engineering cost where unneeded | ☑ closed-form allocation, Opacus wrapper, 3 benchmarks + TOST |
| IDE Escape Routes Toll Roads | ☑ multi-vendor/permissive ecosystems = open escape routes | — (not privacy-focused) | ☑ toll audit = tech-debt valuation; optionality protection | ☑ audit checklist + portfolio discipline + agentic re-weighting |

---

## 4. Cross-Category Synthesis

**The cycle's connective tissue is *exit costs and who pays them*.** Agentic OSS economics shows the commons' revenue model (engagement) failing while usage grows — maintainers holding an asset nobody pays for. The license-axis skill shows labs inventing new claim structures on downstream users (business type, group revenue, geography) precisely because weights alone no longer capture value. The toll-roads skill shows the same structure at language scale: ecosystems monetize (or ease) exit. And Fulcrum is the counter-example: a privacy mechanism deliberately engineered to *degenerate to the status quo* where it adds nothing — the opposite of a toll road.

The strategic thread for A-Tech: wherever a gatekeeper can raise exit costs (licenses, ecosystems, engagement dependencies), the durable plays are (a) preserve escape routes (MIT substitutes, multi-vendor stacks, permissive ecosystems), (b) fund the commons through channels that survive engagement collapse, and (c) adopt mechanisms whose failure mode is "revert to uniform," not "lock-in."

---

## 5. Candidates Considered but Deferred

- **Amazon code-safety reset as its own skill**: real governance signal, but `vibe-coding-state-of-art-review` and `ai-agent-open-source-governance` already carry the content; revisit only if Amazon publishes a formal post-mortem with new mechanics.
- **Neuromarketing EEG/eye-tracking accuracy-race papers** (TCN/GAT 88–89% lines, ranking-EEG study from Fuzhou): crowded space (`multimodal-eeg-*`, `hybrid-eeg-*`); no new mechanism. The ranking-scrutiny angle (low-ranked products drawing early visual attention) is a plausible future skill if replicated beyond one study.
- **AI-assisted development within the JetBrains escape-routes post itself**: covered by existing adoption/DevEx skills; this cycle kept only the migration/toll frame, which was genuinely new.
- **DP-FedSOFIM (TMLR 2026, second-order Fisher preconditioning for DP-FL)**: interesting optimizer-level contribution, but the FL-optimization shelf is deep (FedGSA, FedMOP, FedNewton, FedSOFIM-adjacent entries) and its practical delta for A-Tech deployment guidance is thinner than Fulcrum's decision-test framing.

## 6. Next-Cycle Watchlist

1. **Moonshot ↔ US-cloud deal close/split** — first market price for a frontier open model's distribution meter (tracked in `kimi-cloud-revshare-watch`; license-axis skill will absorb the outcome either way).
2. **DeepSeek holding the MIT line** — the substitution test's live control; if V4's successor stays unmodified MIT, business-type carve-outs stay speculative.
3. **Independent reproduction of Fulcrum's topology gaps** — preprint with author-run results; one external replication would upgrade it from promising to adoptable.
4. **Whether Qwen Community License 1.0 (no-threshold) spreads to flagship releases** — the tightening-direction signal.
5. **First maintainer-funding products built explicitly on the engagement-collapse thesis** (e.g., corporate-sponsored-triage programs) — would validate the compensation-design table.
6. **Agentic rewriting benchmarks on brownfield estates** — tests the toll-roads skill's prediction that organizational (not craft) tolls now dominate language lock-in.

---

*Report generated 2026-08-31 (Pacific/Auckland) · A-Tech Research Division · Cycle 14b (second run) · 4 skills verified on disk with YAML frontmatter and "Use when" triggers; 6 duplicate candidates rejected with evidence.*
