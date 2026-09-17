---
name: open-core-enterprise
description: Sustainable open-core monetization for AI and developer tools. Based on the Hugging Face model and 2026 market data. Use when designing business models where the core product is open-source but enterprise revenue is needed for sustainability.
---

# Open-Core Enterprise Model

## Overview

The open-core model is the dominant sustainable architecture for open-source AI monetization in 2026. Unlike donation-based or ad-supported models, open-core creates a clear, defensible boundary: the core remains open forever while enterprise features, support, and legal protections generate revenue. Hugging Face proved this model at scale — $235M valuation, 5M+ users, profitable enterprise unit — without ads, paywalls on core access, or venture-capital-driven growth-at-all-costs.

This skill provides the complete architecture for building an open-core business around A-Tech products.

---

## When to Use

- Designing a business model for an open-source AI product that needs sustainable revenue
- Deciding which features belong in open core vs. enterprise tier
- Pricing enterprise tiers for developer tools, AI infrastructure, or content platforms
- Communicating the open-core value proposition to enterprise buyers and community contributors
- Evaluating whether a product is suitable for open-core vs. fully proprietary or fully donation-based

### NOT for
- Consumer apps where network effects or data moats are the primary value
- Situations requiring rapid monopoly formation (open-core builds trust, not lock-in)
- Products where the core IP is the only differentiator and cannot be exposed

---

## The Three-Layer Architecture

### Layer 1: Open Core (The Commons)
**Principle:** This layer remains open-source, freely available, and community-governed forever. It is not a trial version. It is a complete, production-grade product.

**What belongs here:**
- Core functionality that demonstrates the product's primary value
- APIs, SDKs, and CLI tools
- Documentation, tutorials, and examples
- Community support (forums, Discord, GitHub issues)
- Basic integrations and plugins

**What does NOT belong here:**
- Administrative or governance features for large organizations
- Advanced security, compliance, or audit capabilities
- SLA-backed support or dedicated customer success
- Legal protections (indemnification, warranty)
- Proprietary-use licensing (for copyleft cores)

### Layer 2: Enterprise Bridge
**Principle:** Features that large organizations need to adopt the open core at scale, but that individual developers do not need.

**Typical Enterprise Bridge Features:**
- SSO / SAML / SCIM identity integration
- Role-based access control (RBAC) and audit logging
- Advanced analytics and usage dashboards
- Custom deployment options (self-hosted, VPC, air-gapped)
- Integration with enterprise tools (SIEM, ITSM, identity providers)
- Bulk operations and admin APIs

### Layer 3: Commercial Protections
**Principle:** Not features, but legal and operational safety nets that reduce enterprise risk.

**Commercial-Only Protections:**
- **Indemnification:** Legal defense if the software is challenged
- **Warranty / SLA:** Guaranteed uptime, response times, resolution commitments
- **Custom terms:** Negotiable contracts, procurement flexibility
- **Dedicated support:** Named engineers, private channels, escalation paths
- **Security guarantees:** Penetration testing, vulnerability response SLA, security advisories

---

## Feature Boundaries: The Permeability Test

Use this test for every feature to decide which layer it belongs in:

| Question | If YES → | If NO → |
|----------|----------|---------|
| Does this feature demonstrate the core product value to a new user? | Open Core | Enterprise |
| Would an individual developer use this in a personal project? | Open Core | Enterprise |
| Is this required for large-team governance or compliance? | Enterprise | Open Core |
| Does this reduce legal or operational risk for the buyer? | Commercial | Open Core |
| Would the community contribute this if asked? | Open Core | Enterprise |
| Is this a differentiator against competitors? | Consider carefully | — |

**Anti-Pattern:** Holding back core functionality (e.g., basic auth, core API limits) to force upgrades. This destroys community trust and turns open core into a crippled demo.

---

## Pricing Architectures

### The GitLab Model (Tiered SaaS + Self-Managed)
| Tier | Price | Includes |
|------|-------|----------|
| Free | $0 | Open core, community support |
| Premium | $19/user/mo | Code review, CI/CD minutes, priority support |
| Ultimate | $99/user/mo | Security scanning, compliance, portfolio management |

### The Hugging Face Model (Usage + Enterprise)
| Tier | Price | Includes |
|------|-------|----------|
| Free | $0 | Model hub, community, inference API (rate-limited) |
| Pro | $9/mo | Higher rate limits, early access |
| Enterprise Hub | Custom | SSO, audit logs, dedicated support, private model hub |

### The A-Tech Recommended Model (Hybrid)
| Tier | Price | Includes |
|------|-------|----------|
| **Community** | Free | Full open core, public docs, community support |
| **Professional** | $29/mo or $299/yr | Commercial license (no copyleft obligations), email support |
| **Team** | $199/mo (5 users) | RBAC, SSO, admin dashboard, priority support |
| **Enterprise** | Custom | SLA, indemnification, self-hosted option, dedicated success |

---

## The Open-Core Revenue Flywheel

```
Open Core → Community Growth → Trust + Talent →
Enterprise Adoption → Revenue → Core Investment →
Better Open Core → (loop)
```

### Each Phase
1. **Open Core:** Free, complete product attracts developers and builds reputation
2. **Community Growth:** Users contribute code, docs, plugins, and word-of-mouth
3. **Trust + Talent:** Enterprise decision-makers notice; engineers already know the product
4. **Enterprise Adoption:** Organizations buy protections and governance features they need
5. **Revenue:** Funds team expansion, security audits, and infrastructure
6. **Core Investment:** Revenue is partially reinvested in open core (feature development, documentation, community events)
7. **Better Open Core:** Improved product attracts more community, restarting the loop

### Critical Reinvestment Ratio
Industry benchmark: **20-40% of enterprise revenue should be visibly reinvested in the open core.**
- Below 20%: Community perceives extraction, trust erodes
- Above 40%: Enterprise tier may lack differentiation, revenue suffers

**A-Tech Commitment:** Publish annual Open-Core Reinvestment Report showing percentage of revenue funding open-source development, documentation, and community programs.

---

## Competitive Positioning

### Against Fully Proprietary Competitors
"Our core is open — you can audit it, fork it, and build on it without vendor lock-in. Our enterprise tier adds the legal and operational safety nets your procurement team needs. You get both freedom and safety."

### Against Fully Open Competitors
"We sustainably fund our core through enterprise revenue. That means security patches, documentation updates, and community events continue year after year — not just when volunteers have free time."

### Against Closed-Source AI Platforms
"In the AI era, trust requires inspectability. Our open core lets you verify what the model does, how it handles your data, and where it sends information. Closed-source AI is a black box; we are a glass box with a support contract."

---

## Governance and Trust Preservation

### The Open-Core Promise
Write this explicitly in your project's public documentation:

> "The open core of [Product Name] will remain open-source under [License] forever. We will not move features from the open core to enterprise tiers retroactively. We will not introduce artificial limitations in the open core to force upgrades. Enterprise tiers add governance, legal protections, and support — not functionality that belongs in the commons."

### Community Council
- Establish a Community Council with veto power over license changes
- Council members elected by contributors, not appointed by company
- Any change to open-core licensing requires 75% Council approval
- Publish meeting minutes publicly

### Feature Reversion Policy
If a feature is mistakenly placed in the enterprise tier but belongs in the open core (per the Permeability Test), commit to reverting it within 90 days.

---

## A-Tech Application

### A-Coder IDE
- **Open Core:** Full IDE, all editing features, basic AI assistance, local-first processing, open-source plugins
- **Enterprise Bridge:** Team collaboration (shared workspaces, RBAC), SSO, admin dashboard, usage analytics
- **Commercial Protections:** Indemnification, SLA, dedicated support, custom model hosting, air-gapped deployment

### Be Practical
- **Open Core:** All playbook content under CC-BY-SA, community discussion, basic progress tracking
- **Enterprise Bridge:** Team learning paths, manager dashboards, SCORM export, LMS integration
- **Commercial Protections:** Institutional licensing, custom curriculum development, certification exams, dedicated success manager

### Builder's Club
- **Open Core:** All tools, MCP servers, and community resources under OSI-approved licenses
- **Enterprise Bridge:** Private registry, verified server directory with SLA, team contribution tracking
- **Commercial Protections:** Legal review of community contributions, indemnification for enterprise use, priority security response

---

## Measurement Framework

| Metric | Definition | Target |
|--------|-----------|--------|
| Open Core Health Score | Composite: commit velocity, issue response time, contributor growth | ≥ 7/10 |
| Enterprise Revenue Ratio | Enterprise revenue / total revenue | 60-80% |
| Reinvestment Ratio | Open-core spend / enterprise revenue | 20-40% |
| Community-to-Enterprise Conversion | % of active community users who become enterprise buyers | 2-5% |
| Net Promoter Score (Open Core) | Community NPS | ≥ 50 |
| Net Promoter Score (Enterprise) | Enterprise NPS | ≥ 40 |
| Feature Boundary Violations | # of features moved from open core to enterprise (should be 0) | 0 |

---

## References
- See [references/open-core-market-data.md](references/open-core-market-data.md) for Hugging Face financials, GitLab pricing evolution, MongoDB and Elastic licensing shifts, and 2026 market context.
- See [references/license-boundary-case-studies.md](references/license-boundary-case-studies.md) for examples of successful and failed open-core boundary decisions.
