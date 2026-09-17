# GenAI Interaction Type Selection — Evidence Base

## Source Study

Brandebusemeyer, C., Zunic, K., Zimmermann, T., Schimmer, T. & Arnrich, B. (2026, July 2). "Developers' Experience with Generative AI Beyond Productivity Assessment — Insights from an Empirical Mixed-Methods Field Study." arXiv:2607.02337. University of Potsdam / SAP.

## Study Design

- **Participants**: 22 professional developers at SAP (California sites), 5-10 years avg experience, Java required
- **Duration**: 4 days (2 controlled sessions + 3 days uncontrolled work)
- **Tool**: GitHub Copilot (GPT-4o default, ask mode default)
- **Data sources**: Questionnaires, screen recordings, keyboard/mouse, wristband physiological data
- **Controlled tasks**: 6 categories (coding, debugging, documentation, testing, summarizing, brainstorming) from HumanEval-X
- **Uncontrolled**: 445 documented work tasks across 3 days
- **A/B design**: Group A (no Copilot first session), Group B (Copilot first session), all used Copilot second session

## Key Statistical Results

### Controlled Sessions (from prior work [7])

**Efficiency (task duration)**:
- In-code only: significantly shorter than no-Copilot
- Chat only: significantly shorter than no-Copilot
- Combined: comparable to no-Copilot (benefits eliminated)
- Excessive interactions diminished or reversed time savings

**Accuracy (task completion)**:
- Chat only: significantly increased completion likelihood vs no-Copilot
- In-code only: no significant effect
- Combined: no significant effect
- Coding and brainstorming: highest success rates
- Debugging, testing, documentation, summary: significantly less likely to complete

**Perceived workload (NASA-TLX)**:
- In-code only: significantly reduced vs no-Copilot
- Chat only: significantly reduced vs no-Copilot
- Combined: comparable to no-Copilot, significantly higher than in-code alone

### Uncontrolled Sessions

**Task categories** (445 tasks):
- Development-heavy: 41.6% of tasks, 54.6% involved AI, 88.1% helpful when used
- Collaboration-heavy: 28.1% of tasks, 8% involved AI, 90.9% helpful when used
- Other: 30.3% of tasks, 8.1% involved AI, 100% helpful when used

**Cognitive load by task category** (linear mixed-effects models):
- Development-heavy: EMM = 4.57 (highest)
- Collaboration-heavy: EMM = 3.30
- Other: EMM = 2.23
- All pairwise differences significant (p<0.0001), medium-to-very-large effect sizes

**Productivity by task category**:
- Development-heavy: EMM = 4.57 (highest)
- Collaboration-heavy: EMM = 3.30
- Other: EMM = 2.25
- All pairwise differences significant (p<0.001)

**AI effect on development-heavy tasks**:
- Cognitive load: EMM_yes=4.72 vs EMM_no=4.14, difference=0.58, t(183)=2.67, p=0.008, d=0.34 (small-to-moderate increase)
- Productivity: EMM_yes=3.93 vs EMM_no=3.72, difference=0.22, t(178)=1.10, p=0.275 (no significant change)

**AI helpfulness effect on development-heavy tasks**:
- Cognitive load: EMM_helpful=4.70 vs EMM_not=4.88, difference=-0.18, p=0.659 (no significant change)
- Productivity: EMM_helpful=4.00 vs EMM_not=3.27, difference=0.73, t(98)=2.05, p=0.043, d=0.83 (large significant increase)

**Random effects**: 18% of variance attributable to between-participant differences (ICC=0.18); 82% from task-level variability

## Qualitative Findings

### Satisfaction Drivers
- Output quality for repetitive/structured tasks (unit tests, debugging, boilerplate)
- Work efficiency (time savings on monotonous tasks)
- New ideas and fresh perspectives (especially when prior knowledge limited)

### Dissatisfaction Drivers
- Inaccuracies with complex topics
- Hallucinations
- Prompt sensitivity (requiring repeated adjustments)
- Need for constant human verification
- Outdated internal models lacking business domain knowledge

### Interaction Type Preferences
- In-code suggestions: preferred for coding, less complex tasks, small changes
- Chat: preferred for debugging, non-coding tasks, broader context, explanations
- Combined: participants noted switching happened when one type was suboptimal

### Future Vision from Developers
- AI takes over strenuous/tedious coding tasks
- Developers focus on high-level reasoning, domain knowledge, product management
- Key challenge: defining requirements in non-ambiguous, machine-understandable way
- Risk: over-reliance and loss of deeper code understanding

## Team Dynamics

- Developers turn to GenAI faster than to team members for feedback (Md=4)
- But find team member feedback more helpful for solving problems (Md=2 pre-study, Md=3 during study)
- GenAI complements rather than replaces team interactions

## Personality Correlations (exploratory, small sample)

- Emotional stability positively correlated with GenAI use duration (τ=0.38, p=0.035)
- Other traits (extraversion, agreeableness, conscientiousness, intellect): non-significant
- Small sample (n=21) limits power; results preliminary

## Study Impact

- 12 of 22 participants reported the study changed their view of GenAI
- Those affected had significantly lower prior GenAI proficiency (U=20, p=0.036) and usage (U=24, p=0.013)
- 73% planned to adjust their GenAI interaction going forward
- Changes included: tool configuration, interaction behavior, use case expansion

## Threats to Validity

1. **Construct**: Perception-based measures, not objective performance; recall bias
2. **Internal**: Realistic work environment introduces confounds; controlled + uncontrolled design mitigates
3. **External**: Single tool (Copilot), single language (Java), single company (SAP); 22 developers limits generalizability
4. **Temporal**: Data collected at specific point in time; rapid tool evolution may change patterns

## Cross-References to A-Tech Skills

- `prompt-wait-evaluate-flow-collapse`: The flow collapse mechanism — this study provides the interaction-type dimension
- `ai-skill-formation-interaction-patterns`: Six interaction patterns — this study adds task-matching
- `calm-technology-ai-coding`: Calm technology — this study provides the interaction selection mechanism
- `ai-productivity-output-volume-paradox`: Output volume — this study explains the interaction-type variable
- `devex-verification-bottleneck-framework`: Verification bottleneck — this study shows interaction type affects verification load
- `unified-devex-measurement-stack-2026`: Measurement stack — this study adds interaction-type metrics