# IMF Note 2026/004 Summary

## Publication Details
- **Title:** How Agentic AI Will Reshape Payments
- **Publisher:** International Monetary Fund (IMF)
- **Date:** April 2026
- **Type:** IMF Note 2026/004
- **URL:** https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560

## Core Argument
Agentic AI introduces probabilistic decision-making systems capable of initiating financial actions at machine speed. Payment infrastructures (RTGS, card networks, instant payment platforms, DLT settlement) operate deterministically — predictable rules, binary outcomes, legal finality. The IMF framework resolves this tension by concentrating probabilistic reasoning upstream while preserving deterministic controls downstream.

## The Three-Layer Model

### Layer 1: Intent and Orchestration (Probabilistic)
Translates high-level user objectives into structured, machine-readable instructions. No authorization or execution occurs here.
- **Capabilities:** Reasoning, planning, search, negotiation, multi-agent coordination, dynamic adaptation
- **Standards:** MCP (agent access to tools), A2A (inter-agent interoperability), x402 (payment requirement embedding), UCP (Google shared grammar), ACP (OpenAI/Stripe commerce protocol)
- **Industry pilots:** Visa Intelligent Commerce, Mastercard Agent Suite, PayPal Cymbio

### Layer 2: Control and Authorization (Deterministic)
Enforces deterministic constraints on whether proposed actions may proceed. This is the trust boundary.
- **Core mechanism:** AP2 mandates — cryptographically verifiable scopes binding agent actions to explicit user consent
- **Controls:** Issuer rules, AML/CFT filters, velocity checks, sanctions screening, tokenization, programmable wallets (ERC-4337, ERC-6900), ERC-8004 agent identity registry
- **Authorization shift:** From transaction-level instructions to structural mandate-based authorization

### Layer 3: Settlement (Deterministic, Legally Final)
Executes authorized instructions with irrevocable legal finality. Agentic algorithms do not operate here.
- **Components:** RTGS, instant payment networks, card clearing engines, CBDC platforms, DLT rails, smart contract wallets
- **Principle:** Non-probabilistic by design. Takes only instructions that passed deterministic Layer 2 controls.

## Key Quotations
- "Agentic AI introduces a new element into this framework: probabilistic decision-making systems capable of initiating financial actions at machine speed."
- "The core tension: payment infrastructures operate deterministically. Agentic AI systems rely on probabilistic reasoning."
- "Concentrating probabilistic reasoning upstream while preserving deterministic controls downstream."
- "Legal finality, systemic stability, and auditability depend on this separation."

## A-Tech Implications
- A-Coder plugin marketplace billing must respect Layer 2/3 separation
- Builder's Club open-source dispute arbitration should align with IMF mandate model
- Be Practical curriculum should teach the three-layer model as foundational commerce literacy
