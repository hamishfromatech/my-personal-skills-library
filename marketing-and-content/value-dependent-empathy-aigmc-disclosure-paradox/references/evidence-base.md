# Evidence Base: Value-Dependent Empathy-Mediated AIGMC with AI Disclosure Paradox

## Source
Gao, X., Li, W. & Zhao, Y. (2026). Value-dependent and empathy-mediated: how artificial intelligence-generated marketing content influences customer engagement, and when to disclose its origin. *Frontiers in Psychology*, 16:1701085. doi:10.3389/fpsyg.2025.1701085

## Theoretical Framework

### Elaboration Likelihood Model (ELM)
- Central route: content value (functional vs hedonic) — deep processing of message arguments
- Peripheral route: content credibility — heuristic evaluation of source trustworthiness
- Functional content → central route (credibility is "cornerstone" and "bottleneck")
- Hedonic content → peripheral route can sustain engagement even with low credibility

### Cognitive-Affective Processing System (CAPS)
- Consumer behavior emerges from interplay of cognitive and affective responses
- Different situational cues (content value types) activate distinct cognitive-affective units
- Functional value → cognitive empathy pathway
- Hedonic value → affective empathy pathway

### Empathy Dimensions
- **Cognitive empathy**: Understanding others' mental states, intentions, beliefs, needs; perceiving brand values; interpreting values
- **Affective empathy**: Sharing emotional experiences; feeling relaxed, interested, part of the brand story

## Experimental Design

### Study 1: Content Value × Credibility Interaction

**Design:** 2×2 between-subjects (functional vs hedonic × high vs low credibility)
**Context:** Fictional skincare brand "Dewphoria" on Weibo
**Participants:** N=152 (91 female, 61 male), aged 18-30, Chinese university students

**Pre-test (N=40):**
- Functional/hedonic manipulation: M_HV=5.08 vs M_FV=3.12, t(18)=3.81, p=.001, d=0.82
- Credibility manipulation: M_HC=4.13 vs M_LC=3.35, t(18)=3.79, p=.001, d=0.72

**Results (Two-way ANOVA):**

| Effect | F | p | η² |
|---|---|---|---|
| Content value (main) | 15.57 | <.001 | .095 |
| Credibility (main) | 20.66 | <.001 | .122 |
| Interaction | 18.09 | <.001 | .109 |

**Simple effects:**
- Low credibility: Hedonic > Functional (M=5.64 vs 3.66, t(36)=4.15, p<.001, d=0.87)
- High credibility: No significant difference (M=5.96 vs 5.14, t(36)=0.92, p=.361, d=0.19)

### Study 2: Moderated Mediation with AI Disclosure

**Design:** 2×2 between-subjects (functional vs hedonic × AI disclosure present vs absent)
**Participants:** N=186 (107 female, 79 male), aged 18-35

**AI Disclosure Manipulation:**
- Present: "This ad was developed using AI" label
- Absent: No label

**Measures (all α > .83):**
- Customer engagement: α=.87 (like, share, comment intentions)
- Cognitive empathy: α=.84 (4 items: information alignment, brand value perception, need understanding)
- Affective empathy: α=.83 (4 items: feeling relaxed, interested, part of brand story)

**Results (PROCESS Model 7, 5,000 bootstrap samples):**

| Path | β | 95% CI | Supported? |
|---|---|---|---|
| Functional → Cognitive Empathy → Engagement | — | (0.243, 0.615) | Yes |
| Hedonic → Affective Empathy → Engagement | — | (0.341, 0.730) | Yes |
| AI Disclosure × Functional → Cognitive Empathy | +0.306 | p<.01 | Yes (amplifies) |
| AI Disclosure × Hedonic → Affective Empathy | -0.245 | p<.01 | Yes (attenuates) |

**Conditional effects of AI disclosure:**
- Functional content with disclosure: β=0.650 (CI: 0.568, 0.862)
- Functional content without disclosure: β=0.403 (CI: 0.221, 0.691)
- Hedonic content with disclosure: β=0.255 (CI: 0.105, 0.418)
- Hedonic content without disclosure: β=0.501 (CI: 0.357, 0.651)

## Theoretical Contributions

1. **Context-dependent AI disclosure effect**: First systematic demonstration that AI disclosure impact is fundamentally context-dependent — not a single-valence variable
2. **Dual empathy pathways**: First in AIGMC field to introduce parallel cognitive and affective empathy mediation, bridging ELM with CAPS
3. **Asymmetric credibility role**: Credibility is "cornerstone" for functional content (central route) but less critical for hedonic content (peripheral route can sustain engagement)
4. **"Competence label" vs "emotional authenticity reducer"**: AI disclosure acts as a competence label for functional content (amplifying cognitive empathy) but as an emotional authenticity reducer for hedonic content (attenuating affective empathy)

## Managerial Implications

### Content Strategy
1. **Functional AIGMC**: Leverage AI strengths in data integration; explicitly highlight AI technology labels
2. **Hedonic AIGMC**: Focus on embedding emotional elements; omit AI labels or indicate human-AI collaboration
3. **Credibility investment**: Prioritize credibility for functional content; invest in emotional authenticity for hedonic content

### Human-AI Collaboration
- AI: data aggregation, precise descriptions, consistency checks, CTR/share prediction
- Humans: emotional nuance, cultural sensitivity, authenticity, empathy assessment

### Engagement Metrics
- Functional: comprehension, willingness to recommend, cognitive empathy indicators
- Hedonic: sharing rate, emotional resonance intensity, brand favorability

## Limitations
- Chinese social media context (Weibo) — cross-cultural validity untested
- Cosmetics industry only — generalizability to other sectors untested
- Functional/hedonic treated as unidimensional (future: subdivide hedonic into positive/negative)
- Pre-existing trust in AI not measured as moderator
- Scenario-based experiments — field experiments with multimodal measurement (eye-tracking, GSR) recommended for future work

## Cross-References to Existing Skills
- `ai-model-label-framing-self-expression`: Both examine AI labeling effects; this skill focuses on content value type × disclosure interaction, while the other focuses on product type × AI model label interaction
- `ai-generated-ad-pretesting-effectiveness`: Both examine AI-generated content effectiveness; this skill provides the disclosure paradox mechanism, while the other shows pretesting equivalence under specific conditions
- `promotional-preventive-framing-erp-neuromarketing`: Both examine AI-generated content framing; this skill's functional/hedonic distinction complements the promotional/preventive framing distinction
- `topic-stereotype-semantic-anchoring-llm-persuasion`: Both examine LLM persuasion mechanisms; this skill's empathy pathway complements the semantic anchoring framework
- `neuromarketing-paradox-cognitive-privacy`: Both address the tension between personalization effectiveness and consumer autonomy