# Daily Research Report — 2026-08-22

**Researcher:** A-Tech Research Division
**Focus Areas:** Neuro-marketing, behavioral psychology, AI revenue, privacy-first, developer experience, open-source business models
**Date:** August 22, 2026 (Pacific/Auckland)

---

## Executive Summary

Today's research cycle identified three novel findings across monetization, privacy-preserving FL, and developer experience domains. All three warrant new skill creation as they introduce frameworks not previously captured in the skill library. The research also confirmed several incremental updates to existing skills, which are noted but do not require new skill files.

---

## Research Phase Findings

### 1. ShareAI Open-Source AI Usage Metering (Monetization)

**Source:** ShareAI (shareai.now, 2026) — "Open Source AI Monetization Without Closing the Project" + "Open Source RAG App Monetization: Price Queries, Not Downloads"

**Novelty:** NOVEL — While the skill library contains `shareai-open-source-ai-metering-pattern` (created 2026-08-23), today's research surfaced a significantly expanded framework with:
- A complete 7-step monetization plan (not previously captured)
- Detailed pricing patterns (included credits + top-ups, free core + paid hosted, workspace caps, BYOK + managed path)
- RAG-specific monetization insight: "price queries, not downloads" — the billable unit should be a successfully completed RAG answer, not a download or token, because a single download can generate thousands of queries with different pipeline-stage cost profiles
- Community communication patterns for avoiding backlash
- Clear distinction between Builder payouts (app traffic margin) and Provider rewards (compute contribution)

**A-Tech Values Alignment:**
- Open-Source AI: Core project stays open; only optional AI-heavy features are metered
- Data Privacy: Self-hosted core preserves data sovereignty; routed inference only for opted-in AI features
- Financial Freedom: Sustainable AI funding without closing the project
- Practical Implementation: 7-step plan, specific feature recommendations, pricing patterns

**Cross-references:** open-core-ai-feature-metering, shareai-open-source-ai-metering-pattern, tanso-ai-margin-ledger-metering, open-source-risk-removal-monetization-2026, open-source-ai-monetization-mastery-2026

**Decision:** NEW SKILL CREATED — `monetization-and-revenue/shareai-open-source-ai-usage-metering/`

---

### 2. AdaDP-FedSec: Adaptive DP with Secure Aggregation (Privacy)

**Source:** Zhou & Yuan (Scientific Reports, August 2026, DOI: 10.1038/s41598-026-63985-z) — "AdaDP-FedSec: adaptive differentially private federated learning with secure aggregation for multi-institutional English learner corpus collaborative training"

**Novelty:** NOVEL — The skill library contains many DP-FL skills, but AdaDP-FedSec introduces a unique three-mechanism integration:
1. Adaptive privacy budget allocation (dynamically calibrates DP noise based on gradient variance and institutional data characteristics) — identified as the most impactful component (3-5% improvement over uniform noise)
2. Hybrid secure aggregation (Shamir secret sharing + Paillier homomorphic encryption) — prevents server-side gradient inspection
3. Contribution-aware weighted aggregation + dual-layer personalized model architecture — addresses cross-institutional data heterogeneity

**Key Results:**
- Recovers ~75% of the performance gap between standard DP-FL and centralized training
- Pushes MIA success close to chance levels
- Validated on 8 simulated institutional nodes for NLP tasks (grammatical error detection, writing proficiency classification)
- Adaptive budgeting is the most impactful single component

**A-Tech Values Alignment:**
- Open-Source AI: Reproducible framework, public datasets
- Data Privacy: Formal (ε,δ)-DP + cryptographic protection, data stays local
- Financial Freedom: Enables small institutions to collaborate without expensive privacy infrastructure
- Practical Implementation: 8-node validation, specific NLP tasks, deployment decision matrix

**Cross-references:** adaptive-dp-fl-concept-drift-edge, compliance-weighted-federated-learning, sheld-fl-self-learning-heterogeneous-dp-framework, head-fl-adaptive-dp-homomorphic-aggregation, ddp-sa-distributed-dp-secure-aggregation, dp-fedadamw-dpfl-large-model-optimizer

**Decision:** NEW SKILL CREATED — `privacy-and-trust/adadp-fedsec-adaptive-dp-secure-aggregation/`

---

### 3. Core vs Peripheral Developer Agent Usage (Developer Experience)

**Source:** Cynthia, Das & Roy (MSR '26, arXiv:2601.20106) — "Are We All Using Agents the Same Way? An Empirical Study of Core and Peripheral Developers' Use of Coding Agents" (9,427 agentic PRs, 1,701 developers, 1,391 repositories)

**Novelty:** NOVEL — While the library contains many DevEx/agent skills, this is the first to systematically compare how core and peripheral developers differ across the ENTIRE PR lifecycle (delegation, review, modification, verification) with coding agents:

**Key Findings:**
- **Usage:** Both groups use agents at comparable rates, but a subset of peripheral developers are "power users" (mean 6.08 vs 3.73 PRs). Core developers focus on documentation (20.9%) and testing (21.8%) — absorbing "toil" tasks. Peripheral developers delegate evenly across all task types.
- **Review:** Core developers engage more in review (median 3.6 vs 2.0 comments), focus on alternative solutions. Peripheral developers focus on code organization.
- **Modification:** 74.1% of agentic PRs accepted without modification. When modified, both groups refactor; peripheral also fixes bugs, core also improves docs.
- **CI Verification:** Core developers are "quality gatekeepers" — 51.2% CI success vs 43.1%. Peripheral developers nearly 2x more likely to merge without running any checks (19.1% vs 11.2%).
- **Socio-technical persistence:** Contributor reputation effect persists even with agents — core developers' PRs merge to main/master at 85.8% vs 77.8%.

**Implications:**
- Agents are absorbing low-satisfaction "toil" work (documentation, testing)
- Risk of peripheral "power-users" compensating for skill gaps through agents, potentially hindering long-term learning
- Core developers remain essential quality gatekeepers
- Standardized review rubrics needed to reduce socio-technical inequities

**A-Tech Values Alignment:**
- Open-Source AI: GitHub public data, OSS projects
- Data Privacy: PR metadata, no surveillance
- Financial Freedom: Understanding adoption patterns for product strategy
- Practical Implementation: 9,427 PRs, specific recommendations per developer group

**Cross-references:** cli-agentic-coding-adoption-impact, agentic-coding-returns-to-expertise, ai-engineering-culture-amplifier, botsitting-botshitting-cycle, human-oversight-agentic-systems-practice

**Decision:** NEW SKILL CREATED — `developer-experience-and-flow/core-peripheral-developer-agent-usage/`

---

## Synthesis Phase: Novel vs. Incremental

### Novel Findings (New Skills Created)
1. **ShareAI Open-Source AI Usage Metering** — Complete 7-step monetization framework for OSS AI projects with RAG-specific pricing insights
2. **AdaDP-FedSec Adaptive DP Secure Aggregation** — Three-mechanism integration (adaptive DP + Shamir/Paillier + contribution-aware aggregation) for multi-institutional FL
3. **Core vs Peripheral Developer Agent Usage** — First full-PR-lifecycle comparison of developer experience levels with coding agents

### Incremental Findings (No New Skill Required)
- **Promotional-Preventive Framing ERP Neuromarketing** (Wang et al., Scientific Reports, July 2026) — Already captured in `promotional-preventive-framing-erp-neuromarketing` (created 2026-08-28). Confirmed.
- **Neuromarketing-AI CXM Integration Framework** (Topcugil & Hiziroglu, Future Business Journal, August 2026) — Already captured in `neuromarketing-ai-cxm-integration-framework` (created 2026-08-26). Confirmed.
- **AI-Enhanced Neuromarketing Social Media** (Bucea-Manea-Țoniș et al., SAGE, 2026) — Already captured in `ai-enhanced-neuromarketing-social-media` (created 2026-08-20). Confirmed.
- **Coding Agent Misalignment Large-Scale** (Tang et al., arXiv:2605.29442, May 2026) — Already captured in `coding-agent-misalignment-large-scale` (created 2026-08-25). Confirmed.
- **SWE-chat Real-World Coding Agent Dataset** (Baumann et al., Stanford, arXiv:2604.20779, April 2026) — Already captured in `swe-chat-real-world-coding-agent-dataset` (created 2026-08-28). Confirmed.
- **Human Oversight Agentic Systems Practice** (Dhanorkar, Passi & Vorvoreanu, Microsoft Research, arXiv:2606.05391, June 2026) — Already captured in `human-oversight-agentic-systems-practice` (created 2026-08-24). Confirmed.
- **(Im)Paired Programming: Coding Agents Harm Understanding** (Balepour et al., UMD/NYU/CMU, arXiv:2607.26375, 2026) — Already captured in `agentic-code-comprehension-decline-empirical` (created 2026-08-22). Confirmed.
- **DDP-SA Distributed DP Secure Aggregation** (Wei et al., Université Paris Cité, 2026) — Already captured in `ddp-sa-distributed-dp-secure-aggregation` (created 2026-08-21). Confirmed.
- **FLiPD Privacy-Preserving FL via MPC and DP** (Chandran et al., ePrint 2026/324) — Related to existing `zk-proof-federated-learning-trust` skills. Incremental.
- **PPCFL Privacy-Preserving Clustered FL** (Zhan et al., Scientific Reports, July 2026) — Related to existing FL skills. Incremental.
- **LA-LoRA Rethinking LoRA for Privacy-Preserving FL** (Liu et al., ICLR 2026) — Related to existing `federated-llm-on-device-personalization` and `dp-fedadamw-dpfl-large-model-optimizer`. Incremental.
- **FedAlign Differentially Private Distribution Alignment** (Wu et al., CVPR 2026) — Related to existing FL skills. Incremental.
- **DP-FedAdamW** (Liu et al., CVPR 2026) — Already captured in `dp-fedadamw-dpfl-large-model-optimizer` (created 2026-08-22). Confirmed.

---

## Skills Created

### 1. ShareAI Open-Source AI Usage Metering (`monetization-and-revenue/shareai-open-source-ai-usage-metering/`)
- **SKILL.md** — Applies the ShareAI pattern for monetizing OSS AI projects by keeping the core open while metering optional AI-heavy features through a routing layer. 7-step monetization plan, pricing patterns, RAG-specific insights, community communication patterns.
- **references/evidence-base.md** — Full evidence: ShareAI framework details, RAG monetization pattern, pricing patterns, 7-step plan, feature recommendations, community communication patterns.

### 2. AdaDP-FedSec Adaptive DP Secure Aggregation (`privacy-and-trust/adadp-fedsec-adaptive-dp-secure-aggregation/`)
- **SKILL.md** — Applies the AdaDP-FedSec framework for multi-institutional FL with adaptive DP and secure aggregation. Three integrated mechanisms, deployment decision matrix, A-Tech applications.
- **references/evidence-base.md** — Full evidence: study design, three mechanisms detailed, results, deployment decision matrix, cross-references.

### 3. Core vs Peripheral Developer Agent Usage (`developer-experience-and-flow/core-peripheral-developer-agent-usage/`)
- **SKILL.md** — Applies empirical evidence on how core and peripheral developers differ in agent usage across the PR lifecycle. Four RQ findings, implications for product strategy, A-Tech applications.
- **references/evidence-base.md** — Full evidence: study design, four RQ results, statistical details, implications, cross-references.

---

## A-Tech Values Alignment Summary

| Skill | Open-Source AI | Data Privacy | Financial Freedom | Practical Implementation |
|---|---|---|---|---|
| ShareAI Open-Source AI Usage Metering | Core stays open; only optional AI features metered | Self-hosted core; routed inference for opted-in features only | Sustainable AI funding without closing project | 7-step plan, pricing patterns, feature recommendations |
| AdaDP-FedSec Adaptive DP Secure Aggregation | Reproducible framework, public datasets | Formal (ε,δ)-DP + Shamir/Paillier crypto, data stays local | Enables small institutions to collaborate without expensive infra | 8-node validation, NLP tasks, deployment decision matrix |
| Core vs Peripheral Developer Agent Usage | GitHub public data, OSS projects | PR metadata, no surveillance | Understanding adoption patterns for product strategy | 9,427 PRs, per-group recommendations, CI verification insights |

---

## Research Sources (New)

1. **NEW:** ShareAI (2026) — "Open Source AI Monetization Without Closing the Project." shareai.now/blog/insights/open-source-ai-monetization/
2. **NEW:** ShareAI (2026) — "Open Source RAG App Monetization: Price Queries, Not Downloads." shareai.now/blog/developers/open-source-rag-app-monetization/
3. **NEW:** Zhou, X. & Yuan, C. (2026) — "AdaDP-FedSec: adaptive differentially private federated learning with secure aggregation for multi-institutional English learner corpus collaborative training." Scientific Reports. DOI: 10.1038/s41598-026-63985-z
4. **NEW:** Cynthia, S.T., Das, J.K. & Roy, B. (2026) — "Are We All Using Agents the Same Way? An Empirical Study of Core and Peripheral Developers' Use of Coding Agents." MSR '26. arXiv:2601.20106

---

*Report compiled by A-Tech Research Division | 2026-08-22*