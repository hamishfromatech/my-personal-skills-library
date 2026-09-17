# Optimal Nudging — Evidence Base

## Primary source

Callaway, F., Hardy, M., & Griffiths, T. L. (2023). "Optimal nudging for cognitively bounded agents: A framework for modeling, predicting, and controlling the effects of choice architectures." *Psychological Review*. DOI: 10.1037/rev0000445. Princeton University (Department of Psychology + Department of Computer Science). Pre-registered; code/data at https://github.com/fredcallaway/optimal-nudging.

## Secondary sources (2026 extensions)

- Santilli, T., Alipour, M., Moghaddam, M.T. (2026). "Designing Adaptive Digital Nudging Systems with LLM-Driven Reasoning." arXiv:2604.11206. Accepted at SAGAI-ICSA2026. Software architecture with LLM-driven cognitive reasoning, 13-architect validation, 15-user proof-of-concept in energy sustainability.
- Costa, S., Mills, S., Duyck, W., Dirix, N. (2026). "Advancing applied behavioral science: the GAP framework." *Humanities and Social Sciences Communications*, 13(1). Integrates General Tools (SHELL), Algorithms (AI), and Practical Considerations (TEAM).
- Lofgren, A. & Nordblom, K. (2020). "A theoretical framework of decision making explaining the mechanisms of nudging." *Journal of Economic Behavior & Organization*, 174, 1-12.

## Core theoretical framework

### Resource-rational analysis

Resource-rational analysis derives cognitive models by assuming people make optimal use of limited computational resources. A cognitive process is the solution to an optimization problem trading off external utility against internal computational cost. This extends bounded rationality (Simon 1955) by providing a mathematical characterization of how an optimal agent navigates constraints.

### Meta-level Markov decision processes (meta-level MDPs)

A meta-level MDP extends the standard MDP formalism to model the sequential decision problem posed by resource-bounded computation:

- **States** S: true state of the world (e.g., feature values X and preference weights w for multi-attribute choice)
- **Actions** A: object-level choices (select one option)
- **Object-level reward** r_object(s, a) = Σ_f w_f · x_{a,f} (utility of chosen option)
- **Beliefs** B: distributions over states — what the agent currently knows
- **Computations** C: cognitive operations (consider one feature of one option) + termination operation ⊥
- **Meta-level transition** T_meta: how computations update beliefs (considering feature x_{a,f} sets μ_{a,f} = true value, σ_{a,f} = 0)
- **Meta-level reward** r_meta: for non-terminating c, r_meta = −cost(c) (λ_{a,f}); for ⊥, r_meta = r_object(s, a*(b)) (true utility of the option with highest expected value given current belief)

The agent at each step chooses: continue deliberating (execute a computation, update a belief, incur cost) or terminate (choose the option with highest expected utility, receive object-level reward). The optimal policy maximizes expected cumulative meta-level reward.

### Meta-greedy policy (one-step lookahead approximation)

Computing the exact optimal policy is intractable for realistic belief-space sizes. The meta-greedy policy (Russell & Wefald 1991) assumes termination on the next step and greedily selects the computation with maximal expected value:

```
π_greedy(b) = argmax_c Q_greedy(b, c)

Q_greedy(b, ⊥) = max_a Σ_f w_f · μ_{a,f}   (expected termination reward)

Q_greedy(b, c_{a,f}) = E[max(V_considered, V_alternative)] − cost(c)
```

where:
- V_considered ~ Normal(mean = current EV of option a, std = w_f · σ_{a,f}) — the option's value after learning feature f
- V_alternative = max_{a'≠a} EV(a') — the best competing option (a constant)
- E[max(X, z)] = P(X ≤ z)·z + P(X > z)·E[X | X > z] (Normal CDF + truncated Normal expectation)

This produces quantitative predictions without free parameters.

## Nudge-to-meta-level-modification mapping

### Default options (Experiment 1)

**Model:** Default nudge changes the agent's initial belief state. The default is assumed to be the option best for the "average" person (w = 1). Agent knows this and integrates the information:
- Default option features: μ+ (higher prior mean), σ+ (lower variance)
- Non-default features: μ− (lower prior mean), σ− (lower variance)

**Predictions:**
1. Presenting an option as default increases probability of choosing it.
2. Effect is larger for more complex decisions (more options/features) — priors matter more when more features are uncovered.
3. Effect is smaller for more idiosyncratic preferences — the default is best for the average person, less so for outliers.
4. Default chosen without deliberation on ~75% of trials; even after deliberation, default more likely chosen because its informational content persists.

**Results (N=298 after exclusions, 9536 trials):**
- Default chosen 89.8% (nudge) vs 55.7% (control); z = 34.77, p < .001
- Significant positive interactions: many-options (z = 5.06), many-features (z = 1.66, p = .049)
- Significant negative interaction: idiosyncrasy (z = −5.87)
- Net earnings higher with default (174.44 vs 164.64 points); t(9534) = 14.60, p < .001
- Unifies "cognitive-effort" (Johnson & Goldstein 2003) and "recommendation" (McKenzie et al. 2006) theories of defaults in one framework.

### Suggested alternatives (Experiment 2)

**Model:** Suggestion = add new option to choice set + immediately reveal its best feature at no cost.

Two variants:
- **Early suggestion (pre-choice):** suggested option present from start with best feature highlighted.
- **Late suggestion (post-choice):** agent makes initial decision, then suggested option revealed with option to switch.

**Predictions:**
1. Suggested options chosen more than chance.
2. Early suggestions more effective than late — early can preclude later deliberation.
3. Effect larger for complex problems (when the model gathers most info and there's room for suggestion to influence).

**Results (N=272, 8160 trials):**
- Suggested option chosen 32.9% vs 16.7% chance; χ²(1) = 1030, p < .001
- Early > late: z = −12.46, p < .001
- Complexity interaction not significant (z = −0.55, p = .293) — model prediction partially supported.

### Information highlighting (Experiment 3)

**Model:** Highlighting reduces cost of considering a feature: λ_highlighted = 1 (was 3).

**Predictions:**
1. Reducing cost → more consideration of highlighted feature.
2. Highlighted feature has greater impact on choice.
3. Agent chooses options higher on the highlighted feature.

**Results (N=88, 2464 trials):**
- Highlighted feature values revealed: 3.20 (nudge) vs 1.89 (control); t(2458.7) = 16.57, p < .001
- Chosen option's highlighted-feature value: 6.29 vs 5.89; t(2457.7) = 5.86, p < .001
- Basket maximizing highlighted feature chosen 67.4% vs 54.4%; χ²(1) = 44, p < .001
- Insight: labeling benefits those who already weight the feature highly; those who don't won't use the label — survey data overestimates label effectiveness.

### Optimal nudge construction (Experiments 4 & 5)

**Method:** Five steps — model decision as meta-level MDP, specify nudge space (e.g., reveal 3 feature values or reduce cost of 3 features), specify objective function g (g_meta = cumulative meta-level reward recommended), specify architect's belief b_arch (may integrate over unknown preferences), optimize via hill climbing.

**Experiment 4 — belief modification (N=250, 7500 trials):**
- Random nudge: 161.3 points
- Heuristic (extreme-value) nudge: 166.1 points
- Optimal nudge: 169.5 points
- Optimal significantly better than both: random t(7497) = −10.55, heuristic t(7497) = −4.42, p < .001
- Optimal improved decision quality (172.8 vs 169.6 vs 165.0) AND reduced decision cost (3.3 vs 3.5 vs 3.7).

**Experiment 5 — cost reduction (N=250, 7500 trials):**
- Optimal: 162.1 points; heuristic: 156.4; random: 154.6
- Optimal significantly better: heuristic t(7497) = −6.73, random t(7497) = −8.87, p < .001
- Even indirect/stochastic cost reduction (no direct belief manipulation) works.

## Digital nudging architecture (Santilli et al. 2026)

### Architecture

Three core processing layers + two cross-cutting modules:
1. **Data Capture:** Context Tracker (device, location, time), Behavioral Collector (clicks, hesitation, navigation, facial emotion)
2. **User Modeling:** Cognitive Mode (System 1 vs 2), Behavioral Stage (Transtheoretical Model: pre-contemplation → maintenance), Attention Capacity (high/medium/low)
3. **Nudge Intelligence:** Strategy Optimizer (constraint-satisfaction selection from 23-strategy taxonomy), Nudge Generator (LLM message creation), UI Adaptation (font size, color, chart type)
4. **Adaptation Module:** Explainability (transparency reports), Adaptive Dashboard, User Feedback
5. **Evaluation Module:** Fairness Monitor, Ethics Compliance, Outcome Tracker

### Architectural decisions
- **AD1:** Sequential pipeline (not event-driven) — deterministic reasoning chains, causal consistency, compliance auditability.
- **AD2:** Side-mounted evaluation (not embedded validators) — separation of concerns; no nudge reaches users without validation.
- **AD3:** LLM-driven reasoning (not rule-based) — handles ambiguous behavioral signals; stochasticity managed with low temperature (0.3).
- **AD4:** Backend-driven UI adaptation — personalization without exposing interaction data to browser.

### 11 quality attributes
Acceptability, awareness, effectiveness, fairness, healthfulness, helpfulness, intrusiveness, motivation, transparency, trustworthiness, user impact.

### Validation
- 13-architect questionnaire: mean 4.62/5; UI Adaptation highest (4.85), Explainability lowest (4.38).
- 15-user proof-of-concept (residential energy): nudge quality 4.73/5, explanation quality 4.27/5, measurable positive emotional impact across all participants.

## Limitations and future directions

1. **Modeling cognitive architecture:** Mouselab externalizes deliberation; real cognitive architectures (noisy evidence accumulation, direct comparison) need integration.
2. **Modeling nudge-to-meta-level mapping:** Defaults-as-recommendation works for public-choice contexts; less clear for commercial contexts with divergent agent/goal interests.
3. **Rationality assumption:** People may not optimally adapt; future work should use more accurate adaptation models (cognitive architectures, strategy selection, instance-based learning).
4. **Personalization:** Inferring preferences from mouse/eye-tracking within a single decision; privacy-preserving integration over unknown preferences.
5. **Self-nudging:** Letting individuals specify the objective or the nudge space — transparency and autonomy to address manipulation/paternalism critiques.

## Cross-references to adjacent skills

| Adjacent skill | Relationship |
|---|---|
| `bottom-nudge-analysis-framework` | BOTTOM provides the mechanism-analysis typology (6 dimensions); Optimal Nudging provides the computational model underneath the mechanisms. |
| `digital-nudging-ethical-persuasion` | Ethical nudging principles; Optimal Nudging operationalizes ethics as architectural guardrails. |
| `nudge-disclosure-transparency-effectiveness` | Transparency of nudges; Optimal Nudging provides Explainability as a first-class architectural component. |
| `ai-behavioral-loop-design` | AI behavioral loop design; Optimal Nudging is the formal model for the nudge-generation loop. |
| `hyper-nudging-ai-personalization-ethics` | Hypernudging (big-data personalization); Optimal Nudging enables privacy-preserving personalization (integrate over unknown preferences). |
| `boosts-vs-nudges-public-preference` | Boosts vs nudges; Optimal Nudging can model both — a boost changes the computational architecture (capability), a nudge changes the meta-level problem (context). |

## Novelty confirmation

Grep across `/home/user/.skills` for "resource-rational", "meta-level MDP", "meta-greedy" returned no matches. The existing 35+ behavioral-psychology-and-nudging skills cover nudge theory, transparency, boosts vs nudges, ethical persuasion, behavior-change synthesis, BOTTOM mechanism analysis, and GAP framework — but none provides the computational/meta-level-MDP framework that models nudges as modifications to the meta-level problem, enables parameter-free quantitative predictions, and automates optimal nudge construction. This is a new, computationally foundational skill that sits above the typology and ethics skills.