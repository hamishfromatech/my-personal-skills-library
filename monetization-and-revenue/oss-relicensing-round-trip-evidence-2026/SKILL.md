---
name: oss-relicensing-round-trip-evidence-2026
description: Applies the completed relicensing round trip (Redis and Elastic both re-adding OSI-approved licenses after restrictive-license experiments) plus Percona's contributor-loss data and 2026 managed-cloud revenue shares as the strongest empirical evidence that the license is a distribution decision, not a monetization lever. Use when advising open-source founders on licensing strategy, evaluating fork risk, or quoting evidence on where open-source revenue actually accrues.
---

# The Relicensing Round Trip: License as Distribution, Not Monetization

## Overview
Two of the three canonical restrictive-relicensing cases have walked it back: Redis re-added AGPLv3 in May 2025 (CEO: the SSPL move "hurt our relationship with the Redis community"), Elastic re-added AGPL in August 2024. HashiCorp kept BSL and was acquired. The 2024–2026 record is a clean experiment: vendors tightened licenses, lost contributors to permanent forks, gained no visible protection, and reversed. Revenue kept accruing to whoever operated the service.

## When to Use
- Advising an open-source founder considering a restrictive-license change
- Quantifying what a license change actually costs (community, not just optics)
- Explaining to procurement or leadership why an OSI license now has commercial value
- Content: "The license was never the product — Redis and Elastic just proved it twice"

**NOT for**: the original fork-cycle mechanics and the 12-week rule (use `oss-license-trap-fork-cycle` — this skill is its empirical completion), AI-era license design (use `license-axis-business-type`), dual-license implementation (use `dual-license-monetization`).

## Core Process

### 1. The round-trip sequence (compressed in retelling — lay it out)
- Elastic: Apache 2.0 → SSPL/Elastic License (Jan 14, 2021) → AWS forks OpenSearch 7 days later → OpenSearch Foundation under Linux Foundation (Sept 2024, SAP/Uber premier) → AGPL re-added (Aug 2024).
- Redis: BSD → RSALv2/SSPLv1 (Mar 20, 2024) → Valkey under Linux Foundation 8 days later (AWS, Google Cloud, Oracle, Ericsson, Snap) → AGPLv3 option re-added (May 2025, Redis 8).
- HashiCorp: MPL 2.0 → BSL (Aug 10, 2023) → OpenTofu fork (Sept 2023) → IBM acquisition $6.4B closed Feb 27, 2025, BSL not reversed. Terraform has not returned to an OSI license.
- Wider mix: RedMonk's March 2026 analysis — permissive licenses in its dataset ticked down from 82% (2022) to 73% (2025), copyleft picking up the difference. After five years of source-available experiments, the market is edging back toward OSI-blessed licenses.

### 2. The contributor data (what a license change actually costs)
Percona (December 2025), Redis: in the four months before the fork, 24 contributors committed; **37.5% never committed to Redis again**. Valkey started with 18 contributors the month after the fork and grew to 49. Across 2025, Valkey opened **865 PRs against Redis's 537**. A license change is reversible in a blog post; a community split is not. Once a hyperscaler funds a fork and a neutral foundation adopts it, the concrete is poured — neither OpenSearch nor Valkey retired when the original vendor returned to open licenses.

### 3. Where the revenue actually shows up (FY2025–27 shares)
| Company | License position | Managed-cloud share of revenue | Growth note |
|---|---|---|---|
| MongoDB | SSPL (Oct 2018, never reversed) | Atlas ~$565.9M of $771.8M Q2 FY27 (~73%) | total +30% |
| Elastic | AGPL re-added | Elastic Cloud $837.3M = 48% of $1.739B FY26 | cloud +22%, NER ~112% |
| Confluent | Apache 2.0 (doesn't control Kafka) | Cloud $624M of $1.167B FY25 (~53%) | cloud +27% |
| GitLab | open core | Q1 FY27 $264.2M, +23% | NDR 117%, 1,519 customers >$100K ARR |

Four companies, four license positions, one pattern: **the managed service carries 48–73% of revenue and usually outgrows the company**. The defensible asset is what a competitor cannot copy by reading source — a control plane holding the customer's production workload qualifies.

### 4. Procurement now pays for openness
2026 State of Open Source Report (OpenLogic + OSI + Eclipse Foundation, 700+ respondents): **55% named avoiding vendor lock-in as a primary open-source adoption driver** — the OSI calls it a 68% YoY increase — and the figure is **63% in Europe vs 51% in North America**. Fewer than 2% reduced open-source usage. An OSI-approved license is now a prepared answer to a question buyers ask unprompted; a source-available license invites the opposite conversation. Caveat the counter-case: AGPL is itself a procurement blocker at enterprises with blanket copyleft prohibitions — Redis and Elastic both kept commercial licenses alongside AGPL. The **licensing menu**, not any single license, is the unit.

### 5. The sequence fast growers actually ran
1. Ship the project under a license nobody has to think about (adoption is the only currency; every clause is friction).
2. Price the managed service against **operational pain**, not the free tier (upgrade windows, replication, backups, on-call — none show in a feature table).
3. Add the enterprise control plane last (SSO, audit logs, residency, RBAC — most reasonably commercial).

The OpenTelemetry template (graduated by CNCF May 21, 2026; 12,000+ contributors; second-highest velocity behind Kubernetes): nobody sells the instrumentation standard — it's a commons; the storage, correlation, alerting, and access control on top are the product. Grafana Labs passed $400M ARR as a top contributor. That split is the reference architecture for infrastructure monetization.

### 6. Three caveats before treating this as a plan
Open-source distribution is a top-of-funnel asset, not a conversion mechanism — if self-hosting is genuinely easy for your ICP, the managed service must be dramatically better, not marginally cheaper. Running someone else's production workload puts infrastructure COGS on your P&L (watch AI-driven margin compression). And Tomasz Tunguz's 2015 conclusion still holds eleven years later: there isn't a strong correlation between an open-source startup's license and its ultimate success.

## References
- Pairs with `oss-license-trap-fork-cycle` (the original trap mechanics this completes), `open-source-license-economics-2026`, `open-source-monetization-hybrid-trends-2026` (the assurance-layer trend), `open-source-ai-monetization-playbook-2026` (give-away/keep matrix), `open-commons-acquisition-neutrality-2026` (the consolidation counterweight).
- A-Tech alignment: open source (evidence-based defense of permissive licensing against panic relicensing), financial freedom (the 48–73% managed-cloud revenue share is the number that reframes "how do we make money"), practical (the three-step sequence + procurement framing), privacy (N/A — honestly flagged).