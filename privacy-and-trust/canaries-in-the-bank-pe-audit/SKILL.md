---
name: canaries-in-the-bank-pe-audit
description: Applies "Canaries in the Bank: Auditing User-Level Privacy in Private Evolution" (Aketi, Ullah & Gade, arXiv:2609.13499, Sept 11, 2026) as the CANARY-PROBE-AUDIT pattern — the first protocol-aware empirical audit of Private Evolution (PE), replacing ~1% of a shared candidate bank with known non-private canaries and evaluating 8 attacks to quantify how close protocol-valid manipulation gets to the DP privacy ceiling. Natural-text attacks stay well below the theoretical bound; high-entropy nonce attacks come closest. Use when [auditing synthetic-data or federated-synthetic pipelines for realized privacy loss, designing empirical privacy audits, threat-modeling shared-candidate or vote-aggregation mechanisms, or distinguishing formal DP guarantees from achievable leakage]. NOT for [central DP-SGD auditing, secure-aggregation topology attacks (use the SA-attack skill), or DP accounting theory].
---

# Canaries in the Bank: Auditing User-Level Privacy in Private Evolution

## Overview
Private Evolution (PE) generates high-fidelity synthetic data in federated settings **without raw-data exposure** — users submit clipped votes over a shared candidate bank, and the server publishes a differentially private histogram with noise calibrated to worst-case user contribution. The open question the paper (Aketi, Ullah & Gade, arXiv:2609.13499, Sept 11, 2026) answers: **can an adversary realize that worst-case privacy loss while following the protocol?** Their answer is an empirical audit: the server commits to a single shared candidate bank, ~1% of entries are replaced with probes derived from a known non-private canary, and eight attacks are evaluated on Yelp and Sentiment140 — unchanged-bank baseline, exact copies, plausible paraphrases, high-entropy synthetic nonces, and more. Results: **natural-text attacks remain substantially below the theoretical DP bound; nonce-based attacks yield considerably stronger bounds and come closest to the mechanism's privacy ceiling.** The contribution is the gap quantification — formal worst-case privacy vs leakage achievable through *protocol-valid* candidate-bank manipulation — plus a reusable audit methodology (seed the pipeline with canaries, measure realized loss, report the gap).

## The Evidence Base
- **arXiv:2609.13499 (Sept 11, 2026, 14 pages):** protocol-aware empirical audit; server commits to shared candidate bank; ~1% canary probes; 8 attack classes; Yelp + Sentiment140; natural-text far below bound; nonces closest to ceiling.
- **Protocol context:** PE aggregates clipped user votes into a DP histogram; noise calibrated to worst-case user contribution — the audit tests whether that worst case is *realizable* by a protocol-following adversary.
- **Library context:** the DP-FL optimizer family (FedGSA, DDP-SA, DDP-SOFIM, etc.) holds composition mechanics; `topology-betrays-privacy-sa-attack` holds the DFL-attack axis; this is the **audit** axis — first work measuring realized-vs-promised privacy in PE specifically.

## Core Findings (the pattern)
1. **Worst-case ≠ realizable — and the gap is measurable.** The audit's headline is a quantified gap between the DP bound and achievable leakage. This is the first PE-specific realization measurement.
2. **Attack entropy predicts proximity to the ceiling.** Natural text (high prior plausibility) underperforms the bound by a wide margin; synthetic nonces (low prior) come closest. Prior distributions, not just mechanisms, determine realized privacy.
3. **Protocol-valid manipulation is the threat model that matters.** No deviation from the PE protocol is required — the adversary operates inside the rules. Security reviews should assume rule-following adversaries with adversarial *inputs*.
4. **Canary-seeding generalizes.** The audit method (commit to a bank, seed ~1% canaries, evaluate attack families, report realized-vs-bound gap) is applicable to any vote-aggregation or shared-resource DP mechanism beyond PE.
5. **Auditing is the privacy-evidence standard.** A DP guarantee with no realized-loss audit is a claim; this paper supplies the methodology that turns claims into measurements.

## When to Use
- Auditing synthetic-data pipelines or federated synthetic-data generation (PE-style) before deployment claims.
- Designing empirical privacy audits for any shared-candidate / vote-aggregation mechanism.
- Threat-modeling federated systems where participants influence shared state (candidate banks, shared prompts, model soups).
- Distinguishing formal DP guarantees from achievable leakage in buyer conversations or security reviews.
- Privacy-content framing: "your DP pipeline may leak less than it promises — audit the gap."

## NOT For
- Central DP-SGD gradient auditing (different mechanism family).
- DFL secure-aggregation topology attacks (use `topology-betrays-privacy-sa-attack`).
- DP composition theory or accounting (the optimizer family holds that).

## Core Process / Workflow
1. **Map the shared-state surface.** Identify what participants can influence: candidate banks, vote tallies, shared prompt pools, aggregation inputs.
2. **Design the canary probe.** Choose a known non-private canary; replace a small fraction (~1%) of shared-state entries with probes derived from it.
3. **Run the attack family ladder.** Baseline → exact copies → paraphrases → high-entropy nonces; measure realized leakage per family.
4. **Report the gap, not just the bound.** Deliverables are (a) the formal bound, (b) realized leakage per attack family, (c) the gap — with nonces as the ceiling-approaching case.
5. **Treat protocol-valid manipulation as in-scope threat modeling.** Security review checklists should include "what can a rule-following participant do to shared state?"

## A-Tech Alignment
- **Privacy:** the audit-first privacy posture — A-Tech's privacy positioning is strengthened by evidence standards: not "we use DP" but "here is our realized-loss audit."
- **Open source:** PE-style federated synthetic data is an open-stack privacy pattern; auditing it strengthens the open-privacy stack (`google-parfait-open-privacy-ai-stack` lineage).
- **Practical:** canary-seeding is implementable in any A-Tech federated or synthetic-data product; the audit report format is the deliverable.

## Honesty Caveats
- Results are task-specific (Yelp, Sentiment140) — realized-loss gaps will vary by data modality and mechanism parameters.
- The audit is a research artifact, not a production tool; canary-seeding requires operator cooperation (it is a *defensive* methodology, not an external attack toolkit).
- "Closest to the ceiling" for nonces is comparative, not an absolute leakage claim.
- PE is one mechanism family; generalization to other DP pipelines is argued, not measured.

## Pairs-with
`topology-betrays-privacy-sa-attack` (the DFL-attack axis), `federated-learning-for-privacy-preserving-ai` (the FL foundation), `differential-privacy-synthetic-data` (the synthetic-data family), `google-parfait-open-privacy-ai-stack` (the open-privacy stack), `privacy-first-ai-pipeline-defense`, `zk-proof-federated-learning-trust`.

## References
- Aketi, S.A., Ullah, E. & Gade, S. "Canaries in the Bank: Auditing User-Level Privacy in Private Evolution." arXiv:2609.13499, Sept 11, 2026 (cs.CR; 14 pages).

*Created: 2026-09-17 (Cycle 28) — A-Tech Research Division*