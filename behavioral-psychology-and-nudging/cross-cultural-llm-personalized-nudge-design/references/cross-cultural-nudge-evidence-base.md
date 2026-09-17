# Cross-Cultural LLM-Personalized Nudge Design — Evidence Base

## Primary Source

Maksimenko, Xin, Gupta, Zhang & Bansal (arXiv:2508.12045, 2025) — "Large Language Models Enable Design of Personalized Nudges across Cultures"
- National University of Singapore + Beijing Institute of Technology + Max Planck Institute
- Preregistered (OSF: https://osf.io/u8kyv)
- N=3,495 air travellers across 5 countries

## Study Design

### LLM Simulation Phase
- Model: GPT-4o-mini, temperature 0.8
- 160 unique segments (5 countries × 2 gender × 2 age × 2 income × 2 trust × 2 concern)
- 46 choice situations per segment (1 pairwise + 45 decoy configurations)
- 4 randomized option orders × 25 repetitions = 100 LLM responses per situation
- 3,000 LLM runs per segment; 30 random flight/emission/price scenarios

### Decoy Parameter Space
- Area I (asymmetrically dominated): 35 configurations; price adjustment μ ∈ {0, 0.1, 0.2, 0.3, 0.4, 0.5}
- Area II (non-dominated): 10 configurations; μ ∈ {−0.1, −0.2}; offset reduction ∈ {30%, 40%, 50%, 60%, 70%}
- Total: 45 decoy parameter combinations

### Human Validation Phase
- N=3,495 (China: 713, Germany: 638, India: 714, Singapore: 694, US: 736)
- 5 choice scenarios per respondent (1 no-decoy + 4 decoy conditions)
- Mixed between-within subject design
- Randomized scenario order and option order

## Key Results

### H1: LLM predicts offsetting probability (without decoy)
- Segments predicted to fully offset: 96% CTR vs 81% for non-offsetters
- z = −11.307, p < 0.001, r = 0.187
- Trust and environmental concern are strongest predictors (confirmed in human logistic regression)

### H2: LLM predicts decoy-induced change — REJECTED
- No significant difference between groups LLM predicted to increase vs not increase
- Only Singapore showed marginal significance

### H3: Country-optimal vs country-non-optimal — CONFIRMED
- Country-optimal significantly outperforms country-non-optimal (p < 0.001 in all countries)
- Country-optimal does NOT significantly differ from no-decoy baseline
- The decoy doesn't help on average, but the LLM correctly identifies which configurations are worse

### H4: Personalized segment-optimal vs country-optimal vs no-decoy — CONFIRMED
- Pooled: segment-optimal 0.85 vs country-optimal 0.81 vs no-decoy 0.82
- Significant improvement over both country-optimal (z = −4.610, p < 0.001) and no-decoy (z = −5.253, p < 0.001)

### Country-level breakdown
| Country | Segment-optimal vs no-decoy | Segment-optimal vs country-optimal |
|---|---|---|
| Germany | +7pp (p = 0.00003, r = 0.253) | p = 0.020 (not corrected-significant) |
| Singapore | +7pp (p = 0.00002, r = 0.295) | +7pp (p = 0.0007, r = 0.22) |
| US | +3pp (p = 0.00095, r = 0.184) | +3pp (p = 0.0007, r = 0.18) |
| China | n.s. (p = 0.743) | n.s. |
| India | n.s. (p = 0.901) | n.s. |

### Sceptical travellers (those lacking trust in offset programs)
- Significant increase: 0.79 → 0.82 (z = −5.31, p < 0.001, r = 0.151)
- Country: Germany +7pp, Singapore +8pp, US +4pp; China and India: no effect
- CO₂ mitigation potential: 2.3M tonnes/year additional offsetting

## Cultural Bias Analysis

### Why China and India showed no effect
1. GPT-4o training data contains more Western marketing/behavioral-science examples
2. Internal "default" assumptions mirror US/European patterns for demographic-segment reactions
3. Sparse China/India-specific data → lacks calibrated priors for these cultural contexts
4. Decision-making strategies and personal traits differ from Western countries (Anlló et al. 2024)

### Mitigation strategies
1. Use country-specific LLMs (national models emerging)
2. Fine-tune on small-scale representative data from target country (Binz et al. 2025 showed this works)
3. Always validate empirically — LLM simulations narrow the candidate space but cannot replace real-world testing

## LLM Decoy-Effect Susceptibility

Key finding: LLMs are themselves susceptible to the decoy effect (first demonstration)
- Biases encoded in linguistic patterns of training data
- Reinforced during preference alignment phase
- Implications for agentic commerce (OpenAI ACP): marketing may shift toward nudging LLM agents, not human buyers

## Limitations

- Only GPT-4o tested (Claude, Gemini, Mistral not evaluated)
- Stated preference setting (real-world persistence untested)
- Cognitive processes behind decoy response not understood (eye-tracking, response time, neuroimaging needed)
- Cultural bias attribution: cannot distinguish model assumptions from underlying heterogeneity in responsiveness

## A-Tech Alignment

- **Open-source AI**: Method works with any LLM; open-weight models (Qwen, Llama) could reduce Western bias
- **Data privacy**: No personal data required for LLM simulation; human validation uses stated preference only
- **Financial freedom**: LLM piloting reduces experimental cost by orders of magnitude (160 segments × 3000 runs vs 160 human experiments)
- **Practical implementation**: Preregistered, OSF-published, reproducible pipeline