# License Boundary Case Studies

## Successful Boundary Decisions

### GitLab: CI/CD in Open Core
- GitLab CI/CD remained in the open core despite being a major enterprise feature
- Rationale: CI/CD demonstrates core value to individual developers and small teams
- Result: Massive community adoption, CI/CD became the gateway to enterprise features
- Enterprise tier adds: advanced security scanning, compliance dashboards, portfolio management

### Hugging Face: Model Hub Fully Open
- The model hub — the core value proposition — is entirely free and open
- Anyone can upload, download, and use models without payment
- Revenue comes from: rate-limited API (usage), private enterprise hub (governance), dedicated inference (operations)
- Result: 5M+ users, dominant platform position, strong community trust

### Sentry: Error Tracking Open, Operations Commercial
- Error tracking core is open-source (Apache-2.0 after BSL transition)
- Commercial revenue from: hosted SaaS operations, enterprise governance, dedicated support
- Result: Strong community adoption + profitable enterprise business

## Failed or Controversial Boundary Decisions

### MongoDB: SSPL Shift
- Original license: AGPL
- Shifted to Server Side Public License (SSPL) in 2018
- Reason: Cloud providers (AWS, Azure, GCP) offering MongoDB as a service without contributing back
- Backlash: OSI did not approve SSPL; some distributions removed MongoDB
- Result: Effective at protecting revenue but cost community trust

### Elastic: SSPL + Proprietary Dual License
- Original license: Apache-2.0
- Shifted to SSPL + proprietary in 2021
- Reason: AWS forked Elasticsearch into OpenSearch
- Backlash: Significant community anger; some contributors left
- Result: Elastic retained enterprise revenue but community sentiment damaged

### Redis: License Change to RSAL
- Original license: BSD
- Shifted to Redis Source Available License (RSAL) for certain modules in 2018, then full license change in 2024
- Backlash: Forked by Linux Foundation (Valkey)
- Result: Most significant open-source fork event of 2024; community fragmented

## Lessons for A-Tech Boundary Decisions

### The Permeability Test in Practice
| Feature | Permeability Test Result | Layer |
|---------|-------------------------|-------|
| Basic AI code completion | Demonstrates core value to individuals | Open Core |
| Multiplayer real-time editing | Large-team governance | Enterprise Bridge |
| Local model execution | Individual developer use | Open Core |
| SSO/SAML integration | Enterprise compliance | Enterprise Bridge |
| Plugin API | Community contributes | Open Core |
| Audit logging for compliance | Legal risk reduction | Commercial Protections |
| On-device privacy processing | Core privacy promise | Open Core |
| Air-gapped deployment | Enterprise operations | Commercial Protections |

### The Retroactive Change Rule
Never move a feature from open core to enterprise retroactively. This destroys trust permanently.

**Exception:** If a feature was mistakenly placed in open core (e.g., it was never supposed to be free, or it creates unsustainable support load), the proper path is:
1. Announce the mistake transparently
2. Offer 12-month grandfathering for existing users
3. Provide migration path or open-source alternative
4. Accept community council oversight of the decision

### The Reinvestment Visibility Rule
Community trust is proportional to visible reinvestment.

**High-Trust Signals:**
- Publish quarterly open-core development reports
- Fund community events, documentation sprints, and accessibility improvements
- Hire maintainers from the community, not just from the commercial team
- Recognize community contributors in enterprise marketing

**Low-Trust Signals:**
- Enterprise revenue invisible to community
- Core maintenance deferred while enterprise features are prioritized
- Community issues ignored while enterprise tickets are fast-tracked
- No public commitment to open-core perpetuity

## A-Tech Boundary Principles

1. **Core Promise First:** The open core must be a complete, compelling product on its own. If it is not, the model is extraction, not open-core.

2. **Governance Over Growth:** Community council has veto power over license and boundary changes. Growth targets do not override governance.

3. **Transparency as Default:** All boundary decisions, revenue allocation, and reinvestment ratios are published.

4. **Generosity Signals:** Occasionally move enterprise-appropriate features to the open core to demonstrate good faith. This builds more trust than any marketing campaign.

5. **Legal Safety as Product:** Frame commercial licensing as risk transfer, not feature access. Enterprise buyers pay for confidence; community users get capability.
