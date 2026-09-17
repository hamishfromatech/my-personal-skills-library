# TIDEs Technostress-Aware IDEs — Evidence Base

## Source
Radan, S. (2026). "TIDEs: Technostress-Aware Integrated Development Environments." Doctoral thesis, Birmingham City University, School of Architecture, Built Environment, Computing and Engineering. ORCID: 0000-0001-5465-0920. Open access: https://www.open-access.bcu.ac.uk/17105/

## The Five Technostressors (formal definitions from technostress literature)

| Stressor | Definition |
|---|---|
| Techno-overload | Technology forces users to work faster or process excessive information |
| Techno-complexity | Systems are perceived as difficult to understand or use |
| Techno-insecurity | Users feel threatened about their competence or fear being replaced due to insufficient technical skills |
| Techno-uncertainty | Constant updates or unclear system behaviour create instability and unpredictability |
| Techno-invasion | Technology intrudes into personal space, disrupts workflow, or creates a sense of constant connectivity |

## Research Motivation (Contextual Data)

- HESA 2019/20: 7.7% of Computing undergraduates did not continue after first year, vs 5.3% across all subjects
- Student attrition is multifactorial, but computing's higher rate suggests distinctive academic/cognitive pressures
- Programming-environment challenges and technostress-related factors are one possible contributing influence
- Despite increasing recognition of technostress in broader technological contexts, technostress within programming environments remains insufficiently examined as a measurable and design-addressable phenomenon

## Framework Contributions

### C1: Taxonomy
Establishes a taxonomy exploring the intersection between technostress, mental health, and assistive technology research — systematically synthesised and categorised.

### C2: Derived Metrics
Three metrics to measure the effects of design decisions on user technostress levels:
- **Performance**: task completion rates, accuracy, time-on-task
- **Productivity**: throughput, sustained engagement, efficiency
- **Satisfaction**: subjective experience, perceived ease of use, emotional state

### C3: Iterative IDE Enhancement Framework
Operationalises the metrics by integrating:
- User feedback (qualitative + quantitative)
- Biometric data (emotional + cognitive signals)

to iteratively refine IDEs in response to emotional and cognitive needs, with particular focus on technostress.

## Key Findings

### Biometric/Emotional Results
- Positive emotional markers (joy, engagement) correlate with higher task success
- Negative affect is linked to underperformance
- Emotional trajectories improved under the modified IDE
- Design interventions can reduce technostress — measured, not just self-reported

### Demographic Moderation
- Gender: significantly influenced perception of modified IDE
- Educational level: significant effect
- Programming experience: significant effect
- Ethnicity: significant effect
- Implication: technostress design must be personalised; one-size-fits-all fails

### Framework + Dataset Validation
Two evaluator groups:
1. Professional developers
2. MSc User Experience Design participants

Results:
- Framework perceived as **credible** (methodologically sound)
- Framework perceived as **relevant** (addresses real technostress)
- Framework + TIDE dataset perceived as **complementary resources**
- Combined: effective in supporting evidence-based approach to identifying and mitigating technostress in programming environments

## Design Decision → Metric → Technostressor Mapping

| Design Decision | Metric Affected | Technostressor Addressed |
|---|---|---|
| Progressive disclosure | Cognitive load, satisfaction | Techno-overload, techno-complexity |
| Notification budgeting | Performance, productivity | Techno-overload, techno-invasion |
| Transparent defaults | Performance, satisfaction | Techno-complexity |
| Error message humanisation | Satisfaction, performance | Techno-complexity |
| Frame AI as augmentation | Satisfaction (insecurity reduction) | Techno-insecurity |
| Skill preservation cues | Satisfaction | Techno-insecurity |
| Stability contracts | Satisfaction (uncertainty reduction) | Techno-uncertainty |
| Predictable AI behaviour | Productivity, satisfaction | Techno-uncertainty |
| Focus mode toggle | Performance, productivity | Techno-invasion |
| Context recovery | Performance | Techno-invasion |

## TIDE Dataset

Structured emotional and interaction data providing empirical foundation for:
- Researchers aiming to understand role of technostress in digital environments
- Building more technostress-aware IDEs
- Affective computing research in programming contexts

The dataset captures:
- Emotional states during programming tasks
- Interaction patterns with IDE features
- Biometric signals (where consented)
- Task performance data
- User feedback on design modifications

## Scope and Limitations

### Primary Users (of framework and dataset)
- Designers and developers of IDE systems (they use the framework to inform design decisions)

### Secondary Users (beneficiaries)
- Students and programming learners (benefit from reduced technostress)

### Limitations
- Framework validation is perception-based (professional developers + UX students evaluating)
- Biometric data collection requires careful ethical handling
- Demographic moderation means the framework needs personalisation, increasing implementation complexity
- The 5 stressors are interrelated — addressing one may exacerbate another (e.g., reducing techno-complexity via more guidance may increase techno-invasion via more notifications)