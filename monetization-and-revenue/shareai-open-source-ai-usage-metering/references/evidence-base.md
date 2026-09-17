# ShareAI Open-Source AI Usage Metering — Evidence Base

Source documentation for the ShareAI open-source AI usage metering skill. This file captures the underlying problem economics, the 7-step monetization plan in detail, pricing pattern comparisons, community communication templates, Builder vs. Provider reward mechanics, and the RAG pipeline cost analysis.

Primary source: ShareAI (2026). "Open Source AI Monetization Without Closing the Project." shareai.now/blog/insights/open-source-ai-monetization/

RAG-specific source: ShareAI (2026). "Open Source RAG App Monetization: Price Queries, Not Downloads." shareai.now/blog/insights/rag-monetization/

---

## 1. The Core Problem: AI Cost Asymmetry in Open Source

### Why traditional OSS economics break with AI features

Traditional open source has a flat cost structure: the maintainer writes code, publishes it, and the marginal cost of each new user approaches zero. Users download, self-host, and run locally. The maintainer's cost is development time, not serving time.

AI features introduce a variable, per-user, per-request cost that scales linearly with usage:

- Every RAG answer requires: query embedding, vector search, context retrieval, LLM generation. Each stage has a real cost.
- Every agent run may chain multiple model calls, tool executions, and verification steps.
- Every code review, summary, or generation triggers an inference call priced by the model provider.

The asymmetry: heavy users and light users both star the same repo, both file issues, both appear in the community — but one generates cents of inference cost per month and the other generates hundreds of dollars. A single power user automating workflows across a large codebase can create more inference cost than a thousand casual users combined.

### Why sponsorships cannot absorb this

Sponsorships and donations scale with goodwill and community size, not with the inference bill. A project that gains 10,000 new users — 100 of whom are heavy AI users — sees its inference costs spike while sponsorship revenue grows marginally. The maintainer is effectively subsidizing the heaviest users through their own sponsorship income.

The result is a forced choice between three bad options:
1. **Eat the cost** — unsustainable; the maintainer pays for others' AI usage from their own pocket.
2. **Close the source** — betrays the community that made the project valuable.
3. **Remove the AI features** — kills the differentiator that made the project worth adopting.

ShareAI provides the fourth path: keep everything open, meter only the AI traffic, and let the users who generate the cost pay for it directly.

---

## 2. The ShareAI Pattern in Detail

### Architecture

The ShareAI framework provides three infrastructure layers that the maintainer does not need to build:

1. **Routing layer** — receives AI inference requests from the project, routes them to compute providers in the marketplace, and returns results. The project's AI calls point at ShareAI instead of directly at OpenAI/Anthropic/Together/etc.

2. **Billing layer** — tracks usage by user and workspace, enforces free allowances and caps, calculates the maintainer's configured margin, handles payment processing, and issues invoices. The maintainer never sees payment details.

3. **Payout layer** — aggregates the maintainer's earned margin across all routed traffic for the month and pays out. The maintainer receives a monthly payout based on actual usage.

### What gets routed vs. what stays local

| AI Path | Routed Through ShareAI? | Why |
|---|---|---|
| Hosted AI endpoint (maintainer's managed AI) | Yes | This is the metered path; users pay for convenience |
| Local model (Ollama, llama.cpp, etc.) | No | No inference cost to the maintainer; stays free |
| BYOK (user's own API key) | No | User pays the provider directly; no margin for maintainer |
| Non-AI features (search, indexing, CRUD) | No | No inference cost; core product stays free |

The maintainer chooses which AI calls to route. The typical starting point: route the most popular AI feature that has clear per-unit value and clear inference cost. Expand only after the first feature is proven.

### Money flow

```
User triggers AI feature in the project
        ↓
Project routes inference request through ShareAI
        ↓
ShareAI checks: free allowance remaining? cap exceeded?
        ↓
If within allowance: fulfill request, decrement allowance
If exceeded: prompt user to top up / pay
        ↓
ShareAI routes inference to compute provider in marketplace
        ↓
Provider fulfills request at listed price
        ↓
ShareAI adds maintainer's configured margin
        ↓
User pays: inference cost + maintainer margin
        ↓
ShareAI aggregates maintainer margin across all traffic
        ↓
Monthly payout to maintainer
```

The maintainer's revenue is the configured margin on routed traffic — not the inference cost, not the user's total payment, not a subscription fee. The margin scales with actual AI usage and actual value delivered.

---

## 3. The 7-Step Monetization Plan in Detail

### Step 1: Define what stays free

Write down explicitly what remains open and free. This is not a vague commitment — it is a specific list the community can reference.

**Template:**
- Source code: stays open under [license]
- Self-hosting: free, always
- Local model support: free, always
- BYOK: free, always (users bring their own API keys)
- Core product features: [list specific features] — free
- Community access: free
- Non-AI features: free

This list becomes the foundation of community trust. When users ask "what changed?" the answer is "nothing in the free tier — we added a managed AI path for users who want it."

### Step 2: Name the successful outcome

The billing unit should map to a user-visible outcome, not an infrastructure event. Users don't buy "inference calls" — they buy "answers," "reviews," "summaries," "agent runs."

**Examples:**
- RAG tool: "Every developer gets instant, accurate answers from your documentation without reading 50 pages."
- Code review tool: "Every PR gets a thorough AI review with security, style, and correctness checks before a human looks at it."
- Note-taking app: "Every document gets a structured summary and semantic search across your entire knowledge base."
- Agent tool: "Every workflow run completes autonomously with verification at each step."

The outcome statement becomes the pricing anchor: "X included answers per month, $Y per additional answer."

### Step 3: Measure the full cost path

Trace the actual inference cost for one unit of the successful outcome. This requires understanding every model call in the pipeline, not just the final generation.

**RAG answer cost path example:**
1. Query embedding: $0.0001 (embedding model, ~100 tokens)
2. Vector search: $0.00005 (compute, negligible per query at scale)
3. Context retrieval: $0 (database read, negligible)
4. LLM generation: $0.008–$0.02 (depends on model, context length, output length)
5. Optional reranking: $0.0005 (cross-encoder model)
6. **Blended cost per answer: ~$0.01–$0.02**

The maintainer sets the per-answer price above this blended cost. If the answer price is $0.03, the margin is $0.01–$0.02 per answer.

### Step 4: Set an allowance and paid path

**Free allowance design principles:**
- Generous enough that casual/community users never hit the paywall.
- Defined in customer-understandable units (answers, runs, documents), not tokens.
- Resets monthly (typical) or is a one-time grant (for project-specific reasons).
- Should cover normal participation: a few RAG queries per day, a handful of code reviews per week, a few summaries per session.

**Paid path design principles:**
- Top-ups: pay-per-unit for additional usage beyond the allowance.
- Workspace caps: team-level limits that prevent surprise spend and align cost with the team generating it.
- Clear pricing: users should know what one additional unit costs before they use it.
- No lock-in: users can always switch to BYOK or local models to avoid paid usage.

### Step 5: Route selected inference through ShareAI

The integration is a routing change, not a product rewrite. The project's AI calls that were previously directed to an inference provider (OpenAI, Anthropic, Together, etc.) are redirected to ShareAI's endpoint. ShareAI handles the rest: routing to marketplace providers, billing, allowances, caps, and payouts.

**Integration checklist:**
- [ ] Identify which AI calls to route (start with ONE feature)
- [ ] Replace direct inference provider calls with ShareAI routing
- [ ] Configure margin/surcharge percentage
- [ ] Set free allowance per user/workspace
- [ ] Set caps and failure behavior
- [ ] Test the routed path end-to-end
- [ ] Add UI: show remaining allowance, top-up flow, usage history
- [ ] Add fallback: if ShareAI is unavailable, fall back to BYOK or local model

### Step 6: Add limits and failure rules

What happens when a user hits their cap or ShareAI is unavailable?

| Scenario | Behavior | User Message |
|---|---|---|
| Free allowance exhausted | Prompt to top up or switch to BYOK/local | "You've used your free AI answers for this month. Top up for more, or use your own API key / local model to continue free." |
| Workspace cap hit | Pause AI features, allow manual override by admin | "Your workspace has reached its AI usage cap. An admin can increase the cap or you can use BYOK." |
| ShareAI unavailable | Fall back to BYOK or local model if configured | "Managed AI is temporarily unavailable. Your request will use your configured API key / local model." |
| Payment failure | Grace period, then reduce to free allowance | "Your payment method needs updating. You still have your free monthly allowance." |

Never silently break. Always explain why the limit exists, what the user's options are, and how to continue.

### Step 7: Explain the model in plain language

The community communication should be published before the metering goes live. Users should understand the model before they encounter a paywall.

**Communication template:**

> **What's changing:** Nothing in the free tier. The source, self-hosting, local models, and core features stay exactly as they are.
>
> **What's new:** We're adding a managed AI path for [feature]. If you want AI-powered [answers/reviews/summaries] without managing API keys or running local models, you can use the managed path. It includes [X] free [units] per month, and additional usage is [ $Y ] per [unit].
>
> **Why:** AI features create real per-request inference costs. Sponsorships cover some of this, but heavy usage — which we're grateful for — generates costs that sponsorships can't absorb. The managed path lets users who want convenience pay for the AI compute they use, while everyone else keeps using the project for free with their own keys or local models.
>
> **What stays free:** [Explicit list from Step 1]
>
> **What costs money:** Only the managed AI path for [feature], and only beyond the free allowance.

---

## 4. Pricing Pattern Comparison

| Pattern | How It Works | Best For | Risk |
|---|---|---|---|
| **Included credits + paid top-ups** | Monthly free credit allowance; top-ups are paid per unit | Consumer-facing tools, individual developers | Users may churn when credits run out if top-up friction is high |
| **Free core + paid hosted AI** | Self-hosted AI stays free; hosted AI endpoint is metered | Projects with strong self-hosting community | Self-hosting users never convert to paid; revenue depends on convenience-seekers |
| **Workspace caps for heavy teams** | Free allowance per workspace; overage is paid | Team/enterprise deployments | Caps may frustrate teams if set too low; requires admin UI |
| **BYOK + managed usage path** | BYOK is free (no margin); managed path routes through metering | Developer tools with technical and non-technical users | BYOK users generate no revenue; margin depends on convenience preference |

**Hybrid approach:** Most projects should combine patterns. A typical starting configuration:
- BYOK: free, always (captures technical users, builds goodwill)
- Free allowance: [X] units/month per user (captures casual users)
- Top-ups: $Y per unit (captures heavy individual users)
- Workspace caps: configurable per team (captures enterprise teams)
- Managed hosted AI: the default path for users who don't configure BYOK

---

## 5. Builder Payouts vs. Provider Rewards

ShareAI operates two distinct reward mechanisms. Confusing them undermines community trust and miscalibrates expectations.

### Builder payouts

- **Who earns:** Open-source maintainers and application developers who route AI traffic through ShareAI.
- **What they earn for:** The margin/surcharge configured on AI traffic their project routes.
- **How it works:** The maintainer integrates ShareAI routing, sets a margin percentage, and users pay for routed AI usage. ShareAI calculates the total margin earned across all routed traffic and pays the maintainer monthly.
- **Revenue driver:** App traffic volume. More users routing more AI requests = more margin earned.
- **Analogy:** Like a payment processor fee — the maintainer earns a percentage of the AI usage their project generates.

### Provider rewards

- **Who earns:** Compute providers who contribute inference capacity to the ShareAI marketplace.
- **What they earn for:** Fulfilling inference requests at their listed price.
- **How it works:** Providers list available compute (GPU capacity, model endpoints) on the ShareAI marketplace. ShareAI routes inference requests to providers based on availability, price, and performance. Providers earn based on compute fulfilled.
- **Revenue driver:** Compute contributed. More capacity listed and fulfilled = more revenue.
- **Analogy:** Like a cloud provider — the provider earns for the compute they supply.

### Can a maintainer be both?

Yes. A maintainer can earn Builder payouts (from app traffic margin) AND Provider rewards (from contributing compute). But the two streams are distinct and should be tracked separately:

- **Builder revenue** depends on building AI-powered applications that users want to use.
- **Provider revenue** depends on supplying the compute infrastructure that runs inference.

A maintainer who both builds an AI-powered OSS tool and contributes GPU capacity to the marketplace earns from both sides — but the skills, investment, and risk profiles are completely different.

---

## 6. RAG-Specific Monetization: Price Queries, Not Downloads

### The insight

> **"Open Source RAG App Monetization: Price Queries, Not Downloads"**

Downloads are the wrong billing event for RAG applications because a single download can generate thousands of queries over the lifetime of a deployment. The billable unit should be a successfully completed RAG answer — not a download, not a token, not a session.

### Why downloads fail

| Problem | Explanation |
|---|---|
| One download → thousands of queries | A user downloads the RAG app once, then runs queries daily for months. The download fee captures none of the ongoing inference cost. |
| Heaviest users pay least (per query) | Production deployments generate the most queries but pay the same one-time download fee as a user who tries it once and abandons it. |
| No recurring revenue | A download fee is one-time; inference costs are recurring. The maintainer pays ongoing costs with one-time revenue. |
| Misaligns price with cost | Inference cost is driven by query volume, not download count. Download-based pricing captures adoption, not the cost driver. |

### Why tokens fail

| Problem | Explanation |
|---|---|
| Invisible to end users | Users don't know what a token is. They can't estimate what a query will cost. |
| Unpredictable costs | Token counts vary with context length, model choice, prompt structure, and retrieval depth. The same query can cost 10x more depending on configuration. |
| Conflates cost stages | Tokens blend retrieval cost (embedding, vector search) with generation cost (LLM output) even though these have different cost profiles and scaling behavior. |
| Hard to budget | Teams cannot predict monthly token spend without detailed usage analytics. |

### Why a completed RAG answer is the right unit

| Advantage | Explanation |
|---|---|
| User-understandable | Users know what "an answer" is. They can estimate how many answers they need per day/month. |
| Bundles the full cost path | One price covers embedding → retrieval → generation → optional reranking. No surprise sub-charges. |
| Aligns with value | Users pay for the successful outcome (a useful answer), not the input volume (tokens consumed). |
| Predictable for budgeting | A team can estimate "we need ~500 answers/month" and know the cost. |
| Margin is transparent | The maintainer knows the blended cost per answer and sets the price above it. |

### RAG pipeline cost profiles

Each pipeline stage has a different cost structure:

| Stage | Cost Driver | Cost Scale | Per-Query Cost | Notes |
|---|---|---|---|---|
| **Indexing** | Number of documents, document size | Upfront, per-document | $0 (per query) | One-time cost when documents are added; not part of per-answer pricing |
| **Embedding (query)** | Query length | Per-query | ~$0.0001 | Negligible per query; scales with query volume |
| **Vector search** | Corpus size, index type | Per-query | ~$0.00005 | Cheap but not free; scales with query volume and corpus size |
| **Context retrieval** | Number of chunks retrieved | Per-query | ~$0 (database read) | Negligible at typical scale |
| **Generation (LLM)** | Context length, output length, model choice | Per-query | $0.008–$0.02 | Dominant cost; varies 2–5x by model and context |
| **Reranking** (optional) | Number of candidates, reranker model | Per-query | ~$0.0005 | Optional quality boost; good candidate for tiered pricing |
| **Workflow steps** (multi-hop, summarization, verification) | Number of additional model calls | Per-query, variable | $0.001–$0.01 per step | Optional; advanced features that add cost and value |

**Blended cost per completed answer:** Typically $0.01–$0.03 depending on model choice, context length, and pipeline complexity. The maintainer should measure their own blended cost, add margin, and set the per-answer price.

### Tiered pricing for RAG

Different pipeline configurations deliver different value. Tiered pricing captures this:

| Tier | What's Included | Price per Answer | Target User |
|---|---|---|---|
| **Basic** | Single-pass retrieval + standard model generation | $0.02 | Casual users, small corpora |
| **Pro** | Multi-pass retrieval + reranking + larger context model | $0.05 | Power users, larger corpora, higher accuracy needs |
| **Enterprise** | Multi-hop retrieval + verification + frontier model + custom indexing | $0.10+ | Production deployments, mission-critical answers |

Tiered pricing lets users self-select based on their accuracy needs and budget, while the maintainer captures more margin from higher-value configurations.

---

## 7. Feature Selection Decision Framework

### Criteria for a good first metered feature

| Criterion | Why It Matters |
|---|---|
| Users already love it | Metering a feature nobody uses generates no revenue and signals the maintainer is desperate. |
| Clear per-unit value | Users can see why one unit is worth paying for (an answer, a review, a summary). |
| Obvious inference cost | The maintainer can explain why this feature costs money without sounding greedy. |
| Uneven usage across users | If everyone uses it the same amount, flat pricing works; if usage varies wildly, metering captures the variance fairly. |
| Self-contained | The feature can be metered without affecting other features or the core product. |
| Graceful degradation | When the allowance is exhausted, the user can fall back to a free alternative (BYOK, local model, manual process). |

### Scoring a candidate feature

Rate each criterion 1–5. A feature scoring below 3 on any criterion is a poor first candidate.

| Feature | Users love it | Clear per-unit value | Obvious cost | Uneven usage | Self-contained | Graceful degradation | Total |
|---|---|---|---|---|---|---|---|
| RAG answers | 5 | 5 | 4 | 5 | 5 | 4 | 28 |
| Code review | 5 | 5 | 5 | 5 | 4 | 3 | 27 |
| Document summary | 4 | 4 | 4 | 4 | 5 | 5 | 26 |
| Agent run | 4 | 4 | 5 | 5 | 3 | 2 | 23 |
| Chatbot conversation | 3 | 3 | 4 | 4 | 3 | 3 | 20 |

RAG answers and code reviews are typically the strongest first candidates because they score high on value clarity, cost obviousness, and usage unevenness.

---

## 8. Anti-Patterns to Avoid

- **Metering everything at once.** Start with one feature. Metering the entire AI surface in one move creates pricing complexity, user confusion, and community backlash. Expand only after the first feature is proven.
- **Setting the free allowance too low.** If casual users hit the paywall in their first session, the community perceives the change as a bait-and-switch. The allowance should cover normal participation generously.
- **Hiding the cost structure.** If users can't see what a unit costs before they use it, trust erodes. Transparent per-unit pricing is non-negotiable.
- **Gating the core behind AI paywall.** The core product must remain useful without paid AI. If the core is broken without the metered AI features, the project is effectively closed-source with an open-source label.
- **Calling the surcharge a tax.** "AI tax" frames the charge as a penalty. The right framing: "managed AI path for users who want convenience" — a service, not a tax.
- **Removing previously free AI features.** If an AI feature was free and becomes metered, users who built workflows around it feel betrayed. Always add a new managed path alongside the existing free path (BYOK/local) rather than converting a free feature to paid.
- **Ignoring the BYOK path.** BYOK users generate no revenue, but they generate goodwill, community engagement, and bug reports. Suppressed BYOK in favor of the managed path signals the maintainer is prioritizing revenue over the community.

---

## 9. Monthly Review Checklist

After the first month of metered usage, review:

- [ ] What percentage of active users exceeded the free allowance?
- [ ] What was the average usage per user? Per workspace?
- [ ] What was the total inference cost? Total margin earned?
- [ ] Did any users switch to BYOK after hitting the paywall? (Good — they're still users.)
- [ ] Did any users churn after hitting the paywall? (Bad — allowance may be too low or messaging unclear.)
- [ ] Were there any support tickets about the paywall? What did users find confusing?
- [ ] Is the margin sufficient to cover costs plus a sustainable return?
- [ ] Should the allowance be adjusted up (too many hitting the wall) or down (too few converting to paid)?

Adjust allowances, caps, messaging, and margin based on real data — not assumptions.

---

## Cross-References to Adjacent Skills

- **`open-core-ai-feature-metering`** — The broader three-layer separation model (core access / commercial value / AI consumption). ShareAI usage metering is one implementation of the AI consumption layer within that framework. The three-layer model provides the strategic context; ShareAI provides the routing/billing/payout infrastructure.
- **`shareai-open-source-ai-metering-pattern`** — The foundational ShareAI metering pattern. This skill extends it with the detailed 7-step monetization plan, the Builder vs. Provider distinction, the RAG-specific billing-unit insight, and the feature selection decision framework. Together they provide the complete ShareAI implementation guide.
- **`tanso-ai-margin-ledger-metering`** — For teams that need per-customer, per-feature, per-model margin visibility on metered AI revenue. Tanso provides the dual-sided ledger (revenue + cost per event) that complements ShareAI's routing layer. Use Tanso for internal margin accounting; use ShareAI for external billing and payout.
- **`open-source-risk-removal-monetization-2026`** — The broader 2026 shift from selling code access to selling risk removal (compliance, sovereignty, maintenance, migration). ShareAI metering is one implementation of "usage-based controls" within that framework. The risk-removal model provides the market context; ShareAI provides the AI-specific metering infrastructure.
- **`open-source-ai-monetization-mastery-2026`** — The comprehensive OSS AI monetization playbook (Give-Away/Keep Matrix, 5-Layer Monetization Stack, license-trap analysis). ShareAI metering fits within the Self-Host Loss Leader layer (free adoption) and the Managed Cloud layer (paid hosted AI). The mastery framework provides the strategic portfolio; ShareAI provides the tactical implementation for the AI usage layer.