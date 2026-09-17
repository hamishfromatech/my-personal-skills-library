# DIKW Agentic Behavioral Experiment Learning — Evidence Base

## Primary Source

Luo, Agarwal & Gao (arXiv:2606.02458, 2026) — "Beyond One-shot: AI Agents for Learning in Field Experiments"
- Johns Hopkins School of Medicine + Carey Business School
- Two-stage field experiments in healthcare prescription messaging
- 693,139 total patient visits (Stage 1: 444,691; Stage 2: 248,448)

## Study Design

### Stage 1 (June 16 – July 3, 2025)
- 13 message variants co-designed by 6 behavioral science researchers + GPT-4
- 444,691 patient visits over 18 days
- Principles: salience, authority, socialNorms, gainFraming, timeliness, commitmentPrompt, simplification, emotionalCue, progressFeedback, goalReinforcement, futureSelf, socialIdentity
- CTR range: 54.3% (socialNorms) to 66.1% (salience); baseline default: 62.5%
- Top 3: salience (66.1%), progressFeedback (64.5%), default (62.5%)

### Stage 2 (August 25 – September 8, 2025)
- 20 variants: 17 DIKW-generated + 3 Stage 1 baselines (salience, progressFeedback, default)
- 248,448 patient visits over 15 days
- Stratified randomization by age, therapeutic category, provider specialty
- ~12,400 visits per variant; 80% power to detect 2.5% CTR difference

## DIKW Architecture Details

### Implementation Stack
- **Orchestration**: LangGraph (open-source, stateful multi-agent LLM framework)
- **LLM**: Claude 4 Sonnet (all agents)
- **D/I-level tools**: autonomous Python code generation (150-300 lines), sandboxed execution
- **Libraries**: pandas, scipy, statsmodels, matplotlib
- **Outputs per topic**: executable script + CSV statistical tables + structured Markdown report with embedded figures

### Agent-Unit Formal Specifications

```
D : Σ, T_D(t).spec → T_D(t).output
I : Σ, {T_D(t).output}, T_I(t).spec → T_I(t).output  
K : {T_I(t).output}, T_K(t).spec → T_K(t).output
W : {T_K(t).output}, open, T_W(t).spec → T_W(t).output
```

Where Σ is the raw dataset, T(t).spec encodes task specification, T(t).output produced artifacts, and `open` is open-domain prior knowledge invoked at Wisdom level.

### Bidirectional Flow
- Bottom-up: D/I publish outputs upward → I/K/W build richer abstractions
- Top-down: K/W propagate queries downward → trigger additional D/I analyses when evidence insufficient
- Failure recovery: agents report diagnostics upward → plan revision (not system failure); preserved completed work across sessions

### Execution Mode Strategies
- INIT: load foundational analyses from templates
- GENERATE: create new custom topics via LLM synthesis
- LOOP: resume previous work (SKIP complete, RUN CODE for scripts without reports, regenerate from scratch)
- SKIP: bypass entire level when complete

## Stage 2 Results

### Click-Through Rates (top performers)
| Variant | CTR | Δ vs default | Significance |
|---|---|---|---|
| efficiencyTech | 69.8% | +6.5pp | p < 0.001 |
| completePro | 66.5% | +3.2pp | p < 0.001 |
| authorityTrad | 65.5% | +2.2pp | p < 0.001 |
| clarityAction | 65.1% | +1.8pp | p < 0.001 |
| salience (Stage 1 best) | 66.7% | +3.4pp | p < 0.001 |
| default (baseline) | 63.3% | — | — |

### Authentication rates confirm engagement persists beyond initial click (same ordering)

### Frontier LLM Comparison (without experimental data)
| Model | Spearman ρ vs actual | MAE (ranks) | Notes |
|---|---|---|---|
| GPT-4o | 0.271 (n.s., p=0.248) | 5.70 | Random baseline: 6.7 |
| Claude 3.5 Sonnet | −0.120 (n.s., p=0.613) | 7.70 | Worse than random |

7 consensus failures (both models err >5 ranks). The best message (efficiencyTech, 69.8%) was ranked #15 by GPT-4o and #14 by Claude.

## Domain-Specific Knowledge Discovery

### Efficiency framing (the winner)
- I-level evidence: message length negatively correlates with engagement; action-oriented language ("review", "new") overperforms; consistency across age, gender, medical context
- K-level synthesis: efficiency framing is reliable (confidence 0.85, cross-subgroup robust)
- W-level design: "New Rx info needs quick review" (54 chars, novelty cue + action verb)
- Why it works: conveys urgency implicitly without triggering reactance; cognitive load sensitivity rewards brevity

### Social proof (the failure)
- I-level evidence: underperforms baseline among both male and female patients, across all age groups, in every medical context
- K-level synthesis: social proof is ineffective in this domain; excluded from W-level generation
- Why it fails: patients resist conformity pressure about personal health decisions

### Reciprocity (the failure)
- W-level test: "thank you for reviewing" — performed worst (−7.8pp)
- Why it fails: premature gratitude triggers persuasion knowledge model (Friestad & Wright 1994)

## Theoretical Interpretation

Three domain-specific factors explain why widely used behavioral principles backfire in healthcare:

1. **Autonomy preservation** (Brehm 1966): patients resist directive language; efficiency framing conveys urgency implicitly
2. **Professional norm governance** (Pornpitakpan 2004): casual tone undermines credibility; competence signaling matters
3. **Cognitive load sensitivity**: patients with competing medical demands reward brevity; completion framing (Zeigarnik 1938) succeeds by framing action as finishing, not starting

## Architecture Design Principles (validated)

1. **Hierarchical abstraction**: prevents jumping from raw data to designs; requires intermediate pattern extraction and knowledge synthesis
2. **Transparency through evidence chains**: every assessment traces back to specific statistical findings → raw observations
3. **Bidirectional flow**: bottom-up discovery + top-down refinement (not simple feed-forward pipeline)
4. **Computational grounding**: separates objective code execution (D/I) from subjective synthesis (K/W); prevents hallucination
5. **Domain independence**: architecture generalizes; behavioral principles vary by domain (healthcare ≠ consumer ≠ education)

## Limitations

- Validated in healthcare prescription notifications only; generalizability to other domains untested
- Short-term engagement (click-through within 24h); downstream health outcomes (adherence, clinical endpoints) not measured
- No formal test of whether agent confidence scores predict Stage 2 outcomes at individual-hypothesis level
- Human expert comparison (could expert analysts extract similar knowledge given full Stage 1 outputs?) not conducted
- Architecture depends on quality of Stage 1 experimental data; cannot substitute for well-designed experiments

## A-Tech Alignment

- **Open-source AI**: LangGraph (open-source), Python, standard libraries; architecture is reproducible
- **Data privacy**: processes organizational experimental data locally; no external data sharing required
- **Financial freedom**: reduces cost of behavioral design from expert-intensive to infrastructure-intensive; democratizes evidence-based intervention design
- **Practical implementation**: Docker-deployable; demonstrated at 693K-patient scale; architecture is domain-independent