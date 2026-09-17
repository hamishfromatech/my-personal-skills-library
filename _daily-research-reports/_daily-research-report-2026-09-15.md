# A-Tech Daily Research Report — 2026-09-15 (Cycle 27, Run 1)

**Scope:** Open-source AI monetization & business models · Agentic payments & adoption metrics · Behavioral psychology & nudging · Neuromarketing & consumer neuroscience · Privacy-first AI · Developer experience · Community & growth · EU regulation
**Method:** Eleven web sweeps across the standing domains (Sept 2–15, 2026 window) → targeted grep dedup against the on-disk library (10 candidate queries) → 5 new standalone SKILL.md folders across 4 category directories + 1 in-place refresh → this report → README cycle-27 run-1 index update
**Deliverables:** 5 new SKILL.md folders + 1 refresh + README cycle-27 index + this report
**Coordination note:** library state surveyed before writes; cycle-26 run-1 (pgBackRest crisis, Precision Proactivity, topology SA attack, TRM agentic share, K2 self-audit, Ray-Ban bystander gap) was fully on disk before this run's creation. All five selections passed targeted greps against the live library before creation; each new skill names its nearest neighbors in Pairs-with.

---

## 1. Research Summary

The Sept 2–15 window is dominated by two cross-cutting facts: the first peer-reviewable population-scale audit of an agentic-payment adoption metric, and an open-core monetization proof at infrastructure scale. Four of five new skills are mechanism-level firsts; the refresh converts a capital datapoint into a margin-quality thesis. The clusters:

1. **The open-source reliability layer got its monetization proof.** Temporal's $550M Series E at $12.55B (Sept 14, 2026) — valuation doubled in seven months — with company-reported ARR >$250M, NDR >200% since February, 4,300+ paying customers (+139% YoY), 1.9T August billable actions (+368% YoY), 43.1M open-source installs, and OpenAI's usage growing 60× in under a year. The mechanism is the cleanest open-core split in the library: MIT server free (compute), managed cloud paid (trust — reliability, security, enterprise controls). Durable execution was built for a decade before AI; AI raised the stakes (agents running for days/weeks/months), not the problem.
2. **x402's headline metric is a Goodhart metric.** Ling, Zhou, Wu & Wang (arXiv:2607.12575, POMACS 2026) deliver the first population-scale measurement of x402 adoption *and authenticity*: over 280 days on Base, 136.7M settlements worth $44.1M decompose into 21.2% provably fictitious (a self-payment fleet that settled 27.5M times to itself; four fully-closed clusters sweeping $779K back to funders to the dollar), 63.8% operator-internal (funding-linked clusters: a funder seeds 337 wallets, 31.5M settlements into its own hub, 98.34% of hub inflow returns), and a genuine-economy bound of $187,861 (demonstrably reaches a nameable catalog service) to $20.26M (45.9%, not provably manufactured). The entire count is reproducible for ~$355,583 in facilitator-sponsored gas. The daily series' largest movements are operator campaigns switching on/off; the x402 V2 release moved the series not at all; the Jan 1, 2026 per-settlement fee caused the largest sustained fall. This is the measurement-grade successor to the library's attribution-bound work: TRM's funnel bounds *agency*; this paper audits the *metric itself*.
3. **AI-ad aversion is exposure-mediated, not dispositional.** Shitova, Kabalska, Varga & Wagner (Psychology & Marketing, "Minds Versus Codes"): EEG + eye-tracking + qualitative triangulation shows AI-generated video ads matching or outperforming human-made ads on emotional response (surprise, fascination, empathy — not the predicted unnaturalness disbelief), and consumers who *doubted* AI's emotional authenticity at pre-test changed attitudes after real exposure. The complement to the held Bellman equivalence skill: that one holds outcome equivalence; this holds the attitude-inversion mechanism.
4. **Europe legislated self-evolving machines.** The Machinery Regulation (replaces the Machinery Directive Jan 20, 2027) is the first regulatory category written around machine learning: AI-based safety functions and machinery with self-evolving behaviour now require notified-body conformity assessment instead of manufacturer self-declaration. Nobody has assessed one yet — the regime does not start for sixteen months. Skild AI ($100M run-rate, S1 model learning tasks from a single video, 66% on tasks absent from training data) sits at the definitional center and has not said whether it sells into Europe. The compliance-safe design response — constrained post-deployment behavior envelopes, separated deterministic safety layers — turns federated/on-device update controls into a compliance surface.
5. **Agentic capability is a switching event, not a feature.** Nylas' 2026 State of Agentic AI (1,026 builders/product leaders): 94% would possibly/very likely switch vendors for stronger agentic functionality; 85% expect table stakes within three years (a third within 12 months); 67% building custom internal agentic workflows today; only 4% allow agents to act without human approval; switching criteria are reliability + integration coverage + security, not demo quality. The retention-winning design is the graduated-trust ladder.
6. **The Zhipu margin inversion (refresh).** ARR $1.6B monthly-annualized (>$2B weekly-annualized), the open-weight sector's revenue leader at ~2.5× MiniMax — but total gross margin fell 50.0% → 26.4% even as average API pricing rose ~101%, because the revenue structure inverted to 86.5% API. Pricing power is not converting to margin (R&D at 2.2× revenue). The tracker's implication: every lab row now needs a margin-quality tag — a $2B ARR at 26% gross margin is a different business from the same ARR at 60%.

### Fresh Signals (5 new skills + 1 refresh)

| # | Skill | Category | Source | Why it matters now |
|---|---|---|---|---|
| 1 | Temporal Durable Execution Open-Source | monetization-and-revenue | Temporal Series E, Sept 14, 2026 | The RELIABILITY-PRIMACY FLYWHEEL: free compute (MIT core, 43M installs), paid trust (managed cloud, NDR >200%) as the open-core wedge for the agent era |
| 2 | x402 Population-Scale Authenticity Measurement | ai-agents-and-workflows | arXiv:2607.12575 (POMACS 2026) | The GOODHART-METRIC-AUDIT: 136.7M settlements decompose into 21.2% provably fictitious + 63.8% internal; genuine demand bounded $188K–$20.3M; count reproducible for ~$356K |
| 3 | Minds-versus-Codes Aversion Inversion | marketing-and-content | Psychology & Marketing, 2026 | The AVERSION-INVERSION: pre-test AI skeptics flip after exposure; AI video ads match/outperform on emotional response |
| 4 | EU Machinery Regulation Self-Evolving AI | monetization-and-revenue | EU Reg 2023/1230, effective Jan 20, 2027 | The SELF-EVOLVING-BEHAVIOUR REGIME: notified-body assessment for learning-after-deployment machines; Skild-class vendors at the definitional center |
| 5 | Agentic Vendor Switching Event | community-and-growth | Nylas State of Agentic AI 2026 | The SWITCHING-EVENT: 94% vendor-switch intent on agentic capability; 4% full autonomy; table stakes in ≤3 years |
| — | China Open-Source LLM ARR Tracker (refresh) | monetization-and-revenue | 36Kr Zhipu H1 print, Sept 2 | Margin-quality tag added: $1.6B–$2B ARR at 26.4% gross margin; pricing +101% did not move cloud margin |

---

## 2. Deduplication & Novelty Assessment

All five selections passed targeted greps against the live library:

- **Temporal / durable execution** — grep `Temporal|durable execution|Durable Execution` (both glob variants): zero relevant matches (only unrelated "temporal dynamics" neuroscience strings). The library holds the demand-side survey (`state-of-development-2026-agent-maturity`) but not the vendor's monetization mechanism. **Novel.**
- **x402 population-scale authenticity** — grep `Goodhart|operator-internal|self-payment|lnpay|fictitious`: zero matches outside the TRM funnel skill's "self-payment" screen mention (a screen in TRM's three-filter funnel, not this paper's graph-tier methodology or its $356K manufacture-price economics). **Novel** — complementary to, not duplicative of, `x402-agentic-share-measurement`.
- **Minds-versus-Codes aversion inversion** — grep `Minds versus Codes|AI-generated ad|generative advertisement|pre-test algorithm aversion`: zero relevant matches; `ai-generated-ad-pretesting-effectiveness` holds the Bellman equivalence study (different mechanism: outcome equivalence vs attitude inversion through exposure). **Novel as a mechanism skill.**
- **EU Machinery Regulation** — grep `Machinery Regulation|self-evolving behaviour|notified body`: zero matches. The library holds AI Act and CRA skills; this is a distinct regime. **Novel.**
- **Nylas switching event** — grep `Nylas|switching event|vendor switching`: zero matches. **Novel.**
- **Zhipu margin print** — grep `gross margin|margin leverage|Zhipu` (monetization dir): zero matches. The tracker refresh adds the margin-quality lane. **Refresh, not new skill** (consistent with the tracker's standing pattern of in-place addenda).

### Already tracked (no action)
- Z.AI $5B HK raise, Moonshot $2B ARR target / $50B IPO filing, Mistral €3B Series D (held by the ARR tracker + capital-stack lane; no new mechanism)
- Zhipu's SOTA-retention question and the post-training-vs-pretraining debate (capital/strategy context inside the tracker; the margin inversion is the only new mechanism)
- Anthropic 2026 State of AI Agents report, State of AI 2026, State of Agentic Coders, Anthropic Agentic Coding Trends, Sonar State of Code (all held by the adoption/verification/trust families; run-2 style re-confirms held findings — no new mechanism)
- Temporal's State of Development 2026 survey (held by `state-of-development-2026-agent-maturity` — this run's Temporal skill is the vendor-side monetization complement)
- Skild AI's $100M run-rate as a robotics-business datapoint (capital-market print; the Machinery Regulation is the mechanism, captured in skill #4)
- Chainalysis x402 100M-transactions preview and Coinbase/Linux-Foundation x402 Foundation launch (April 2026) — held by the payment-protocol family
- Prescription-error non-mandatory information intervention (arXiv:2609.09673; 2.81M prescriptions, 8.6% DDI-error reduction, ~$4.8M annualized savings) — a healthcare-domain case of the library's non-mandatory-information pattern, below the standalone bar (previously flagged in cycle-25 run-2)
- DP-FL family papers: XCal-FL (held cycle-21), α-split Component-Aware DP (held cycle-24), FGLGuard (held cycle-22), heterogeneous multi-LLM federated inference (held), split-LLM arc (held)
- Privacy-Preserving Split Learning for Federated LLM Fine-Tuning (arXiv:2609.09794) — extends the held Split-LLM arc with a learned obfuscate-and-recover scheme for split-based federated LLM fine-tuning; same family, no new pattern class
- Decentralized-FL multi-layer defense (Sivan et al., Discover AI, Sept 4, 2026) — pipeline integration (FLARE + STRIP + TeCo + SentiNet + trimmed mean + public-pretrain/DP-finetune) of held defense components, below the mechanism-first bar

### Below-bar candidates (flagged, not created)
- **Skild S1 out-of-distribution benchmark claims** (96% seen / 66% unseen): promote if independent evaluation or a notified-body pre-assessment lands
- **EEG-driven verbalizer/visualizer classifier** (Panteli et al., Information 16:757, N=22): below the cycle-16 small-N bar; promote on replication with N≥40 per the authors' own power guidance
- **Frontiers eco-label multimodal study** (Parasuram et al., N=13 valid): below small-N bar; the valence-continuum framing is a candidate for the neuromarketing landscape map's citation layer, not a standalone skill
- **Prescription-error learning mechanism** (reactive correction → proactive learning over time): promote if a software-engineering analogue of the non-mandatory alerting pattern lands

---

## 3. Library State After This Run

- **New skills:** 5 (monetization-and-revenue ×2, ai-agents-and-workflows ×1, marketing-and-content ×1, community-and-growth ×1)
- **Refreshes:** 1 (china-open-source-llm-arr-tracker — cycle-27 addendum: Zhipu margin inversion + margin-quality tag)
- **Index:** README updated to 2026-09-15, cycle 27 run 1
- **Watch triggers carried forward:** Bolt Forge consent-corpus survival past Oct 14; EU CRA Article 14 enforcement actions against agentic stacks (live since Sept 11, 2026); Machinery Regulation notified-body guidance publications (first assessments expected mid-2027); x402 V2 access-rights convergence; DP-DyLoRA promotion trigger; no-threshold sensitive-data trigger (fourth US state or federal movement); whether Moonshot's hosting-toll negotiations close

*Report compiled by A-Tech Research Division — 2026-09-15, Pacific/Auckland.*