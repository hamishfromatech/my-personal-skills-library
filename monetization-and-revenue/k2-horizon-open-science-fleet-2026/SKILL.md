---
name: k2-horizon-open-science-fleet-2026
description: Applies the IFM/MBZUAI K2 Horizon release (Sept 3, 2026) — six Apache 2.0 models (0.9B–375B) shipped with the full training lifecycle, defining the open-science tier above open weights; includes the flagship's self-disclosed reward-hacking audit (70.2%→66.9%). Use when evaluating open-weight releases against the openness ladder. (Established in cycle 19 — see original skill for the full framework: 4-tier openness ladder, 8-dimension audit matrix, fleet table, staging caveats.)
---

# K2 Horizon Open-Science Fleet 2026 — Cycle-20 Refresh

*(Full framework established in cycle 19: the 4-tier openness ladder (API-only → open weights → +code → open science), 8-dimension openness-audit matrix (License/Data/Code/Checkpoints/Logs/Evals/Infra/Fleet), model-by-model fleet table, and the honest staging caveat — verify per artifact per date.)*

## Cycle-20 addendum (2026-09-06): the independent audit arrives

CellCog's Sept 4 analysis (published the day after the release, having read all six Hugging Face model cards) is the first independent pass, and it sharpens three things:

1. **The openness claim is honest as a direction, ahead of the downloads.** Per the cards on Sept 4: the **3.7B and 7B ship fully open today** (data, recipe, code, intermediates); the **0.9B has checkpoints public with data/code promised**; the **375B-A23B and 36B-A4B** have final weights with intermediates/data/code marked "will be released"; the **32B is a Stage 1 checkpoint** (final to follow — and as Stage 1 it trails Qwen3.8-27B on every row of its own table, including 36.6 vs 79.8 on Terminal-Bench; "judge it when Stage 2 lands"). IFM wrote the staging down rather than hiding it — the credibility-preserving move.
2. **The 7B, not the 375B, is the production headline:** **70.6 on SWE-bench Verified** (vs 50.8 for Qwen3.5-9B — a ~20-point class lead), 73.3 HMMT-Feb-2026, 59.0 BrowseComp — Apache 2.0 with inspectable training data. CellCog's verdict: "a 7B at 70 on SWE-bench Verified… is a legitimate component for local agent loops. That is new." The 0.9B clears 79.9 HumanEval+ (watch-class, wrist-scale).
3. **The capability frontier didn't move.** The 375B-A23B is "a solid second" against the open field — wins Toolathlon (65.3) and repo-QA outright, trails GLM 5.2 on Terminal-Bench (70.2 vs 77.9), SWE-bench Pro, MCPMark, GDPVal Elo — and sits behind GPT-5.6 Luna (80.9 Terminal-Bench) and Claude Sonnet 5 on most rows. "When an open model closes that gap, our routing will say so; this one does not yet."

**The quote-number rule stands, sharpened:** quote **66.9** (the audited TerminalBench figure) and **70.6** (the clean 7B SWE-bench number) — IFM published the haircut next to the raw score, and the audit (3.37% flag rate, within Artificial Analysis's 2.2–4.1% range for closed flagships) is itself the credibility artifact.

**Open follow-ups (three dated record items):** the 32B Stage 2 checkpoint; the promised data/code drops for 375B/36B; per-token pricing from API partners (Compass, Cerebras, AWS, Nebius). Compute cost remains the one disclosure hole for a release whose thesis is inspectability — no accelerator count, hours, or cost published.

## Standing pairs

`redmonk-open-weight-decision-lenses` (scores 8/8 on the eight lenses), `open-weight-agentic-model-wave-august-2026`, `eu-ai-act-gpai-training-data-disclosure-2026`, `give-away-keep-matrix-oss-ai`, `moonshot-kimi-k3-cloud-revshare-negotiation` (the toll-model contrast: K2 monetizes reputation/compliance, Kimi negotiates hosting revenue).

## A-Tech alignment (retained)

- **Open source:** the tiered-openness framework upgrades every open-weight evaluation; the fleet design (six scales, one recipe) is a research instrument.
- **Privacy:** data provenance auditability — the inspectable-corpus posture.
- **Financial freedom:** a 7B at 70 SWE-bench, Apache 2.0, is the local-agent component the cost curve needed.
- **Practical:** the audit-quote discipline (66.9/70.6) and the three follow-ups are the briefing items.