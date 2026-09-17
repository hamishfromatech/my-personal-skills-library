# Funding Model Comparison for Open Source Sustainability

## The Landscape (2026)

| Model | Mechanism | Strengths | Weaknesses | Best For |
|-------|-----------|-----------|------------|----------|
| **Open Source Pledge** | $2,000/dev/year to upstream | Normalizes payment; transparent; public accountability | Requires corporate buy-in; not all companies participate | Organizations with 10+ developers |
| **GitHub Sponsors** | Direct monthly donations | Low friction; GitHub-native | Volatile; tip-jar fallacy; unpredictable | Individual maintainers with audience |
| **Open Collective** | Transparent budget + expenses | Full transparency; multiple funding sources | Requires active management; not all enterprises comfortable | Community projects with governance |
| **Tidelift** | Enterprise subscription → maintainer pay | Direct link between usage and payment | Platform fee; limited to supported packages | Enterprises with compliance requirements |
| **Dual Licensing** | Open core + commercial license | Sustainable revenue; legal clarity | Governance complexity; relicensing risk | Infrastructure projects |
| **Foundation Membership** | Linux Foundation, Apache, etc. | Legal/admin infrastructure; credibility | Overhead; not direct maintainer funding | Large, established projects |
| **Time Banking** | Engineering hours to upstream | Builds relationships; knowledge transfer | Requires management support; opportunity cost | Teams with senior engineers |
| **Government Grants** | EU, US, Asia funding programs | Significant amounts; security focus | Bureaucratic; slow; political risk | Security-critical infrastructure |

## The A-Tech Recommendation

**Stack multiple models.** No single funding mechanism is sufficient.

**Tier 1 (Critical dependencies):**
- Open Source Pledge allocation ($1,200/dev/year)
- Direct maintainer contracts where possible
- Time banking (40 hrs/quarter senior engineer)
- Foundation membership for legal infrastructure

**Tier 2 (Important dependencies):**
- Open Source Pledge allocation ($600/dev/year)
- Tidelift or Open Collective where available
- Community advocacy for corporate sponsors

**Tier 3 (Monitoring):**
- Include in funding pool
- Low-priority outreach
- Standard monitoring via Dependabot/Snyk

## 2026 Regulatory Tailwinds

| Regulation | Impact on Funding |
|------------|-------------------|
| EU Cyber Resilience Act | Mandates SBOMs and vulnerability management; increases upstream visibility |
| US Executive Orders on Supply Chain | Federal vendors must disclose open-source dependencies; creates procurement pressure |
| Insurance Underwriting | Carriers now ask about open-source posture; unfunded dependencies = higher premiums |
| EU AI Act | High-risk AI systems require transparency; open-source AI models need governance |

## Anti-Patterns

| Anti-Pattern | Why It Fails |
|-------------|-------------|
| "We'll donate when profitable" | Perpetually deferred; crisis arrives first |
| One-time grants | Do not sustain ongoing maintenance |
| Silent sponsorship | No trust signal; no community accountability |
| Extractive fork-and-forget | Violates social license; creates divergence |
| AI-generated contributions without oversight | Increases maintainer burden; quality degradation |
| Maintainer celebrity culture | Fragile; personality-dependent; no succession |
