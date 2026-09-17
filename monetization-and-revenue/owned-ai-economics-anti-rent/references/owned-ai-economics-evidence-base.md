# Owned-AI Economics Evidence Base

This reference collects the quantitative and strategic evidence backing the `owned-ai-economics-anti-rent` skill: A-Tech's documented transition, the self-hosting tipping point, the de-platforming case, the open-source convergence market data, and the detailed TCO calculation.

---

## 1. A-Tech's Documented Transition (Primary Evidence)

Source: "The Owner's Guide to AI" and "Be Practical: The only Handbook you need to use AI in your business" (Hamish Aman Prakash, A-Tech Corporation).

### The rented baseline (~$2,850/month = $34,200/year)

| Rented service | Monthly cost |
|----------------|--------------|
| ChatGPT Plus and API usage | $450 |
| Zapier automation | $50 |
| Notion | $10 |
| Slack | $15 |
| Cloud hosting | $200 |
| Various other tools | $2,125 |
| **Total** | **~$2,850/month → $34,200/year** |

### The owned alternative

| Item | Cost |
|------|------|
| Hardware investment (one-time) | $25,000 |
| Ongoing costs (electricity, maintenance) | $2,400/year |

### The crossover

- Break-even: $25,000 / ($34,200 − $2,400) = $25,000 / $31,800 ≈ **0.79 years ≈ 9.5 months**
- Annual savings after year one: **$31,800**
- Cumulative 5-year savings (rented keeps rising; owned is fixed): **~$159,000+** (before price hikes)

### The strategic benefits A-Tech documented (beyond cost)

- **Data control:** data never leaves the infrastructure; models fine-tuned on proprietary data.
- **Customization:** tools modified to fit exact needs; not limited by vendor offerings.
- **Integration:** everything integrates seamlessly because A-Tech controls all the pieces.
- **No ongoing subscription costs:** the only ongoing cost is electricity, which the business is already paying for.
- **The A-Coder proof:** building A-Coder cost ~$2,000 in AI services over 5 months of development; ongoing costs are essentially zero (runs on owned hardware with open-source models); savings of ~$6,000/year on previously rented coding-related AI services.

### The "rent to build, then own" principle

A-Tech's documented approach: use the best available paid tools *temporarily* to build your own permanent solutions. This is the practical path for businesses that cannot afford the one-time hardware investment upfront — rent the capability to build the owned replacement, then cancel the rental.

---

## 2. The Self-Hosting Tipping Point (Market Evidence)

Source: `open-source-ai-2026-convergence-maturity` skill; 2026 market data.

- **The tipping point:** ~500K–1M tokens/day. Above this usage level, the per-token API cost of renting exceeds the compute cost of self-hosting the equivalent open model.
- **Why it has fallen:** open small models (1–9B parameters) now run on consumer GPUs; vLLM and Text Generation Inference make serving fast and reliable; LoRA/PEFT fine-tuning adapts a 7B model on a single GPU.
- **Implication:** most serious business users cross the tipping point within months of daily use. The own-vs-rent crossover is not a future scenario — it is a present one for the majority of non-trivial users.

### Open-source AI market size (context)

- Open-source AI model market: **$23.08B (2026) → $50B+ (2030)**, 21% YoY growth.
- Hugging Face hosts **2M+ public models**; 332,000 uploads in a single quarter (2025); 3× growth 2023–2025.
- Benchmark gap: open models within **1–2 points** of proprietary on critical benchmarks.
- Most-deployed models: **1–9 billion parameters** (small models dominate real-world deployment).
- **Apache 2.0** as the enterprise standard (unrestricted commercial use).

---

## 3. The De-Platforming Case

Source: `open-source-profitability-evidence-framework` skill; UNICEF Venture Fund / Somleng case.

- **The Somleng case (David Wilkie):** "If a telco builds on a proprietary service and gets kicked off, their business collapses. Open source is the safety net." A real business built on a rented API was de-platformed; the open-source-owned version made the business resilient.
- **The enterprise sales lever:** "Your AI coding infrastructure can't be revoked." This is the pitch for owned, local-first architecture: the vendor cannot pull the plug, raise prices 3×, or shut down.
- **The de-platforming risk of renting is real:** cloud AI providers have revoked API access, changed pricing models, and shut down products (e.g., multiple model deprecations, API sunsetting). A business whose AI capability depends on a single vendor's continued goodwill is structurally fragile.

---

## 4. The Open-Source Profitability Evidence

Source: `open-source-profitability-evidence-framework` skill; UNICEF Venture Fund portfolio data.

- **>70% of open-source companies generate revenue**; **>40% reached profitability**; **12× follow-on funding**; **8 acquisitions**.
- **50+ dedicated VC firms** (OSS Capital, Open Core Ventures, Battery Ventures' BOSS Index), Sequoia/a16z OS programs, Zerodha $1M FOSS fund, the Sovereign Tech Fund — validate that owned open-source with revenue pathways is fundable on mature terms.
- **Government adoption:** France Etalab, Germany Sovereign Tech Fund, US Code.gov, India OSI, China Open Atom Foundation — public procurement of open-source AI validates the owned model at the sovereign level.
- **The Sovereign Tech Fund causal evidence** (`sovereign-tech-fund-causal-impact` skill): public funding causally increases open-source project velocity (commits +143.8%, merged CRs +175.5%) — the public sector is investing in owned open-source infrastructure.

---

## 5. Detailed TCO Calculation Template

For any business evaluating own-vs-rent, fill in:

```
RENTED ANNUAL COST (R)
  AI model/API subscriptions:        $______
  Automation tools (Zapier/Make):    $______
  Knowledge/docs (Notion/etc):       $______
  Communication (Slack/etc):         $______
  Cloud hosting/inference:           $______
  Other SaaS:                        $______
  Price-hike buffer (e.g., +15%):    $______
  TOTAL R =                          $______

OWNED ONE-TIME (O)
  Hardware (GPU/CPU workstations):  $______
  Setup labor (time → $):            $______
  TOTAL O =                          $______

OWNED ANNUAL OPERATING (P)
  Electricity:                       $______
  Maintenance/amortized upgrades:    $______
  Fine-tuning compute (intermittent):$______
  TOTAL P =                          $______

CROSSOVER TIME = O / (R − P) = ____ years

ANNUAL SAVINGS AFTER CROSSOVER = R − P = $______

5-YEAR NET SAVINGS = 5R − O − 5P = $______
```

Decision rule: if crossover < 18 months and the business values data control / de-platforming resilience, owning is the economically and strategically dominant choice.

---

## 6. The Open-Source MLOps Stack (No Software-License Cost)

A complete production ML stack that can be owned without software-license cost (only compute and know-how):

| Layer | Open-source standard (2026) |
|-------|-----------------------------|
| Scaling | Kubernetes |
| Experiment tracking / model registry | MLflow |
| Orchestration | Kubeflow |
| Feature stores | Feast |
| Model monitoring | Evidently, NannyML |
| LLM observability | Langfuse, Arize Phoenix |
| Data versioning | DVC |
| Pipeline orchestration | Metaflow, ClearML, ZenML |
| Serving | vLLM, Text Generation Inference, Seldon Core |

Key insight: the entire owned stack's software cost is zero. The only costs are hardware, electricity, and the skill to operate it — and the skill is increasingly democratized (Hugging Face Transformers, PEFT/LoRA, vLLM make a single developer capable of what required teams a few years ago).

---

## 7. The Fine-Tuning Edge (The Moat Rented Models Cannot Offer)

- Owned models can be fine-tuned on the business's proprietary data — customer profiles, product language, sales metrics, support transcripts — for a domain edge rented generic models cannot match.
- Documented A-Tech outcomes of fine-tuned owned AI: marketing copy that speaks to the exact customer; customer service that understands the specific product; data analysis that knows what "a good month" looks like for that business.
- The win-win-win: (1) save money, (2) get better results, (3) own the asset. Rented models can offer (1) at best, never (2) on proprietary data, and never (3).
- The fine-tuned model is a proprietary asset on the balance sheet; the rented API is an expense on the income statement.

---

## Sources

1. Hamish Aman Prakash — "The Owner's Guide to AI" (A-Tech Corporation). The documented $34,200/yr → $2,400/yr transition; the strategic benefits; the A-Coder development cost and savings; the "rent to build, then own" principle.
2. Hamish Aman Prakash — "Be Practical: The only Handbook you need to use AI in your business." The own-your-AI argument; the fine-tuning edge; the win-win-win framing.
3. `open-source-ai-2026-convergence-maturity` skill — the $23.08B market, 2M+ models, self-hosting tipping point (~500K–1M tokens/day), small-model dominance.
4. `open-source-profitability-evidence-framework` skill (UNICEF Venture Fund) — the Somleng de-platforming case, 70%/40%/12x/8 portfolio data, 50+ VC firms.
5. `sovereign-tech-fund-causal-impact` skill (Burin, arXiv:2607.05413) — public funding causally increases open-source velocity (+138–175%).
6. `slm-enterprise-deployment` skill — small-model (1–9B) deployment on consumer hardware.
7. `privacy-preserving-local-ai` and `privacy-first-competitive-differentiator` skills — the data-control and privacy-as-competitive-position arguments.