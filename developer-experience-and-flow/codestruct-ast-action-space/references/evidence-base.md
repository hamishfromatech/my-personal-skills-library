# CODESTRUCT Evidence Base

## Source

Kim, Hsu, Wang, Garg, Kumar, Ramanathan (AWS AI Labs, ACL 2026). "CODESTRUCT: Code Agents over Structured Action Spaces." Code: github.com/amazon-science/CodeStruct

## Experimental Setup

- Benchmarks: SWE-Bench Verified (500 Python GitHub issues), CodeAssistBench (135 multi-turn tasks, 7 languages)
- Models: GPT-5, GPT-5-mini, GPT-5-nano, Qwen3-Coder-480B, Qwen3-32B, Qwen3-8B
- Baselines: SWE-Agent (with repository map), OpenHands
- Budgets: $5 (large), $3 (mid), $1 (small) per task
- Same system/task prompts between baseline and CODESTRUCT

## SWE-Bench Verified Results

### Accuracy and Efficiency

| Model | Interface | Pass@1 (%) | Input Tokens | Output Tokens | LLM Calls | Cost ($) |
|-------|-----------|------------|--------------|---------------|-----------|----------|
| GPT-5 | Baseline | 66.0 | 452.7M | 0.81M | 16,436 | 574.0 |
| | CODESTRUCT | 67.2 (+1.2) | 366.3M (-19.1%) | 0.44M (-45.7%) | 16,307 (-0.8%) | 462.2 (-19.5%) |
| GPT-5-mini | Baseline | 60.4 | 593.7M | 1.27M | 18,560 | 151.0 |
| | CODESTRUCT | 62.0 (+1.6) | 404.5M (-31.9%) | 0.35M (-72.4%) | 14,811 (-20.2%) | 101.8 (-32.6%) |
| GPT-5-nano | Baseline | 19.6 | 808.0M | 0.86M | 24,037 | 40.7 |
| | CODESTRUCT | 40.4 (+20.8) | 1,137.4M (+40.8%) | 0.95M (+10.5%) | 27,278 (+13.5%) | 57.3 (+40.8%) |
| Qwen3-Coder-480B | Baseline | 61.2 | 805.8M | 1.50M | 26,961 | 365.3 |
| | CODESTRUCT | 66.2 (+5.0) | 705.3M (-12.5%) | 2.17M (+44.6%) | 32,346 (+20.0%) | 321.3 (-12.1%) |
| Qwen3-32B | Baseline | 14.8 | 366.0M | 1.23M | 25,543 | 55.6 |
| | CODESTRUCT | 16.0 (+1.2) | 302.1M (-17.5%) | 1.03M (-16.3%) | 24,653 (-3.5%) | 45.9 (-17.4%) |
| Qwen3-8B | Baseline | 13.2 | 84.0M | 0.11M | 8,833 | 2.36 |
| | CODESTRUCT | 13.0 (-0.2) | 51.8M (-38.3%) | 0.08M (-27.3%) | 8,313 (-5.9%) | 1.46 (-38.1%) |

### Key Patterns

1. **Accuracy improvements**: 1.2-5.0% for capable models, 20.8% for GPT-5-nano
2. **Token reductions**: 12-38% for most models (input tokens dominate cost)
3. **Output token reduction**: up to 72.4% (GPT-5-mini) — structure-aware edits specify only entity name + new content
4. **GPT-5-nano exception**: increased compute (+40.8%) but +20.8pp accuracy — structured actions enable sustained exploration that would otherwise terminate in failure
5. **Qwen3-8B exception**: no accuracy gain — failures stem from reasoning limitations, not interface brittleness

## Addressing Motivating Limitations

### Irrelevant Context → Input Tokens
- Selector-based retrieval returns only targeted syntactic unit, not entire files
- Example: text agent reads ~300 lines; CODESTRUCT reads ~50 lines (Figure 1 in paper)
- 12-38% input token reduction across most models

### Wasteful Exploration → Interaction Steps
- On django__django-11211: text agent 21 steps to locate target; CODESTRUCT 2 steps
- Total steps: 54 → 24 (55% reduction)
- LLM calls decrease up to 20.2% (GPT-5-mini)

### Brittle String-Matching → Tool-Level Errors
- For capable models: errors per instance decrease by 76-88%
- GPT-5: 0.845 → 0.103 (-87.8%)
- GPT-5-mini: 1.125 → 0.252 (-77.6%)
- Qwen3-Coder-480B: 0.909 → 0.216 (-76.2%)

### Redundant Code Regeneration → Output Tokens
- Structure-aware edits specify entity name + new content only
- GPT-5: output tokens -45.7%
- GPT-5-mini: output tokens -72.4%

## Empty Patch Analysis

| Model | Baseline Empty Patches | CODESTRUCT Empty Patches | Reduction |
|-------|----------------------|------------------------|-----------|
| GPT-5-mini | 35 | 6 | -82.9% |
| GPT-5-nano | 233 | 36 | -84.5% |
| Qwen3-8B | 179 | 138 | -22.9% |

- GPT-5-nano's empty-patch reduction correlates directly with +20.8pp accuracy gain
- These were instances where model had correct intent but could not express valid text edits
- Qwen3-8B reduces empty patches but without accuracy improvement — reasoning limitations persist

## Error Analysis

| Model | Approach | Total Errors | Errors/Instance | Reduction |
|-------|----------|--------------|-----------------|-----------|
| GPT-5 | Text-based | 426 | 0.845 | — |
| | CODESTRUCT | 52 | 0.103 | -87.8% |
| GPT-5-mini | Text-based | 568 | 1.125 | — |
| | CODESTRUCT | 127 | 0.252 | -77.6% |
| GPT-5-nano | Text-based | 459 | 0.911 | — |
| | CODESTRUCT | 551 | 1.093 | +20.0% |
| Qwen3-Coder-480B | Text-based | 458 | 0.909 | — |
| | CODESTRUCT | 109 | 0.216 | -76.2% |
| Qwen3-32B | Text-based | 6,556 | 13.008 | — |
| | CODESTRUCT | 5,155 | 10.228 | -21.4% |
| Qwen3-8B | Text-based | 7,179 | 14.247 | — |
| | CODESTRUCT | 6,031 | 11.966 | -16.0% |

**Capability threshold:** For sufficiently capable models (GPT-5, GPT-5-mini, Qwen3-Coder-480B), CODESTRUCT dramatically reduces errors. For smaller models, structured interfaces impose additional syntactic demands they struggle to satisfy.

## Ablation Study (SWE-Bench Verified, Qwen3-32B and GPT-5-mini)

### Removing readCode
- Qwen3-32B: -7.8% Pass@1 (8.2% vs 14.8% baseline), input tokens +41%, LLM calls +44%
- GPT-5-mini: -5.2% Pass@1, input tokens +7.6%, LLM calls +6.1%
- Without structured navigation, agents resort to exhaustive file reading and trial-and-error
- Qwen3-32B without readCode underperforms even text-based baseline (8.2% vs 14.8%) — hybrid config creates mismatch

### Removing editCode
- Qwen3-32B: -3.2% Pass@1
- GPT-5-mini: -1.4% Pass@1, but cost +38.7% ($141.22 vs $101.83)
- Without precise editing, agents fall back to brittle string-based edits requiring more validation cycles
- Small accuracy gap but disproportionate cost penalty

### Complementary Roles
- readCode: structured navigation minimizes exploration cost
- editCode: structured editing minimizes transformation cost
- Together: minimize both exploration and transformation costs

## CodeAssistBench Results (135 multi-turn tasks, 7 languages)

| Model | Interface | Accuracy (%) | Input Tokens | Cost ($) |
|-------|-----------|--------------|--------------|----------|
| GPT-5 | Baseline | 53.3 | 143.9M | 19.57 |
| | CODESTRUCT | 54.1 (+0.8) | 122.1M (-15.1%) | 16.74 (-14.5%) |
| GPT-5-mini | Baseline | 51.1 | 125.3M | 3.45 |
| | CODESTRUCT | 51.9 (+0.8) | 83.1M (-33.7%) | 2.30 (-33.3%) |
| GPT-5-nano | Baseline | 46.7 | 56.0M | 0.34 |
| | CODESTRUCT | 48.1 (+1.4) | 52.8M (-5.7%) | 0.32 (-5.9%) |
| Qwen3-32B | Baseline | 15.6 | 110.5M | 29.01 |
| | CODESTRUCT | 20.0 (+4.4) | 142.4M (+28.9%) | 38.71 (+23.7%) |

- Consistent accuracy gains across all models
- GPT-5-mini: largest efficiency improvement (-33.3% cost)
- Qwen3-32B: largest accuracy gain (+4.4) but at increased cost (deeper exploration)
- Results generalize beyond patch-based benchmarks to interactive code assistance

## Case Study: django__django-11211

| Metric | CODESTRUCT | Text-Based |
|--------|-----------|-----------|
| Total Steps | 24 | 54 |
| Steps to Locate Target | 2 | 21 |
| Edit Success on First Try | Yes | Yes |
| Final Outcome | Success | Success |

- CODESTRUCT: directly retrieves method bodies by qualified name (QuerySet.delete)
- Eliminates manual grep/sed searches and line-range guessing
- 55.6% step reduction

## AST Parsing Overhead

- tree-sitter for local parsing
- Median execution: 146-171ms (readCode), 189-212ms (editCode)
- Median LLM call latency: 4-12 seconds
- AST operations consumed 35.7 minutes across 500 SWE-Bench runs with GPT-5 (75.95 compute hours total)
- Less than 0.8% of total runtime — substantially outweighed by 12-38% token reduction

## Limitations

- File-level AST scope (no cross-file dependencies like inheritance hierarchies or call graphs)
- Python focus on SWE-Bench Verified (CodeAssistBench covers 7 languages)
- Requires syntactically valid source files for AST parsing
- Non-linear transformations (Min-Max, Min-Sum aggregation) not supported
- AST construction introduces tool execution time (negligible vs LLM latency)

## A-Tech Alignment

- **Open-source**: code available at github.com/amazon-science/CodeStruct
- **Data privacy**: local AST parsing (no data leaves the environment); MCP-based integration supports privacy-preserving deployment
- **Financial freedom**: 12-38% token reduction lowers agent operating costs; 19-33% cost reduction
- **Practical implementation**: MCP-compatible, integrates into existing agent frameworks without modification