# Acceleration Whiplash Defense Patterns

## Implementation templates for each defense layer.

---

## 1. Review Capacity Scaling

### Pattern: AI-Assisted First-Pass Triage

```
# Review Triage Pipeline
# Goal: filter AI-generated PRs so human reviewers see only what needs human judgment

INPUT: AI-generated PR
  ↓
[Automated structural scan]
  - Style/convention compliance (linter, formatter)
  - Test coverage check (does it have tests?)
  - Complexity threshold (cyclomatic complexity > X?)
  - Security scan (static analysis)
  ↓
[Decision gate]
  - PASS all → Fast-track (light review only)
  - FAIL any → Route to human review with flagged issues
  ↓
[Human review]
  - Reviewer sees: PR + flagged issues + AI structural scan summary
  - Reviewer focuses on: intent, logic, architectural fit (NOT style/syntax)
```

### Pattern: Review Queue Load Balancing

| Signal | Action |
|--------|--------|
| Reviewer has >5 PRs in queue | Do not assign new PRs to this reviewer |
| Senior engineer review time >40% of sprint | Redistribute to mid-level reviewers (with pair-review support) |
| PR complexity > threshold | Route to senior reviewer (don't waste seniors on simple PRs) |
| Reviewer has 0 AI-generated PRs this sprint | Check for avoidance pattern; review queue health |

### Pattern: Review Time Budget

```
# Per-PR review time budget
MAX_REVIEW_TIME_MINUTES = 30

if review_time > MAX_REVIEW_TIME_MINUTES:
    escalate_to_pair_review()
    log_overtime_incident()  # This PR needed more than budget — investigate why
    tag_for_postmortom()  # Was the code too complex? Was the reviewer unfamiliar?
```

---

## 2. CI/CD Pipeline Hardening

### Pattern: Agent-Volume-Aware Pipeline Scaling

| Signal | Action |
|--------|--------|
| CI queue depth > N builds | Spin up additional ephemeral runners |
| Commit frequency >2x baseline | Enable batched builds (merge-then-build) |
| Agent-generated commit ratio >50% | Require canary stage before production merge |
| Pipeline failure rate >5% | Investigate — is the pipeline breaking under volume? |

### Pattern: MCP-Based Build Diagnostics

Encode build failure expertise as guided workflows so AI agents and developers can perform root cause analysis:

```yaml
# MCP Build Diagnostic Skill
skill_name: diagnose-build-failure
trigger: build_failure_event
workflow:
  - step: classify_failure
    categories: [compilation, test, lint, dependency, cache_miss, flaky_test, resource_limit]
  - step: fetch_context
    data: [build_log, changed_files, cache_diff, test_output]
  - step: suggest_root_cause
    based_on: failure_category + context_patterns
  - step: suggest_fix
    based_on: root_cause + codebase_conventions
```

### Pattern: Cold-Start Cache Management

```
# Ephemeral CI cache strategy
PROBLEM: Ephemeral CI environments lose cache → cold start → slow builds → agent volume bottleneck

SOLUTION:
  1. Persistent cache layer (shared across ephemeral runners)
  2. Pre-warm cache on schedule (before peak agent commit hours)
  3. Cache hit rate dashboard (>80% target)
  4. Cache miss diagnosis (which tasks miss? why?)
```

---

## 3. Incident Infrastructure

### Pattern: Incident-to-PR Correlation

```python
# Link production incidents back to the merges that caused them
def correlate_incident_to_prs(incident):
    """
    Given a production incident, find the PRs likely responsible.
    """
    # Time window: PRs merged in the 1-72 hours before incident
    time_window = (incident.timestamp - 72h, incident.timestamp)
    candidate_prs = get_prs_merged_in_window(time_window)

    # Score by: files changed overlap with incident-affected components
    scored = []
    for pr in candidate_prs:
        overlap = len(set(pr.files_changed) & set(incident.affected_components))
        scored.append((pr, overlap))

    # Return top candidates for postmortem investigation
    return sorted(scored, key=lambda x: x[1], reverse=True)[:5]
```

### Pattern: Rollback Velocity Measurement

| Metric | Target | Alert |
|--------|--------|-------|
| Mean time to detect incident | <5 min | >10 min |
| Mean time to rollback | <10 min | >15 min |
| Rollback success rate | >95% | <90% |
| Incidents requiring rollback | tracked (trend) | rising trend |

### Pattern: Canary Deployment Default

```
# No AI-generated code goes directly to production
RULE: All AI-generated PR merges → canary stage → production

canary_stage:
  duration: 15-30 min
  traffic_percentage: 5-10%
  monitors: [error_rate, latency, incident_alerts]
  auto_rollback: true  # if monitors breach threshold

production_stage:
  trigger: canary_pass
  gradual_rollout: 10% → 25% → 50% → 100% (10 min intervals)
```

---

## 4. Code Churn Investigation

### Pattern: Git-Level Line Provenance

```bash
# Determine whether deleted code was recently written (rework) or legacy (refactor)

# For a file with high churn:
git log --follow --numstat -- <file> | \
  awk '$1==0 && $2>0 {print "deleted " $2 " lines"} \
       $1>0 && $2==0 {print "added " $1 " lines"}'

# Classify deleted lines:
# - Deleted within 1 sprint of being added → REWORK (AI code insufficient)
# - Deleted after multiple sprints → REFACTOR (productive cleanup)
# - Deleted as part of large-scale change → MIGRATION
```

### Pattern: Rework Hotspot Tracking

| Signal | Interpretation | Action |
|--------|---------------|--------|
| Same file modified >3x in 1 sprint | Rework hotspot | Investigate — is AI generating insufficient code for this area? |
| Same function rewritten >2x | Persistent insufficiency | Manual review required; consider no-AI zone for this function |
| Code survival rate <50% after 1 sprint | Throughput is hollow | Reduce AI generation speed; increase review before merge |
| Code survival rate >80% after 2 sprints | Throughput is real | Continue current pace |

### Pattern: AI-Code Survival Rate Dashboard

```
# The real throughput metric: not what was shipped, but what survived
WEEK_1_SURVIVAL = (lines_still_present_after_1_sprint / lines_merged) * 100
WEEK_2_SURVIVAL = (lines_still_present_after_2_sprints / lines_merged) * 100
REWORK_RATIO = (lines_deleted_within_1_sprint / lines_merged) * 100

# Target: WEEK_1_SURVIVAL > 70%, REWORK_RATIO < 30%
# Alert: WEEK_1_SURVIVAL < 50% → throughput is hollow, not real
```

---

## 5. Headcount Defense Argument Template

```
# When AI output gains are cited to justify engineering cuts:

ARGUMENT STRUCTURE:
1. Acknowledge: "Yes, output is up — epics +66%, tasks +33.7%, PRs +16.2%."
2. Present the asterisk: "But code churn is up 861%, meaning much of that output
   doesn't survive. We're measuring what was shipped, not what stuck."
3. Present quality costs: "Bugs per developer are up 54% (from 9% last year).
   Production incidents per PR are up 242.7%. This is now a reliability problem."
4. Present review collapse: "31.3% more PRs merge with no review at all.
   Median review time is up 441.5%. The senior engineers you'd keep are buried."
5. Present the key insight: "The work to ensure output is safe, correct, and
   maintainable has INCREASED, not decreased. The engineers being cut are
   the ones absorbing the quality gap."
6. Close: "Cutting engineers based on output numbers alone ignores the 861%
   asterisk. We need MORE review and quality capacity, not less."
```

---

## 6. The "What Does Protect You?" Investigation

The whiplash finding that strong foundations DON'T protect raises the question: what DOES?

### Hypotheses to test (community research opportunity):

1. **Review capacity scaling** — teams that proactively scaled review capacity (AI-assisted triage, load balancing) before AI adoption spiked may have lower whiplash
2. **Pre-merge quality gates** — teams with automated quality gates (complexity, coverage, security) before human review may filter more AI-generated issues upstream
3. **Local-first context** — teams using context-rich, local-codebase AI (A-Coder pattern) may produce fewer "superficially convincing but structurally wrong" PRs
4. **Canary/default deployment** — teams with mandatory canary stages may catch incidents before they compound
5. **Code survival tracking** — teams that measure survival rate, not just shipment rate, may detect hollow throughput earlier
6. **Finishing support** — teams with AI tools that help finish (not just start) may have lower stale-task rates

### Measurement framework:

```
# Compare whiplash severity across teams with different defense investments
whiplash_severity = (bugs_increase + incidents_increase + review_time_increase +
                     churn_increase + unreviewed_increase + stale_task_increase) / 6

# Correlate with defense investment score
defense_score = review_scaling + pre_merge_gates + local_context +
                canary_deployment + survival_tracking + finishing_support

# Hypothesis: higher defense_score → lower whiplash_severity
# (This is the open research question the whiplash finding raises)
```

---

*Defense patterns compiled from Faros AI Engineering Report 2026 findings and A-Tech application synthesis.*