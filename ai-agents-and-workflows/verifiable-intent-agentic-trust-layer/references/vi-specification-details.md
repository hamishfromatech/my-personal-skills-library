# Verifiable Intent Specification Details

## Source

Mastercard (March 5, 2026). "Mastercard Unveils Open Standard to Verify AI Agent Transactions." Co-developed with Google. Open-sourced on GitHub and at verifiableintent.org. Partner announcements from Google, Fiserv, IBM, Checkout.com, Basis Theory, Getnet.

Additional technical analysis: Boboev, S. (March 15, 2026). "Deep Dive: Mastercard Verifiable Intent vs Visa Trusted Agent Protocol." Fintech Wrap Up.

## The Credential Format Specification

### Layer 1: Identity Binding
- **Issuer:** Identity issuer provisions credential into a credential provider wallet
- **Format:** SD-JWT (Selective Disclosure JSON Web Token)
- **Signature algorithm:** ES256 (ECDSA with P-256)
- **Key binding:** cnf.jwk (confirmation method per RFC 7800)
- **Lifetime:** Long-lived, on the order of one year
- **Key discovery:** JWKS (JSON Web Key Set)
- **Purpose:** Binds user identity to a public key

### Layer 2: Purchase Intent
- **Signer:** The user key bound in Layer 1
- **Binding to L1:** sd_hash (hash of the Layer 1 SD-JWT)
- **Immediate mode content:** Final checkout values (amount, merchant, items)
- **Autonomous mode content:** Constraint-bearing mandate + agent key binding
- **Lifetime:** Short-lived (session-scoped)
- **Purpose:** Expresses what the user authorized the agent to do

### Layer 3: Agent Fulfillment (Autonomous mode only)
- **Signer:** The agent key bound in Layer 2
- **Format:** Short-lived, key-bound SD-JWTs
- **Split:**
  - **L3a (network-facing):** Payment mandate — what the payment network needs to authorize and settle
  - **L3b (merchant-facing):** Checkout mandate — what the merchant needs to fulfill the order
- **Purpose:** Records what the agent actually did within the authorized constraints
- **Privacy design:** The L3a/L3b split enforces a privacy boundary by construction — the network and merchant see different views of the same fulfillment

## The Eight Registered Constraint Types

1. **spend_limit** — maximum total spend, per-transaction spend, per-merchant spend
2. **merchant_restriction** — allow lists, deny lists, category restrictions
3. **item_specification** — required attributes (refundable, includes breakfast, etc.)
4. **time_window** — deadline for transaction completion; or earliest start time
5. **geographic_scope** — jurisdiction limits, delivery location restrictions
6. **confirmation_threshold** — human re-confirmation required above a value or for certain criteria
7. **reversibility** — transaction must be cancelable within a specified window
8. **disclosure_scope** — what personal information the agent may share with which parties

### Strictness Modes
- **Strict mode:** Unknown constraint types cause transaction failure
- **Permissive mode:** Unknown constraint types trigger a warning; transaction proceeds
- **Critical rule:** Unknown constraint types in open mandates must be rejected — they can silently unbound authority

## Partner Ecosystem and Statements

### Google
"Strong, interoperable trust infrastructure like Verifiable Intent that is compatible with Agent Payments Protocol is a natural accelerator for scaling agentic commerce."
— Stavan Parikh, VP and General Manager, Payments at Google

### Fiserv
"Enables merchants to proactively reduce fraud, strengthen dispute outcomes, and maintain customer trust."
— Sanjay Saraf, Fiserv

### IBM
"Makes user authorization simple and secure, so agents can act safely across platforms." IBM plans to align VI with its orchestration layer for enterprise deployments.
— Kristin Kirtley Silva, IBM

### Checkout.com
"An important move to ensure the right parties can cryptographically validate intent without oversharing sensitive data."
— Meron Colbeci, Checkout.com

## Built-On Standards

| Standard Body | Standards Used |
|---------------|---------------|
| FIDO Alliance | Identity verification foundations |
| EMVCo | Payment industry standards (tokenization, 3DS) |
| IETF | SD-JWT, JWT, JWKS, key binding |
| W3C | Web platform standards (VC data model) |

## Verifiable Intent vs Visa Trusted Agent Protocol (Comparison)

Both are "trust layers" for agentic commerce, but they sit in different parts of the stack and optimize for different verifiers.

| Dimension | Mastercard Verifiable Intent | Visa Trusted Agent Protocol |
|-----------|------------------------------|---------------------------|
| Architecture | Layered credential chain (SD-JWT) | Agent identity and authorization framework |
| Core mechanism | Cryptographic delegation chain binding identity → intent → action | Know Your Agent (KYA) + tokenized credentials |
| Execution modes | Immediate (human-present) + Autonomous (delegated) | Agent registration + scoped tokenization |
| Privacy approach | Selective Disclosure (share minimum per party) | Tokenization (agent never sees raw credentials) |
| Constraint handling | First-class, eight registered types, verifier must validate | Scoped to merchant and transaction type |
| Interoperability | AP2, UCP, Agent Pay | Visa network native |
| Open source | Yes (GitHub + verifiableintent.org) | Visa-led |

**Key insight from the Fintech Wrap Up analysis:** VI is opinionated — it tries to turn "intent" into machine-checkable bounds and makes the verifier do explicit constraint validation against fulfillment artifacts. This is more than an "agent header"; it is a composable evidence object that multiple parties can verify independently.

## Integration Timeline

- **March 5, 2026:** Specification open-sourced, reference implementation published
- **Coming months:** Integration into Mastercard Agent Pay's intent APIs
- **Ongoing:** Deepening through Verifiable Credentials platform integration
- **Future:** Complementary standards for conversational AI in commerce

## The Mastercard Quote That Frames the Entire Problem

"As autonomy increases, trust cannot be implied. It must be proven. And if something goes wrong, everyone needs facts, not guesswork."
— Pablo Fourez, Chief Digital Officer, Mastercard

"In this new payments paradigm, trust becomes the product."
— Pablo Fourez, Mastercard

"Agent Pay for Machines will create the conditions for a superbloom of AI business models. Machine payments can make it possible for services to be bought and sold among agents at fundamentally different scales than payments today — very high volumes, very small values, very fast and at extremely low latency."
— Jorn Lambert, Chief Product Officer, Mastercard

## Enterprise Signal

PYMNTS Intelligence found that the highest-ranked use case for agentic AI is dynamic budget reallocation based on fresh cost data. ~43% of CFOs expect a high impact from using agents for this function, with another 47% expecting moderate impact. This is the enterprise angle that VI's constraint validation directly serves — agents managing budgets need verifiable proof that they stayed within constraints.

*Reference: Mastercard (March 5, 2026). "Mastercard Unveils Open Standard to Verify AI Agent Transactions." PYMNTS. Boboev, S. (March 15, 2026). "Deep Dive: Mastercard Verifiable Intent vs Visa Trusted Agent Protocol." Fintech Wrap Up.*