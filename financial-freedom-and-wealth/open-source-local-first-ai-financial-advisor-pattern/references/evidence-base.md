# Evidence Base: Open-Source Local-First AI Financial Advisor Pattern

## Detailed Project Comparison

| Feature | Ray Finance | NeoCash | Prospere | Fiduciary | HaloFinance | Wealth Freedom |
|---------|------------|---------|----------|-----------|-------------|----------------|
| **License** | MIT | Apache 2.0 | GPL v3 | MIT | MIT | MIT |
| **Stars (~)** | 285 | — | — | — | — | — |
| **Tech Stack** | SQLite, Python | IndexedDB, Claude Agent SDK | Python (model-agnostic) | CLI tools, 8 skills | Claude Project knowledge | Electron + Vue 3 |
| **Bank Integration** | Plaid + Bridge | Plaid | Plaid | Plaid CLI | Plaid | Plaid |
| **AI Provider** | OpenAI, Anthropic, Ollama | Claude Agent SDK (Anthropic) | Model-agnostic (any) | Model-agnostic | Claude (Anthropic) | Model-agnostic |
| **Encryption** | AES-256 (SQLite) | IndexedDB (browser-local) | Local encrypted DB | Local encrypted storage | Local encrypted storage | AES-256-GCM |
| **Key Features** | Bank sync, PII masking, clean reference impl | 5 specialist agents, 18 tools, agent routing | Monte Carlo simulation, behavior-aware optimization | CFP fiduciary process, Plaid CLI, 8 skills | 9-pillar knowledge architecture, Claude Project | Three-stage philosophy, desktop app, AES-256-GCM |
| **UI Type** | — | Web (local-first) | — | CLI + advisory | Claude Project interface | Desktop (Electron) |
| **Monetization** | Free (BYOK) | — | — | Free (BYOK) | Free (BYOK) | — |
| **Privacy Posture** | PII masking, no telemetry | Local-first IndexedDB, no cloud | Local encrypted, model-agnostic | Local, fiduciary standard | Claude Project (local context) | AES-256-GCM, no cloud |

### Notes on the Comparison

- **Plaid dominance:** All six projects use Plaid for bank aggregation. Only Ray Finance adds Bridge as a secondary aggregation source. This creates a shared dependency — if Plaid changes pricing or access terms, all six projects are affected.
- **Encryption spectrum:** Wealth Freedom's AES-256-GCM (authenticated encryption) is the strongest spec. Ray Finance uses AES-256 (likely CBC without explicit authentication). NeoCash relies on browser IndexedDB isolation, which is not encryption-at-rest in the same sense — it depends on browser security boundaries. Prospere, Fiduciary, and HaloFinance describe "local encrypted storage" without specifying the exact cipher.
- **AI provider concentration:** NeoCash and HaloFinance are built specifically on Anthropic's Claude ecosystem (Agent SDK and Projects, respectively). Ray Finance, Prospere, Fiduciary, and Wealth Freedom are model-agnostic. The Claude-specific projects gain deeper agent orchestration but lose the model-portability advantage.

## Architectural Pattern Analysis

### The Convergent Pipeline

All six projects independently arrived at the same four-layer architecture:

```
Layer 1: Bank Aggregation API (Plaid / Bridge)
    ↓ OAuth flow, transaction/balance pull
Layer 2: Local Encrypted Database (SQLite / IndexedDB / file store)
    ↓ AES-256 / SQLCipher / Fernet encryption at rest
Layer 3: LLM Agent with Financial Context (PII-masked injection)
    ↓ Model-agnostic routing or specialist agent dispatch
Layer 4: Conversational Advisory UI (Desktop / Web / CLI)
    ↓ Natural language advice grounded in user's actual data
```

### Why This Pattern Emerged

1. **Trust gap:** Consumer finance AI requires financial data, but users don't trust cloud services with it. Local-first resolves this paradox — data stays local, only masked context reaches the AI.
2. **Plaid maturity:** Plaid's API has become reliable enough that bank aggregation is a solved problem. All six projects leverage this rather than building custom integrations.
3. **LLM agent maturity:** 2025-2026 saw agent frameworks (Claude Agent SDK, OpenAI function calling) mature to the point where structured financial advisory is feasible — not just chat, but tool-augmented reasoning.
4. **FIRE movement growth:** The Financial Independence community has grown significantly, creating demand for tools that go beyond basic budgeting into projection and advisory.
5. **Open-source AI normalization:** With Ollama and local models becoming viable, the "I can run this fully offline" promise is now deliverable, not aspirational.

### Architectural Differentiators

| Dimension | Simple End (Ray Finance) | Complex End (NeoCash) |
|-----------|--------------------------|----------------------|
| Agent design | Single agent, general financial advice | 5 specialist agents with routing |
| Tool count | Minimal tool set | 18 visible tools |
| Storage | SQLite (server-side local) | IndexedDB (browser-local) |
| AI binding | Model-agnostic | Claude Agent SDK (Anthropic-specific) |
| Advisory standard | Implicit best practices | Explicit CFP fiduciary process (Fiduciary project) |
| Quantitative engine | Basic calculations | Monte Carlo simulation (Prospere) |

## Privacy Comparison

| Project | Encryption | PII Masking | Telemetry | Cloud Storage | Audit Logs | Local Model Support |
|---------|-----------|-------------|-----------|---------------|------------|---------------------|
| Ray Finance | AES-256 SQLite | Yes (before AI calls) | None | None | — | Yes (Ollama) |
| NeoCash | IndexedDB (browser) | Likely (Claude SDK patterns) | None | None | — | Via Claude SDK (limited) |
| Prospere | Local encrypted DB | Likely | None | None | — | Yes (model-agnostic) |
| Fiduciary | Local encrypted storage | Likely | None | None | — | Yes (model-agnostic) |
| HaloFinance | Local encrypted storage | Likely (Claude Project) | None | None | — | Via Claude (limited) |
| Wealth Freedom | AES-256-GCM | Likely | None | None | — | Yes (model-agnostic) |

### Privacy Architecture Assessment

- **Strongest privacy:** Wealth Freedom (AES-256-GCM authenticated encryption + model-agnostic including Ollama) and Ray Finance (AES-256 + PII masking + Ollama support + dual Plaid/Bridge)
- **Weakest privacy (relatively):** NeoCash and HaloFinance — their Claude SDK/Project binding means the AI layer is Anthropic-specific. While PII is masked, the inference is cloud-based unless a local Claude-compatible proxy exists. IndexedDB (NeoCash) also relies on browser security boundaries rather than explicit encryption-at-rest.
- **All six:** No telemetry, no cloud storage, no third-party trackers. This is the baseline for the category — any project that breaks this is not in the pattern.

## Monetization Models

### Model 1: Fully Free (BYOK)

| Aspect | Detail |
|--------|--------|
| Cost to user | $0 (plus their own LLM API costs) |
| Developer cost | $0 (user pays for AI) |
| Sustainability | Donations, sponsorships, portfolio value |
| Examples | Ray Finance, HaloFinance, Fiduciary |
| Best for | Open-source purists, developers, privacy-maximalists |

**Trade-off:** Maximum freedom and privacy; minimum sustainability for the developer. The project survives only as long as the developer is motivated to maintain it for free.

### Model 2: Managed API Key (~$10/month)

| Aspect | Detail |
|--------|--------|
| Cost to user | ~$10/month flat |
| Developer cost | LLM API costs (variable, usage-based) |
| Sustainability | Margin = $10/mo minus API costs per user |
| Data flow | Financial data stays local; only AI inference is proxied |
| Best for | Non-technical users who want set-and-forget |

**Trade-off:** User doesn't manage API keys (convenience), but their AI queries flow through the developer's proxy (minor privacy trade-off — financial data is still local, but the developer could theoretically log prompts). PII masking at the app layer mitigates this.

### Model 3: Pro Features

| Aspect | Detail |
|--------|--------|
| Cost to user | Free tier + paid Pro features |
| Developer cost | API costs for free tier users |
| Sustainability | Conversion rate from free to Pro |
| Likely Pro features | Monte Carlo (Prospere), estate planning (NeoCash), advanced tax optimization, multi-account aggregation, historical trend analysis |
| Best for | Users who outgrow the free tier |

**Trade-off:** Balances accessibility (free tier) with sustainability (Pro revenue). Risk: feature gating can feel extractive if the free tier is too limited.

### Sustainability Verdict

No single model is clearly dominant. The most resilient approach is likely a **combination**: BYOK free tier (builds community and trust) + managed key tier (~$10/mo for convenience users) + Pro features (revenue from power users). None of the six projects appear to have implemented all three tiers yet — this is an opportunity.

## Specialist Agent Routing Patterns (NeoCash Deep Dive)

### The 5-Agent Architecture

NeoCash's agent design is the most sophisticated in the category. It uses the Claude Agent SDK to implement a multi-agent system with explicit routing:

```
User Query
    ↓
[Intent Classification / Router]
    ↓
┌─────────────┬──────────────┬──────────────┬──────────────┬──────────────┐
│ Tax Agent   │ Portfolio    │ Budget Agent │ Estate Agent │ Generalist   │
│             │ Agent        │              │              │ Agent        │
├─────────────┼──────────────┼──────────────┼──────────────┼──────────────┤
│ Tax bracket │ Portfolio    │ Budget       │ Estate doc   │ Cross-domain │
│ calculator  │ analyzer     │ categorizer  │ generator    │ synthesis    │
│ Deduction   │ Risk         │ Spending     │ Beneficiary │ Fallback     │
│ finder      │ assessor     │ trend        │ tracker      │ responses    │
│ Capital     │ Rebalancing  │ analyzer     │ Legacy       │ Routing      │
│ gains       │ recommender  │ Savings rate │ planner      │ coordination │
│ analyzer    │              │ calculator   │              │              │
└─────────────┴──────────────┴──────────────┴──────────────┴──────────────┘
    ↓
[Response Synthesis] → User
```

### Routing Logic

1. **Intent classification:** The user's query is classified into one of the five domains
2. **Specialist dispatch:** The query + relevant financial context is sent to the specialist agent
3. **Tool execution:** The specialist invokes tools from the 18-tool registry to ground its response in data
4. **Cross-domain coordination:** For queries spanning domains (e.g., "Should I sell investments to pay off tax debt?"), the Generalist agent invokes both Tax and Portfolio agents and synthesizes
5. **Response synthesis:** The final response is composed and returned to the user

### 18-Tool Registry

NeoCash exposes 18 visible tools to its agents. Based on the 5-agent structure, the likely distribution:

- **Tax Agent (~4-5 tools):** tax bracket calculator, deduction finder, capital gains analyzer, tax-loss harvesting scanner, marginal rate calculator
- **Portfolio Agent (~4-5 tools):** portfolio analyzer, risk assessor, rebalancing recommender, asset allocation visualizer, correlation matrix
- **Budget Agent (~4-5 tools):** budget categorizer, spending trend analyzer, savings rate calculator, category budget setter, forecast projector
- **Estate Agent (~2-3 tools):** estate document generator, beneficiary tracker, legacy planner
- **Generalist (~1-2 tools):** routing logic, context aggregation

### Why This Matters for the Pattern

NeoCash demonstrates that multi-agent routing is viable for personal finance — a domain complex enough to benefit from specialization but structured enough for clear domain boundaries. This is a template that other projects (and new entrants) can adopt:

- **Single-agent projects** (Ray Finance, Fiduciary, HaloFinance, Wealth Freedom) can evolve toward multi-agent by adding specialist agents
- **Prospere** sits in the middle — model-agnostic but with a strong quantitative engine (Monte Carlo) that could become a specialist agent in a multi-agent architecture

## FIRE Calculation Approaches

### Monte Carlo Simulation (Prospere)

**What it does:** Runs N simulated market-return scenarios (typically 1,000–10,000) against the user's portfolio to compute the probability of financial survival over a given retirement horizon.

**Inputs:**
- Current portfolio value
- Annual contribution rate
- Expected real return (e.g., 7% for 60/40 portfolio)
- Return volatility (standard deviation, e.g., 12%)
- Annual withdrawal amount (or withdrawal rate, e.g., 4%)
- Retirement age and target horizon (e.g., 40 years)

**Process:**
1. For each simulation run, generate a sequence of annual returns sampled from a distribution (normal, lognormal, or historical bootstrap)
2. Apply contributions pre-retirement and withdrawals post-retirement
3. Track portfolio balance year by year
4. Record whether the portfolio survives the full horizon (balance > $0 at end)
5. After N runs, success probability = (surviving runs / total runs) × 100%

**Outputs:**
- Success probability (e.g., "87% chance of portfolio surviving 40 years")
- Distribution of terminal portfolio values
- Worst-case, median, and best-case scenarios
- Sensitivity to withdrawal rate (4% rule validation)

**Why it matters:** Monte Carlo is the gold standard for retirement modeling because it accounts for sequence-of-returns risk (the danger of bad returns early in retirement). Static calculators that assume average returns are dangerously optimistic. Prospere bringing this to open-source, local-first software is significant — previously this was locked behind paid tools.

### Coast FIRE Calculation

**Definition:** The point where your current investments, if left untouched (no further contributions), will grow to your FIRE number by your target retirement age.

**Formula:**
```
Coast FIRE reached when:
Current Portfolio × (1 + real_return)^(retirement_age - current_age) ≥ FIRE Number

Where:
FIRE Number = Annual Expenses × 25 (based on 4% rule)
real_return = inflation-adjusted expected return (typically 5-7%)
```

**Example:**
- Current portfolio: $200,000
- Current age: 35
- Target retirement age: 60
- Expected real return: 7%
- Annual expenses: $60,000
- FIRE Number: $60,000 × 25 = $1,500,000

```
$200,000 × (1.07)^25 = $200,000 × 5.43 = $1,086,000
```

$1,086,000 < $1,500,000 → **Not yet Coast FIRE.** Need ~$276,000 today to be Coast FIRE ($1,500,000 / 5.43).

**Significance:** Coast FIRE is a psychological milestone — it means you could theoretically stop saving for retirement and still hit your number. Several of the six projects compute and visualize this.

### Financial Independence Progress Tracking

**Standard metrics computed across projects:**

| Metric | Formula | Example |
|--------|---------|---------|
| FIRE Number | Annual Expenses × 25 | $60K × 25 = $1.5M |
| FI Progress | (Current Portfolio / FIRE Number) × 100 | $500K / $1.5M = 33% |
| Years to FI | Based on savings rate and returns | 12 years at 50% savings rate, 7% returns |
| Safe Withdrawal Rate | 4% (Trinity Study) or variable | $1.5M × 4% = $60K/yr |
| Savings Rate | (Income - Expenses) / Income × 100 | ($120K - $60K) / $120K = 50% |

### Behavior-Aware Optimization (Prospere)

Prospere's unique contribution: projections that adapt to actual user behavior rather than idealized assumptions.

- **Static calculators assume:** "You will save 50% of your income consistently for 15 years"
- **Behavior-aware optimization observes:** "You save 50% in months 1-3, then 30% in months 4-6, then 40% average"
- **The model adjusts:** Projections use actual spending patterns to produce realistic FIRE timelines
- **Recommendations adapt:** If the user consistently overspends dining out, the model doesn't just flag it — it recalculates the FI timeline with the actual dining spend, then shows the delta (e.g., "Your actual spending delays FI by 3 years vs. your target budget")

This is more honest and more useful than static projections, which breed false confidence or false discouragement.

## Limitations and Gaps

### 1. Regulatory Gray Zone
- AI financial advice occupies an undefined space between "educational content" and "licensed financial advice"
- The CFP Board's fiduciary standard (which Fiduciary encodes) applies to licensed professionals — an AI tool following the same process is not legally a fiduciary
- All six projects include disclaimers, but the boundary between "AI-assisted education" and "unlicensed advisory" is unclear
- **Risk:** Regulatory action could constrain the advisory scope of these tools

### 2. Plaid Dependency
- All six projects rely on Plaid for bank aggregation
- Plaid is a paid API (free tier exists but is limited); production usage requires paid plans
- If Plaid restricts access, changes pricing, or is acquired/restricted, all six projects are affected
- **Gap:** No project implements an open aggregation standard (e.g., OAuth-based open banking APIs) as a Plaid alternative
- Ray Finance's Bridge integration is a partial mitigation but Bridge has the same business-model dependency

### 3. No Real-Time Data
- Bank sync is periodic (manual or scheduled), not real-time
- No live market data integration is visible in the six projects
- Portfolio values are snapshots, not live — limiting the usefulness of real-time advisory
- **Gap:** Integration with market data APIs (Alpha Vantage, IEX, Finnhub) for live portfolio tracking

### 4. Tax Complexity
- NeoCash's Tax Agent handles general tax optimization (brackets, deductions, capital gains)
- But multi-jurisdiction tax (state + federal + international), complex pass-through income, and alternative minimum tax are beyond current scope
- Estate tax (NeoCash's Estate Agent) varies dramatically by jurisdiction
- **Gap:** No project handles non-US tax systems or multi-country situations

### 5. No Portfolio Execution
- All six are advisory-only — none connect to brokerages for automated trade execution
- Rebalancing recommendations are generated but the user must manually execute
- **Gap:** Integration with brokerage APIs (Alpaca, Interactive Brokers) for automated rebalancing would close the loop

### 6. Adoption Scale
- The largest project (Ray Finance) has ~285 GitHub stars — this is an emerging pattern, not a mainstream category
- Total combined stars across all six is likely under 1,000
- **Implication:** The pattern is validated (six independent implementations) but adoption is early. Content covering this is ahead-of-curve, not chasing a trend.

### 7. Encryption Transparency
- Several projects describe "local encrypted storage" without specifying the cipher, key derivation, or encryption library
- NeoCash's IndexedDB relies on browser security boundaries rather than explicit encryption-at-rest
- **Gap:** Standardized security documentation across projects; independent security audits

### 8. Mobile Gap
- No project targets mobile (iOS/Android) — all are desktop or web
- Mobile is where most consumers manage personal finance
- **Gap:** A React Native or Flutter implementation of this pattern for mobile would be a significant contribution

## Summary Assessment

The open-source local-first AI financial advisor is a **validated emerging pattern** (six independent implementations) with strong A-Tech alignment (open-source, privacy-first, democratizing financial advisory, practically implemented) but early adoption (~285 stars for the largest project). The convergent architecture (Plaid → local encrypted DB → LLM agent → conversational UI) is well-defined and reproducible. The key differentiators are agent sophistication (NeoCash), quantitative depth (Prospere's Monte Carlo), advisory standardization (Fiduciary's CFP process), and encryption strength (Wealth Freedom's AES-256-GCM).

For content creators and developers, this pattern represents an opportunity to get ahead of a category that will likely grow as FIRE movement adoption increases and trust in cloud finance apps decreases. The gaps (mobile, open aggregation standards, portfolio execution, non-US tax) are clear opportunities for new projects.