# Share of Model Measurement Protocol

## Overview

Share of Model (SoM) measures how often a brand appears in AI-generated answers, with what frequency, in what position, and with what sentiment. This document specifies a reproducible monthly measurement protocol.

## Query Selection Methodology

### Step 1 — Define Query Clusters

Group priority queries into clusters by buying journey stage:

| Cluster | Description | Example (A-Coder) |
|---------|-------------|-------------------|
| **Category awareness** | Broad category questions a buyer asks when first exploring | "What are the best AI coding assistants?" / "Open-source AI IDE options" |
| **Comparison** | Head-to-head comparison queries | "A-Coder vs Cursor" / "A-Coder vs Copilot" |
| **Evaluation** | Specific capability or trust questions | "Is A-Coder privacy-first?" / "A-Coder open source license" |
| **Decision** | Purchase-intent queries | "A-Coder pricing" / "A-Coder enterprise plan" |
| **Reputation** | Brand reputation queries | "A-Coder reviews" / "A-Coder developer experience" |

Target: 15–25 queries per cluster, 75–125 total priority queries.

### Step 2 — Query the Five Major AI Platforms

For each query, collect results from:
1. ChatGPT (with and without web search, if applicable)
2. Google Gemini
3. Anthropic Claude
4. Perplexity
5. Google AI Overviews (search a Google account in the target market)

Record per query:
- **Cited?** Yes / No
- **Position** when cited: 1st mentioned, 2nd, 3rd, later, not mentioned
- **Sentiment**: Positive / Neutral / Negative / Mixed
- **Cited sources**: list the third-party sources the AI cites when mentioning the brand
- **Claim accuracy**: does the AI state accurate facts about the brand? (Yes / Partially / No)

### Step 3 — Score Each Query

| Outcome | Score |
|---------|-------|
| Cited, positive sentiment, accurate | 1.0 |
| Cited, neutral sentiment, accurate | 0.8 |
| Cited, mixed sentiment, accurate | 0.5 |
| Cited, negative sentiment | 0.0 |
| Cited, inaccurate | -0.2 (penalty — being cited with wrong information is worse than absent) |
| Absent | 0.0 |

### Step 4 — Aggregate to Share of Model

```
Share of Model (SoM) = Σ(query scores) / (number of queries × number of platforms)
```

Range: -0.2 to 1.0. Target: ≥ 0.50 on priority queries across at least 3 of 5 major AI platforms.

Also track:
- **Citation rate** = (queries where brand is cited) / (total queries)
- **First-mention rate** = (queries where brand is first mentioned) / (cited queries)
- **Source dependency map** = frequency table of third-party sources the AI relies on

## Sentiment Scoring Rubric

| Sentiment | Criteria |
|-----------|----------|
| **Positive** | AI recommends, endorses, or highlights strengths without significant criticism |
| **Neutral** | AI mentions the brand factually without endorsement or criticism |
| **Mixed** | AI mentions both strengths and significant weaknesses or limitations |
| **Negative** | AI criticizes, warns against, or explicitly does not recommend |

## Monthly Dashboard Template

| Metric | This Month | Last Month | Trend | Target |
|--------|-----------|-----------|-------|--------|
| Share of Model (overall) | | | | ≥ 0.50 |
| Citation rate | | | | ≥ 50% |
| First-mention rate | | | | ≥ 30% |
| Avg sentiment (cited queries) | | | | Positive/Neutral |
| Claim accuracy rate | | | | ≥ 90% |
| AI-referred traffic (GA4) | | | | ↑ MoM |
| AI-referred conversion rate | | | | ≥ 3x Google organic |
| AI-referred revenue | | | | ↑ MoM |

## Citation Source Gap Analysis

For each priority query where the brand is absent or poorly cited, identify the sources the AI relies on:

| Query | AI Platform | Top 5 cited sources | Brand present in those sources? | Action |
|-------|------------|--------------------|---------------------------------|--------|

Actions typically fall into:
- **Earn coverage** in a cited publication the brand is absent from
- **Create or improve** a profile on a cited review/comparison platform
- **Correct inconsistency** in entity definition across cited sources
- **Publish structured answer-first content** that directly addresses the query

## A-Tech-Specific Audit Templates

### A-Coder Priority Query Seed List

**Category awareness:**
- "Best AI coding assistants 2026"
- "Open-source AI IDE"
- "Privacy-first AI coding tools"
- "AI IDE for flow state"
- "Alternatives to Cursor"
- "Alternatives to GitHub Copilot"

**Comparison:**
- "A-Coder vs Cursor"
- "A-Coder vs Copilot"
- "A-Coder vs Windsurf"
- "A-Coder vs Claude Code"
- "Best agentic coding IDE"

**Evaluation:**
- "Is A-Coder open source?"
- "A-Coder privacy features"
- "A-Coder local-first AI"
- "A-Coder federated learning"
- "A-Coder developer experience"

**Decision:**
- "A-Coder pricing"
- "A-Coder enterprise plan"
- "A-Coder team subscription"
- "A-Coder self-hosted"

**Reputation:**
- "A-Coder reviews"
- "A-Coder developer feedback"
- "A-Coder community"

### Be Practical Priority Query Seed List

- "AI learning platform for developers"
- "Be Practical course reviews"
- "Skill acquisition AI platform"
- "Be Practical learning outcomes"
- "Be Practical pricing"

### Builder's Club Priority Query Seed List

- "Open-source AI community"
- "Builder's Club A-Tech"
- "Open-source contributor community"
- "AI builder community"
- "Builder's Club membership"

## Measurement Cadence

- **Monthly**: Full Share of Model audit across all priority queries and 5 platforms
- **Weekly**: Spot-check 10 priority queries on ChatGPT and Google AI Overviews (highest-volume platforms)
- **Quarterly**: Review and update the priority query list based on buyer journey changes and new competitors

## Tooling Notes

- Manual querying is the current baseline; AI citation patterns are sensitive to query phrasing, so use consistent phrasing across months.
- Several emerging tools (e.g., Profound, AthenaHQ, Otterly.ai) attempt to automate AI citation tracking — evaluate for accuracy against manual baselines before adopting.
- Google AI Overviews results vary by market and account; standardize the market and account used for measurement.
- Sentiment scoring is inherently subjective; use a single trained reviewer or a documented rubric with calibration samples to maintain consistency.