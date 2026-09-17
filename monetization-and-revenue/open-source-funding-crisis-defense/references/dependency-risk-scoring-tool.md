# Dependency Risk Scoring Tool

## Purpose
A lightweight framework for calculating open-source dependency risk scores based on maintainer health, funding status, and production criticality.

## Scoring Formula

```
Risk Score = (Criticality × 0.25) + (MaintainerCount × 0.20) + (FundingLevel × 0.20) + (SecuritySensitivity × 0.20) + (LicenseRisk × 0.15)
```

### Factor Definitions

**Criticality (0.25 weight)**
- 1: Developer convenience only
- 2: Improves experience but not essential
- 3: Important operational dependency
- 4: Direct revenue impact if failed
- 5: Production stops immediately

**Maintainer Count (0.20 weight)**
- 1: 5+ active maintainers
- 2: 3–4 maintainers
- 3: 2 maintainers
- 4: 1 primary maintainer with occasional contributors
- 5: Single maintainer, no deputies

**Funding Level (0.20 weight)**
- 1: Well-funded (corporate backing, foundation, or sustainable commercial model)
- 2: Moderate funding (consistent sponsorships)
- 3: Some funding (sporadic donations)
- 4: Minimal funding (tip jar only)
- 5: Zero dedicated funding

**Security Sensitivity (0.20 weight)**
- 1: No security implications (documentation, linting)
- 2: Indirect security role (monitoring, logging)
- 3: Handles non-sensitive data
- 4: Handles authentication or sensitive data
- 5: Core security component (crypto, auth, sandbox)

**License Risk (0.15 weight)**
- 1: Stable permissive license (MIT, Apache 2.0) with no change history
- 2: Stable copyleft (GPL, AGPL) with no change history
- 3: Recently changed license or dual-licensed
- 4: Source-available license with commercial restrictions
- 5: License under active dispute or recently relicensed

## Risk Bands

| Score | Band | Action |
|-------|------|--------|
| 4.0–5.0 | Critical | Immediate direct funding + co-maintainership offer + internal fork with governance |
| 3.0–4.0 | High | Pledge funding allocation + dependency isolation + community advocacy |
| 2.0–3.0 | Moderate | Monitor + include in funding pool + low-priority outreach |
| <2.0 | Low | Standard monitoring |

## Automation Notes

**GitHub API queries:**
```bash
# Get repository health indicators
curl -s "https://api.github.com/repos/{owner}/{repo}" | jq '{stars: .stargazers_count, forks: .forks_count, open_issues: .open_issues_count, pushed_at: .pushed_at}'

# Get contributor count
curl -s "https://api.github.com/repos/{owner}/{repo}/contributors?per_page=1" | jq 'length'

# Get recent activity
curl -s "https://api.github.com/repos/{owner}/{repo}/commits?per_page=100&since=$(date -d '90 days ago' -I)" | jq 'length'
```

**Tidelift integration:**
```bash
tidelift align --json | jq '.dependencies[] | select(.risk_score > 3) | {name, risk_score, funding_status}'
```

## Example Scores

| Dependency | Criticality | Maintainers | Funding | Security | License | Score | Band |
|------------|-------------|-------------|---------|----------|---------|-------|------|
| Express.js | 5 | 5 | 5 | 4 | 1 | 4.15 | Critical |
| lodash | 4 | 3 | 4 | 2 | 1 | 2.90 | Moderate |
| left-pad | 2 | 5 | 5 | 1 | 1 | 2.65 | Moderate |
| openssl (sys) | 5 | 2 | 2 | 5 | 1 | 3.10 | High |
