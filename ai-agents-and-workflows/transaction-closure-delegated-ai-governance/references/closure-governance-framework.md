# Transaction Closure Governance Framework — Detailed Reference

## Source

He, C., Zhou, X., Wan, D., Xu, H., Wei, et al. (2026). "From Super-Apps to Agent Economies: Delegated AI Requires Transaction Closure." Preprint (April 2026).

Position paper arguing that the evolution from super-app-based AI assistants to autonomous agent economies requires a fundamental shift in how delegated AI is evaluated and governed: from task-completion benchmarks to portable transaction-closure.

## The Core Argument

Super-app AI assistants (WeChat, ChatGPT super-app vision) operate within a single platform context. When an agent helps a user book a flight inside one app, "task completion" is a sufficient evaluation: did the agent surface the right options and help the user finish the booking?

Agent economies break this model. An autonomous agent acting on behalf of a human may:
- Search across multiple platforms
- Compare options from different merchants
- Execute a payment through one protocol
- Arrange delivery through another
- Coordinate with other agents for sub-tasks

In this multi-party, cross-protocol context, "the agent completed its steps" is no longer sufficient. The question becomes: **was the transaction closed in a way that satisfies the human's intent, produces verifiable evidence, and remains contestable if something goes wrong?**

## Why Task Completion Fails for Delegated Agents

### Failure Mode 1: Intent Drift
An agent is asked to "book a hotel in Berlin for next weekend under €200/night." The agent finds a hotel at €180/night but in a suburb 45 minutes from the city center. Task completion: yes. Intent satisfaction: questionable. Without a closure verification layer that checks the outcome against the human's actual constraints (location, not just price), the agent reports success for a transaction the human would not have authorized.

### Failure Mode 2: Scope Expansion
An agent is authorized to spend up to €500 on travel booking. It finds a flight + hotel package at €480 and adds travel insurance for €40 "because it's recommended." Total: €520. Task completion: yes. Constraint adherence: no. Without constraint validation at closure, the agent has exceeded its authorization.

### Failure Mode 3: Evidence Fragmentation
An agent initiates a search on Platform A, compares on Platform B, and pays through Protocol C. When the human disputes the charge, the evidence is spread across three systems with no unified record. Closure requires a portable evidence artifact that binds the full transaction arc.

### Failure Mode 4: Uncontested Errors
An agent buys the wrong product (similar name, wrong variant). The transaction "completes" but the human doesn't discover the error until delivery. Without a contestation window and dispute-resolution protocol built into the closure framework, the human has no recourse.

## The ClosureBench Evaluation Concept

The paper proposes moving agent evaluation from task-completion benchmarks to transaction-closure benchmarks. Where existing benchmarks (SWE-bench, GAIA, τ-bench) ask "did the agent produce the correct output?", ClosureBench asks "did the agent close the transaction in a way that is intent-satisfying, verifiable, contestable, and portable?"

### Evaluation Dimensions

1. **Intent Alignment Score** — Does the final outcome match the human's stated intent, including implicit constraints (location preferences, quality expectations, timing)?
2. **Constraint Adherence Rate** — Percentage of transactions where the agent stayed within all explicitly stated constraints.
3. **Evidence Completeness** — Can a third-party auditor reconstruct the full transaction arc from the closure record? Are the agent's reasoning steps, merchant selections, and payment authorizations all documented?
4. **Contestation Resolution Success** — In simulated dispute scenarios, does the evidence trail support correct resolution?
5. **Cross-Protocol Portability** — Does the closure record survive transitions across platforms, protocols, and agents?

## The Delegation Mode Taxonomy

### Immediate Mode (Human-Present)
- Human signs final checkout and payment values
- Two-layer credential flow: Layer 1 (identity binding) + Layer 2 (final purchase intent)
- Short-lived Layer 2 credentials
- Closure is straightforward: human approved the final values

### Autonomous Mode (Delegated, Bounded)
- Human signs constraints up front and binds an agent key
- Three-layer credential flow: Layer 1 (identity) + Layer 2 (constraint-bearing mandate + agent key) + Layer 3 (agent fulfillment credentials)
- Layer 3 splits into L3a (network-facing payment mandate) and L3b (merchant-facing checkout mandate) — enforcing a privacy boundary by construction
- Constraints are normative; the closure verification layer must validate fulfillment against constraints
- This is the production default for agentic commerce

### Open-Ended Mode (Delegated, Unbounded)
- Human gives broad mandate with minimal constraints
- Relies on agent judgment and post-hoc review
- Highest risk; requires real-time monitoring, spend alerts, immediate notification on suspicious activity
- Should be avoided for financial transactions until closure infrastructure and agent reputation systems mature

## Constraint Specification

The framework treats constraints as first-class authorization limits. Eight registered constraint types:

1. **spend_limit** — maximum total spend, per-transaction spend, or per-merchant spend
2. **merchant_restriction** — allow lists, deny lists, category restrictions (e.g., "no alcohol purchases")
3. **item_specification** — required attributes (e.g., "must be refundable", "must include breakfast")
4. **time_window** — transaction must complete within a deadline; or must not execute before a start time
5. **geographic_scope** — jurisdiction limits, delivery location restrictions
6. **confirmation_threshold** — human re-confirmation required for transactions above a value or matching certain criteria
7. **reversibility** — transaction must be cancelable within a specified window
8. **disclosure_scope** — what personal information the agent may share with which parties

**Strictness modes:** Verifiers must support all registered constraint types. Unknown constraint types in open mandates must be rejected (they can silently unbound authority). In strict mode, unknown types cause transaction failure; in permissive mode, they trigger a warning but the transaction proceeds.

## Relationship to Existing Trust Infrastructure

Transaction closure is the governance layer that sits above:
- **Payment execution** (AP2, x402, Mastercard Agent Pay, Visa Intelligent Commerce) — these move the money
- **Trust proof** (Mastercard Verifiable Intent, cryptographic credential chains) — these prove what was authorized
- **Agent identity** (Know Your Agent frameworks) — these establish who the agent is

Transaction closure defines what "done and accountable" means. The payment infrastructure executes the payment. The trust layer proves authorization. The identity framework establishes provenance. Closure ties them together into a verifiable, contestable end-to-end record.

## Implications for Agent Economy Maturation

The paper argues that the agent economy cannot mature beyond spending (agents buying services) to earning (agents selling services and running businesses) without transaction closure infrastructure. Earning agents need:
- Verifiable proof that they satisfied a buyer's intent (to build reputation)
- Contestable records (to resolve disputes with other agents or humans)
- Portable closure records (to carry reputation across marketplaces)

Without closure, agent reputation systems have no ground truth. With closure, reputation can be anchored to verified transaction outcomes rather than self-reported claims.

*Reference: He, C. et al. (2026). "From Super-Apps to Agent Economies: Delegated AI Requires Transaction Closure." Preprint. Keywords: agentic AI, contestable transaction closure, AI governance, ClosureBench, delegated AI, transaction closure.*