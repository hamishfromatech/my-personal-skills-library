# Platform Engineering & IDPs: MLR Reference Detail

## Source

Anjum, M. A. (2026). "Platform engineering and internal developer portals: a multivocal literature review." *Frontiers in Computer Science*, 8:1814498. DOI: 10.3389/fcomp.2026.1814498. License: CC BY (open access).

## Methodology

- **Type:** Multivocal literature review (MLR) following Garousi et al. (2019) guidelines
- **Academic databases searched:** IEEE Xplore, ACM Digital Library, Springer Link, ScienceDirect, Google Scholar
- **Date range:** January 2020 – February 2026
- **Total sources included:** 88 (44 Tier A peer-reviewed, 26 Tier B lower-tier/theses, 9 Tier C high-quality gray, 9 Tier D moderate gray)
- **Gray literature quality assessment:** AACODS framework (Tyndall, 2010), 0-12 scale, threshold ≥4
- **Academic quality assessment:** 5-item checklist, 0-10 scale
- **Two-pass screening:** 97.9% self-agreement (140/143 sources)

## Key Statistics

- 94% of organizations have adopted or plan to adopt platform engineering (Puppet 2024, N=470)
- 70% of developers spend 3-4 hours daily on non-core work due to insufficient internal tooling (Port 2024)
- Gartner: 80% of large engineering organizations will have dedicated platform teams by 2026
- Backstage: 89% penetration among IDP tool users (Port 2024, N=100 — vendor bias acknowledged)
- Only 2 of 88 sources (2.3%) from tier-1 venues with PE as primary topic
- Only 2 of 88 sources (2.3%) from tier-1 venues with PE as primary topic
- Academic engagement lags practitioner knowledge by 2-3 years

## The IDP Component Taxonomy (6 categories, from 36 architecture sources)

1. **Service Catalog** (28 sources) — software entities, ownership metadata, dependency graphs
2. **Golden Paths** (22 sources) — standardized workflows, "guardrails not gates"
3. **Self-Service Provisioning** (20 sources) — eliminates ticket-based workflows
4. **Scorecards** (16 sources) — component health vs. organizational standards; NO empirical evidence of effectiveness
5. **Workflow Automation** (14 sources) — Level 1 (manual) → Level 2 (template) → Level 3 (declarative)
6. **Governance & Standards** (12 sources) — policy-as-code, compliance, audit trails; least mature category

## The Three-Layer Measurement Model (from 51 sources)

| Layer | Framework | Key Indicators | Data Source | Cadence | Evidence Tier |
|---|---|---|---|---|---|
| Delivery Performance | DORA | Deploy frequency, lead time, CFR, MTTR | Git logs, CI/CD | Weekly | Tier A |
| Developer Experience | SPACE / DevEx Core 4 | Satisfaction, flow, efficiency, communication | Surveys + telemetry | Quarterly | Tier A |
| Platform-Specific | PE metrics | Self-service adoption, golden path adherence, internal NPS, onboarding time | Platform telemetry, surveys | Monthly | Tier C/D (unvalidated) |

## The Four Maturity Models

1. **CNCF PE Maturity Model** (2023) — 4 levels (Provisional, Operational, Scalable, Optimizing) × 5 dimensions. Qualitative only.
2. **Humanitec State of PE** (2022-2025, 4 volumes) — 5 stages, tool-adoption focused. Vendor bias.
3. **DORA Performance Clusters** (2024, N=39,000) — 4 levels (Low, Medium, High, Elite). Delivery outcomes, not capabilities. Strongest evidence.
4. **Puppet Evolution Stages** (2024, N=470) — 3 stages, DevOps-to-PE journey. Self-selected sample.

## The Five Adoption Barriers (from 28 sources)

1. Organizational resistance & mandate failure (DORA 2024: mandated platforms → lower satisfaction)
2. Cognitive load trade-off (hypothesized J-curve: productivity dips before recovering)
3. Measurement attribution (concurrent changes confound PE's contribution)
4. Technical sustainability (plugin debt, Backstage 800+ plugins, CNCF 1,100+ projects)
5. Skills & staffing (infrastructure + product management + UX combination is scarce; no formal education pathways)

## The Nine Research Opportunities (Prioritized)

1. Validated PE maturity model (highest impact)
2. PE-specific measurement instrument
3. Multi-organization case study
4. Longitudinal adoption study
5. PE definition consensus (Delphi study)
6. AI-PE integration evaluation (time-sensitive)
7. PE in regulated industries
8. IDP tool comparison with empirical data
9. PE and developer burnout

## Tool Comparison Summary

| Tool | Service Catalog | Templates | Scorecards | Self-Service | Open Source |
|---|---|---|---|---|---|
| Backstage | ✓ | ✓ | Plugin | Plugin | Yes (CNCF) |
| Port | ✓ | ✓ | ✓ | Partial | No |
| Cortex | ✓ | – | ✓ | Partial | No |
| OpsLevel | ✓ | – | ✓ | ✓ | No |
| Humanitec | Partial | ✓ | – | ✓ | Partial |
| Compass | ✓ | Basic | ✓ | Partial | No |

## Adjacent Research Referenced

- SPACE framework (Forsgren et al., 2021) — 5 dimensions of developer productivity
- DevEx Core 4 (Noda et al., 2024) — feedback loops, cognitive load, flow state
- Team Topologies (Skelton & Pais, 2019) — platform team as one of four team types; thinnest viable platform
- Razzaq et al. (2025b) — SLR of 166 papers, tooling environment is top-3 DevEx→productivity factor
- Development Environment as Code (Ghanbari et al., 2025) — codifying dev environments as version-controlled artifacts

## Data Availability

Complete literature database (88 sources with quality scores, RQ mappings, tier classifications), screening records, AACODS rubric, and figure scripts: https://github.com/mateenali66/pe-mlr-data, archived at Zenodo: https://doi.org/10.5281/zenodo.18713861

## Citation

Anjum MA (2026) Platform engineering and internal developer portals: a multivocal literature review. Front. Comput. Sci. 8:1814498. doi: 10.3389/fcomp.2026.1814498