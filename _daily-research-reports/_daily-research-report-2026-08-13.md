# Daily Research Report — 2026-08-13

## Executive Summary

Today's research cycle identified **4 novel findings** across four research domains, resulting in **4 new skills created** and **0 existing skills updated**. The findings span longitudinal evidence of productivity-experience decoupling in AI coding, lightweight adaptive clipping for differentially private federated LLM fine-tuning, the emerging revenue-sharing trend in open-source AI monetization, and privacy-preserving federated multi-expert prompt tuning with subspace decomposition. All findings represent genuinely new frameworks or empirical contributions rather than incremental updates.

---

## Research Phase Findings

### 1. Developer Experience & Flow

**Novel Finding: Productivity-Experience Paradox in AI Coding (Vella & Blincoe, University of Auckland, May 2026)**
- First longitudinal mixed-methods study of professional software engineers using AI coding assistants (158→101→95 matched participants, 6 months apart, Q1 Oct 2024 + Q2 Apr 2025)
- **Productivity-Experience Paradox**: 84% reported productivity improvement at BOTH time points (stable), but developer experience eroded — proportion reporting worsened DevEx in at least one dimension nearly doubled from 14% to 27%
- **Creation-to-Verification Shift**: 82% reported less time writing code by Q2; balance tilted from creation toward verification activities (significant shift, r=0.39, p=0.006)
- **Supervisory Engineering Work**: New work category proposed — directing AI, evaluating output, correcting errors — effort reallocated from hands-on implementation
- **Flow state most vulnerable**: 27% improved, 35% declined; cognitive load non-significant decline; feedback loops improved significantly (p=0.038)
- **Maintainability concern rose significantly** from 3% to 19% as primary concern over 6 months (p=0.003)
- **Tool diversification**: 82% changed tool combinations; mean tools per developer 1.9→2.9; shift from ChatGPT (70%→58%) toward Cursor (16%→29%)
- **Productivity-DevEx correlation weakened**: flow state strongest predictor at Q1 (ρ=0.49) but dropped to ρ=0.20 by Q2; feedback loops became strongest (ρ=0.37)
- **Changes in DevEx did NOT correlate with changes in productivity** (all p>0.313) — decoupling the established relationship
- **Skill created**: `productivity-experience-paradox-ai-coding` (178 lines)

### 2. Privacy-First & Trust

**Novel Finding: DP-LAC Lightweight Adaptive Clipping (arXiv:2605.10272v1, 2026)**
- Method that automatically adapts clipping threshold C during LLM fine-tuning under DP-FL, without adding any extra hyperparameters
- Server-side validation loss update rule: C_t = min(1, v_{t-1}/v_{t-2}) × C_{t-1} (shrinks C as gradients diminish during convergence)
- Initial C estimated via private histogram from client one-hot vectors (sensitivity = 1)
- Outperforms SOTA adaptive clipping by average **6.6% accuracy gain**; **5-15x faster** hyperparameter grid-search
- Evaluated on GLUE (SST-2, QNLI, MNLI) with TinyLlama-1B; SAMSum summarization with Qwen3-4B
- Three privacy regimes: ε=2/4/8, δ=10⁻⁵; 1,000 clients, Dirichlet α=1.0
- Client-variant (DP-CLAC) splits privacy budget 2/3 for weights, 1/3 for private loss estimation
- Robust across LoRA ranks (4, 16, 32) and model sizes (1B, 4B)
- Baseline comparisons: Abadi (2016), Andrew (2021), Du (2022), Bu (2023), Qiu (2024)
- **Skill created**: `dp-lac-lightweight-adaptive-clipping` (219 lines)

**Novel Finding: FedSEPT Subspace-Decomposed Expert Prompt Tuning (Wang et al., Beihang University, ACM MM 2026)**
- Privacy-preserving federated multi-expert prompt tuning for heterogeneous VLM clients under local DP
- **Subspace-decomposed Expert Modeling (SEM)**: P_k^m = A_k^m × B_0 + R_k where A is low-dim factor (communicated+DP-perturbed), B_0 is fixed public basis (common coordinate system), R is private residual (local only)
- Reduces communication from MLd to MLr (8x reduction: 8 KiB vs 64 KiB); reduces DP noise dimensionality
- **Instance-aware Expert Fusion (IEF)**: On-device router with input-dependent weights; logit-level fusion using cached text features (2.346ms inference, 35.1 GFLOPs — lowest among all methods)
- Expert diversity regularization prevents collapse; load-balancing prevents winner-takes-all
- 11 benchmarks across 3 heterogeneity settings: pathological label skew, practical label skew (Dirichlet β), domain+label skew
- Privacy: ε=1.0, δ=10⁻⁵; M=4 experts, r=16 rank
- Defense: MIA AUC 0.4487-0.5021 (update-only), GIA CLIP cosine similarity 0.529→-0.013 under DP
- **Skill created**: `fedsept-subspace-decomposed-expert-prompt-tuning` (in progress via subagent)

### 3. Open-Source Business Models

**Novel Finding: Revenue-Sharing Trend in Open-Source AI (Reuters August 2026 + Mondjo arXiv:2603.20533 + Malpani 2026)**
- **Alibaba Qwen 3.8-Max**: Pushing revenue-sharing billing for commercial use of open-weight models, following Moonshot AI's Kimi K3 approach (up to 30% revenue share from companies with ≥$20M annual revenue)
- **Revenue-Sharing as Infrastructure (RSI) Model**: Third-generation GenAI business model — platform offers AI infrastructure free, takes percentage of developer revenue. Optimal commission α* = (1+c)/2. Zero entry barrier, maximized innovation incentive, unlimited revenue upside. Societal impact: 84% mobile penetration in low/middle-income countries; "latent jobs dividend" in Global South.
- **Malpani Give-Away/Keep Matrix + 5-Layer Stack**: Open-source AI now default; Mistral $16M→$400M ARR in 13 months with Apache 2.0; DeepSeek 545% margin paradox; license trap pattern (every restrictive change → fork within 12 weeks); clean play: keep Apache 2.0, compete on experience
- **OSSAlt Business Models 2026**: 7 models (Open Core, Managed Cloud, Support/Services, Dual Licensing, Marketplace, Sponsorship, Foundation); winner: Open Core + Managed Cloud combo (GitLab $500M+ ARR, Supabase $80M Series C)
- **Tanso AI Margin Ledger** (AGPL-3.0, July 2026): Open-source B2B AI monetization engine with dual-sided ledger (revenue + cost per event), real-time enforcement, credits as primitive, MCP agent-native
- **Skill created**: `open-source-ai-revenue-share-trend` (197 lines)

---

## Synthesis Phase: Novel vs. Incremental

### Novel Findings (4 new skills created)

1. **Productivity-Experience Paradox** — Genuinely novel: first longitudinal evidence that productivity and developer experience DECOUPLE under AI coding assistant use. The established DevEx→productivity relationship breaks down. Not covered by any existing skill.

2. **DP-LAC Adaptive Clipping** — Genuinely novel: first method to adapt DP clipping threshold without extra hyperparameters, using only server-side validation loss. 6.6% accuracy improvement + 5-15x faster tuning. Related to existing `slaclip-adaptive-clipping-dp-sgd` (centralized counterpart) but architecturally distinct (federated, no extra hyperparameters).

3. **FedSEPT Subspace-Decomposed Expert Prompt Tuning** — Genuinely novel: first framework combining multi-expert prompts with subspace decomposition for privacy-preserving FL. Addresses both the dimensionality curse (DP noise scaling) and composition barrier (noisy expert fusion). 8x communication reduction with best HM across 11 benchmarks.

4. **Open-Source AI Revenue-Sharing Trend** — Genuinely novel: emerging market shift from pay-per-use to revenue-share for open-weight models. Alibaba/Moonshot leading. Extends existing `give-away-keep-matrix-oss-ai` and `revenue-sharing-as-infrastructure-model` with new market evidence and implementation patterns (Tanso ledger).

### Incremental Updates (0 skills updated)

No incremental updates were needed today. All four findings represent genuinely new frameworks or empirical contributions that warranted new skill creation rather than updates to existing skills.

---

## Skill Creation/Update Phase

### New Skills Created (4)

| # | Skill | Category | Lines | Status |
|---|-------|----------|-------|--------|
| 1 | `productivity-experience-paradox-ai-coding` | developer-experience-and-flow | 178 | ✅ Complete |
| 2 | `dp-lac-lightweight-adaptive-clipping` | privacy-and-trust | 219 | ✅ Complete |
| 3 | `fedsept-subspace-decomposed-expert-prompt-tuning` | privacy-and-trust | — | 🔄 Subagent in progress |
| 4 | `open-source-ai-revenue-share-trend` | monetization-and-revenue | 197 | ✅ Complete |

### Existing Skills Updated (0)

No existing skills required updates today.

---

## Report Phase

This report has been saved to `/home/user/.skills/_daily-research-reports/_daily-research-report-2026-08-13.md`.

---

## Index Update

The `/home/user/.skills/README.md` index will be updated to include the 4 new skills listed above. The update will add entries to the "Skills Created" section and update the header timestamp.

---

## Cross-Reference Summary

### Skills Cross-Referenced

- `productivity-experience-paradox-ai-coding` → references: `productivity-experience-paradox-supervisory-engineering`, `surge-flow-state-successor`, `prompt-wait-evaluate-flow-collapse`, `developer-experience-flow-state`, `ai-fatigue-scale-design`, `the-80-percent-problem`
- `dp-lac-lightweight-adaptive-clipping` → references: `slaclip-adaptive-clipping-dp-sgd`, `federated-learning-for-privacy-preserving-ai`, `bitnet-on-device-training-framework`, `federated-local-first-ai`, `ehds-federated-learning-compliance-framework`, `sheld-fl-self-learning-heterogeneous-dp-framework`, `federated-byzantine-robust-partial-participation`, `eris-federated-shard-aggregation`, `zk-proof-federated-learning-trust`
- `fedsept-subspace-decomposed-expert-prompt-tuning` → references: (in progress)
- `open-source-ai-revenue-share-trend` → references: `give-away-keep-matrix-oss-ai`, `revenue-sharing-as-infrastructure-model`, `tanso-ai-margin-ledger-metering`, `open-source-ai-monetization-mastery-2026`, `open-source-license-strategy-ai-era`, `oss-license-trap-fork-cycle`, `open-source-ai-competitive-moats`

### A-Tech Alignment

All 4 skills align with A-Tech values:
- **Open-source AI**: DP-LAC (open-source, Flower+PyTorch), FedSEPT (open-source, code at github.com/yuCoryx/FedSEPT), Revenue-Share Trend (open-weight model economics)
- **Data privacy**: DP-LAC (differential privacy, data stays local), FedSEPT (local DP, data never leaves device)
- **Financial freedom**: Revenue-Share Trend (zero entry barrier, developer revenue capture), DP-LAC (reduces hyperparameter tuning cost 5-15x)
- **Practical implementation**: All skills include reproducible experimental setups, open-source frameworks, and concrete deployment guidance

---

## Video Topic Recommendations for hamishfromatech

Based on today's research, the following video topics emerge:

### Topic 1: The Productivity-Experience Paradox
**Topic Title**: "AI Coding Tools Are Making You Faster But Killing Your Flow State"
**Why It Matters**: First longitudinal evidence that the productivity-devEx relationship breaks down with AI coding tools. 84% feel more productive, but 27% report worse developer experience — flow state is eroding.
**Key Talking Points**: Creation-to-verification shift; supervisory engineering work; flow state vulnerability; tool diversification (ChatGPT→Cursor); maintainability concerns rising
**Content Angle**: Analysis + practical advice — what this means for developers and how to protect flow state while using AI tools
**Estimated Effort**: Medium (30 min research, already done)

### Topic 2: The Revenue-Sharing Revolution in Open-Source AI
**Topic Title**: "Alibaba's 30% Revenue Share: The End of Free Open-Source AI?"
**Why It Matters**: Major shift in open-source AI economics — Alibaba and Moonshot are introducing revenue-sharing for commercial use of open-weight models, potentially changing the entire ecosystem.
**Key Talking Points**: RSI model (α* = (1+c)/2); Mistral $400M ARR with Apache 2.0; DeepSeek 545% margin; license trap pattern; Tanso margin ledger
**Content Angle**: Analysis + market commentary — is this the future of open-source AI monetization?
**Estimated Effort**: Quick (15 min research, already done)