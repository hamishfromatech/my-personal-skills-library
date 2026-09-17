---
name: sovereign-pii-masking-middleware
description: Applies the Sept 2026 wave of privacy-first middleware that masks PII before any LLM call — Septum (MIT, docker-compose, three-layer detection Presidio+NER+optional local model, approval gate on every cloud prompt, 17 regulation packs, 7-module zone architecture) and ElyAgent (Elastic License v2, native PII anonymization, structural HITL on 30+ tool categories, encrypted vault, immutable audit trail) — the reference pattern for "your data never leaves, your AI still works." Use when [designing PII masking before LLM calls, choosing a privacy middleware for teams handling sensitive docs, implementing approval gates on cloud prompts, or building regulation-pack compliance surfaces (GDPR/KVKK/HIPAA)]. NOT for [federated learning (see federated-learning-for-privacy-preserving-ai), fully-local agent runtimes (see privacy-preserving-local-ai), or TEE hardware isolation (see plugclaw-tee-consumer-agent-hardware)].
---

# Sovereign PII-Masking Middleware: The Data Leaves, the PII Doesn't

## Overview
The Sept 2026 wave of privacy-first middleware (Septum, MIT, byerlikaya/Septum; ElyAgent, Elastic License v2, franckolv-dev/ElyAgent) formalizes the **"mask-before-call"** pattern: keep the agent's intelligence local or routed, but strip PII from every prompt before it reaches a cloud LLM, restore the values locally before display. The reference architecture: three-layer detection (Presidio + NER + optional local model), an approval gate where users review the masked prompt, retrieved chunks, and assembled request before any LLM call, deterministic placeholder reversal on the response, and a zone architecture where air-gapped modules handle raw PII and internet-facing modules never see it.

## When to Use
- Designing PII masking before any LLM call in a document/RAG product
- Building approval gates on cloud LLM requests for teams handling regulated data
- Choosing between fully-local agents and masking middleware for compliance-constrained deployments
- Implementing regulation-pack surfaces (GDPR, KVKK, CCPA, HIPAA, LGPD, PIPL...)

## Core Process / Workflow
1. **Mask before the call, restore before display.** Detect PII in documents AND typed prompts; mask deterministically with placeholders ([PERSON_1], [IBAN_1]); the cloud model answers in placeholders; restore locally for display.
2. **Approval gate on every send.** Show masked question, retrieved chunks, assembled prompt side by side; the user approves before anything leaves. Nothing is sent without review — the consent-bearing surface is the send button.
3. **Zone architecture.** Air-gapped modules handle raw PII (core, MCP, API, web); the bridge transports only masked placeholders; internet-facing modules (gateway, audit) never import core. Raw PII and the internet-facing surface never coexist in one module.
4. **Regulation packs as the compliance surface.** 17 packs (GDPR, KVKK, CCPA, HIPAA, LGPD, PIPEDA, PDPA, APPI, PIPL, POPIA, DPDP, UK GDPR...) run simultaneously with "most restrictive wins"; region-specific validators (TCKN, Aadhaar, NRIC, CNPJ) as checksum logic.
5. **Structural HITL on irreversible actions.** 30+ tool categories (mail send, file delete, SSH, sharing) pause for explicit approval — allow once / deny once / ban permanently, persisted across sessions. The approval is cryptographically the action that runs (fail-closed fingerprint).
6. **Vault + audit trail.** AES-256-GCM encrypted vault, per-user key derivation; immutable JSON-lines audit trail with entity-detection metrics; no raw PII in audit events.

## Key Evidence
- Septum (byerlikaya/Septum, MIT): three-layer detection (Presidio + NER + optional Ollama), approval gate, 17 regulation packs, 7-module zone architecture (air-gapped core/MCP/API/web; gateway/audit never import core); hybrid retrieval BM25 + FAISS with RRF; MCP server for Claude Desktop/Cursor.
- ElyAgent (franckolv-dev/ElyAgent, Elastic License v2, free for personal and internal business use): native PII anonymization before any model call; structural HITL on 30+ tool categories; AES-256-GCM zero-knowledge vault; immutable audit trail; 10 channels incl. voice, iOS, Android, Telegram, WhatsApp, Slack, Discord; 190+ tools; MCP client + server; self-hosted on user hardware.
- Shared design language: **"Your data never leaves. Your AI still works."** — the mask-before-call pattern with approval gates and zone separation.
- Context: the pattern fills the gap left by cloud agents (ChatGPT/Claude/Gemini) that require sending raw data to US servers, and the fully-local agent wave (Kryzz AI v1.1 MIT, KIRA Superapp Apache-2.0 on Apple silicon, Sutra Apache-2.0) that avoids the cloud entirely — masking middleware is the middle path for teams that need cloud intelligence but can't send PII.
- Compliance surfaces: Septum ships 17 regulation packs with most-restrictive-wins logic; ElyAgent ships DPIA templates + DPA availability.

## Pairs-with
- `privacy-first-ai-pipeline-defense` (the pipeline posture this instantiates)
- `consent-fatigue-progressive-permissioning` (the approval-gate UX this implements)
- `privacy-preserving-local-ai` (the fully-local alternative)
- `federated-local-first-ai` (the federated middle path)
- `privacy-first-personalization-2026` (privacy-first personalization posture)
- `mcp-security-trust` (the MCP surfaces both implement)
- `trust-design` (the trust-architecture frame)

## A-Tech Alignment
- **Open source:** both projects are open source (Septum MIT; ElyAgent Elastic License v2) with auditable source; the pattern is implementable without vendor lock-in.
- **Privacy:** the strongest privacy-first agent posture of the Sept 2026 wave — mask-before-call, approval gates, zone architecture, zero-knowledge vault.
- **Financial freedom:** solo/self-hosted deployments can offer compliance-grade PII protection without enterprise SaaS pricing.
- **Practical:** the seven-module zone architecture + approval gate + regulation-pack table is a deployable architecture template.

## Honesty Caveats
- Septum's NER layer is optional (off by default) — regex-based detection catches structured PII but not free-text names reliably; the docs are explicit about coverage limits.
- ElyAgent is a personal project (46 releases since March 2026, 4 stars) — small community, Elastic License v2 restricts SaaS resale.
- Masking is imperfect by design: NER layer off means free-text names/orgs may leak through regex gaps; the approval gate is the backstop, not the guarantee.
- Zone architecture requires ops discipline (five supported topologies in Septum); single-container demo exists but production deployment is non-trivial.
- Both are early-stage OSS projects, not audited enterprise products; treat "17 regulation packs" as a compliance-surface map, not a legal guarantee.

*Sources: byerlikaya/Septum (GitHub, MIT, 2026); franckolv-dev/ElyAgent (GitHub, Elastic License v2, v2.2.0 June 2026); both project docs.*