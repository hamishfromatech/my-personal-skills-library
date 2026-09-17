---
name: omla-open-weight-royalty-license
description: Applies the OMLA (Open Model License Agreement) royalty framework — 30% commercial-use royalty with recursive lineage splits, self-metered usage, and direct creator payment — to design open-weight AI monetization that compensates upstream creators. Use when designing royalty-based open-weight licensing, evaluating OMLA vs Apache 2.0 vs revenue-share models, or building payment flows for fine-tuned model lineage.
---

# OMLA Open-Weight Royalty License

## Overview
OMLA is a royalty-based open-weight licensing model that charges 30% of commercial revenue (or run cost, whichever is greater) while routing payments directly to creators through a recursive lineage split — fine-tunes retain at most 5%, the rest flows upstream to parent model creators. It solves the structural problem Apache 2.0 leaves open: open weights generate zero revenue for the original creators.

## When to Use
- Designing a monetization model for an open-weight AI model or fine-tune
- Evaluating whether OMLA, Apache 2.0, MIT, or a custom revenue-share license fits a model release
- Building payment flows for model lineage (who gets paid when a fine-tune is used commercially)
- Comparing creator compensation mechanisms across open-weight licensing approaches
- Advising fine-tuners on their royalty obligations to upstream model creators
- NOT for fully proprietary API-only models (use revenue-share or outcome-based pricing instead)
- NOT for non-commercial research-only releases (Apache 2.0 or MIT suffices)

## Core Process / Workflow

### 1. The OMLA Core Rule

```
Non-commercial use:    FREE (academic research, personal projects, internal training)
Commercial use:       30% of the GREATER of revenue OR run cost
                       (self-assessed by the commercial user)
Payment direction:    Direct to creator wallets (no intermediary, no reporting to OMLA)
```

**Key distinction from Apache 2.0:** Apache 2.0 permits unrestricted commercial use with no compensation obligation. OMLA explicitly defines commercial use as any activity generating revenue — including sales, subscriptions, advertising, paid services, fundraising, sponsorships, or enhancing another product's value.

**Key distinction from revenue-share (RSI):** RSI is a platform-level model (platform takes a cut of developer app revenue). OMLA is a model-level license (commercial users pay the model creators directly).

### 2. The Recursive Lineage Split

OMLA's most novel mechanism is recursive royalty attribution through model lineage:

```
Model A (original) ←─ fine-tune ── Model B (fine-tune) ←─ fine-tune ── Model C (derivative)
     ↑                                                              ↑
     └── receives most of the royalty from C's commercial use         │
                                                                       │
              C's commercial user pays 30% royalty                     │
              C's manifest declares its lineage split                   │
              Open resolver computes each wallet's percentage          │
              Money flows: C's user → C's creator (≤5%) → B → A       │
```

**Rules:**
- A fine-tune retains **at most 5%** of the royalty
- The remainder flows upstream through the published lineage splits
- Deep upstream creators are paid automatically through recursive resolution
- Each model publishes a **signed manifest**: identity, lineage, royalty split, public payment pointers

### 3. The Four-Step Workflow

**Step 1 — Creators publish & sign:**
Register the model with a signed manifest (JSON, mirrorable):
- Creator identity
- Lineage (parent models, if fine-tuned)
- Royalty split (what percentage each contributor receives)
- Public payment pointers (wallet addresses)

**Step 2 — OMLA serves the registry:**
- Publishes signed manifests as mirrorable JSON snapshots
- Provides a thin read API
- OMLA stores NO usage data, payer data, or payment records — ever

**Step 3 — Users meter & resolve:**
- Commercial users self-meter their own usage
- Self-assess the 30% royalty (greater of revenue or run cost)
- Run the open resolver to compute each wallet's percentage

**Step 4 — Users pay wallets directly:**
- Payment goes straight to each creator's published payment pointers
- No accounts, no reporting, no waiting on OMLA or any intermediary

### 4. License Comparison Matrix

| Dimension | Apache 2.0 | MIT | OMLA | RSI (platform-level) | Qwen3.8-Max custom |
|-----------|------------|-----|------|----------------------|-------------------|
| Commercial use | Free | Free | 30% royalty | Platform takes % of app revenue | Revenue share above $50M MAU |
| Creator compensation | None | None | Direct, recursive | Via platform | Via Alibaba contract |
| Fine-tune obligations | None | None | Honor upstream lineage | N/A (platform model) | Triggered at scale |
| Self-metering required | No | No | Yes | No (platform tracks) | No |
| Privacy of usage data | N/A | N/A | Maximum (OMLA sees nothing) | Low (platform tracks) | Low |
| Enterprise legal friction | Minimal | Minimal | Medium (royalty accounting) | Low | High (custom terms) |
| Non-commercial use | Free | Free | Free | N/A | N/A |

### 5. Designing an OMLA-Compatible Release

```markdown
## Model Release Checklist

1. [ ] Define commercial use threshold clearly (revenue? run cost? both?)
2. [ ] Set the royalty rate (OMLA default: 30% of greater of revenue or run cost)
3. [ ] Declare lineage: list all parent models and their OMLA manifests
4. [ ] Set royalty split: what % to each contributor (you + upstream creators)
5. [ ] Publish payment pointers (wallet addresses for each recipient)
6. [ ] Sign the manifest and publish to OMLA registry
7. [ ] Provide the open resolver configuration for your lineage
8. [ ] Document the self-metering expectations for commercial users
9. [ ] Attach any sublicenses for acceptable-use restrictions (OMLA governs payment, not use)
```

### 6. OMLA vs Apache 2.0 Decision Framework

**Choose Apache 2.0 when:**
- Maximum adoption is the only goal (Mistral, Qwen3.8-27B strategy)
- Revenue comes from hosted inference, enterprise skins, or consulting — not the model license itself
- The model is a commodity; the moat is infrastructure, brand, or network effects
- Enterprise legal teams must approve in 30 minutes

**Choose OMLA when:**
- The model creators need direct compensation to sustain development
- The lineage is deep (multiple fine-tune generations) and upstream creators deserve payment
- Privacy of usage data matters (OMLA's no-records architecture)
- The model is differentiated enough that commercial users will pay 30%
- You want to keep the model open-weight while still capturing value

**Avoid when:**
- The model has no commercial use cases (use MIT or Apache 2.0)
- Commercial users cannot self-meter reliably (requires operational discipline)
- The lineage is unclear or contested (recursive splits need clean manifests)

## References
- See [references/omla-evidence-base.md](references/omla-evidence-base.md) for full evidence: OMLA license terms, lineage split mechanics, comparison with RSI and Apache 2.0, fine-tune retention rules, self-metering workflow, and A-Tech alignment analysis.