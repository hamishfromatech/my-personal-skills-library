# Flywheel Metrics and Dashboard Design

## The Metrics Stack

### Layer 1: Community Health (Leading Indicators)

| Metric | Definition | How to Measure |
|--------|-----------|----------------|
| **New contributors (30-day)** | Unique individuals making first contribution | GitHub API, Discord bot |
| **Time to first contribution** | Hours from first visit to first merged PR | GitHub API + analytics |
| **Contributor diversity** | % of contributions from outside the core team | GitHub commit authors |
| **Issue resolution velocity** | Median days from open to close | GitHub Issues API |
| **Documentation freshness** | % of docs updated within last 90 days | Git log on docs/ directory |
| **Community sentiment** | Manual or automated sentiment on Discord/forum | NLP or periodic survey |

### Layer 2: Product Health (Mid Indicators)

| Metric | Definition | How to Measure |
|--------|-----------|----------------|
| **Release cadence** | Days between stable releases | Git tags |
| **Community-sourced features** | % of features in each release from external PRs | Git log + PR metadata |
| **Plugin ecosystem growth** | New plugins per quarter | Plugin registry API |
| **Playbook downloads** | Organic downloads of Be Practical playbooks | Analytics (privacy-respecting) |
| **Adoption velocity** | New user growth rate | Download counts, install metrics |

### Layer 3: Business Health (Lagging Indicators)

| Metric | Definition | How to Measure |
|--------|-----------|----------------|
| **Free-to-paid conversion rate** | % of free users who upgrade | CRM + product analytics |
| **Net Revenue Retention** | Revenue from existing customers over time | Financial data |
| **CAC payback period** | Months to recover customer acquisition cost | Financial data |
| **Gross margin** | (Revenue - COGS) / Revenue | Financial data |
| **Community-funded ratio** | Revenue attributable to community-driven demand | Attribution survey |

## Dashboard Principles

### Public Dashboard
Publish a subset of metrics publicly to demonstrate transparency and build trust:
- Contributor count and growth
- Issue resolution velocity
- Release cadence
- Plugin ecosystem size
- Free-to-paid ratio (not revenue numbers)

### Internal Dashboard
Track the full metrics stack for strategic decisions, but do not share revenue figures publicly unless required.

### Privacy-Respecting Measurement
- Use first-party analytics only (no Google Analytics, no Meta pixel)
- Aggregate data before displaying; never show individual behavior
- Allow opt-out of all measurement without degrading experience
- Publish a public data policy explaining what is measured and why
