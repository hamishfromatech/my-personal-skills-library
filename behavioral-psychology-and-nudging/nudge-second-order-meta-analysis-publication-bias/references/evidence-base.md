# Evidence Base: Nudge Second-Order Meta-Analysis

## Primary Source

**Hu, B., Xia, Z., Guo, Q., Lu, C., Constantino, S. M., & Ju, X. (2025).** Assessing Nudge Impact: A Comprehensive Second-Order Meta-Analysis. *Journal of Behavioral Decision Making*, 38(5), e70053. https://doi.org/10.1002/bdm.70053

- Preregistered: https://osf.io/nmpqv
- Data and analysis scripts: https://osf.io/ugnf9/

## Study Design

### Scope
- 13 articles containing 14 meta-analyses
- 1,638 primary studies
- ~30 million participants
- Second-order meta-analysis (meta-analysis of meta-analyses)

### Methodology
- Robust Bayesian model-averaged meta-analysis (RoBMA)
- Publication bias adjustment via selection models, PET-PEESE, and robust Bayesian methods
- AMSTAR-2 quality assessment of included meta-analyses

## Key Results

### Effect Sizes
| Metric | Value |
|--------|-------|
| Aggregated effect size (raw) | d = 0.27 (95% CI [0.16, 0.38]) |
| Effect size after publication bias adjustment | d = 0.004 |
| AMSTAR-2 quality ratings | Most rated low or critically low |

### Interpretation
- The raw aggregated effect (d=0.27) is small by conventional standards
- After adjusting for publication bias, the effect drops to d=0.004 — statistically indistinguishable from zero
- The authors note that most included meta-analyses were rated as low or critically low quality on AMSTAR-2, suggesting findings should be interpreted with caution

## Context: The Nudge Effectiveness Debate

### Supporting Evidence (Larger Effects)
- **Mertens et al. (PNAS 2022)**: d=0.43 across 447 effect sizes, 2.1M participants. Found decision structure nudges consistently outperform decision information and decision assistance nudges. Food domain showed 2.5x larger effects. Acknowledged moderate publication bias.
- **Hummel & Maedche (2019)**: Median effect 21%, 62% of treatments statistically significant. Defaults most effective, precommitment least effective.

### Null/Contradictory Evidence
- **Maier et al. (PNAS 2022)**: "No evidence for nudging after adjusting for publication bias" — d≈0 after bias correction
- **This study (Hu et al. 2025)**: Most comprehensive synthesis to date, d=0.004 after bias adjustment

### The Convergence
Both Maier et al. (2022) and Hu et al. (2025) converge on the same conclusion: after rigorous publication bias adjustment, the average nudge effect is effectively zero. The Mertens et al. (2022) finding of d=0.43 is consistent with these results when one accounts for the moderate publication bias they acknowledged.

## Domain Variation (from Mertens et al. 2022, retained even after bias adjustment)

| Domain | Effect Size (Mertens) | Notes |
|--------|----------------------|-------|
| Food | d = 0.65 | 2.5x larger than other domains |
| Environment | d = 0.43 | |
| Prosocial | d = 0.41 | |
| Health | d = 0.34 | |
| Other | d = 0.31 | |
| Finance | d = 0.24 | Smallest effects |

## Intervention Category Variation (from Mertens et al. 2022)

| Category | Effect Size | Mechanism |
|----------|-------------|-----------|
| Decision structure | d = 0.54 | Defaults, effort, composition changes |
| Decision information | d = 0.34 | Translation, visibility, social reference |
| Decision assistance | d = 0.28 | Reminders, commitment devices |

Decision structure nudges consistently outperform decision information (b=0.19, p=0.001) and decision assistance (b=0.26, p<0.001) nudges.

## Implications for A-Tech

### Investment Decision Framework
1. **Default to skepticism**: Treat unadjusted nudge effect sizes as upper bounds
2. **Bias-adjustment lens**: Apply d≈0 as the prior for pure choice architecture
3. **Domain sensitivity**: Food and environmental domains may retain meaningful effects
4. **Intervention type**: Decision structure (defaults) > decision information > decision assistance
5. **Cost-benefit**: Only invest developer time in nudges when marginal cost is near zero

### What This Does NOT Invalidate
- Specific, well-designed nudges in responsive domains (food, environment)
- Nudges combined with other interventions (incentives, education)
- Technology adoption channel (Brandon et al. — ~50% persistence via durable artifact adoption)
- LLM-personalized nudges (Li et al. — preregistered RCT, 18.3pp advantage)
- Mecha-nudges for machines (Frey & Ethayarajh — robustness-checked natural experiment)

### What This DOES Invalidate
- The claim that nudging has a robust, generalizable average effect across domains
- Investment in nudge optimization expecting d=0.27 returns
- Product strategies built solely on choice architecture without complementary interventions

## Relationship to Existing A-Tech Skills

### Skills That Retain Validity
- `nudge-persistence-technology-adoption` — Identifies a specific mechanism (technology adoption), not just an average effect; ~50% persistence is a structural finding
- `llm-iterative-personalized-nudging` — Preregistered RCT with specific mechanism (LLM personalization)
- `mecha-nudges-for-machines` — Different target (AI agents), robustness-checked
- `ai-motivational-interviewing-scale` — RCT with specific mechanism (MI protocols)
- `nudge-transparency-disclosure-effectiveness` — Specific finding about disclosure effects, not average nudge effectiveness

### Skills That Should Be Calibrated
- Any skill recommending nudge-based interventions should include the bias-adjustment caveat
- Product design skills that assume nudge effectiveness should reference this reality check

## Open Questions

1. **Domain-specific effects**: Do food and environmental nudges retain meaningful effects after bias adjustment? (Hu et al. did not break down by domain)
2. **Decision structure vs. information**: Does the structural advantage survive bias adjustment?
3. **Preregistered studies**: Do preregistered nudge experiments show larger effects than non-preregistered?
4. **Combined interventions**: What is the bias-adjusted effect of nudges combined with incentives?
5. **Long-term effects**: Does the bias-adjustment picture change for long-term follow-up studies?

## A-Tech Alignment

- **Open-source AI**: Preregistration and open data are the behavioral science equivalent of open source — they prevent selective disclosure
- **Data privacy**: Publication bias is selective disclosure of results; the research equivalent of hiding data
- **Financial freedom**: Prevents over-investment in ineffective interventions; redirects resources to durable strategies
- **Practical implementation**: The bias-adjustment framework is immediately actionable