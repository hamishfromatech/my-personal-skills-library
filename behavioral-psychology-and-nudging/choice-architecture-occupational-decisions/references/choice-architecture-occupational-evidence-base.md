# Choice Architecture in Occupational Choices — Evidence Base

## Study Design

### Setting
- **Platform**: Yousty, Switzerland's largest private online job board for apprenticeship positions
- **Market coverage**: ~90% of all Swiss apprenticeship positions posted
- **Population**: Swiss adolescents (age 15-16) making their first occupational choice after compulsory schooling
- **Decision stakes**: 3-4 year apprenticeships setting career trajectory; ~240 occupations to choose from
- **Sample**: 246,869 users who completed the occupation interest test between 2019 and 2024

### Why This Setting
- First occupational choice = unconfounded by prior work experience
- High stakes and long-term consequences
- Complex information environment (240 occupations)
- Strong identity concerns (gendered occupational norms)
- Near-complete market coverage = ecological validity

## Data and Variables

### Match Score and Rank
- Users complete a 33-question interest test
- Platform recommends 20 best-matched occupations with match scores
- When multiple occupations share identical match scores, rank is assigned based on a random occupation identifier
- This creates exogenous variation in rank among equally well-matched occupations

### Outcome Variables
1. **Activity**: Count of distinct actions (viewing job ads, viewing profile, saving to watch list, applying to trial apprenticeship, applying to apprenticeship)
2. **Watch List**: Binary indicator for saving any occupation; count of occupations saved (conditional on saving ≥1)
3. **Apply**: Binary indicator for applying to any apprenticeship; count of distinct occupations applied to

### Summary Statistics
| Variable | N | Mean | SD | Min | Max |
|----------|------|------|-----|-----|-----|
| Occupations applied to | 246,869 | 0.095 | 0.401 | 0 | 12 |
| Saved any occupations | 246,869 | 0.189 | 0.392 | 0 | 1 |
| Occupations on watch list (conditional) | 46,680 | 4.381 | 3.416 | 1 | 20 |

Only 7.1% of users apply to any apprenticeship in a given period. Application outcome is highly zero-inflated with a long right tail.

## Empirical Strategy

### Rank Effect Identification

**Baseline specification (Eq. 1)**:
```
Y_ij = β · Rank_ij + f(Score_ij) + α_i + γ_j + ε_ij
```
Where:
- `f(Score)`: flexible match score controls (score-bin fixed effects)
- `α_i`: individual fixed effects (absorb all person-specific characteristics)
- `γ_j`: occupation fixed effects (baseline popularity differences)
- Identification: within-person comparisons across occupations differing in rank but with similar match scores

**Within-tie specification (Eq. 2)**:
```
Y_ij = β · Rank_ij + α_is + γ_j + ε_ij
```
Where `α_is` = individual-score fixed effects. Identifies solely from differences in rank among occupations with the same match score for the same user.

**Within-tie ordering (Eq. 3)**:
Replaces overall rank with the order among score-tied occupations, controlling for overall rank position fixed effects. Isolates display order orthogonal to both score and absolute position.

### Redesign Effect Identification

**Difference-in-discontinuity (DD-RD) design**:
- Running variable: days relative to June 1st in each calendar year
- Compares discontinuity at June 1, 2023 (redesign date) vs. same date in non-treated years (2020-2022, 2024)
- Redesign date was unknown to users and never announced → prevents manipulation
- Local linear regressions on either side of cutoff, symmetric bandwidths

## Results

### Rank Effect (Motivated Reasoning)

**Baseline (Table 2)**: Moving an occupation down one rank reduces:
- Activity: -0.0047 to -0.0049 (p<0.01)
- Watch list: -0.0042 (p<0.01)
- Apply: -0.0002 (p<0.05)

Consistent across baseline, within-tie, and within-tie ordering specifications.

**Heterogeneity by predicted earnings (Table 4)**:
- Rank × Top Earnings Quartile interaction: -0.0073 to -0.0079 (p<0.01)
- Rank effect at least doubles for high-earning occupations
- Consistent with motivated reasoning: users place greater weight on rank when it reinforces ex ante more desirable choices

**Heterogeneity by gender typicality (Table 5)**:
- Rank × Male × Female-Occupation interaction: +0.0044 to +0.0046 (p<0.01)
- Rank effects muted for gender-atypical occupations
- Consistent with identity-driven motivated reasoning

**Decomposition by preference congruence (Table 6)**:
- Gender-congruent preference subsample: strong attenuation for gender-atypical occupations (0.0074 interaction)
- Gender-non-congruent subsample: markedly weaker rank effects (0.0027 interaction)
- Confirms rank is selectively used to justify psychologically/socially acceptable choices

### Redesign Effect (Cognitive Load)

**Applications (Table 7, DD-RD)**:
| Bandwidth | Estimate | SE |
|-----------|----------|-----|
| 120 days | 0.032** | 0.012 |
| 90 days | 0.033** | 0.014 |
| 60 days | 0.036** | 0.018 |
| 30 days | 0.052** | 0.024 |

Sizable relative to average discontinuity of 0.085 in control years.

**Watch list mechanism (Table 8, DD-RD)**:
| Bandwidth | Saved Any | Occupations Saved (conditional) |
|-----------|-----------|-------------------------------|
| 120 days | 0.444*** | 1.737*** |
| 90 days | 0.433*** | 1.818*** |
| 60 days | 0.387*** | 2.015*** |
| 30 days | 0.345*** | 1.704*** |

Large, persistent increase in watch list usage explains the application increase. Reducing cognitive load → more saving → more applications.

## Robustness

- Alternative control years (Tables A2-A3)
- Including transition month of May (Tables A4-A6)
- Traditional sharp RD without seasonality adjustment (Table A7)
- Non-parametric rank specifications (Figure A6)
- Quadratic rank heterogeneity (Table A1)
- All confirm main findings.

## Limitations

1. **Single platform**: Results from Swiss VET system may not generalize to all labor markets
2. **Application vs. enrollment**: Measures applications, not final enrollment or labor market outcomes
3. **Short-run effects**: Does not track long-term career outcomes from broadened search
4. **Gender-typicality measure**: Based on predicted gender from interest test; alternative operationalizations possible
5. **Redesign confound**: June 1 date may coincide with other seasonal patterns (addressed via DD-RD with control years)

## Theoretical Implications

1. **Laboratory-to-field generalization**: Motivated reasoning and cognitive load, previously documented primarily in labs, operate in high-stakes real-world decisions with identity concerns and long-term consequences.
2. **Behavioral mechanisms in matching markets**: Platform design features interact with behavioral tendencies in ways that shape allocation and potentially inequality.
3. **Salience vs. information**: Rank effects are not purely mechanical salience; users interpret rank as an informational cue whose relevance varies with context (motivated reasoning).
4. **Cognitive load as search bottleneck**: Information overload restricts search; reducing load broadens consideration sets and improves match quality.

## Policy Implications

1. Public career guidance platforms should audit ranking algorithms for identity-driven bias amplification
2. Interactive, one-at-a-time presentation can broaden occupational search among young people
3. Watch list / save-for-later features are not just convenience tools — they are cognitive load mitigation mechanisms that increase search breadth
4. Platform design is not neutral; it actively structures decision environments and can amplify or mitigate behavioral biases