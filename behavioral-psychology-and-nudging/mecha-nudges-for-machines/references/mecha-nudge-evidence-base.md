# Mecha-nudge Evidence Base

## Source
Frey, G. & Ethayarajh, K. (2026). "Mecha-nudges for Machines." University of Chicago. arXiv:2603.23433.

## Empirical Study: Etsy Listings

### Dataset
- Raw dataset: 6M+ Etsy listings (Bright Data scrape, Nov 12, 2025)
- Pre-ChatGPT: 1.06M listings (listed_date < 2022-11-30, USD)
- Post-ChatGPT: 5.0M listings (listed_date ≥ 2022-11-30, < 2025-08-01, USD)
- Working samples: medium (500K/period), small (100K/period)
- Train/val/test: 80/10/10

### Methodology Pipeline
1. Generate buying decision labels B_M with GPT-5-mini (proxy for ChatGPT consumer version)
2. Fine-tune open-weights model (Llama-3.1-8B baseline) as content model g' and null model g
3. Compute pointwise V-information (pvi) for each listing
4. OLS regression: pvi_i = α + β·after_i + ε_i

### Main Result
- **β = 0.143 bits (p < 0.01)** — machine-usable information increased post-ChatGPT
- Temporal dynamics: flat pre-ChatGPT (half-year coefficients ~0); sharp jump post-release; decline; renewed climb after ChatGPT Search (Oct 2024); peak in Jan-Jun 2025

### Robustness Checks

#### 1. Token Pair Variations (Gemma-3-27B labels)
| Token Pair | β | SE | N |
|---|---|---|---|
| SELECT/PASS | 0.143*** | 0.015 | 19,898 |
| YES/NO | 0.123*** | 0.007 | 58,315 |
| BUY/SKIP | 0.100*** | 0.024 | 9,610 |
| PICK/PASS | 0.126*** | 0.019 | 11,189 |

#### 2. Prompt Variations (Gemma-3-27B)
| Prompt | β | SE | N |
|---|---|---|---|
| V1 (Minimal Oracle) | 0.110*** | 0.012 | 15,296 |
| V2 (Output Format) | 0.092*** | 0.018 | 11,078 |
| V3 (Recommendation) | 0.078** | 0.036 | 5,907 |
| V4 (Selective Curator, baseline) | 0.120*** | 0.019 | 12,662 |

#### 3. Labeling Model Variations
| Model | β | SE | N |
|---|---|---|---|
| GPT-5-mini (baseline) | 0.143*** | 0.015 | 19,898 |
| Gemma-3-27B-IT | 0.099*** | 0.009 | 51,033 |
| Qwen3-32B | 0.122*** | 0.019 | 16,429 |

#### 4. Fine-tuning Model Variations
| Model | β | SE | N |
|---|---|---|---|
| Llama-3.1-8B (baseline) | 0.143*** | 0.015 | 19,898 |
| Qwen3-8B | 0.096*** | 0.014 | 19,898 |
| Gemma-3-12B | 0.166*** | 0.017 | 19,898 |

#### 5. Controls
Adding all listing-level controls (price, reviews, rating, discount) depresses β from 0.143 to 0.117 (still p < 0.01).

### Placebo Tests

#### Rephrasing Placebo
- Pre-ChatGPT listings rephrased by GPT-5-mini → β = 0.018 (order of magnitude below baseline)
- Mechanical rephrasing does NOT replicate mecha-nudging information gains

#### DailyMed Control (Pharmaceutical Drug Labels)
- Regulated content with no market incentive to adapt
- β = 0.003 (statistically indistinguishable from zero)
- Rules out generic temporal trend

### Category-Level Interactions (Gemma-3-27B labels)
| Category | β | SE | N |
|---|---|---|---|
| All Categories | 0.1165*** | 0.0077 | 51,033 |
| Pet Supplies | 0.3775** | 0.1478 | 626 |
| Clothing | 0.2904*** | 0.0349 | 2,305 |
| Electronics & Accessories | 0.2479*** | 0.0699 | 633 |
| Bags & Purses | 0.2321*** | 0.0606 | 854 |
| Shoes | 0.2242* | 0.1215 | 193 |
| Accessories | 0.1746*** | 0.0439 | 1,595 |
| Toys & Games | 0.1678*** | 0.0372 | 2,035 |
| Jewelry | 0.1652*** | 0.0234 | 5,344 |
| Home & Living | 0.1146*** | 0.0177 | 13,460 |
| Books, Movies & Music | -0.0411 | 0.0742 | 1,665 |
| Art & Collectibles | 0.0140 | 0.0130 | 16,718 |

**Key pattern**: Effect absent in art/collectibles (where humans are AI-sensitive); stronger in consumer staples.

## Mathematical Framework

### V-Usable Information (Xu et al., 2020; Ethayarajh et al., 2022)

**Definition (V-Usable Information)**: Given predictive family V ⊆ Ω, the predictive V-entropy is:
H_V(Y) = inf_{f∈V} E[-log_2 f[∅](Y)]

The conditional V-entropy is:
H_V(Y|X) = inf_{f∈V} E[-log_2 f[X](Y)]

The V-usable information is:
I_V(X→Y) = H_V(Y) - H_V(Y|X)

**Definition (Pointwise V-Information)**: For instance (x,y) ~ (X,Y):
pvi(x→y) = -log_2 g[∅](y) + log_2 g'[x](y)

where g = arg inf_{f∈V} E[-log_2 f[∅](Y)] and g' = arg inf_{f∈V} E[-log_2 f[X](Y)]

### Mecha-Nudging Design (Definition 3.1)

Let X = (E,U) with E (controllable environment) and U (uncontrollable exogenous characteristics). Let Y_H, Y_M represent decisions for human and machine decision-makers. Let H, M denote predictive families. Let ε be tolerable decrease in human-usable information. Where T is the set of available transformations:

**Constrained Mecha-Nudging Design**:
arg max_{τ∈T} I_M(τ(X)→Y_M)
s.t. I_H(τ(X)→Y_H) ≥ I_H(X→Y_H) - ε

When ε ≥ I_H(X→Y_H), the problem is unconstrained.

### Proposition 1 (Bounded-Receiver Bayesian Persuasion)
Consider a bounded-receiver analog where both choice architect and decision-maker have log-scoring utility log_2(·), and the decision-maker is restricted to predictive family M. Then arg max_{τ∈T} I_M(τ(X)→Y_M) also maximizes the best achievable expected utility for the decision-maker.

**Proof**: Maximizing I_M(τ(X)→Y_M) is equivalent to minimizing H_M(Y_M|τ(X)). By definition of conditional V-entropy, -H_M(Y_M|τ(X)) = sup_{f∈M} E[log_2 f[τ(X)](Y_M)], which is the best expected log-score attainable by a decision-maker restricted to M. ∎

## Token Ablation Results (Top/Bottom ΔPVI words)

### High ΔPVI (increase machine predictability)
| Word | ΔPVI | Selection Freq |
|---|---|---|
| prolific | 0.759 | 96% |
| splitting | 0.741 | 76% |
| qt | 0.708 | 26% |
| happier | 0.682 | 91% |
| junk | 0.636 | 27% |
| snazzy | 0.627 | 10% |
| dripping | 0.617 | 42% |
| accident | 0.614 | 52% |
| relieve | 0.573 | 51% |
| simplistic | 0.538 | 64% |
| oddities | 0.529 | 85% |
| scarce | 0.480 | 83% |
| forged | 0.476 | 74% |
| intimacy | 0.472 | 97% |
| unwanted | 0.469 | 68% |

### Low ΔPVI (decrease machine predictability)
| Word | ΔPVI | Selection Freq |
|---|---|---|
| attracts | -0.996 | 80% |
| fissures | -0.656 | 61% |
| sincere | -0.638 | 37% |
| radiance | -0.570 | 71% |
| cheery | -0.567 | 43% |
| barrier | -0.559 | 39% |
| unfortunate | -0.556 | 59% |
| inflammation | -0.550 | 40% |
| brightest | -0.535 | 65% |
| majesty | -0.530 | 80% |
| favored | -0.465 | 81% |
| spacious | -0.464 | 39% |
| wound | -0.457 | 75% |
| adorns | -0.456 | 66% |

**Pattern**: High ΔPVI words focus on scarcity (scarce, oddities, unwanted) and market value (junk, forged). Low ΔPVI words carry overtly positive affect (cheery, radiance, sincere, favored) — affective copywriting makes machines behave less predictably.

## Human-Side Constraint Validation

Indirect test via marketplace stability:
- Gross Merchandise Sales per active buyer: $117-$136 (trailing 12-month, 2020-2025) — stable
- Repeat buyers: 47-49% of active buyers throughout period — stable
- Buyer survey (eRank): 90%+ say product descriptions very/somewhat important — stable

Conclusion: Human-usable information did not materially decline; consistent with machine-targeted signal additions that are redundant for human buyers.

## Implications for Open-Source AI Market Design

1. **Agent discoverability is a new optimization surface**: Open-source projects can increase their machine-usable information for AI coding agents, recommendation agents, and search agents
2. **The constraint matters**: Optimization must not degrade human-readable documentation quality (the human-usable information constraint)
3. **Scarcity/specificity > affect**: For machine agents, specific factual cues (performance benchmarks, compatibility matrices, dependency specifications) increase predictability more than promotional language
4. **Cultural sensitivity**: Mecha-nudging effectiveness varies by domain (absent in art, strong in staples); open-source project mecha-nudging should be domain-aware
5. **Defense**: Sovereign AI agents need mecha-nudge awareness — the ability to detect when their decision environment has been optimized to influence them