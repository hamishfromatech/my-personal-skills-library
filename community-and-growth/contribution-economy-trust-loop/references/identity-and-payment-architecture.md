# Privacy-First Identity and Payment Architecture

## Overview

The trust loop requires identity that is verifiable, portable, and privacy-preserving, plus payment that doesn't require contributors to surrender personal financial data. This reference documents the technical architecture.

## Self-Sovereign Identity (W3C DID)

### What it is

A Decentralized Identifier (DID) is a globally unique identifier that the contributor controls — not a platform. The contributor's DID resolves to a DID Document containing their public keys and service endpoints.

```
did:web:builderclub.com:members:alice
→ DID Document:
  {
    "id": "did:web:builderclub.com:members:alice",
    "verificationMethod": [{
      "id": "...#keys-1",
      "type": "Ed25519VerificationKey2018",
      "publicKeyMultibase": "z6Mk..."
    }],
    "service": [{
      "id": "#reputation-service",
      "type": "ReputationService",
      "serviceEndpoint": "https://builderclub.com/reputation/alice"
    }]
  }
```

### Why it matters for the trust loop

- **Portability:** Alice can take her DID and credentials to another platform. She owns her reputation.
- **Verifiability:** Anyone can verify Alice's credentials cryptographically without contacting the platform.
- **Privacy:** Alice can use pairwise DIDs (a different DID per relationship) to prevent correlation across contexts.

## Verifiable Credentials (W3C VC)

### Contribution Provenance Credential

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://builderclub.com/credentials/contribution/v1"
  ],
  "id": "https://builderclub.com/credentials/ctr-2026-07-19-001",
  "type": ["VerifiableCredential", "ContributionProvenanceCredential"],
  "issuer": "did:web:builderclub.com",
  "issuanceDate": "2026-07-19T10:00:00Z",
  "credentialSubject": {
    "id": "did:web:builderclub.com:members:alice",
    "contributionType": "code",
    "artifact": {
      "url": "https://github.com/atech/project/pull/42",
      "hash": "sha256:abc123..."
    },
    "reviewedBy": ["did:web:builderclub.com:members:bob"],
    "mergedBy": "did:web:builderclub.com:members:maintainer",
    "impact": {
      "downstreamPRs": 7,
      "issuesResolved": 3
    }
  },
  "proof": {
    "type": "Ed25519Signature2018",
    "verificationMethod": "did:web:builderclub.com#keys-1",
    "proofValue": "z3X..."
  }
}
```

### Reputation Score Credential

```json
{
  "@context": [...],
  "id": "https://builderclub.com/credentials/rep-alice-2026-07",
  "type": ["VerifiableCredential", "ReputationCredential"],
  "issuer": "did:web:builderclub.com",
  "credentialSubject": {
    "id": "did:web:builderclub.com:members:alice",
    "reputationScore": 847,
    "domains": {
      "payment-systems": { "score": 720, "rank": "top-10%" },
      "security": { "score": 510, "rank": "top-25%" },
      "documentation": { "score": 890, "rank": "top-5%" }
    },
    "asOf": "2026-07-19"
  },
  "proof": { ... }
}
```

### Selective Disclosure

Alice can present a credential revealing only the domains she wants to share:

- Applying for a security consulting gig → reveal only the "security" domain score
- Applying for a docs role → reveal only "documentation" domain score
- Applying for a general maintainer role → reveal overall score + all domains

This uses zero-knowledge proof techniques (e.g., BBS+ signatures) so the verifier can confirm the credential's authenticity without seeing the full credential.

## Payment Architecture

### The KYC Problem

Traditional payment platforms require Know Your Customer (KYC) verification: legal name, address, government ID, bank details. For open-source contributors who value privacy, this is a participation barrier.

### The x402 / AP2 Solution

For microtransactions (bounties, marketplace sales, tip contributions), use the agentic payment protocols that don't require full KYC:

- **x402:** HTTP-native payment protocol. Contributor has a wallet (self-custody); payments settle directly. No platform holds funds. Founding members: Visa, Google, AWS, Stripe, Coinbase (per `agent-to-agent-economy-operating-guide`).
- **AP2 (Agentic Payments Protocol):** Agent management + authorization layer. Can scope payments to specific actions (bounty resolution, marketplace sale) without exposing the contributor's full identity.
- **Stablecoin settlement:** For cross-border contributors, stablecoin settlement avoids banking friction and currency conversion costs.

### Hybrid Payment Architecture

```
┌─────────────────────────────────────────────┐
│  Contributor Wallet (self-custody)          │
│  - Receives x402 micropayments              │
│  - No KYC for amounts below threshold       │
│  - Privacy: only wallet address is public    │
└─────────────────────────────────────────────┘
                    │
                    │ x402 protocol
                    ↓
┌─────────────────────────────────────────────┐
│  Builder's Club Payment Layer                │
│  - Routes payments (x402 for micro,         │
│    traditional for macro)                   │
│  - Escrow for bounties (release on merge)   │
│  - Fee: 5-10% (funds community infrastructure)│
└─────────────────────────────────────────────┘
                    │
                    │ For amounts above KYC threshold:
                    ↓
┌─────────────────────────────────────────────┐
│  Traditional Payment (optional)              │
│  - Bank transfer / Stripe for large amounts  │
│  - KYC only when contributor opts in         │
│  - Contributor chooses: privacy (crypto)     │
│    or convenience (fiat)                      │
└─────────────────────────────────────────────┘
```

The architecture gives contributors a choice: full privacy via crypto micropayments, or fiat convenience for larger amounts. The platform never requires KYC for microtransactions. This is the privacy moat that surveillance-based competitor platforms cannot replicate.

## Implementation Roadmap

### Phase 1 (Weeks 1-4): Attribution
- Implement structured provenance records for all contributions
- Hash-link contributions to artifacts
- Track outcome metrics (downstream references, issues resolved)

### Phase 2 (Weeks 5-8): Identity
- Issue W3C DID to each contributor
- Issue Verifiable Credentials for each contribution
- Build the contributor credential wallet (view/manage credentials)

### Phase 3 (Weeks 9-12): Reputation
- Compute per-domain reputation scores from attribution data
- Build reputation dashboard (per-domain scores, contribution history)
- Implement recency decay

### Phase 4 (Weeks 13-16): Monetization
- Integrate x402 for microtransaction payments (bounties, tips, marketplace)
- Build bounty system (post → claim → resolve → escrow release)
- Build marketplace listing with reputation-driven visibility

### Phase 5 (Weeks 17-20): Trust Loop Optimization
- Run first trust audit
- Measure loop health metrics
- Optimize the weakest stage

## A-Tech Application Matrix

| Product | Identity | Reputation | Monetization |
|---|---|---|---|
| **A-Coder** | Contributor DID for code contributions | Per-domain reputation (languages, frameworks, domains) | Bounty system for bug fixes; marketplace for A-Coder extensions |
| **Be Practical** | Learner DID with learning-progress credentials | Reputation for teaching contributions (answers, reviews, mentorship) | Revenue share for top teachers; paid certification issuance |
| **Builder's Club** | Full trust loop implementation; contributor DID + credentials + wallet | Per-domain reputation across all contribution types | Full monetization: bounties, marketplace, sponsorship, consulting introductions |