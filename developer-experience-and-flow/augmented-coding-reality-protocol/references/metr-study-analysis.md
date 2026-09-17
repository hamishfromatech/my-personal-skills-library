# METR Study: How Early-2025 AI Tools Affect Experienced Developer Productivity

## Study Design

METR (Model Evaluation and Threat Research) conducted a randomized controlled trial (RCT) in early 2026 measuring how AI coding tools affected experienced open-source developers.

- **Participants:** 16 developers from major repositories averaging 22,000+ stars and over 1 million lines of code each
- **Tasks:** 246 real issues (not toy problems), averaging two hours each
- **Tools:** Participants could use any AI tool; most chose Cursor Pro with Claude models
- **Design:** Randomized assignment of AI tool access vs. baseline (no AI tools)
- **Measurement:** Objective completion time (primary); self-reported productivity (secondary)

## Key Findings

### Objective Performance
- Developers took **19% longer** when AI tools were permitted
- This effect persisted across task difficulty levels
- The slowdown was driven by: tool learning time, debugging AI-generated code, reviewing/refactoring outputs, context switching, and managing tool limitations

### Perception vs. Reality
- **Pre-study prediction:** Participants predicted a 24% speedup
- **Post-study self-report:** Participants still believed AI had made them 20% faster
- **Objective measurement:** 19% slower
- **Perception-reality gap:** 39 percentage points (the largest METR documented in productivity research)

### Why the Gap Exists
AI tools reduce cognitive load during initial coding. This feels faster and easier. However, the time saved typing is consumed by:
- Verifying suggested APIs exist
- Testing edge cases AI overlooked
- Understanding generated code before extending it
- Debugging hallucinations (non-existent libraries, wrong signatures)
- Refactoring duplicated or inconsistent output

### Methodology Comparison: Vendor vs. Independent

| Dimension | Vendor Benchmarks (GitHub, Cursor) | Independent Research (METR) |
|-----------|-----------------------------------|----------------------------|
| Study design | Self-selected early adopters | Randomized controlled trial |
| Tasks | Isolated functions, greenfield | Multi-file, existing codebase |
| Measurement | Self-reported satisfaction | Objective completion time |
| Context | Toy problems | Real architectural context |
| Incentives | Vendor sells the tool | Non-profit AI safety org |

## Implications for Engineering Leaders

1. **Pilot before scale.** Run an internal RCT measuring objective outcomes before enterprise rollout.
2. **Measure the right things.** Completion time, refactoring rate, duplication, and security findings — not developer satisfaction.
3. **Account for the tax.** Time saved during generation is partially or fully offset by verification and debugging.
4. **Senior developers matter most.** The METR study used experienced developers. Junior developers may face larger skill development gaps.

## Citation
METR (Model Evaluation and Threat Research). "How early-2025 AI tools affect experienced open-source developers' productivity." Published 2026.
