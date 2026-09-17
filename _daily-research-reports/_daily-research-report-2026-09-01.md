# A-Tech Daily Research Report — 2026-09-01 (Cycle 15, run 1)

**Scope:** Behavioral / digital well-being economics · AI marketing reception · AI revenue and licensing watch · Developer experience · Differential-privacy research
**Method:** Web scan across the six standing domains → hard dedup against the on-disk library (cycle-14b's lesson: indexed-only dedup is unsafe) → 3 new skills, 1 updated skill, 1 addendum → index updated → this report
**Deliverables:** 3 new SKILL.md folders + 1 skill SKILL.md update + 1 addendum doc + README cycle-15 entries + this report

---

## 1. Scan & Research Summary

Six web sweeps across the standing A-Tech research domains. Strongest signals came from the behavioral-economics and developer-experience sweeps; the privacy and licensing sweeps were dominated by known entities.

### What Was Checked and Deliberately Skipped

| Candidate | Reason for rejection |
|---|---|
| GitHub Copilot's agentic traces at production scale (Liu et al., Microsoft Azure, Aug 2026) | Known duplicate — `agentic-coding-production-characterization` already houses it (explicitly rejected in cycle 14b) |
| "The Devil Is in the Interface" (Xu et al., tool architecture ablation across 6 tool setups, 11,700 trajectories) | `developer-experience-and-flow/tool-architecture-coding-agent-behavior` already covers the tool-architecture/agent-behavior question |
| Topcugil & Hiziroglu, "Neuromarketing and AI: CXM from a firm perspective" (Future Business Journal, Aug 14 2026) | Same paper already flagged and rejected as duplicate in cycle 13's report |
| DDP-SA-Adaptive (Wei et al. follow-up to DDP-SA) | Not a new concept — an incremental update to `ddp-sa-distributed-dp-secure-aggregation`. Handled as an addendum inside that skill, not a new skill |
| Wellspent (JMIR), context-aware CBT intervention app, adversarial-design friction framework (IJHCI) | The intervention-design space is already crowded (`ai-motivational-interviewing-scale` + `adaptive-digital-nudging-llm-architecture` + `behavioral-design-practical-playbooks`) and these add no new mechanism beyond what this cycle's new skills already capture |

### Fresh Signals (this cycle's 3 new skills)

| Signal | Source | Why it matters now |
|---|---|---|
| Digital-addiction field experiment quantifies both behavioral levers (habit formation, self-control problems) and shows self-set limits outperform externally-imposed limits | Allcott, Gentzkow & Song (Stanford/UIUC), Hamilton Project brief, Jun 2026 (exp. originally AER 2022) | First rigorous economic model to quantify *both* nudge levers; gives A-Tech concrete, empirically-grounded parameters for any screen-time/attention-wellbeing product design, plus a litigation-relevant policy landscape (K.G.M. v. Meta verdict, Feb 2026) |
| An AI assistant that *steers attention* by scoring every screenshot against a user-stated intention and intervening with dismissible nudges; exposes the "harmful-intention reinforcement" failure mode | Choi, Lee, Kim, Min, Knox, M.K. Lee & K. Lee (KAIST/UT-Austin/Yonsei/U-Seoul), arXiv 2026 | Novel — first end-to-end implementation of the LLM-as-attention-governor pattern (INA), including an intention clarification stage; introduces the novel safety trap: the assistant will happily encourage a *harmful* stated goal unless an external guardrail is bolted on |
| AI-ad "theme match" acceptance rule | Choi & Magee (U. Mississippi), Journal of Interactive Advertising, reported Aug 31 2026 | Fills a gap `ai-generated-ad-pretesting-effectiveness` only partially covered — a pre-production *decision rule* (not a post-hoc metric) for when AI-generated content is acceptable to audiences, grounded in two natural experiments (Coca-Cola's 2023 vs 2025 holiday campaigns) |

---

## 2. Skills Created (3)

### 2.1 Digital Addiction Economics 2026
**Category:** `behavioral-psychology-and-nudging/digital-addiction-economics-2026/`
**Evidence:** Allcott/Gentzkow/Song field experiment (2 interventions, ~2,000 Android users, 2020, AER 2022; June 2026 Brookings/Hamilton Project brief): (a) paying users $2.50/hr to reduce social media for 3 weeks → −39% use (56 min/day), **still −12 min/day (8%) 6 weeks post-incentive** — direct habit-formation signature; (b) self-set screen-time limits → −22 min/day (16%) with **89% adoption and WTP $4.20 per 3 weeks** for the limit tool — direct self-control-problem signature. Combined model: self-control + habit account for **~31% (~48 min/day) of total social media use**; ~$4.62/person/3 weeks welfare loss ≈ **$20.3B/yr US-wide**; well-being lift +0.09 SD (Bonus arm).
**Skill value:** field-experiment blueprint (Bonus→persistent effect, Limit→WTP); the commitment-flexibility ("flexibility through delay") design principle; a policy/litigation exposure table (K.G.M. v. Meta et al. — first liable verdict + $6M damages, Feb 2026; NY SAFE for Kids Act; EC preliminary TikTok addictive-design finding under DSA); honest-effect-size guidance; a privacy-preserving measurement pattern (on-device telemetry without content capture).
**A-Tech fit:** extends `boosting-empowering-behavior-change` (which only cites one sec app anecdotally) into a full economic model with quantified magnitudes; informs privacy-first digital well-being product design.
**Files:** `SKILL.md`, `references/allcott-gentzkow-song-evidence-base.md`

### 2.2 Intent Assistant (INA): Attention-Steering Agents
**Category:** `behavioral-psychology-and-nudging/intent-assistant-attention-steering/`
**Evidence:** Choi et al. (KAIST/UT-Austin/Yonsei/U-Seoul; arXiv:2510.14513, 2026). Gemini-2.0-Flash vision-LLM scores every 2s screenshot + app metadata against a user-stated intention (rubric-anchored 0–1 distractibility score, chain-of-thought before scoring); sustained-transition trigger fires polite, dismissible notifications; user feedback refines the scoring prompt in-session. IntentionBench: 0.878 accuracy / 0.845 F1. Three-week field study (N=22, within-subject): off-task ratio 0.104 vs 0.166 (simple reminder), intention-alignment 4.44 vs 4.23, focused immersion 3.74 vs 2.90 (logging-only). Key negative findings that transfer: ~half the sample raised privacy concerns despite on-device masking; notification fatigue; forced per-session Q&A frustration; the harmful-intention reinforcement trap.
**Skill value:** the reusable design pattern (intention elicitation → continuous scoring → gentle intervention → feedback refinement); exact prompt-structure constants (6-band rubric, JSON-only schema); the INA safety trap as a mandatory guard (the system will actively *encourage* a user-stated harmful intention like "hack a hospital server" and nudge them *back to it*); mitigation via WildGuard or similar guardrail-model gating, framed as non-optional.
**A-Tech fit:** open-source replicable with open-weights; frames on-device processing as an architectural requirement (privacy concerns appeared even with cloud masking); pairs directly with `digital-addiction-economics-2026` for the attention-economy product-design story.
**Files:** `SKILL.md`

### 2.3 AI-Ad Theme Match: The AI-Necessity Decision Rule
**Category:** `marketing-and-content/ai-ad-theme-match-necessity/`
**Evidence:** Choi & Magee (U. Mississippi, *Journal of Interactive Advertising*, 2026; reported Aug 31). The "AI-ad theme match": audiences accept AI in advertising when AI is central to the campaign's idea (Coca-Cola's 2023 Create Real Magic — fan co-creation at a scale impossible without AI) and reject it when AI substitutes for humans without adding a new capability (Coca-Cola's 2025 AI "Holidays Are Coming" — mocked on YouTube, "the most profitable commercial in Pepsi's history"). Uncanny-valley response is triggered when the machine simulates emotion ("machines don't feel, but this machine is telling me it does"); organizations should remind consumers "we are an organization of people."
**Skill value:** a pre-production three-question test — (1) "What does the *AI-necessary* part of this ad do that a human can't do at all?", (2) "Would a consumer understand in one sentence *why* AI was needed here?", (3) "Does the AI carry the emotional payload?" — with the case-study pair (2023 success vs 2025 flop) as evidence anchors, plus norm-drift guidance.
**A-Tech fit:** complements `ai-generated-ad-pretesting-effectiveness` (post-test validity conditions) with a *pre-production* acceptance decision rule; the necessity test doubles as an authenticity/credibility screen for A-Tech's own AI-generated marketing assets.
**Files:** `SKILL.md`

---

## 3. Updated Skills & Addendum (2)

### 3.1 Kimi Cloud Revenue-Share Watch — 2026-09-01 verification entry
**Category:** `monetization-and-revenue/kimi-cloud-revshare-watch/`
**Update:** re-verified via Reuters/Globe & Mail/Business Times/Silicon Republic/ETCIO/IBTimes/Yahoo/Quartz wire checks (Aug 26–30, 2026): talks remain open and unsigned; the three unresolved issues (split, data access, token-usage audit) are unchanged. New confirmed financing context: May round **$2B at ~$20B valuation**, late-August reported **$3.5bn at ~$35bn** (National AI Industry Investment Fund lead), reported **$50bn pre-money talks for a HK IPO timing "in the next few months."** Alibaba reported as seeking analogous rev-share terms with major users of Qwen3.8-Max. Geopolitical overhang unchanged (possible trade-blacklisting; unproven Fable-distillation claims, denied). Next scheduled re-check: any signed deal / split / audit-mechanism terms.
**Update type:** watchlist append (no conceptual change; the "who pays / what's unresolved" structure stands).

### 3.2 DDP-SA — Adaptive successor addendum
**Category:** `privacy-and-trust/ddp-sa-distributed-dp-secure-aggregation/`
**Evidence:** Wei, Jammine & Nait-Abdesselam (Université Paris Cité), arXiv:2608.15153v1 — "DDP-SA-Adaptive": replaces DDP-SA's *static* per-client clipping threshold with **round-wise, layer-wise median-based adaptive clipping** on the *unclipped per-sample* gradient norms. Reported vs. static DDP-SA on federated linear regression: −6.81% communication rounds, −19.21% training time, −13.33% per-round time, −98.74% test loss, +3.41% R² (0.9666→0.9996), and **4× tighter ε at equal utility** (ε≈0.1 vs ε≈0.4 for R²=0.99). Encoding + secret-sharing pipeline and the DP guarantee (post-processing invariance, composition) are unchanged — a client-side, protocol-compatible upgrade.
**Skill value:** extends the DDP-SA skill with the adaptive mechanism, the privacy-budget compression result, the design implications (client-side-only deployment shift, no infrastructure rework), and stated caveats (regression-only benchmark, IID partitions, Laplace mechanism, no non-IID evidence yet). Cross-referenced against `adaptive-verifiable-federated-learning-2026` and `dp-lac-lightweight-adaptive-clipping` (adjacent server-side adaptive-clipping work).
**Update type:** addendum file + References-pointer patch (skill concept unchanged; an incremental improvement to an existing mechanism).

---

## 4. A-Tech Values Alignment Matrix

| Skill | Open-Source AI | Data Privacy | Financial Freedom | Practical Implementation |
|---|---|---|---|---|
| Digital Addiction Economics 2026 | ☑ open-standard commitment-tool design space | ☑ on-device behavioral telemetry without content capture | ☑ $20.3B/yr quantified internality + WTP $4.20 for limit tools | ☑ field-experiment blueprint + litigation-exposure table |
| Intent Assistant Attention-Steering | ☑ IntentionBench public; fully open-weights replicable architecture | ☑ surfaces ~50-participant privacy concerns; on-device processing as required end-state, not optimization | ☑ ~1.7h/day attention reclaimed at zero inference cost to user; pairs with `digital-addiction-economics-2026` | ☑ 4-loop design pattern + exact rubric/JSON constants + mandatory safety guard layer |
| AI-Ad Theme Match Necessity | ☑ open-weights content generation with an explicit authenticity/acceptance gate | ☑ discourages synthetic-emotion and uncanny-valley manipulations | ☑ avoids wasted AI production spend on assets audiences will reject | ☑ 3-question pre-production decision rule with two real case studies |
| Kimi Cloud Rev-Share Watch (update) | ☑ tracks whether open weights survive monetization at the distribution layer | ☑ data-access terms are a live negotiation point | ☑ threshold cliffs and split exposure for anyone building on open models | ☑ exposure triage table + three watch signals |
| DDP-SA (update) | ☑ open arXiv lineage, protocol-compatible enhancement | ☑ 4× tighter ε at equal utility on the same architecture | ☑ −19% training time = −19% private-FL infra cost | ☑ client-side patch, no deployment rework |

---

## 5. Cross-Category Synthesis

The unifying thread this cycle: **"the measured, self-imposed constraint" beats both its opposite extremes.**

- In **behavioral** work, Allcott/Gentzkow/Song show self-set limits (with calibrated friction, not immediate override) outperform blanket platform rules, and that paying people (extrinsic) as well as self-imposing (intrinsic) both move behavior but through different signatures: persistence and revealed WTP respectively.
- In **attention-steering** (INA), the constraint is a *stated intention* — not a blanket blocklist. The design shift from rule-based blockers to LLM-based semantic scoring is the same self-imposed-constraint pattern, upgraded with a vision model.
- In **marketing**, the constraint is the *necessity test*: AI-generated content is accepted only when AI is integral to the central creative idea, not as a cheap substitute — a self-regulation gate on the tool, not a ban on the tool.
- In **licensing** economics, the trend runs the other way: labs now monetize their *own* openness (Kimi K3's up-to-30% distribution-meter proposal), a constraint *imposed* rather than self-chosen — and the cycle's open question is whether adoption follows or bypasses such meters via unaltered MIT alternatives.

**Strategic thread for A-Tech:** build products where the constraint is *chosen by the user* and *enforced by transparent, auditable mechanisms* (self-set limits with delay-based friction, stated-intention steering with safety guardrails, necessity-tested AI content). This is the exact intersection of digital well-being, privacy-first design, and honest monetization.

## 6. Candidates Considered but Deferred

- **Wellspent RCT (JMIR mHealth 2026, N=70 iPhone users)** — sound autonomy-supportive design, modest effects (−29 min/day on the most-used app), no new mechanism beyond what `digital-addiction-economics-2026` + `adaptive-digital-nudging-llm-architecture` already capture.
- **"Adversarial Design for Digital Well-Being" (IJHCI, June 2026)** — five friction-design principles (Dynamic Delay, Progressive Obfuscation, Emotional Adaptation, Multimodal Resistance, Transparent Controllability). Conceptually adjacent to the INA/digital-addiction pair; would be a strong follow-up synthesis if A-Tech builds a friction-design playbook.
- **Context-aware CBT smartphone intervention (Computers in Human Behavior, Oct 2026)** — adaptive feedback beats self-monitoring-only in a 92-person RCT; worth a note in the adaptive-nudging architecture skill if that skill is revisited, but no independent skill.
- **Multifaceted nudge cross-over trial (Mobile Media & Comm., 2026)** — grayscale-mode / remove-from-home-screen nudges reduce screen time but raise user stress; methodologically solid but mechanism-overlapping with the above two.

## 7. Next-Cycle Watchlist

1. **INA safety-guard layer** — whether WildGuard-class guardrails become standard practice for intention-steering assistants, and whether the harmful-intention reinforcement trap is named in future system papers (it wasn't in INA's original disclosure).
2. **Meters for intention** — whether a commercial product ships the INA pattern first (A-Tech should prototype the two-loop, open-weights version: open-source local scorer + WildGuard-gated notification).
3. **Allcott/Gentzkow/Song follow-up** — the "flexibility through delay" extension paper (Allcott, Maxted & Meyer, forthcoming) is cited but unpublished; a published version with delay-calibration data would strengthen the commitment-tool design guidance.
4. **Kimi↔US-cloud talks close?** — first real price for a frontier open model's distribution meter; still open per the Sep 1 verification.
5. **Qwen3.8-Max successor licensing** — whether Alibaba's next release reverts to Apache-2.0 or doubles down on the metered custom license.
6. **DeepSeek revenue disclosures** — next reported revenue/valuation figure for the DeepSeek-costco-strategy skill's "disciplined pricing" thesis.

---

*Report generated 2026-09-01 (Pacific/Auckland) · A-Tech Research Division · Cycle 15 (run 1) · 3 skills created + 1 skill updated + 1 addendum → all verified on disk with YAML frontmatter and "Use when" triggers; 5 duplicate/incremental candidates correctly rejected with evidence.*