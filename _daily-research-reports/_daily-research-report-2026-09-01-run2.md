# A-Tech Daily Research Report — 2026-09-01 (Cycle 15, run 2)

**Scope:** Behavioral / digital-wellbeing economics + AI companion products · AI-assisted maintainability and developer experience · AI-driven behavior change and nudging longevity
**Method:** Two standing-domain web sweeps (late-August / September 2026 window) → hard dedup against the on-disk library (~570 existing skills across all nine category directories + all 100+ historical daily reports) → 3 new skills, no updates to existing skills (no incremental or watch-file targets surfaced in this scan) → README index updated → this report
**Deliverables:** 3 new SKILL.md folders (one with a `references/` watch file) + README cycle-15-run-2 index entries + this report

---

## 1. Scan & Research Summary

Four web sweeps covered neuromarketing, AI developer experience, open-source AI monetization, and behavioral psychology / nudging. Strongest actionable signals came from two CHI/SE research lineages (AI self-modeling longitudinal dynamics; AI code maintainability RCT) and from extrapolating the standing behavioral-economics literature (digital addiction, habit formation) onto the AI companion product category.

### What Was Checked and Deliberately Skipped

| Candidate | Reason for rejection |
|---|---|
| Tencent Hy4 preview (Aug 28 2026, open-weights 770B/49B-active MoE, recursive self-improvement claims) | Already covered — `recursive-self-improvement-provenance-honesty` + `open-commons-acquisition-neutrality-2026` in cycle 13 |
| NVIDIA Nemotron 3.5 Lightning open-weights release (Aug 11 2026, 30B/3B) | Already covered — `open-weight-agentic-model-wave-august-2026` captures Nemotron 3.5 Lightning + NeMo Switchyard routing + OpenMDW-1.1 license |
| Z.ai Ox Alpha → GLM-5.3-Flash stealth-to-open reveal (Aug 26 2026) | Already covered — `blind-launch-stealth-model-playbook` (cycle 13) is the dedicated skill |
| IBM Granite 4.2 real-sandbox agentic RL (Aug 25 2026, Apache 2.0) | Already covered — `ibm-granite-42-real-sandbox-agentic-rl` |
| DeepSeek V4-Flash-Vision MIT open-weights (Aug 31 2026) | Overlapping with `open-weight-agentic-model-wave-august-2026` licensing/serving content; adding a distinct skill for a vision model checkpoint in an already-saturated category yields no new mechanism — deferred to next-cycle re-evaluation if independent vision-agent benchmarks land |
| JetBrains "How Much Code Do Developers Really Let Agents Write?" (Aug 26 2026) | Already captured — `agentic-coder-segmentation-2026` (cycle 13) covers the three-segment clustering from the same survey |
| Vella & Blincoe longitudinal AI-coding-assistant study (`ai-review-fatigue-mitigation` skill exists) | Already in library |

### Fresh Signals (this cycle's 3 new skills)

| Signal | Source | Why it matters now |
|---|---|---|
| AI companion products have NO dedicated skill coverage yet | Extrapolation from Byrne et al. habit literature + Allcott/Gentzkow/Song digital addiction + INA safety trap + 2026 companion-bot regulatory bills | A-Tech currently has zero skills on the AI companion category despite it being the highest-growth conversational AI market segment and the most regulation-exposed. This skill seeds the lens before a client request lands. |
| Echoes of AI maintainability RCT results | Borg et al., Empirical Software Engineering, June 2026, N=151 RCT | The "AI code will be unmaintainable" fear just got a controlled test and came back null at file level — the actionable story is now *where the real risk lives* (cognitive debt, system-level bloat, architecture drift), which matters for every A-Tech client deciding whether to gate agent-written code |
| AI self-modeling nudge decay curve | He et al., CHI 2026, first 28-day longitudinal AI self-modeling study | The catalyst→habituation→internalization trajectory is the first empirically-characterized answer to "why do AI nudges fade" — directly usable for behavior-change product design and for explaining to clients why their retention effect died at week 2 |

---

## 2. Skills Created (3)

### 2.1 AI Companion Attachment Economics
**Category:** `behavioral-psychology-and-nudging/ai-companion-attachment-economics/`
**Evidence base:** Behavioral-economics habit formation (Byrne, Goette, Martin et al., "How Nudges Create Habits" — habit effects emerge immediately, persist only while treatment habitually continues, and decay slower the longer the treatment ran) crossed with digital addiction economics (Allcott/Gentzkow/Song: ~31% of social media use is self-control-driven; K.G.M. v. Meta verdict = litigation risk template for engagement-optimized design), the INA harmful-intention reinforcement trap (arXiv:2510.14513) as the companion failure mode, and the 2026 companion-bot regulatory bills wave.
**Skill value:** the attachment lens (identity-referent bonds, not brand loyalty, drive companion retention); the three-signal attachment-footprint instrumentation table (displacement / availability-seeking / mood-morphic use, measured openly as trust asset); the four-gate product build checklist (business-model → memory-continuity-with-user-ownership → safety-instrumentation → persona-spec-open); the three-monetization-posture ranking (engagement-farm ✗ / consent-based ✓ / capability ✓); the no-anthropomorphic-deception policy as the regulation-proof default.
**A-Tech fit:** seeds the only missing top-level product category in the library; four gates + footprint table + regulation watch file are directly usable for A-Coder persona/memory features, Be Practical curriculum modules, and hamishfromatech content ("The AI companion business model that won't get you sued").
**Files:** `SKILL.md`, `references/companion-products-regulation-watch.md`

### 2.2 Echoes of AI Maintainability RCT
**Category:** `developer-experience-and-flow/echoes-of-ai-maintainability-rct/`
**Evidence:** Borg, Hewett, Hagatulah, Couderc, Söderberg & Farley, *Empirical Software Engineering* 31:161 (June 2026, open access): preregistered two-phase RCT, N=151 (92% professional developers), Java Spring Boot feature. Phase 2 (the RCT): 75 new participants manually evolved solutions built with/without AI assistance — **no reliable downstream maintainability differences** on completion time or CodeScene Code-Health score. Phase 1 (observational): AI-assisted builds ran **30.7% median faster (56% for habitual AI users, P(Δ<0)>99%)** — the speed gain is real but front-loaded, not persisting into the hand-off. Bayesian posterior means for downstream effects cross zero in both completion time and code health.
**Skill value:** the "null result as product guidance" extraction — for every A-Tech client deciding whether to gate AI-written code differently, the RCT says: same review standards as human code; the real exposures are what the RCT did not test (multi-year architectural drift, cognitive debt, system-level code bloat, security); a recurring team drill ("can a teammate extend this without AI?") operationalizes the RCT's own Phase-2 design as a maintainability canary.
**A-Tech fit:** complements `agent-induced-complexity-debt` (velocity-vs-complexity DiD), `agentic-coding-returns-to-expertise` (skill-moderation), `comprehension-debt-framework`, and `code-health-mcp-integration` with the missing hand-off evidence layer; content angles ready ("Does AI code hold up 6 months later? An RCT says yes").
**Files:** `SKILL.md`

### 2.3 AI Self-Modeling Longitudinal Nudge
**Category:** `behavioral-psychology-and-nudging/ai-self-modeling-longitudinal-nudge/`
**Evidence:** He, Wang, Du, Ding, Shi & Wang (CHI 2026, open access): first 28-day longitudinal evaluation of AI self-modeling for behavior change. Study 1 (1-week, N=28, three-arm): **Video Self-Modeling (VSM)** — user's face swapped onto a peer model exercising correctly — produced clear performance gains, while Audio Self-Modeling (ElevenLabs voice cloning of self-talk) did not. Study 2 (4-week, VSM vs control, N=31): VSM sustained **higher absolute performance levels at week 4** (wall-sit Δ=31.34, p<.01; crunch Δ=27.98, p<.001) while the *improvement rate* converged with control by days 21-28 — catalyzing the **catalyst → habituation → internalization** three-stage trajectory: identity-referent nudges outlast novelty decay.
**Skill value:** the three-stage trajectory as a transferable mechanism (not a fitness-specific finding); the fidelity threshold insight (video crosses "recognizable as me" at attainable quality; audio cloning sits in the uncanny valley due to bone-conduction self-voice perception — offer user calibratable timbre, not perfect cloning); the five design rules (becoming-not-improving, modality-task fit, adaptive ideals, interactivity over demonstration, self-referencing not peer comparison); five honest-limitations flags (N=28/31, single-domain, 4-week horizon, compensation confound, unverified fidelity threshold).
**A-Tech fit:** directly usable for behavior-change, fitness, or education AI products; content angle "Why your AI nudge dies at week 2 — and which ones last"; product playbook for adding a future-self video feature to habit or learning apps.
**Files:** `SKILL.md`

---

## 3. A-Tech Values Alignment Matrix

| Skill | Open-Source AI | Data Privacy | Financial Freedom | Practical Implementation |
|---|---|---|---|---|
| AI Companion Attachment Economics | ☑ persona-spec open-sourcing; WildGuard-class guardrails replicable from open work | ☑ user-owned exportable memory; local-only tier as a paid feature not a dark pattern | ☑ consent-subscription over attention-farming; three-monetization-posture table | ☑ four product gates + three-signal footprint table + regulation watch file |
| Echoes of AI Maintainability RCT | ☑ preregistered open methodology; CodeHealth tooling public | ☐ honest N/A (no user-data angle — misaligned claim deliberately avoided) | ☑ redirects investment from speculative fear-based AI-quality tooling toward architecture and skill retention | ☑ policy checklist + hand-off drill + "same standards as human code" review verdict |
| AI Self-Modeling Longitudinal Nudge | ☑ open-source face-swap + peer-model pipeline replicable; ElevenLabs/VisoMaster tooling cited | ☑ uses user's own face/voice → consent-first, local-generation-first posture | ☑ cheap solo build path: open tools + TTS vs enterprise wellness platforms | ☑ five design rules + stage-timing table + honest-limitations list |

---

## 4. Cross-Skill Synthesis

The unifying thread this run: **behavioral science moving from one-shot effects to temporal dynamics.** Three independent threads all land on the same structural insight:

- **In companion AI:** bonds form *chronically*, so the economics is the habit loop, not the conversion — and the regulatory/ethical failure mode (harmful-intention reinforcement) is also chronic.
- **In code maintainability:** AI speedups are front-loaded and *do not persist* into the hand-off — the durable variable is human skill, not the tool.
- **In self-modeling nudges:** the catalyst effect fades at Day 7-15 but persists as internalized identity at Day 21-28 — interventions must be designed for the internalization stage, not the initial flash.

For A-Tech product strategy, this collapses into one design rule: **build for the steady-state, not the spike.** Companion monetization that assumes a one-time conversion will fail (the habit loop is the moat); AI code review policy that assumes AI-generated code is worse is investing against the steady-state evidence; behavior-change products that measure only week-1 lift will mis-evaluate whether their nudge actually internalizes.

## 5. Candidates Considered but Deferred

- **Echoes-of-AI "cognitive debt" companion analysis** — flagged inside the Echoes skill rather than a separate skill; comprehension debt already has a first-class skill in the library.
- **OSS/agentic PRs at scale (25,264 PRs study)** — already covered by `agentic-oss-economics-2026` (cycle 14b).
- **DeepSeek-V4-Flash-Vision open weights (Aug 31 2026, MIT)** — significant multimodal milestone but the licensing/serving angle duplicates `open-weight-agentic-model-wave-august-2026`; revisit if independent vision-agent benchmarks arrive or if the vision-model-as-agent-harness pattern matures.
- **NVIDIA "free model → GPU demand" strategic framing** — subsumed by the Nemotron portion of the open-weight wave skill; the "free model as GPU sales pitch" thesis is worth revisiting if Q4 NVIDIA earnings show a measurable Nemotron-attributable GPU bump.

## 6. Next-Cycle Watchlist

1. **Companion-bot regulation velocity** — US state bills and EU AI Act Article 50 enforcement actions; the regulation watch file in `ai-companion-attachment-economics/references/` should be re-checked if any state statute reaches governor signature.
2. **First independent replication of the Echoes of AI RCT** — the file-level null result will face replication attempts; a confirming study would harden A-Tech's "no special review gates" recommendation, while a contradicting one would require recalibration.
3. **AI self-modeling beyond fitness** — if a 2026-2027 paper extends the He et al. modality-fit finding to non-visual domains (endurance, language learning), the "VSM-beats-ASM" verdict becomes modality-fit contingent rather than absolute — worth a watch-file update.
4. **Harmful-intention reinforcement as a named failure mode** — if the INA/companion trap gets formal naming in a 2026-2027 system paper or regulatory filing, elevate it from "mandatory guard" to "regulatory expectation" in the companion skill.
5. **Kimi Cloud revshare and metered licensing** — the standing watch continues from run 1; no new signal this sweep.

---

*Report generated 2026-09-01 (Pacific/Auckland) · A-Tech Research Division · Cycle 15 (run 2) · 3 skills created → all verified on disk with YAML frontmatter, "Use when" triggers, and honest N/A markers where values alignment doesn't apply; 7 duplicate/incremental candidates correctly rejected with evidence; README index updated (cycle 15 run 2 header + run-2 date-section entries + run-1 demoted to Previous).*