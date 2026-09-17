---
name: value-dependent-empathy-aigmc-disclosure-paradox
description: Applies the first framework demonstrating that AI disclosure has a paradoxical dual role depending on content value type — amplifying cognitive empathy for functional content while attenuating affective empathy for hedonic content. Use when designing AI disclosure strategies, deciding when to label AI-generated marketing content, or optimizing content value types for AI-mediated persuasion.
---

# Value-Dependent Empathy-Mediated AIGMC with AI Disclosure Paradox

## Overview
This skill applies the first systematic evidence that AI disclosure is not a uniformly negative or positive factor — it acts as a "competence label" that amplifies cognitive empathy for functional content while acting as an "emotional authenticity reducer" that attenuates affective empathy for hedonic content. This reframes the AI transparency question from "whether to disclose" to "in which context and for what purpose."

## When to Use
- Designing AI disclosure strategies for marketing content (when to label, when not to)
- Deciding content value type (functional vs hedonic) for AI-generated marketing
- Optimizing human-AI collaboration frameworks for content production
- Evaluating the interaction between content credibility and content value
- NOT for: situations where AI disclosure is legally mandatory regardless of effectiveness

## Core Process / Workflow

### 1. Determine Content Value Type

Classify your AI-generated marketing content as:

- **Functional value**: Practical, problem-solving, instrumental information (product specs, tutorials, reviews, how-to guides)
- **Hedonic value**: Emotional, experiential, pleasure-oriented content (brand storytelling, entertainment, aspirational narratives)

### 2. Understand the Empathy Pathway

Each content type activates a distinct empathy pathway to engagement:

```
Functional Value → Cognitive Empathy → Customer Engagement
Hedonic Value → Affective Empathy → Customer Engagement
```

**Cognitive empathy**: Understanding others' mental states, perceiving brand values, alignment with product understanding
**Affective empathy**: Sharing emotional experiences, feeling part of the brand story, emotional resonance

### 3. Apply the AI Disclosure Paradox

AI disclosure has **opposite effects** depending on content value:

| Content Value | AI Disclosure Effect | Mechanism |
|---|---|---|
| Functional | **AMPLIFIES** cognitive empathy (β=+0.306, p<.01) | AI perceived as competent, objective, professional |
| Hedonic | **ATTENUATES** affective empathy (β=-0.245, p<.01) | AI perceived as lacking emotional depth, disrupting authenticity |

### 4. Design the Value-Congruent Disclosure Strategy

**For functional AIGMC (product specs, tutorials, reviews):**
- **Explicitly highlight AI technology labels** — disclosure strengthens cognitive empathy
- Use objective, data-driven language
- Emphasize AI's competence in data integration and processing
- Consumers associate AI with professionalism and logical rigor

**For hedonic AIGMC (brand storytelling, entertainment):**
- **Omit AI labels or indicate human-AI collaboration** — disclosure weakens affective empathy
- Embed authentic emotional elements
- Integrate genuine user-generated content (photos, videos, reviews) to compensate for AI's emotional deficiencies
- Use human creators for emotional nuance and cultural sensitivity

### 5. Handle Content Credibility Interaction

Credibility interacts with content value asymmetrically:

- **Low credibility + functional content**: Significantly weakens engagement (credibility is the "cornerstone" and "bottleneck" for central-route processing)
- **Low credibility + hedonic content**: Less damaging (hedonic value can sustain engagement even with reduced credibility — "persuasion knowledge" tolerance)

**Practical rule:** Prioritize credibility verification for functional content; prioritize emotional authenticity for hedonic content.

### 6. Build Human-AI Collaboration Framework

```
Content Creation Phase:
  AI handles: data aggregation, precise descriptions, consistency checks
  Humans handle: emotional nuance, cultural sensitivity, authenticity

Quality Control Phase:
  AI handles: consistency checks, metric prediction (CTR, share rates)
  Humans handle: authenticity verification, empathy assessment

Performance Optimization Phase:
  AI handles: click-through and sharing rate prediction
  Humans handle: empathy-driven engagement assessment
```

### 7. Select Engagement Metrics by Content Type

| Content Type | Primary Metric | Secondary Metric |
|---|---|---|
| Functional | Comprehension, willingness to recommend | Cognitive empathy indicators |
| Hedonic | Sharing rate, emotional resonance intensity | Brand favorability |

## Key Evidence

**Source:** Gao, Li & Zhao, Harbin University of Commerce, Frontiers in Psychology, January 2026
- Two between-subjects experiments (Study 1: N=152; Study 2: N=186)
- Study 1: 2×2 (functional vs hedonic × high vs low credibility) — main and interaction effects
- Study 2: 2×2 (functional vs hedonic × AI disclosure present vs absent) — moderated mediation
- ELM (Elaboration Likelihood Model) + CAPS (Cognitive-Affective Processing System) framework
- PROCESS Model 7 with 5,000 bootstrap samples

**Key statistical results:**
- Hedonic value → higher emotional engagement (M=6.28 vs 4.86, p<.001) and behavioral engagement (M=5.92 vs 4.55, p=.003)
- High credibility → stronger engagement across all dimensions (cognitive p<.001, emotional p<.001, behavioral p<.001)
- Significant interaction: F(1,148)=18.09, p<.001, η²=.109
- AI disclosure amplifies functional→cognitive empathy (β=0.650 with disclosure vs 0.403 without)
- AI disclosure attenuates hedonic→affective empathy (β=0.255 with disclosure vs 0.501 without)

## A-Tech Alignment

- **Open-source:** Applies to AI-generated marketing for OSS products (open-weight models can generate both functional documentation and hedonic brand content)
- **Data privacy:** Behavioral proxies (self-report engagement) over biometric surveillance
- **Financial freedom:** Optimizes content production costs by matching AI vs human effort to content type
- **Practical implementation:** Clear decision framework — disclose for functional, hide for hedonic

## References
- See [references/evidence-base.md](references/evidence-base.md) for full experimental details, statistical results, and theoretical framework.