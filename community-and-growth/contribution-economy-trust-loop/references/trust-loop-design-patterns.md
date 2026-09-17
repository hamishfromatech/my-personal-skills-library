# Trust Loop Design Patterns

## Overview

This reference documents the design patterns for each stage of the contribution economy trust loop, with anti-patterns and case studies.

## Stage 1: Contribution — Lowering the Barrier

### Pattern: The Contribution Ladder

```
Lurker → Reactor → First-Time Contributor → Repeat Contributor → Maintainer → Mentor
```

Each rung has a specific lowering mechanism:

| Rung | Barrier | Lowering mechanism |
|---|---|---|
| Lurker → Reactor | "I don't know enough to contribute" | Low-effort reactions (emoji, upvote, "me too") count as micro-contributions |
| Reactor → First-time | "I don't know where to start" | "Good first issue" labeling + templates + paired contribution sessions |
| First-time → Repeat | "That was hard, not sure I'll do it again" | Recognition ritual (per `choice-closure-effect` confirmation) + mentorship pairing |
| Repeat → Maintainer | "I'm not ready for responsibility" | Gradual authority increase: review one PR → review three → merge small PRs → merge all |
| Maintainer → Mentor | "I'm too busy" | Mentorship counts as a contribution type; reputation accrues from it |

### Anti-pattern: The Contribution Cliff
Community has a high contribution bar (large PR required, complex review process) with no ladder. First-time contributors bounce. Solution: implement the ladder; never require a large first contribution.

### Case Study: Open Source Pledge as Loop Accelerator
The Open Source Pledge ($2,000/developer/year target per `open-source-sustainability-infrastructure`) accelerates the loop by funding contributors at the Maintainer and Mentor rungs, converting volunteer time into funded time.

## Stage 2: Attribution — The Trust Foundation

### Pattern: Structured Provenance

Every contribution produces a structured provenance record (see SKILL.md for the YAML schema). The record is:
- **Hash-linked** to the artifact (tamper-detectable)
- **Identity-linked** to the contributor (DID-based)
- **Outcome-linked** to impact metrics (not just activity)
- **Review-linked** to who reviewed/merged (distributes trust)

### Anti-pattern: Activity-Based Attribution
Attributing by commit count, PR count, or lines of code. This incentivizes volume over quality and creates a vanity reputation problem. Solution: outcome-linked attribution (issues resolved, downstream references, impact metrics).

### Anti-pattern: Platform-Locked Identity
Reputation tied to a single platform (e.g., GitHub stars only). Contributors can't port reputation. Solution: W3C DID + Verifiable Credentials.

## Stage 3: Reputation — Earned, Not Vanity

### Pattern: Outcome-Weighted Multi-Domain Reputation

```
reputation = Σ (impact_weight_i × contribution_i) × type_diversity × recency_decay

per-domain:
reputation[domain] = Σ (impact_weight_i × contribution_i for i in domain)
```

Reputation is tracked per domain (security, payments, docs, mentoring). A contributor can have high reputation in docs and low in security. Trust is calibrated to the domain — you trust someone's code review in an area where they have demonstrated outcomes, not globally.

### Anti-pattern: Global Reputation Score
A single number that rises with activity. Creates the vanity trust score problem (per `trust-calibration-ux-pattern`). A contributor with 10,000 commits but no merged security PRs should not be trusted on security. Solution: per-domain reputation.

### Anti-pattern: Reputation Rot
Reputation never decays. A contributor who was active 5 years ago but inactive since still has high reputation. Solution: recency decay (exponential, half-life configurable; 12-18 months typical).

## Stage 4: Trust — Earned Authority

### Pattern: Gradual Authority Progression

Authority is granted in proportion to demonstrated reputation in the relevant domain:

| Reputation level | Authority granted |
|---|---|
| 0-25th percentile | Suggest, react, propose |
| 25-50th | Review PRs in their domain |
| 50-75th | Merge small PRs in their domain |
| 75-90th | Merge all PRs in their domain |
| 90th+ | Mentor others, define standards, steward the domain |

This is the `ai-employee-agent-team-management` escalation pattern applied to human contributors. Authority scales with demonstrated outcomes, not tenure or assertiveness.

### Anti-pattern: Founder's Syndrome
The community founder retains all authority regardless of who has demonstrated more competence. Solution: authority progression is rule-based, not personal.

## Stage 5: Monetization — Closing the Loop

### Pattern: The Revenue-Back-To-Contribution Reinvestment

The loop closes when contributor earnings fund more contribution time. Design mechanisms:

1. **Transparent earnings:** Contributors publicly show (optionally, at granularity they choose) that their community work is funded. This normalizes paid contribution.
2. **Reinvestment recognition:** Contributors who reinvest earnings into the community (sponsoring others, funding bounties) earn a "Steward" reputation boost.
3. **Sustainable pace:** Monetization should fund sustainable contribution, not burnout-inducing overwork. Track contributor wellbeing alongside contribution metrics.

### Anti-pattern: Extractive Monetization
Platform takes revenue from contributor work without reinvesting in contributors. Contributors leave. Solution: the platform fee should fund community infrastructure and contributor support, not just platform profit.

### Case Study: The Marketplace Flywheel
A contributor builds a tool, earns reputation from its adoption, is invited to a paid marketplace, earns revenue, reinvests time in building more tools, which earn more reputation. The loop accelerates. This is the `agent-marketplace-builder-economy` pattern applied to community contributors.

## Cross-Stage Pattern: The Trust Audit

Quarterly, the community conducts a trust audit:

1. Are attribution records complete and accurate? (> 90%)
2. Is reputation correlated with outcomes, not activity? (correlation test)
3. Are high-reputation contributors earning? (revenue-to-reputation correlation)
4. Are earnings funding more contribution? (reinvestment rate)
5. Is the loop widening? (new contributor retention > 40%)

The audit identifies where the loop is breaking and prioritizes intervention.