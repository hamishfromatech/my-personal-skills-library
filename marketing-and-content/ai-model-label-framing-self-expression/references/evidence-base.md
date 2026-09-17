# Evidence Base: AI Model Label Framing Effect on Consumer Self-Expression

## Source
Cheng, X. & Nam, I. (2026). Understanding the influence of AI labels on consumer psychology: the moderating role of product type. *Frontiers in Psychology*, 17:1878095. doi:10.3389/fpsyg.2026.1878095

## Theoretical Framework

### Emphasis Framing (Chong & Druckman, 2007)
The AI model label functions as an emphasis-framing cue that:
- Brings the otherwise less prominent model-source cue to the foreground
- Directs greater attention to whether the model is a real human
- Increases the weight consumers assign to source information in product evaluation

### Algorithm Aversion (Dietvorst et al., 2015)
- Individuals are less willing to rely on or trust algorithmic decisions vs human
- Once aware content is AI-generated, evaluations systematically decline
- Greater sensitivity to potential errors

### Uncanny Valley (Mori, 1970; Ho & MacDorman, 2010)
- Eeriness arises from the blurred boundary between human and non-human
- Even without visual imperfections (AI images now indistinguishable), the label triggers categorization difficulty
- Cognitive conflict from coexistence of humanlike and non-human characteristics

### Affect-as-Information (Clore et al., 2012)
- Consumers use current emotional states as cues in evaluating external objects
- Eeriness interpreted as a signal that something may be wrong with the product
- Elevates perceived risk (Sjöberg, 2007)

## Experimental Design

### Design
2 (model presentation source: AI vs human) × 2 (product type: symbolic vs functional) between-subjects

### Participants
- N=200 valid responses (MTurk, US residents)
- 70% female (contextually relevant — stimuli featured female models in women's apparel)
- 69.5% in their 30s; 74% bachelor's degree; 38.5% $50K-$75K monthly income; 91% full-time

### Stimuli
- Products: dress (symbolic) and T-shirt (functional), both black
- Images generated using Dreamina AI
- Identical images across AI and human conditions — only the label differed
- Label: "displayed by an AI-generated model" vs "displayed by a human model"

### Measures (all validated, CFA-confirmed)
| Construct | Items | α | CR | AVE |
|---|---|---|---|---|
| Eeriness | 3 | .935 | .936 | .830 |
| Perceived psychological risk | 3 | .942 | .942 | .845 |
| Perceived performance risk | 3 | .912 | .912 | .776 |
| Self-expression | 8 (reverse-coded) | .969 | .969 | .798 |
| Product symbolism | 4 | .910 | .911 | .718 |
| Product functionality | 4 | .868 | .870 | .626 |

### Discriminant Validity
- Four-factor model fit: χ²/df=2.413, CFI=.964, NFI=.940, TLI=.957, RMSEA=.084
- All pairwise constrained correlations significantly worse than unconstrained (all p<.001)

## Results

### Main Effect (H1)
- Self-expression: AI=3.12 (SD=1.62) vs Human=4.54 (SD=1.70), t(198)=-6.03, p<.001

### Mediation Effects (PROCESS Model 81)

| Path | β | t | p | 95% CI |
|---|---|---|---|---|
| AI Label → Eeriness | 1.203 | 4.621 | <.001 | — |
| Eeriness → Self-Expression | -0.060 | -2.328 | <.05 | (-0.166, -0.002) |
| AI Label → Psych Risk | 0.450 | 2.404 | <.05 | — |
| Psych Risk → Self-Expression | -0.665 | -18.575 | <.001 | (-0.594, -0.040) |
| AI Label → Perf Risk | 0.508 | 2.651 | <.01 | — |
| Perf Risk → Self-Expression | -0.219 | -6.264 | <.001 | (-0.240, -0.020) |
| Serial: EE→PPR→SE | — | — | — | (-0.874, -0.299) |
| Serial: EE→PPER→SE | — | — | — | (-0.243, -0.060) |

### Moderation (Product Type)

| Condition | Eeriness | Psych Risk | Perf Risk | Self-Expression |
|---|---|---|---|---|
| **Symbolic (Dress)** | | | | |
| AI | 5.91 | 5.99 | 6.01 | 1.97 |
| Human | 3.97 | 3.67 | 4.13 | 4.37 |
| p | <.001 | <.001 | <.001 | <.001 |
| **Functional (T-shirt)** | | | | |
| AI | 4.01 | 3.63 | 4.20 | 4.27 |
| Human | 3.54 | 3.33 | 3.77 | 4.71 |
| p | .169 | .354 | .149 | .137 |

All interactions significant: Eeriness F(1,196)=9.05 p=.003 η²=.044; Psych Risk F(1,196)=19.55 p<.001 η²=.091; Perf Risk F(1,196)=11.80 p<.001 η²=.057; Self-Expression F(1,196)=23.10 p<.001 η²=.105

## Theoretical Contributions

1. **Product-spillover mechanism of AI labeling**: First study to show that negative perceptions triggered by an AI-generated presentation subject shape subsequent product-related judgments (not just evaluation of the AI content itself)

2. **Algorithm aversion extended to emotional responses**: Introduces eeriness as a first-stage mediator connecting algorithm aversion to risk perceptions and behavioral outcomes

3. **Product type as boundary condition**: Clarifies when AI labeling matters (symbolic) and when it does not (functional), resolving inconsistencies in prior literature

## Practical Implications

### Regulatory Compliance Context
- EU AI Act, California AI Transparency Act, China's Measures for Labeling of AI-Generated Synthetic Content all push toward mandatory AI disclosure
- This study shows disclosure can heighten eeriness, perceived risk, and reduce self-expression
- Firms should not treat AI labels as purely formal compliance statements

### Labeling Design Recommendations
1. Carefully design wording, placement, and contextual explanation
2. Assign AI-generated models names and backstories to enhance perceived social presence
3. Provide body measurements alongside product images
4. Position AI models as brand-affiliated representatives or quasi-employees
5. Reduce perceived uncertainty through detailed material descriptions, close-ups, user reviews, real usage scenarios
6. Strengthen brand trust signals: endorsements, quality guarantees, after-sales commitments

## Limitations
- Scenario-based experiment; product-type manipulation used dress vs T-shirt (may differ in hedonic value, formality, femininity)
- Sample was US MTurk (70% female); generalizability across cultures untested
- Focus on eeriness as negative emotion; positive emotions (novelty, curiosity) not examined
- Self-expression as focal outcome; other outcomes (purchase intent, brand attitude) not measured
- Cultural factors (uncertainty avoidance), brand characteristics (trust, personality) not explored

## Cross-References to Existing Skills
- `ai-generated-ad-pretesting-effectiveness`: Complementary — shows AI disclosure labels have NO effect under low-involvement pretesting conditions, whereas this study shows labels DO matter for symbolic product displays
- `promotional-preventive-framing-erp-neuromarketing`: Both examine AI-generated marketing content framing effects; this skill extends to the labeling/framing of the source itself
- `neuromarketing-paradox-cognitive-privacy`: Both address the tension between personalization effectiveness and consumer autonomy/privacy
- `cognitive-targeting-ai-advertising`: Both examine AI's role in advertising and consumer response