# DevEx Metrics Compass Evidence Base

## Primary Source
"Too Many DevEx Metrics, Too Little Guidance." ACM Queue, published August 17, 2026. URL: queue.acm.org/detail.cfm?id=3831360.

## Tool
DevEx Metrics Compass: public, open-source web app at devexcompass.com.

## Dataset Construction
- Structured curation effort reviewing:
  - Scientific literature (research papers on DevEx measurement)
  - Public company resources: engineering blogs, conference talks, case studies, podcasts
  - 50+ engineering organizations including Google, Microsoft, Atlassian, Spotify, Capital One, Stripe
- Each metric annotated across multiple dimensions:
  - Data collection type (self-reported via surveys/interviews, automated via logs/telemetry, or both)
  - Collection maturity (getting started vs. established vs. advanced)
  - Framework or company of origin
  - AI relevance
  - Outcome goal (developer experience, product excellence, organizational effectiveness)
- Result: 120+ metrics organized across 8 dimensions

## The 8 Dimensions
1. Developer enablement (26× referenced) — conditions for daily work
2. Process (25×) — development workflow
3. Quality (20×) — code/system quality
4. Business (16×) — engineering contribution to business value
5. System (11×) — operational outcomes (reliability, stability)
6. Output (9×) — developer output (throughput, code)
7. Customer (7×) — end-user experience
8. Team (6×) — collaboration and team dynamics

## Key Statistics
- 93% of developers now use AI tools (2025 data, cited from Stack Overflow Developer Survey)
- 120+ metrics in the dataset
- 50+ engineering organizations analyzed
- 9+ frameworks in circulation (DORA, SPACE, DX Core 4, and others)
- 45 sources reference developer satisfaction (most-referenced metric)
- 72% of metrics collected via automated instrumentation
- 63% of metrics appear in both research and industry sources

## Most-Referenced Metrics (Top Cluster)
| Metric | Source Count |
|---|---|
| Developer satisfaction | 45 |
| Change failure rate (DORA) | 27 |
| Throughput | 27 |
| Produced code | 25 |
| Deployment frequency (DORA) | 21 |
| Cycle time (DORA) | 19 |
| AI tool adoption rate | 23 |
| Daily/weekly/monthly active users (AI tools) | 18 |
| MTTR (DORA) | 16 |

## Framework Summaries

### DORA
- Focused delivery-performance metrics: deployment frequency, change lead time, MTTR, change fail rate, deployment rework rate
- Industry standard for delivery performance
- Does NOT address DevEx broadly (team dynamics, conditions for sustainable delivery)

### SPACE
- Multidimensional: Satisfaction, Performance, Activity, Communication, Efficiency
- Framework (not a fixed metric set) — teams derive measures suited to context
- Explicitly pushes back against single-metric thinking

### DX Core 4
- Commercial framework (company: DX)
- Unifies DORA, SPACE, and DevEx framework into: speed, effectiveness, quality, business impact
- Prescriptive rather than navigational
- Shaped by cognitive load and flow state but these are NOT tracked as separate explicit metrics
- Composite Developer Experience Index (DXI) is proprietary

## Developer Intelligence Platforms
| Platform | Approach |
|---|---|
| DX | Self-reported sentiment + system data, organized around DX Core 4 + DXI |
| LinearB | Benchmarking and goal setting |
| Quotient | Automatic bottleneck surfacing + recommended actions |
| Faros | Unified data platform, custom metric definitions |

Note: These platforms track predefined metrics; they do not help organizations determine _which_ metrics to track in the first place.

## The DORA 2025 Report Context
- AI adoption increases individual productivity and job satisfaction
- BUT AI can add cognitive load on complex tasks
- AI invites higher output expectations without reducing demands on developers
- This tension is impossible to detect without the right measurement

## Five Persistent Misconceptions (Detail)
1. More metrics = better insight → Adding metrics without hypothesis creates noise
2. Easy to measure = worth measuring → Automated metrics are abundant but gameable (commits, PRs, LoC)
3. Automated data is enough → Log-based metrics capture system behavior but lack context; self-reported data explains why
4. Perfect metric set exists → No universal set; must tailor to tooling, team, situation (senior engineering leader quote)
5. Numbers tell the whole story → Context-dependent; deployment frequency may drop after service consolidation

## Maturity Levels (Detail)
### Getting Started (59 metrics)
- Available from standard tooling (code repository, CI, issue tracker) or a short survey
- No custom instrumentation needed

### Established (105 metrics)
- Requires combining data sources
- Structured survey instrument
- Light custom instrumentation
- Achievable within a few weeks

### Advanced (120+ metrics, full dataset)
- Requires significant custom tooling
- IDE and/or calendar integration
- Dedicated research infrastructure
- Ongoing qualitative data collection
- Domain of organizations with dedicated DevEx/research team

## Goodhart's Law Warning
"When a measure becomes a target, it ceases to be a good measure." — The stakes of measuring the wrong things: wasted effort, wrong incentives, gaming, misaligned pressure.

## Google's Metric Quality Criteria (cited in the paper)
A good metric is:
1. Intuitive to a broad audience
2. Reflects patterns understood to exist in the world
3. Doesn't reflect patterns we know don't exist
4. Useful in decision-making

## Limitations
- Many companies do not publicly share measurement practices
- Research naturally lags behind fast-moving industry practices (especially AI metrics)
- The open-sourced dataset will continue to evolve
- Academic review cycles delay AI metric validation

## Relationship to DORA 2025 Report
The DORA 2025 report (cited as reference 1 in the ACM Queue article) found:
- AI adoption increases individual productivity and job satisfaction
- AI adds cognitive load on complex tasks
- AI invites higher output expectations without reducing demands
- This tension requires the right measurement to detect and navigate

## A-Tech Integration Notes
- The Compass's "AI relevance" filter is directly applicable to A-Coder's DevEx dashboard design
- The 72% automation bias finding validates A-Tech's existing position (across multiple skills) that automated metrics must be paired with self-reported signals
- The maturity level framework maps to A-Tech's existing DevEx maturity models (unified-devex-measurement-stack-2026, devex-ai-era-measurement-framework)
