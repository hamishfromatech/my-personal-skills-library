# Daily Research Report — 2026-07-27

**A-Tech Corporation — Daily Research Process**
**Generated:** 2026-07-27
**Research domains:** Neuro-marketing, behavioral psychology, AI revenue, privacy-first, developer experience, open-source business models

---

## 1. Research Phase — Sources Reviewed

| Domain | Primary Source | Secondary |
|---|---|---|
| Cognitive science / UX (AI explanations) | Saßmannshausen, Burggräf, Hassenzahl & Sauer — "Effects of AI explanations on trust and reliance: a study in job shop scheduling" (Ergonomics, March 2026, PMID 41811355) | PubMed abstract + plain-language summary; ResearchGate listing; harmful-explanation literature (Buçinca 2021, Bansal 2019) |
| Developer experience | Sonar — "2026 State of Code Developer Survey" (January 2026, N=1,100+) | TFiR coverage (Jan 9, 2026); The New Stack (Feb 20, 2026); SC World; LinkedIn (Milan Jovanovic) |
| Open-source business models / public funding | Domenech Burin — "Measuring the Invisible: Evaluating the Impact of Public Funding on Open Source Software" (arXiv:2607.05413, June 16, 2026, CC BY 4.0, Sovereign Tech Agency) | Sovereign Tech Agency site; GitHub blog on European Sovereign Tech Fund; arXiv full text |
| AI revenue / pricing | Revenera AI Pricing Strategy brief (already captured July 26); Lago 7 AI Pricing Models; Bessemer AI pricing playbook (incremental) | LinkedIn — 3 learnings shaping AI monetization 2026 |
| Privacy-first | MIT News FTTE article (already captured); Nature federated credit risk; Federated Learning for Privacy-Preserving AI (CACM) — all incremental | IntechOpen Federated Learning for Next-Gen AI |
| Behavioral psychology | Fisher & Oppenheimer nudge invisibility (already captured); PNAS nudge meta-analysis; Psychology Today disclosure meta-analysis — all incremental | Nudge theory Wikipedia (context) |
| Neuromarketing | SAGE AI-enhanced neuromarketing study (already captured); Polaris market research (incremental); Harvard 95% subconscious stat (incremental) | Marketing Agent Blog closed-loop cognition (already captured July 26) |

---

## 2. Synthesis Phase — Novel vs. Incremental

### Novel findings (no existing skill covers the core concept)

1. **AI explanation ability-cue trap** — The Saßmannshausen et al. (Ergonomics, March 2026) study is the first clean causal isolation of what brief AI explanations actually do to trust and reliance. The finding: brief rationales raise reliance via *perceived ability* (the AI seems competent), NOT via *understanding* — the explanation functions as an ability cue, not a comprehension-building mechanism. The existing `trust-calibration-ux-pattern` skill warns about unearned trust and vanity trust scores but doesn't explain the *mechanism* by which the most common XAI intervention (short rationale text) manufactures it. The four boundary conditions (task difficulty dilutes the ability cue but not reliance; domain expertise makes experts *more* susceptible; attitudinal vs. behavioral trust decouple; audience specificity is the fix) are not captured anywhere in the library. The harmful-explanation paradox (Buçinca, Bansal, fluency heuristic) connects to `deferred-trust-ai-selection` and `cognitive-fluency-trust-engine` but the specific "brief rationale = ability cue, not understanding cue" finding is novel. **→ NEW SKILL created.**

2. **Sovereign Tech Fund causal impact** — The Burin (arXiv:2607.05413, June 2026) study is the first quasi-experimental causal evaluation of public OSS funding. The finding — STF funding significantly increases project velocity (commits +143.8%, merged CRs +175.5%, new CRs +137.8%, new issues +249%) but has no significant effect on releases, contributors, or closed issues — is not captured anywhere. The GSCM + PSM methodology, the GQM + CHAOSS measurement chain, the portfolio logic (STF for velocity, Fellowship for contributors, Resilience for critical issues), and the policy implication (match metrics to program objectives; money alone doesn't resolve maintainer capacity) are all novel. The existing `open-source-sustainability-infrastructure` skill argues structurally for maintainer support; this study provides the causal evidence that money alone doesn't expand the contributor base. The existing `open-source-funding-platformization-2026` skill covers the funding surge but not the evaluation methodology. **→ NEW SKILL created.**

### Incremental updates (existing skill extended)

3. **DevEx verification bottleneck framework** — The Sonar 2026 State of Code Developer Survey (1,100+ developers, January 2026) independently corroborates the verification-bottleneck thesis from a larger independent sample. Key converging data: 72% daily AI use; 42% AI-committed code (66% expected by 2027); 96% don't fully trust AI code; only 48% always verify; >33% say reviewing AI code takes more effort than human-peer review; 24% toil swap; 4 tools/team; >33% shadow AI. The "toil swap" and "verification debt" (Werner Vogels) framings, the "vibe, then verify" workflow, and the deterministic-verification (not circular AI-checking-AI) mitigation direction reinforce and quantify the existing skill's thesis. The existing skill had the framework; the Sonar data adds the convergent empirical evidence. **→ UPDATED** with a corroboration section and cross-references to `ai-explanation-ability-cue-trap` and `botsitting-botshitting-cycle`.

### Already covered (no action needed)

4. **Privacy-first AI** — The MIT News FTTE article (Apr 29, 2026), the Nature federated credit risk paper, and the CACM federated learning article are all incremental. FTTE is already fully captured in `ftte-federated-tiny-training-engine`. The Nature and CACM articles reinforce existing federated-learning skills without novel frameworks. **→ No update needed.**

5. **Behavioral psychology** — The Fisher & Oppenheimer nudge-invisibility study, the PNAS nudge meta-analysis (Cohen's d = 0.43), and the Psychology Today disclosure meta-analysis are all already captured in `nudge-invisibility-metacognitive-miscalibration`, `nudge-disclosure-transparency-effectiveness`, and related skills. No novel framework surfaced today. **→ No update needed.**

6. **Neuromarketing** — The closed-loop cognition marketing skill (created July 26) covers the AI+neuromarketing pipeline. The SAGE AI-enhanced neuromarketing study is already captured in `ai-enhanced-neuromarketing-social-media`. Polaris and Harvard stats are incremental market-data reinforcements. **→ No update needed.**

7. **AI revenue / pricing** — The Revenera AI Pricing Strategy brief was captured July 26. Lago's 7 AI Pricing Models and Bessemer's AI pricing playbook reinforce existing pricing skills without novel frameworks. **→ No update needed.**

---

## 3. Skill Creation / Update Phase

### New Skills Created

| Skill | Category | Lines (SKILL.md) | Reference files |
|---|---|---|---|
| `ai-explanation-ability-cue-trap` | cognitive-science-and-ux | ~330 | `references/sassmannshausen-evidence-base.md` |
| `sovereign-tech-fund-causal-impact` | monetization-and-revenue | ~330 | `references/stf-causal-impact-evidence-base.md` |

All SKILL.md files include required YAML frontmatter (name + description with discovery triggers) and are under 500 lines. Detailed source material moved to `references/` subdirectories.

### Skills Updated

| Skill | Category | Change |
|---|---|---|
| `devex-verification-bottleneck-framework` | developer-experience-and-flow | Added "Corroborating Evidence — Sonar 2026 State of Code Developer Survey" section: 1,100+ developers; 72% daily AI use; 42% AI code (66% by 2027); 96% don't trust; 48% always verify; >33% more effort than human review; 24% toil swap; 4 tools/team; shadow AI; toil-swap and verification-debt framings; deterministic-verification mitigation. Added cross-references to `ai-explanation-ability-cue-trap` and `botsitting-botshitting-cycle`. |
| `devex-verification-bottleneck-framework/references/devex-verification-evidence-base.md` | developer-experience-and-flow | Added full Sonar 2026 State of Code extraction with all headline findings, the toil-swap and verification-debt framing, and implications for the framework. |

### Skills Reviewed (no change)

- `ftte-federated-tiny-training-engine` (privacy-and-trust) — MIT article adds no new content
- `nudge-invisibility-metacognitive-miscalibration`, `nudge-disclosure-transparency-effectiveness` (behavioral-psychology-and-nudging) — already cover the nudge literature
- `closed-loop-cognition-marketing`, `ai-enhanced-neuromarketing-social-media` (marketing-and-content) — already cover neuromarketing AI integration
- `open-source-sustainability-infrastructure`, `open-source-funding-platformization-2026` (monetization-and-revenue) — structural arguments; new STF skill adds the causal evidence layer (cross-referenced, not duplicated)
- `trust-calibration-ux-pattern` (cognitive-science-and-ux) — the ability-cue trap explains *why* the vanity-trust-score warning happens (cross-referenced from the new skill, not duplicated)

---

## 4. Key Insights for A-Tech

### Cognitive science → the ability-cue trap
- The most common XAI intervention (brief rationale text) may be the one most likely to manufacture unearned trust — it is fluent enough to signal ability but too shallow to build understanding.
- Domain experts are *more* susceptible, not less — their fluency in the domain makes the rationale feel plausible, which itself signals competence (the fluency heuristic). This connects to the `deferred-trust-ai-selection` finding.
- A-Coder application: match explanation depth to task stakes (brief for formatting, detailed+uncertainty for refactoring, detailed+verification-prompt for security). Add "fluency is not verification" framing for senior developers. Track behavioral reliance separately from attitudinal trust.
- The fix is audience- and task-specific explanation depth with uncertainty disclosure — not universal rationales.

### Developer experience → verification bottleneck corroborated
- The Sonar 2026 survey (1,100+ developers) independently corroborates the verification-bottleneck thesis from a larger sample than the original synthesis. The 96%-don't-trust / 48%-always-verify gap is the quantitative measure of verification debt.
- The "toil swap" (24% of the week still on routine tasks despite AI) is the time-saved-vs-time-lost metric made concrete.
- The deterministic-verification direction (static analysis, hard-coded rules, not circular AI-checking-AI) is the mitigation. A-Coder's cognitive guardrails (line-level attribution, semantic diffs, adversarial test layers) are the product moat.
- This makes the verification bottleneck the most convergently evidenced DevEx finding of 2026 (two independent sources: the DX synthesis and the Sonar survey).

### Open-source business models → causal funding evidence
- The STF study provides the first causal evidence that public OSS funding works: +138-175% velocity increase. This is the evidence to take to treasuries and budget committees — not ideology, but quasi-experimental causal estimates.
- The nuance: funding mobilizes existing contributors but doesn't expand the contributor base or resolve backlog. This validates A-Tech's structural maintainer-support thesis (money alone doesn't resolve maintainer capacity).
- The portfolio logic (STF for velocity, Fellowship for contributors, Resilience for critical issues) is a funding-design template for Builder's Club community programs.
- The GQM + CHAOSS measurement chain is an open methodology any funder can adopt — and the evaluation code is open-source (github.com/ldmnch/thesis_mds_2026).
- The AI-noise caveat (AI-generated issues inflate the new-issues count) connects to the `open-source-maintainer-ai-burden` skill.

### Privacy-first → already covered
- FTTE and federated-learning articles are incremental. No new privacy-first skill needed today.

### Behavioral psychology → already covered
- Nudge literature already captured. No new behavioral-psychology skill needed today.

### Neuromarketing → already covered
- Closed-loop cognition and AI-enhanced neuromarketing already captured July 26. No new marketing skill needed today.

---

## 5. Index Update

`/home/user/.skills/README.md` updated with:
- 2 new skills (AI Explanation Ability-Cue Trap, Sovereign Tech Fund Causal Impact)
- 1 updated skill (DevEx Verification Bottleneck Framework — Sonar corroboration)
- Updated A-Tech Values Alignment Summary table (3 rows)
- 10 new key research sources (779-788)
- Updated header date (2026-07-27)

---

## 6. Next-Day Notes

- The Saßmannshausen full text (via Taylor & Francis / Atypon) was paywalled; the PubMed abstract + plain-language summary provided the core findings. Fetching the full text could add the exact mediation analysis coefficients and the full boundary-condition statistics.
- The Sonar 2026 State of Code full PDF returned a rapidocr error (likely a scanned/image PDF); the TFiR and The New Stack coverage provided comprehensive findings. The full report may contain additional data on governance, technical-debt breakdown, and regional differences.
- The arXiv:2607.05413 STF study is fully captured (open-access, CC BY 4.0). The study's GitHub repo (github.com/ldmnch/thesis_mds_2026) contains the replication code — could be referenced from the Builder's Club open-source evaluation toolkit.
- Monitor for the Buçinca et al. (2021) cognitive-forcing-functions paper full text — it's the key supporting reference for the ability-cue trap's harmful-explanation paradox and could deepen the `scaffolded-cognitive-friction` skill.
- The Nature HSS GAP framework article was blocked again (303 redirect); the existing `gap-behavioral-science-framework` skill covers it. Retry tomorrow or find an open-access mirror.
- Watch for the Sovereign Tech Agency's Fellowship Evaluation (Wagner, 2025) and Pilot Round Evaluation (SPRIN-D, 2023) reports — these are the qualitative complements to the quantitative STF causal study and could expand the portfolio-logic evidence.