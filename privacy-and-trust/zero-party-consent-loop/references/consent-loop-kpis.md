# Consent Loop KPIs & Measurement Framework

## Why Measure Differently in Privacy-First Marketing?

Traditional marketing metrics (impressions, clicks, CPMs) assume tracking-based attribution. In the Zero-Party Consent Loop, success is measured by:
- **Trust velocity:** How fast customers move from stranger to advocate through voluntary data sharing
- **Consent quality:** The depth, accuracy, and freshness of volunteered data
- **Activation rate:** How much of that data actually shapes a better customer experience
- **Privacy-adjusted LTV:** Customer lifetime value weighted by retention and referral power

## Core KPIs by Funnel Stage

### Stage 1: Value Give (Awareness → Interest)
| Metric | Definition | Target | Measurement |
|--------|-----------|--------|-------------|
| Gift Uptake Rate | % of visitors who claim the free value offer | >25% | Form completions / unique visitors |
| Gift Satisfaction Score | Self-reported value of the free gift (1–5) | >4.2 | Post-download micro-poll |
| Time-to-Value | Minutes from claim to first meaningful use | <5 min | Product analytics or self-report |
| Zero-Party Opt-In Rate | % of gift recipients who complete a quiz or preference center | >40% | Quiz starts / gift claimers |

### Stage 2: Data Gift (Interest → Intent)
| Metric | Definition | Target | Measurement |
|--------|-----------|--------|-------------|
| Quiz Completion Rate | % of starts who finish all questions | >60% | Completes / starts |
| Profile Completeness | % of available preference fields filled | >50% | Filled fields / total fields |
| Consent Granularity | % of users who customize (not just accept all) | >30% | Customized consents / total consents |
| Data Freshness | % of profiles updated within last 90 days | >40% | Active profiles / total profiles |

### Stage 3: Personalize (Intent → Conversion)
| Metric | Definition | Target | Measurement |
|--------|-----------|--------|-------------|
| Activation Rate | % of zero-party fields used in personalization within 30 days | >60% | Fields activated / fields collected |
| Segment Match Accuracy | % of customers who agree that recommendations fit their stated goals | >70% | Post-interaction survey |
| Privacy-First Conversion Rate | Conversions from consent-based segments vs. control | >+20% lift | A/B test vs. generic messaging |
| Cost Per Consent (CPCs) | Total acquisition spend / new consented profiles | Declining trend | Spend / new ZPD profiles |

### Stage 4: Deepen Trust (Conversion → Advocacy)
| Metric | Definition | Target | Measurement |
|--------|-----------|--------|-------------|
| Preference Center Re-engagement | % of customers who update preferences within 6 months | >15% | Updates / total users |
| Referral Rate from Consent Segment | % of ZPD-engaged customers who refer others | >2x vs. non-ZPD | Referrals by segment |
| Privacy-Adjusted LTV | CLV of consent-based customers | >+30% vs. baseline | Revenue × retention × referral rate |
| Churn Rate (Consent Segment) | % of ZPD-engaged customers who leave | <50% of baseline | Cohort analysis |
| Net Trust Score (NTS) | "How much do you trust this brand with your data?" (0–10) | >7.5 | Quarterly survey |

## Weekly Dashboard Metrics

Track these every Monday morning:

1. **New consented profiles this week**
2. **Top 3 performing value exchanges** (by completion rate)
3. **Bottom 3 performing value exchanges** (flag for copy/design review)
4. **Activation rate by project** (A-Coder / Be Practical / Builder's Club)
5. **Unsubscribe rate** (target: <0.2% per email; <0.1% for ZPD segments)
6. **Complaint/spam report rate** (target: <0.01%)
7. **Consent withdrawal rate** (target: <2% monthly; investigate spikes immediately)

## A/B Testing Priorities

Test these hypotheses monthly:

| Hypothesis | Variable | Success Metric |
|-----------|----------|--------------|
| More questions reduce completion | Quiz length (3 vs. 5 vs. 7 questions) | Completion rate |
| Immediate payoff increases opt-in | Show roadmap before vs. after quiz | Opt-in rate |
| Scarcity drives quality | Limited founder slots vs. open enrollment | Application completion + satisfaction |
| Scars beat wounds in copy | Story frame (scar vs. wound vs. feature list) | Conversion rate + trust score |
| Privacy messaging as headline | "Zero retention" vs. "AI-powered" vs. "Free" | Click-through rate on landing page |
| Preference control reduces churn | Preference center email vs. generic newsletter | Unsubscribe + churn rate |

## Tooling Stack

### Analytics & Measurement
- **Matomo** (self-hosted): Track completion funnels without third-party cookies; 100% data ownership
- **Plausible** (lightweight): Privacy-safe traffic and conversion trends; no consent banner needed
- **PostHog** (product analytics): Feature flags, funnel analysis, and cohort retention for ZPD segments
- **GA4 + Consent Mode v2** (if already in Google stack): Modeled conversions for privacy-safe attribution

### Consent & Preference Management
- **Secure Privacy**: White-label CMP with visual consent logs and multi-client dashboards
- **OneTrust**: Enterprise-grade universal consent across web/mobile/connected channels
- **Usercentrics**: A/B testing for consent optimization; operates in 180+ countries

### Data Activation
- **Segment (CDP)**: Unify ZPD and first-party data; route to 300+ destinations
- **HubSpot / Salesforce**: CRM-native preference centers and lifecycle marketing
- **Customer.io / Iterable**: Behavior + declared-data triggered messaging

### Trust Measurement
- **Typeform / SurveyMonkey**: Net Trust Score quarterly surveys
- **Hotjar (privacy mode)**: Aggregate heatmaps and session recordings with consent-only capture
- **Delighted / AskNicely**: Post-interaction micro-surveys for satisfaction and fit accuracy

## Governance Metrics

Compliance is not a checkbox — it is a trust signal. Track and report these quarterly:

| Metric | Target | Owner |
|--------|--------|-------|
| Consent log completeness | 100% of interactions logged | Data/Compliance |
| DSAR response time | <25 days (GDPR: 30 days) | Support/Legal |
| Data minimization audit | 0 unused fields older than 90 days | Marketing/Product |
| Re-validation rate | >50% of stale fields refreshed annually | Marketing |
| Privacy policy readability score | >60 (Flesch Reading Ease) | Legal/Marketing |
| Cross-border compliance coverage | 100% of active markets | Legal |

## Reporting Rhythm

- **Daily:** Glance at gift uptake, quiz completions, and unsubscribe rate
- **Weekly:** Full dashboard review; flag underperforming value exchanges
- **Monthly:** A/B test readouts; activation rate by project; consent withdrawal analysis
- **Quarterly:** Net Trust Score survey; governance audit; privacy-adjusted LTV update
- **Annually:** Comprehensive data strategy review; taxonomy refresh; tooling evaluation

## Red Flags to Watch

These signal that the Consent Loop is breaking:

1. **Completion rate drops below 30%** → Quiz is too long, payoff is unclear, or trust is eroding
2. **Activation rate below 40%** → You're collecting data but not using it — customers will notice
3. **Unsubscribe rate spikes above 0.5%** → Messaging is irrelevant or frequency is too high
4. **Consent withdrawal rate above 5% monthly** → Value exchange is broken or perceived as deceptive
5. **Net Trust Score below 6.0** → Fundamental positioning or product trust issue; pause marketing and investigate

## Success Benchmarks

After 90 days of operation:
- >1,000 consented profiles per project
- >50% quiz completion rate
- >60% activation rate
- >20% conversion lift vs. non-consent segments
- >7.5 Net Trust Score
- <0.2% unsubscribe rate
- >2x referral rate from ZPD segments
