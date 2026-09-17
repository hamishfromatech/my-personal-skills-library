# A-Tech Daily Research Report — 2026-09-05 (Cycle 18, Run 3)

**Scope:** Neuromarketing/consumer neuroscience · Behavioral psychology & nudging · AI revenue & open-source business models · Privacy-first AI · Developer experience & AI coding · Community & growth · Financial freedom & wealth · Agent payments (watch)
**Method:** Eight web sweeps across the standing domains (late-Aug–Sept 2026 window) → hard grep dedup against the on-disk library (~660 skills incl. cycle-18 runs 1–2 additions) → 6 new skills + 2 update patches across 5 categories → README cycle-18 index updated → this report
**Deliverables:** 6 new SKILL.md folders + 2 reference-file updates + README cycle-18-run-3 index entries + this report

---

## 1. Research Summary

Third run of cycle 18 (2026-09-05). The research window captured the Aug 16–Sept 4 wave. The dominant thread: **closed-API dependence was formally weaponized** (OpenAI terminating Cursor's frontier-model supply on a change-of-control trigger, mirroring Anthropic–Windsurf 2025), **neutral AI infrastructure consolidated** (Stripe–OpenRouter signed; NVIDIA–HF announced Sept 3), **privacy moved to hardware** (PlugClaw consumer TEE device GA'd), and **OSS funding grew a fourth structural leg** (Omacom patron model; CodeRabbit's $10M in-kind commitment). Two behavioral field experiments (NBER AI-tax-agent RCT; JEBO cost-salience study) supplied genuinely new mechanisms. All six selections passed grep dedup; three candidate skips are logged with reasons below.

### Fresh Signals (6 new skills)

| # | Skill | Category | Source | Why it matters now |
|---|---|---|---|---|
| 1 | OpenAI–Cursor Severance Case 2026 | monetization-and-revenue | OpenAI formally terminates frontier-model supply to Cursor (Nov 12 2026 cutoff; change-of-control trigger via SpaceX's $60B Anysphere acquisition); Anthropic–Windsurf 2025 precedent (TechTalks, Sept 4 2026) | First *pattern* skill for the API-dependency-severing playbook: closed labs act as competitors of their best customers whenever the customer becomes an acquisition target; BYOK breaks four ways (features, billing, ZDR compliance, rate limits); Cursor survived because its two-pool architecture held exposure at 5% of traffic — "own the weights to own your destiny" is now an empirically grounded survival rule, not ideology |
| 2 | PlugClaw TEE Consumer Agent Hardware | privacy-and-trust | TrustKernel PlugClaw GA (Sept 4 2026; $149 one-time USB-C stick; PlugOS + Ubuntu; OpenClaw native; cloud frontier calls in hardware TEE with remote attestation; free confidential inference) | The third tier of the privacy-isolation stack: hardware-isolated agent compute at consumer price — the agent *structurally cannot* touch the host's data (blast radius = the stick), completing classifier-gate (PII-Tracer, probabilistic) → local-first (DGX Spark) → hardware-isolation; preview users chose confidential models over cheaper non-confidential ones — early consumer WTP for a structural privacy boundary |
| 3 | Abliteration Hosted Guardrail Removal | marketing-and-content | Abliteration.ai (TechCrunch, Sept 4 2026): hosted web/API access to refusal-stripped GLM-5.3; customers = red-team startups serving banks/airlines/critical infrastructure + cloud-provider deals; VCs courted; KYC = credit card only | The open-weights safety fight formally relocates from release to distribution: a market exists for refusal-free frontier-class capability with browser-level access friction; defenders need to model this adversary tier; open-weight publishers must now decide distribution controls (hosting terms, KYC, monitoring) at release time, not after a hosted-removal service appears |
| 4 | AI Assistance Distributional Inversion | behavioral-psychology | Holz, Perez-Truglia, Simon & Zentner, NBER WP 35632 (Aug 2026): 645 Dallas County households, preregistered RCT, Claude chatbot on a property-tax-appeal site | The first field-RCT inversion of the AI-productivity literature: chatbot raised direct appeal filing 9.1pp (41.4→50.5%) with 78% take-up and no crowd-out of human agents — yet effects were smaller for less-advantaged households (BA +25.0pp vs no-BA +5.8pp, p≈0.07), with take-up gaps much smaller than outcome gaps; universal access to conversational judgment-support can *widen* disparities; design rule: audit the outcome distribution and the take-up-vs-action gap, not the average |
| 5 | Cost-Salience Variety Margin | behavioral-psychology | Hong, Riyanto, Yan & Huo, JEBO 248 (2026), DOI 10.1016/j.jebo.2026.107634: university stationery ordering, cost visibility randomized in the ordering interface | The margin rule for third-party-cost salience: volume (frequency, quantity) unchanged; item VARIETY −27% → 27% consumption cut; people internalize the organization's cost on the add-a-new-item decision, not the reorder — the placement rule for cloud/token/SaaS spend transparency (put the meter on the add-item step, not the invoice) |
| 6 | Omacom Patron Funding Model | community-and-growth | Omacom Foundation (DHH), Aug 21–31 2026: $8M → $12.6M across 14 patrons in 10 days (8 CEOs at $1M; Houston, Steinberger; 1Password + 37signals corporate tiers) | The fourth leg of the OSS-funding quad (endowment, co-op, employment, **patron**): founder-controlled, multi-year, deployed *upstream* — a 3-year exclusive Hyprland sponsorship that **buys out and discontinues the Hyprperks paywall** (gated content released free) and a Quickshell premier sponsorship; patronage as paywall-removal mechanism; key-person dependency flagged honestly |

### Updates (2)

| Skill | What changed |
|---|---|
| `openrouter-inference-routing-economics` | **Stripe–OpenRouter acquisition moved from "pending" to signed/agreed & announced (Aug 19 2026)**. Reported $7.5–8B (NYT $7.5B with ~$1.5B founders / ~$6B investors; Axios >$8B mostly stock; Bloomberg >$7B signed Aug 16) — ~6× the May 2026 $1.3B Series B valuation in ~6 months. Scale at deal: 400+ models / 80+ providers / >10T tokens/day / >10M developers. Neutrality is now a post-close verification item, not an assumption — the second neutral intermediary absorbed in three weeks (after NVIDIA–HF). Update recorded as `references/stripe-acquisition-update-2026-09.md` pending merge. |
| OSS-funding family (`oss-endowment-and-coop-funding-2026`) | CodeRabbit's **>$10M of actual direct cost** committed to open source over 12 months (Aug 26 2026; Series C) — cash sponsorships + free agentic Review/Triage/Change Stack/Security for all public repos, counted at cost not list-price; precedes $1.2M delivered on the $1M Series B pledge + ~$5M absorbed. Adds the **in-kind compute leg** to the funding-structure table. Recorded as `references/coderabbit-10m-agentic-support-addendum.md`. |

---

## 2. Dedup Decisions (checked and deliberately skipped)

| Candidate | Reason for rejection |
|---|---|
| **NVIDIA–HF acquisition re-coverage** (TechFundingNews "signed Sept 2 / Delangue 'We signed yesterday'"; Rediff; Huang blog) | Already comprehensively covered as cycle-18 run-1 UPDATE to `open-commons-acquisition-neutrality-2026` ($12.93B, H1-2027 close, neutrality commitments, OpenAI-penetration + llama.cpp context). The new European-ownership angle (French-founded pillar under US ownership; Mistral/Black Forest Labs investment questions) is a minor enrichment of the existing skill — fold on next touch, no standalone skill |
| **Stripe–OpenRouter as a new skill** | Not new — the acquisition was already known and tracked as "pending $7B+" in `openrouter-inference-routing-economics` (cycle 13/16 context). This run's value is the *signed/agreed + announced + price-range refinement*, recorded as an update patch, not a fork |
| **NVIDIA PAIR / Perplexity Hybrid Compute / Lily** | Cycle-18 run 1 (this week) — `home-ai-network-pair`, `hybrid-compute-privacy-gate` cover both; byteiota/AIWeekly/Superpower Daily re-crawls add no new mechanism |
| **Ollama CEO interview (150× token growth; AT&T 40% to open models)** | Strong enterprise-adoption datapoint but the mechanism (router+frontier architecture, open-weight cost collapse, missing safety tooling for open weights) is already held by `open-weight-adoption-milestone-2026` + `open-source-ai-hosting-economics` + `mozilla-open-source-ai-state-2026`; the 150× number is a candidate addendum for the milestone skill, below the new-mechanism bar this run |
| **Ollama security caveat (open models lack bundled safety stack; "solvable, not solved")** | Same family as Abliteration skill #3 — the distribution/governance gap; covered structurally there |
| **"Corporate America fell hard for open source" (Pachitanglang position essay)** | Position-essay restatement of the library's open-weights-in-enterprise theses (customization, TCO, control); no new mechanism |
| **Open Source AI founder guide (mean.ceo, Sept 3)** | Founder-practicality checklist content; the library's founder plays + license/procurement skills already hold the substance; no new framework |
| **Robinhood Agentic Trading** (MCP server + agentic account; walled budget; read-all-accounts caveat; agentic credit card 3% cashback) | Notable but a single-vendor product walkthrough with no novel mechanism beyond the library's payment/permission skills (walled-budget = the `plan-limit`/metering family; read-broad-than-trade-scope = `mcp-dual-identity-problem` mechanism); watch for the pattern spreading to other brokers before a structural skill is warranted |
| **LessWrong "agents asked to make money" simulation study** (agents fabricate identities; both agents conclude they're in a simulation; safety-post-training pulls toward simulation-belief) | Fascinating single-blog experiment (N=2 exploratory runs); the agent-safety mechanics are adjacent to existing `recursive-self-improvement-provenance-honesty` and `agentic-trust-security-protocols-2026`; below the bar this run, watch for replication |
| **No-code AI agent agency revenue guide (Wealth From AI)** | Solo-operator service-business template; the library's monetization stack covers the pattern; vendor-blog numbers unverifiable |
| **ChatGPT Rs-4,000-7-days first-person experiment (14.5% of target)** | Anecdote confirming the library's "AI plans ≠ executed revenue" stance; no mechanism |
| **AI agent passive-income listicles (InstaClaw, DEV, Coursiv 50-ideas)** | Content-marketing aggregation of patterns the library already holds (content engines, automation consulting, micro-SaaS); no new mechanism, unverifiable case numbers |
| **Anthropic Claude Commerce Agents open-source blueprint (Sept 2)** | Noted: open-source retail blueprint + OpenAI's Instant-Checkout retreat (Walmart conversion ~⅓) — the agent-commerce mechanism is already held by `x402-production-checklist` + `agentic-commerce-2026` + `agent-economy-payment-protocols`; the retailer-conversion datapoint is a candidate addendum; below the bar this run |
| **State of AI 2026 / JetBrains / Sonar / Temporal re-crawls** | All captured in prior cycles (`ai-bubble-developer-sentiment-2026`, `agentic-adoption-trends-sept-2026`, `agentic-coder-segmentation-2026`, `sonar-state-of-code-2026`, `state-of-development-2026-agent-maturity`) — grep-confirmed |
| **RIETI Yokohama / stem-cell / influenza / reminder-WTP / LLM-iterative-nudge re-encounters** | All already covered (cycle-17/18 runs 1–2); the NBER tax paper is NOT a duplicate — different study, different mechanism (AI-agent distributional effects vs nudge complementarity) |
| **Neuromarketing singles: Nagpal S-O-R impulsivity PLS-SEM (N=609), Topcugil & Hiziroglu AI-CXM conceptual framework (Aug 14 2026), NRFHH EEG capsule-network ad recommender (Aug 27), JTAR FMCG review, IULM perspective-taking re-crawl, LJMU supermarket re-crawl, Nehme EEG+ET purchase-intent (N=210), PLOS One physiological-signals paper (Sept 4)** | Per the cycle-16 calibration rule (PRISMA meta-analysis bar, `neurophysiological-consumer-meta-analysis` d=0.47 I²=77–84%): single studies and conceptual frameworks below the bar are skipped; the CXM framework duplicates `neuromarketing-ai-cxm-integration-framework`; survey/SEM family covered by `neuromarketing-sor-trait-moderation-model` |
| **Alibaba profit-sharing pivot (Qwen3.8-Max commercial agreements, TechShots Sept 4)** | License-axis development already tracked by `license-axis-business-type` + `metered-open-license-revenue-share-2026` + `kimi-cloud-revshare-watch`; the "end of the free lunch" framing is a restatement; fold the confirmation into those skills on next touch |
| **Thomson Reuters Thomson-1 / Harvey Tenet re-coverage** | Already tracked (Fortune open-source enterprise piece; kimi skills); no new mechanism |
| **Abliteration.ai considered for skip?** | NOT skipped — zero library coverage (grep-confirmed); the release→distribution relocation is a genuinely new governance mechanism; placed in marketing-and-content (distribution/governance) per the cycle-16 calibration rule rather than as a single-study neuroscience finding |
| **PlugClaw considered for skip?** | NOT skipped — zero coverage of consumer TEE agent hardware; completes the privacy-isolation tier ladder (classifier/local/hardware) the library has been building since cycle 14 |
| **Omacom considered for skip?** | NOT skipped — zero coverage; a fourth funding-structure leg is a structural completion, matching the Rust-MiR precedent in run 2 |
| **Cost-salience study considered for skip?** | NOT skipped — the intensive-vs-extensive-margin split is a new design mechanism (the library's cost skills all target individual metering or architecture, none identifies *where* salience binds) |
| **NBER tax study considered for skip?** | NOT skipped — the distributional inversion is the first field-RCT counterpoint to the AI-productivity-skills-moderation family; genuinely new mechanism |

---

## 3. Cross-Cycle Notes

- **Category distribution this run:** 2× behavioral-psychology-and-nudging, 1× community-and-growth, 1× marketing-and-content, 1× monetization-and-revenue, 1× privacy-and-trust (+2 updates in monetization and community). Untouched: cognitive-science-and-ux, developer-experience-and-flow, financial-freedom-and-wealth, ai-agents-and-workflows — no new mechanism cleared the bar (DevEx survey wave fully harvested; the week's DevEx signal went into the Cursor-severance skill's economics instead).
- **Thematic thread:** the run is dominated by **ownership and boundaries** — who owns the model layer (Cursor), the agent layer (PlugClaw), the router (Stripe–OpenRouter), the commons (NVIDIA–HF), the refusal boundary (Abliteration), and who funds the people (Omacom, CodeRabbit). The two behavioral skills are both about *where an intervention binds* — the NBER study on *who* benefits (distributional), the JEBO study on *which decision step* salience acts on (marginal). Same lesson as cycle 18 run 1: effects live at boundaries, not inside components.
- **The dependency-severance pattern is now a named structure:** Windsurf (2025) → Cursor (2026) with an explicit change-of-control trigger and a 3-month notice window. The library's vendor-risk guidance upgrades from "multi-provider failover is prudent" to "closed-API dependence is a termination-triggered liability with a documented playbook."
- **The OSS-funding quad is complete:** endowments + co-ops (cycle 17) → employment (Rust MiR, run 2) → patronage (Omacom) + in-kind compute (CodeRabbit, this run). Each leg has a distinct control model and failure mode — the funding-structure decision table in `oss-endowment-and-coop-funding-2026` should be extended with all four on next touch.
- **Privacy isolation stack is now three-tier complete:** classifier gate (PII-Tracer, probabilistic) → local-first escalation (DGX Spark) → hardware isolation (PlugClaw, structural). A-Tech can now give a graduated recommendation: what boundary strength does the workload's data justify?
- **Watch items for next cycle:** (1) OpenAI–Cursor actual cutoff behavior on Nov 12 2026 and whether other labs issue similar notices; (2) Stripe–OpenRouter close + first post-close neutrality signals; (3) NVIDIA–HF H1-2027 regulatory path; (4) Omacom disbursements (Hyprland Oct 10 start; any SPI/Arch upstream funding); (5) CodeRabbit tracker cash-vs-in-kind split; (6) whether PlugClaw-style hardware isolation appears in mainstream agent platforms; (7) Abliteration VC round outcome and any cloud-provider policy response; (8) NBER follow-ups on AI-assistance distributional effects in other domains (benefits, legal, investing).

---

## 4. A-Tech Value Alignment Summary

| Skill | Open source | Privacy | Financial freedom | Practical |
|---|---|---|---|---|
| OpenAI–Cursor Severance Case 2026 | the empirical business case for open weights as survival architecture | ZDR breaks under BYOK — privacy lives in the pooled layer | owned weights = owning the meter; closed API = rent with eviction risk | 5%-test + four-BYOK-breakdown table |
| PlugClaw TEE Consumer Agent Hardware | OpenClaw-native; BYOK model choice | structural blast-radius isolation at $149 — the hardware tier | one-time purchase vs subscription tiers; free confidential inference (launch-subsidized) | three-tier isolation decision table |
| Abliteration Hosted Guardrail Removal | the honest second-order caveat on open weights | access-governance discipline parallels data governance | a real (if risky) market exists — covered analytically, not as playbook | release→distribution governance checklist |
| AI Assistance Distributional Inversion | preregistered, replicable RCT design | transcript sensitivity flagged | the equity caveat for "AI for financial freedom" content | take-up vs outcome-gap audit table |
| Cost-Salience Variety Margin | replicable interface experiment | honest N/A flag | make the team meter visible at variety, not volume | one placement rule + measurable outcome |
| Omacom Patron Funding Model | fourth funding leg; paywall-removal pattern | honest N/A flag | unconditional maintainer income; Hyprperks buyout as conversion | four-model funding table + upstream playbook |
| OpenRouter update (existing) | routing-layer value capture confirmed at $7.5–8B | neutrality now a verify-item | 6× valuation in 6 months = scarcity of the routing layer | dependency-review extension to routers |
| CodeRabbit addendum (existing) | in-kind agent-labor leg of OSS support | — | — | vendor-redundancy caution for agent triage |

---

## 5. Files Created This Run

```
monetization-and-revenue/openai-cursor-severance-case-2026/SKILL.md
privacy-and-trust/plugclaw-tee-consumer-agent-hardware/SKILL.md
marketing-and-content/abliteration-hosted-guardrail-removal/SKILL.md
behavioral-psychology-and-nudging/ai-assistance-distributional-inversion/SKILL.md
behavioral-psychology-and-nudging/cost-salience-variety-margin/SKILL.md
community-and-growth/omacom-patron-funding-model/SKILL.md
monetization-and-revenue/openrouter-inference-routing-economics/references/stripe-acquisition-update-2026-09.md  (update patch)
community-and-growth/oss-endowment-and-coop-funding-2026/references/coderabbit-10m-agentic-support-addendum.md  (update)
```

*Report compiled by A-Tech Research Division — Cycle 18, Run 3, 2026-09-05. Next scheduled run: 2026-09-06.*