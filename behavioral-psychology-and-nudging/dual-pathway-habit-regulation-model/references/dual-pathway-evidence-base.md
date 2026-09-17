# Dual-Pathway Habit Regulation — Evidence Base

## Source
Asaoka, N., Pagano, D., Hayashi, Y. (2026). "Dissociable roles of prefrontal plasticity in decision-making strategy and execution of habitual behavior." Nature Communications 17, 6822. https://doi.org/10.1038/s41467-026-75706-1

## Experimental Paradigm

**Two-step operant training (within-subject transition)**:
1. CRF (continuous reinforcement, 3 days): every lever press rewarded with sucrose
2. VR (variable ratio, 6 days): reward after average 10 then 20 presses → goal-directed
3. VI (variable interval, 4 days): reward on first press after average 60s → habitual

**Devaluation test**: 30 min free access to sucrose (devalued) or chow (valued), then 5 min lever press (no reward). Goal-directed = fewer presses when devalued; habitual = insensitive to value.

**Devaluation index** = (lever presses valued − devalued) / (valued + devalued). Positive = goal-directed; ~0 = habitual.

**Execution index** = (press rate VI − press rate VR) / (press rate VI + press rate VR). Positive = increased under VI; negative = decreased.

## Key Behavioral Finding

Devaluation index and execution index are **uncorrelated** across mice. The transition to habitual strategy does not determine the execution level. They are governed by dissociable mechanisms.

## Circuit 1: ACC → RSC (Strategy)

### Ex vivo slice electrophysiology (AMPA/NMDA ratios, L2/3 and L5)
- After CRF: MOFC L2/3 potentiation only
- After VR: ACC L5 + LOFC L5 potentiation; MOFC returns to baseline
- After VI (VR+VI): ACC L5 returns to baseline in both High and Low execution groups; LOFC L5 remains elevated in High execution only

### Projection-specific analysis (retrograde AAV labeling)
- RSC-projecting ACC L5: potentiated after VR, depotentiated after VI (both groups)
- CS-projecting ACC L5, BLA-projecting ACC L5: no consistent potentiation pattern
- **Only RSC-projecting ACC L5 AMPA/NMDA ratio correlates with strategy (devaluation index) — NOT with execution index**

### Chemogenetic manipulation (hM4Di/hM3Dq + CNO)
- ACC inhibition during VR → devaluation index drops to ~0 (becomes habitual)
- ACC activation after VI → devaluation index becomes positive (reverts to goal-directed)
- LOFC inhibition during VR → no effect on devaluation index
- Same results replicated in VI+VI (6 days VI only) mice

### Optogenetic LTP erasure (CFL-SN CALI)
- Express CFL-SN in RSC-projecting ACC neurons; illuminate after each VR session
- Result: devaluation index significantly smaller than control (SN) → behavior became habitual
- Lever pressing during training was comparable between groups → acquisition not impaired
- Sucrose/chow intake during devaluation unaffected → satiety mechanism intact

### In vivo Ca²⁺ imaging (UCLA Miniscope, jGCaMP7s via retroAAV in RSC)
- RSC-projecting ACC neurons: burst-like activity during rewarded nose pokes; minimal during lever pressing or non-rewarded nose pokes
- Reward-excited cells: proportion + activity increase during VR, decrease after VI switching
- Reward-inhibited cells: proportion + inhibition decrease during VR, revert after VI
- Population-weighted reward response tracks strategy transition
- **No correlation with execution index**

## Circuit 2: LOFC → CS (Execution)

### Projection-specific analysis
- CS-projecting LOFC L5: potentiated after VR; persists in High execution, absent in Low execution
- BLA-projecting LOFC L5: no clear pattern
- LOFC does NOT project to RSC (no RSP-projecting LOFC neurons found)
- **Only CS-projecting LOFC L5 AMPA/NMDA ratio correlates with execution index — NOT with devaluation index**

### Optogenetic LTP erasure (CFL-SN CALI in CS-projecting LOFC)
- Illuminate after each VI session
- Result: execution index significantly lower than control; lower proportion of High-execution mice
- Devaluation index: no difference (both groups habitual)
- Omission test decay: no difference
- Motivation (sucrose intake): no difference

### Chemogenetic activation (hM3Dq in CS-projecting LOFC, 2 additional VI sessions)
- CNO → slight but significant increase in lever pressing frequency
- No effect on reward acquisition, nose poke frequency
- No effect on devaluation test

### In vivo Ca²⁺ imaging (jGCaMP7s via retroAAV in CS)
- CS-projecting LOFC neurons: significant responses to both lever pressing AND rewarded nose pokes (contrast with RSC-projecting ACC which responded only to reward)
- Reward-related activity: slow decay following rewarded nose poke onset (contrast with RSC-projecting ACC's multiple brief peaks)
- Functionally distinct subpopulations: lever press cells (inhibited during reward consumption) vs reward cells (inhibited during lever pressing) — mutually exclusive
- Reward cells' amplitude progressively increases across training stages
- At VI stage: reward-related response positively correlated with execution index
- **No correlation with devaluation index**

## The Dual Regulatory Model (Figure 8)

1. **LTP in RSC-projecting ACC neurons** is necessary for predominance of goal-directed system. As it weakens (depotentiation), behavior becomes habitual.
2. **LTP in CS-projecting LOFC neurons** maintains execution of habitual behavior at a comparable level to previous goal-directed execution.

These are spatiotemporally distinct: RSC-projecting ACC encodes during goal-directed action (VR), reverts during habit (VI). CS-projecting LOFC encodes reward across stages, with persistence under VI determining execution level.

## Pathological Implications

Hyperactivity in OFC-striatum pathway is implicated in:
- Obsessive-compulsive disorder (Burguière et al. 2013; Corbit et al. 2019)
- Addiction (Pascoli et al. 2018)
- Eating disorders (Schienle et al. 2009; Wang et al. 2023)

The LOFC→CS execution circuit is the candidate for "abnormally strong habit" severity. Compulsivity = habitual strategy + abnormally high execution driven by persistent LOFC→CS LTP.

Treatment implication: different interventions for different dimensions:
- Reduce execution without restoring deliberation → target LOFC→CS (reduce Pavlovian cue-reward strength)
- Restore goal-directed control → target ACC→RSC (reactivate LTP, make behavior deliberative again)

## Methodological Notes

- Only male C57BL/6J mice used (to minimize estrous cycle variability) — limitation
- Two-step paradigm (VR → VI) enables tracking within-subject transition with defined time window
- Optogenetic LTP erasure uses cofilin-SuperNova (CFL-SN) + chromophore-assisted light inactivation (CALI) — erases LTP post hoc without affecting basal transmission or pre-existing synapses
- In vivo imaging uses UCLA Miniscope + GRIN lens + jGCaMP7s; population-level analysis (individual neurons not longitudinally tracked due to mechanical displacement)
- Linear mixed effects models with Benjamini-Hochberg FDR correction

## Statistics (Selected)

| Experiment | Test | Result |
|---|---|---|
| Devaluation index VR vs VR+VI | — | VR+VI near zero (habitual); VR positive (goal-directed) |
| Execution index: correlation with devaluation index | — | Not significant |
| ACC hM4Di + CNO: devaluation index | — | Drops to ~0 (habitual) vs vehicle (goal-directed) |
| ACC hM3Dq + CNO: devaluation index | — | Becomes positive (goal-directed) vs vehicle (habitual) |
| RSC-projecting ACC CALI: devaluation index | — | CFL-SN < SN (habitual) |
| CS-projecting LOFC CALI: execution index | — | CFL-SN < SN (lower execution) |
| CS-projecting LOFC CALI: devaluation index | — | No difference (both habitual) |
| CS-projecting LOFC hM3Dq: press frequency | — | CNO > vehicle (slight increase) |
| CS-projecting LOFC reward response × execution index at VI | — | Positive correlation |