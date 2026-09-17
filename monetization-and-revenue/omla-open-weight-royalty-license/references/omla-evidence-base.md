# OMLA Open-Weight Royalty License — Evidence Base

## Source
OMLA (Open Model License Agreement), omla-ai.org, 2026. A community-built royalty license for open-weight AI models, created by tinkerers who found their work inside other people's products.

## The Problem OMLA Solves

Apache 2.0 won the open-weight license war in 2026 (Gemma 4, Qwen 3.5, Mistral Large 3, Yi all ship Apache 2.0). But Apache 2.0 has a structural gap: it permits unrestricted commercial use with no compensation obligation to the model creators. This means:

1. Original model creators receive zero revenue when their weights are used commercially
2. Fine-tuners can build on open weights and monetize without paying upstream creators
3. The entire open-weight ecosystem relies on indirect monetization (hosted inference, enterprise skins, consulting) — the model itself generates no direct revenue

OMLA addresses this by making commercial use pay for itself through a self-metered, direct-payment royalty system.

## Core License Terms

### Non-Commercial Use (Free)
Defined as activities that do not directly or indirectly generate revenue, financial contributions, or other material benefit. Includes:
- Academic research
- Personal projects
- Internal training and development of future models
- Hobby experimentation

### Commercial Use (30% Royalty)
Any use that seeks or results in financial gain — whether through:
- Sales
- Subscriptions
- Advertising
- Paid services
- Fundraising
- Sponsorships
- Enhancing the value of another product or service (e.g., a boat tour using AI to announce fish sightings)

**Royalty amount:** The greater of 30% of revenue OR 30% of run cost, self-assessed by the commercial user.

## The Recursive Lineage Split

OMLA's most distinctive feature is recursive royalty attribution through model lineage:

- A fine-tune retains **at most 5%** of the royalty
- The remainder flows upstream through the published lineage splits of parent models
- Deep upstream creators are paid automatically through recursive resolution
- Each model publishes a signed manifest with identity, lineage, royalty split, and public payment pointers

**Example:**
```
Model A (original creator) → fine-tune → Model B (fine-tuner) → fine-tune → Model C (derivative)

Commercial user of Model C pays 30% royalty
C's manifest: 5% to C's creator, 95% flows upstream
B's manifest: 5% to B's creator, 90% flows upstream
A's manifest: 100% to A's creator

Result: A receives 90% of the royalty, B receives 4.75%, C receives 4.75% (approximately)
```

## The Four-Step System

### Step 1: Creators Publish & Sign
A creator registers a model with a signed manifest containing:
- Identity (who created the model)
- Lineage (what parent models it was fine-tuned from, if any)
- Royalty split (what percentage each contributor receives)
- Public payment pointers (wallet addresses)

One record, signed end-to-end.

### Step 2: OMLA Serves the Registry
- Every manifest is published as signed, mirrorable JSON snapshots
- Thin read API
- **OMLA keeps no records of usage, payers, or payments** — because it never receives them

### Step 3: Users Meter & Resolve
- Commercial users meter their own usage
- Self-assess the 30% royalty on the greater of revenue or run cost
- Run the open resolver to get a percentage per wallet

### Step 4: Users Pay Wallets Directly
- Payment goes straight to each creator's published payment pointers
- No reporting, no accounts, no records at OMLA — ever

## Privacy Architecture

OMLA's design choice to never receive usage, payer, or payment data is a deliberate privacy feature:
- No central database of who is using which model
- No records of commercial usage volume
- No payment intermediation
- Commercial users self-meter and self-assess

This contrasts with platform-level revenue-share models (RSI, Qwen3.8-Max custom license) where the platform tracks all usage and payments.

## Comparison with Adjacent Models

### OMLA vs Apache 2.0
| Dimension | Apache 2.0 | OMLA |
|-----------|------------|------|
| Commercial use | Free | 30% royalty |
| Creator compensation | None | Direct, recursive |
| Enterprise legal friction | Minimal | Medium |
| Adoption friction | Lowest | Higher (self-metering) |
| Best for | Maximum adoption, indirect monetization | Direct creator compensation |

### OMLA vs RSI (Revenue-Sharing as Infrastructure)
| Dimension | RSI | OMLA |
|-----------|-----|------|
| Level | Platform (platform takes % of app revenue) | Model (commercial users pay model creators) |
| Who pays | Developer pays platform | Commercial user pays model creators |
| Intermediary | Platform (tracks and distributes) | None (direct payment, OMLA sees nothing) |
| Privacy | Low (platform tracks all) | Maximum (no central records) |
| Optimal commission | α* = (1+c)/2 per Mondjo 2026 | Fixed at 30% |

### OMLA vs Qwen3.8-Max Custom License
| Dimension | Qwen3.8-Max | OMLA |
|-----------|-------------|------|
| Trigger | ≥$50M MAU or tens of millions in monthly revenue | Any commercial use |
| Payment target | Alibaba (via contract) | All creators in lineage (direct) |
| Fine-tune rights | Not specified | Explicit (≤5% retention) |
| Lineage attribution | None | Recursive splits |

## Design Principles

1. **No copyleft, no lock-in** — Keep fine-tunes private; just honor royalties on commercial use
2. **Direct settlement** — No intermediary touches the money
3. **Recursive fairness** — Upstream creators automatically compensated through lineage
4. **Privacy by architecture** — OMLA sees nothing by design
5. **Self-assessment** — Commercial users meter their own usage and self-assess the royalty
6. **One model → one manifest** — A single signed public record contains everything a payer needs

## Limitations and Risks

- **Self-metering trust problem:** Commercial users self-assess; no enforcement mechanism beyond audit risk
- **Adoption friction:** Self-metering and royalty accounting adds operational burden vs Apache 2.0's zero-cost commercial use
- **Fine-tune retention cap (5%):** May disincentivize fine-tuners who add significant value; some may prefer Apache 2.0 base models
- **No usage governance:** OMLA governs payment, not acceptable use; model makers must attach separate sublicenses for use restrictions
- **Legal complexity:** Defining "commercial use" and "run cost" precisely requires careful legal drafting
- **No enforcement:** Without a central authority, underpayment relies on audit risk and reputation

## A-Tech Values Alignment

- **Open-source AI:** OMLA keeps models open-weight while enabling creator compensation — aligns with A-Tech's open-source ethos while addressing the sustainability gap
- **Data privacy:** OMLA's "no records" architecture is the strongest privacy design among monetization models — no central usage database exists
- **Financial freedom:** Recursive lineage splits mean small creators who contribute upstream components get paid, not just large labs — democratizes AI revenue
- **Practical implementation:** The four-step workflow and signed manifest format are concrete and implementable; the open resolver is a real tool

## A-Tech Applications

- **A-Coder:** If A-Tech releases open-weight coding models, OMLA provides a path to direct compensation without closing the weights. The recursive split means contributors to A-Coder's training pipeline (data providers, fine-tuners) receive automatic payment.
- **Be Practical:** OMLA can be taught as a case study in the open-source AI monetization curriculum, alongside Apache 2.0, RSI, and the Give-Away/Keep Matrix.
- **Builder's Club:** The signed manifest and lineage tracking pattern aligns with Builder's Club's community attribution and contribution tracking goals.

## Composes With
- `give-away-keep-matrix-oss-ai` — strategic framework for what to open vs keep; OMLA is a "keep" mechanism for the revenue side
- `open-source-ai-revenue-share-trend` — RSI platform-level model; OMLA is the model-level complement
- `open-source-license-economics-2026` — broader license landscape
- `open-source-ai-monetization-mastery-2026` — comprehensive monetization playbook
- `revenue-sharing-as-infrastructure-model` — the platform-level counterpart to OMLA's model-level approach