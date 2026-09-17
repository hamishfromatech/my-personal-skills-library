---
name: prompt-data-barter-pricing-muse-spark
description: Meta's Muse Spark 1.3 two-SKU pricing — the first explicit per-token market price for prompt data ($1.24/M spread; 92% barter discount). Use when pricing or evaluating AI data-for-compute barter offers. (Established in cycle 18 — see original skill for the two-SKU mechanics, Tunguz price-information synthesis, and data-clause audit checklist.)
---

# Prompt-Data Barter Pricing: Muse Spark — Cycle-20 Refresh

*(Full framework established in cycle 18: Standard ($1.25/$4.25 per M, no training clause) vs Contributor ($0.10/$0.20, Meta may train on prompts AND completions); the $1.24/M blended spread at agentic ratios; compute-as-currency framing; the data-clause audit checklist; per-workload not per-company decisions.)*

## Cycle-20 addendum (2026-09-06): the throughput penalty documented

The deep-dive analysis (progressiverobot.com, Sept 4, 2026) fills the gap the original skill flagged — the Contributor tier's fine print beyond price:

1. **Rate limits are the hidden cost.** Contributor allows **100 requests/minute vs 3,000** on standard (30× lower) and 3M vs 4M tokens/minute. For batch jobs (few huge requests) the token ceiling binds and the tier is nearly a straight discount; **for chatty agent loops — plan, a dozen tool calls, retries, summarization — 100 RPM is a wall** that a handful of concurrent users saturate while barely touching the token ceiling. "The workloads that benefit most from the price cut are often the ones least able to live inside the limit."
2. **The two-tier model is one checkpoint, two identifiers** (`muse-spark-1.3` / `muse-spark-1.3-contributor`) — switching is a configuration change, which is exactly why the routing decision belongs **in a gateway, not in application code** ("at some point one of them will, on a deadline, without thinking").
3. **The Meta-origin story strengthens the mechanism read.** Contributor pricing exists because direct capture failed: the Model Capability Initiative (April 2026; mouse/keystroke/screenshot capture on employee devices, no opt-out) drew a 1,500-employee "Employee Data Extraction Factory" petition, then a SEV-2 internal-leak incident (private conversations, performance info, meeting transcriptions exposed company-wide) paused it June 23. **Training data is the scarce ingredient** — Mario Zechner's point stands: coding agents leapt April→Oct 2025 because Claude Code stored sessions by default for RL; "everything is coding-agent shaped, because that is the only place the training data exists"; contributor pricing is the attempt to buy the missing non-coding traces.
4. **Narayanan's enterprise-premium framing:** firms already pay a large premium *not* to be trained on (enterprise plans vs 10–20×-discounted consumer subscriptions, main difference data retention + governance) — contributor pricing makes that premium explicit, and may push companies to work out which data is genuinely proprietary. **The mixed-routing path is the honest plan:** the 70/30 split-routing example ($364 vs $1,050/$70 bills) prices reality — some traffic carries client names, staff records, unreleased product detail.
5. **UK/EU legal layer:** deliberately routing traffic into a training pipeline is a processing decision — you are the controller choosing purpose; lawful basis before traffic flows; processor-vs-controller relationship changes shape in the ROPA/DPIA; pre-existing client DPAs drafted before this tier existed need onward-disclosure review; personal-data transparency obligations apply regardless of the discount. **Category rules:** client-confidential, personal data, unreleased roadmap/code, regulated content → standard only; prototypes on synthetic fixtures, published material, load tests → contributor viable.
6. **The prediction:** a 21× gap is hard to ignore in a weekly price-comparison market — whether Anthropic/OpenAI answer, and *how*, reveals how badly every lab needs non-coding traces. **Data is being unbundled from compute**; the scarce resource is your workflow, not your text.

## Standing pairs

`ai-inference-investment-rotation-2026`, `open-source-ai-monetization-playbook-2026`, `privacy-first-personalization-2026`, `metered-open-license-revenue-share-2026`, `give-away-keep-matrix-oss-ai`, `hybrid-compute-privacy-gate` (the classification-gate counterpart for routing by sensitivity).

## A-Tech alignment (retained + sharpened)

- **Open source:** the barter model remains the data-axis of the give-away/keep matrix.
- **Privacy:** the first per-token price tag on prompt data — now with the routing-control requirement (gateway-side tier selection, per-request SKU logging, quarterly boundary review) as the privacy engineering companion.
- **Financial freedom:** treat data as a tradable asset with a *throughput* price attached — the blended decision is price × rate-limit fit.
- **Practical:** add the five-line governance table (classification register / separate credentials / gateway-side selection / SKU logging / scheduled review) to the original audit checklist.