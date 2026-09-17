# Evidence Base: 402Pilot Buyer-Side Payment Decision

## Source
Li, Zeng, Tang, Li, He, Yang, Lawana, Tsung. "402Pilot: An x402 Decision Layer for Autonomous Agent Micropayments." arXiv:2608.01341, 2026. Code: github.com/MCCodeAI/402Pilot

## Problem Formulation

### The Buyer-Side Decision Setting
A single autonomous buyer interacts with K payable providers over T rounds, starting with wallet B₀ = B.

At round t:
1. Task arrives with context x_t ∈ X
2. Current wallet: B_t
3. Each provider a has charge p_{a,t}
4. Affordable set: A_t = {a : p_{a,t} ≤ B_t}
5. If A_t = ∅, interaction terminates
6. Policy selects: a_t ~ π_t(·|H_t), support ⊆ A_t
7. Selected provider produces (q_t, f_t) ~ D_{a_t,t}(·|x_t)
   - q_t ∈ [0,1]: realized task quality
   - f_t ∈ {0,1}: paid non-delivery flag
8. Observe (q_t, f_t) and realized charge c_t = p_{a_t,t}
9. Wallet evolves: B_{t+1} = B_t - c_t

### Key Properties
- **Chosen-only paid feedback**: observe outcomes only for selected provider
- **Non-stationary**: provider distributions and charges may change without announced change points
- **Affordability constraint**: can only select from affordable set
- **Autonomous**: must run continuously without human resetting, retuning, or comparison of unchosen outcomes

### Buyer Objective
Realized utility: u_t = q_t - ν·f_t (ν penalizes paid non-delivery)
Normalized cost: c̃_t = clip(c_t/c_max, 0, 1)
Wallet pressure: λ_norm,t = λ_t/(1+λ_t) ∈ (0,1)

Payment-aware reward: r_t = (1 - λ_norm,t)·u_t - λ_norm,t·c̃_t

Objective: π* ∈ argmax E[Σ r_t for t=0 to τ-1]

## 402Pilot Architecture

### Per-Round Decision Flow
1. Upstream components provide task request and candidate payment requirements Q_t = {(a, p_{a,t})}
2. Context encoder maps request to x_t
3. Wallet derives affordable set A_t and wallet pressure λ_t
4. Policy invokes select(x_t, A_t) → a_t
5. Payment substrate (x402 or other) executes transaction
6. Outcome scorer converts response + receipt into (u_t, c_t)
7. Wallet records c_t
8. Policy receives feedback via update(x_t, a_t, u_t, c_t)

### Wallet Pressure Formula
λ_t = λ₀·exp[α·(spending_deviation)]

where spending_deviation = ((B-B_t)/B)/(t/T) - 1 for t > 0, and 0 for t = 0

- λ₀ > 0: baseline pressure
- α ≥ 0: sensitivity to deviations from spending plan
- Exponential form: keeps pressure positive, smoothly increases cost sensitivity

## PA-DCT Algorithm

### Discounted Posteriors
For each provider-context pair (a,k):
- Shared effective count n^{a,k}_t
- Utility sum S^{a,k}_t
- Cost sum S_c^{a,k}_t

Discount at start of each round:
- n̄^{a,k}_t = γ·n^{a,k}_{t-1}
- S̄^{a,k}_t = γ·S^{a,k}_{t-1}
- S̄_c^{a,k}_t = γ·S_c^{a,k}_{t-1}

Q-posterior (Normal-Normal):
- Σ_q^{a,k} = (1/σ²_{0,q} + n̄/σ²_q)^{-1}
- θ_q^{a,k} = Σ_q^{a,k}·(μ_{0,q}/σ²_{0,q} + S̄/σ²_q)

C-posterior (analogous with cost-specific priors)

### Selection Rule
1. Map task to context bucket k_t = k(x_t)
2. For each affordable provider a ∈ A_t:
   - Sample û^{a,k_t} ~ N(θ_q^{a,k_t}, Σ_q^{a,k_t})
   - Sample ĉ^{a,k_t} ~ N(θ_c^{a,k_t}, Σ_c^{a,k_t})
   - Normalize: c̃̂ = clip(ĉ/c_max, 0, 1)
   - Compute: r̂_a = (1-λ_norm)·û - λ_norm·c̃̂
3. Select: a_t = argmax_{a ∈ A_t} r̂_a

### Update Rule
After transaction with selected provider a_t:
- n^{a_t,k_t}_t = n̄^{a_t,k_t}_t + 1
- S^{a_t,k_t}_t = S̄^{a_t,k_t}_t + u_t
- S_c^{a_t,k_t}_t = S̄_c^{a_t,k_t}_t + c_t

All other cells retain discounted statistics.

## 402Pilot-Bench Benchmark

### Design
- Frozen-replay benchmark: responses pre-generated and scored, sampled from same pool across policies
- Enables reproducible comparisons without live-inference variability
- Local x402 witness validates decision-layer interface against actual HTTP 402 payment flow

### Composition
| Dataset | Tasks | Utility Metric |
|---------|-------|---------------|
| HumanEval (coding) | 164 | Execution-based pass@1 |
| HotpotQA (multi-hop QA) | 220 | Max normalized EM/F1 |
| TriviaQA-web (web QA) | 219 | Max normalized EM/F1 |
| OpenAssistant (open-ended QA) | 220 | Cached LLM-as-judge |
| **Total** | **823** | |

- 5 responses cached per task-provider pair
- 823 tasks × 5 providers × 5 responses = 20,575 scored responses

### Provider Set
| Provider | Price | Pipeline |
|----------|-------|----------|
| P-cheap | $0.0005 | qwen3.5-flash; uniform prompt; cheap tier |
| P-mid | $0.002 | GPT-5.4-mini; uniform prompt; mid tier |
| P-premium | $0.01 | GPT-5.4; uniform prompt; premium tier |
| P-adv | $0.002 | GPT-5.4-mini; 60% adversarial prompt; fluent wrong answers |
| P-flaky | $0.002 | GPT-5.4-mini; 40% timeout injection; billed timeouts |

P-adv and P-flaky share P-mid's price tier; true performance must be learned from outcomes.

### Scenario Design
- **S1 Stationary**: No changes
- **S2 Reliability shock**: P-mid experiences 30% forced-failure rate from rounds 3,000-5,500, then recovers
- **S3 Price shock**: P-premium price drops from $0.01 to $0.002 at round 1,000

Evaluation: T=10,000 rounds, $50 budget, 30 paired seeds.
Budget is deliberately binding: at initial premium price, covers only 5,000 of 10,000 rounds.

### Hyperparameters (Locked)
| Parameter | Value | Role |
|-----------|-------|------|
| γ | 0.999 | Discount (effective memory ~1000 rounds) |
| ν | 0.5 | Paid non-delivery penalty |
| c_max | $0.01 | Cost normalizer (highest listed price) |
| α | 2.0 | Pressure sensitivity |
| λ₀ | 1.0 | Baseline pressure |
| μ_{0,q} | 0.5 | Q-prior mean |
| σ²_{0,q} | 1.0 | Q-prior variance |
| σ²_q | 0.09 | Q-likelihood variance |
| σ²_{0,c} | 10^{-4} | C-prior variance |
| σ²_c | 10^{-6} | C-likelihood variance |

## Complete Results

### Main Results (Table 4)

| Policy | S1 Quality | S1 Budget% | S1 PA-gap/T | S2 Quality | S2 Budget% | S2 PA-gap/T | S3 Quality | S3 Budget% | S3 PA-gap/T |
|--------|-----------|-----------|-------------|-----------|-----------|-------------|-----------|-----------|-------------|
| Random | 0.688 | 66 | 0.365 | 0.676 | 66 | 0.375 | 0.688 | 37 | 0.274 |
| Always-P-cheap | 0.610 | 10 | 0.167 | 0.610 | 10 | 0.164 | 0.610 | 10 | 0.195 |
| Always-P-mid | 0.819 | 40 | **0.101** | 0.757 | 40 | 0.174 | 0.819 | 40 | 0.129 |
| Always-P-premium | 0.433 | 100 | 1.072 | 0.433 | 100 | 1.070 | 0.865 | 56 | 0.400 |
| BudgetRule | 0.831 | 80 | 0.692 | 0.769 | 80 | 0.722 | 0.859 | 56 | 0.405 |
| Contextual DS-TS | 0.559 | 100 | 0.858 | 0.514 | 100 | 0.884 | 0.840 | 48 | 0.264 |
| Contextual BTS | 0.612 | 10 | 0.169 | 0.612 | 10 | 0.166 | 0.612 | 10 | 0.197 |
| PM-Greedy | 0.732 | 90 | 0.542 | 0.627 | 99 | 0.660 | 0.836 | 44 | 0.198 |
| LinCBwK-Adapt | 0.824 | 93 | 0.457 | 0.796 | 100 | 0.515 | 0.836 | 45 | 0.195 |
| **PA-DCT** | 0.797 | 42 | 0.133 | 0.761 | 43 | 0.166 | 0.831 | 39 | **0.121** |
| True Oracle (UB) | 0.901 | 32 | 0.000 | 0.901 | 33 | 0.000 | 0.906 | 25 | 0.000 |

### Key Findings
- PA-DCT uses only 39-43% of wallet across all scenarios
- Best non-oracle PA-gap/T in S3 (price shock): 0.121 vs next best 0.195
- Not significantly different from best baseline in S2 (0.166 vs 0.164)
- Exploration cost in S1 (0.133 vs 0.101 for fixed-arm)

### Ablation Study (Table 6)

| Scenario | Variant | PA-gap/T | ROI | Quality | AdaptT |
|----------|---------|----------|-----|---------|--------|
| S1 | Full | 0.133±0.004 | 378 | 0.797 | n/a |
| S1 | -P (no payment-aware) | 0.855±0.058 | 111 | 0.555‡ | n/a |
| S1 | -D (no discount) | 0.109±0.008 | 412 | 0.810 | n/a |
| S1 | -C (no context) | 0.116±0.003 | 390 | 0.812 | n/a |
| S1 | -TS (no Thompson) | 0.130±0.046 | 535 | 0.740 | n/a |
| S2 | Full | 0.166±0.006 | 357 | 0.761 | 1,467 (30/30) |
| S2 | -P | 0.878±0.050 | 103 | 0.517‡ | 1,611 (14/30) |
| S2 | -D | 0.170±0.012 | 399 | 0.745 | 2,249 (30/30) |
| S2 | -C | 0.158±0.006 | 368 | 0.761 | 1,176 (30/30) |
| S2 | -TS | 0.175±0.048 | 537 | 0.687 | 1,125 (30/30) |
| S3 | Full | 0.121±0.004 | 429 | 0.831 | 200 (30/30) |
| S3 | -P | 0.259±0.036 | 351 | 0.840 | 200 (30/30) |
| S3 | -D | 0.114±0.009 | 426 | 0.838 | 279 (30/30) |
| S3 | -C | 0.109±0.003 | 425 | 0.848 | 398 (29/30) |
| S3 | -TS | 0.145±0.025 | 552 | 0.742 | 2,232 (24/30) |
| S3 | -C_post (no learned costs) | 0.144±0.005 | 423 | 0.797 | 200 (30/30) |

‡ = all 30 seeds exhausted wallet before T

### Component Roles (from ablation)
- **Payment-aware ranking**: preserves full-horizon solvency
- **Discounting + realized-cost learning**: enable adaptation
- **Context**: accelerates recovery
- **Thompson sampling**: improves robustness

### Sensitivity Analysis Highlights
- γ=0.99: worse stationary performance (shorter memory)
- γ=0.9995: slightly better S1, similar overall
- α=1.0: less aggressive spending (36.7% budget)
- α=4.0: more aggressive spending (54.7% budget)
- ν and c_max: smaller effects in tested range
- No cell bankrupts the wallet across all sweeps

## Theoretical Analysis

### Proposition 1: Reward Boundedness
For every round t with 0 ≤ ν ≤ 1: r_t ∈ [-1, +1]

### Proposition 2: Posterior Consistency (stationary, no discount)
With γ=1, Q-posterior mean converges a.s. to true utility mean as pull count → ∞

### Lemma 1: Effective Sample Size
For γ ∈ (0,1): n^{a,k}_t ≤ (1-γ^{t+1})/(1-γ) ≤ 1/(1-γ)

### Theorem 1: Clean-Score Regret Reduction
Under well-specified model, γ=1, clean score:
E[PA-Reg_T] ≤ C·√(K·K_b·T·log T)

### Theorem 2: Adaptation Rate Under Abrupt Change
After shift from μ_old to μ_new at t*:
|E[θ_{q,t}] - μ_new| ≤ (A·Δ + κ_q·|μ_{0,q} - μ_new|) / (κ_q + A + B)

where A = discounted stale mass, B = new observation mass

Corollary: Recovery horizon ≈ log(Δ/δ)/log(1/γ) = O(log(Δ/δ)/(1-γ))

## x402 Integration Witness
- Local Anvil fork with USDC contract state
- Python x402 facilitator for verifying/settling exact EVM payments
- FastAPI resource server exposing x402-protected endpoints
- X402PaymentExecutor adapter mapping HTTP 402 to 402Pilot outcome
- bash scripts/witness_smoke.sh validates unpaid 402 response and paid request

## Connection to Existing Agentic Payment Skills

- **Agent Economy Payment Protocols** (existing): Covers the protocol stack (MCP→A2A→Payment→Identity); 402Pilot adds the buyer-side decision layer ABOVE payment execution
- **Agentic Payment Protocol Convergence** (existing): Covers x402/AP2/ACP comparison; 402Pilot is protocol-agnostic and works with any of these
- **Agent-Ready API Monetization** (existing): Covers seller-side pricing; 402Pilot covers buyer-side spending
- **MCP Payment Support Specification** (existing): Covers payment in MCP; 402Pilot decides WHEN to pay

## A-Tech Alignment

- **Open-source AI**: Protocol-agnostic (works with any payment substrate); code open-source (github.com/MCCodeAI/402Pilot); supports open-weight model providers
- **Data privacy**: Buyer-side decision layer doesn't expose user data; payment decisions made locally; x402 settlement is privacy-preserving
- **Financial freedom**: Enables cost-effective agent operation (39-43% budget vs 90-100% for alternatives); adapts to price changes automatically; prevents wallet exhaustion
- **Practical implementation**: Concrete algorithm with locked hyperparameters; reproducible benchmark (402Pilot-Bench); x402 integration witness; immediate deployability