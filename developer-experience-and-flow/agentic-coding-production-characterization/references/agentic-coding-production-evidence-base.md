# Agentic Coding Production Characterization — Evidence Base

## Source
Liu, Qiu, Goiri, Fonseca, Bianchini & Choukse (UIUC / Microsoft Azure Research). "Agentic Coding in the Wild: Characterizing GitHub Copilot at Production Scale." arXiv:2608.00101, August 2026.

## Dataset
- **Source**: Sampled GitHub Copilot coding-agent traces, first week of June 2026
- **Scale**: 13.5M sessions, 3.2M users, 95.1M turns, 760.5M LLM calls, 774.7M tool calls
- **Tokens**: 44.9T prompt, 39.3B completion, 95T total
- **Models**: 27+ distinct models; top 3 = 53% of invocations
- **Tools**: 45+ distinct tools; top 11 = 90% of invocations
- **Geography**: US regions (3 timezones)

## Session Hierarchy
- **Session**: coding agent session lifetime
- **Turn (User-Turn)**: one user prompt + agent's full autonomous response chain
- **Step**: single LLM invocation or tool call within a turn

Canonical execution pattern:
```
User prompt → LLM → [LLM | tools → LLM]* → final response
```

## Per-Session Statistics

| Metric | Median | P75 | P90 | Mean | Skew |
|---|---|---|---|---|---|
| User turns | 3 | 7 | 15 | 6.1 | 2.0× |
| LLM calls | 15 | 42.8 | 100.5 | 40.6 | 2.7× |
| Tool invocations | 13 | 45.6 | 111.4 | 43.6 | 3.4× |
| Session duration (min) | 4.2 | 39.5 | 177.8 | 62.6 | 14.9× |

## Per-Turn Statistics

| Metric | Median | P75 | P90 | Mean | Skew |
|---|---|---|---|---|---|
| LLM calls | 4.5 | 7.9 | 15.9 | 6.6 | 1.8× |
| Tool invocations | 4 | 7.9 | 21 | 7.6 | 2.0× |
| Prompt tokens | 227.6K | 654.0K | 1.50M | 582.5K | 2.6× |
| Cached tokens | 217.2K | 621.7K | 1.43M | 545.3K | 2.5× |
| Completion tokens | 1.9K | 4.6K | 9.2K | 4.0K | 2.1× |
| Turn duration (s) | 63.4 | 163.0 | 392.1 | 396.3 | 6.3× |

## Key Finding 1: 1:1 LLM↔Tool Coupling
- Ratio of LLM calls to tool invocations ≈ 1:1 (mean 40.6 vs 43.6 per session)
- 87% of LLM calls are agent-initiated (auto-continued)
- 13% are user-initiated
- Implication: serving systems must treat LLM calls and tool invocations as inter-dependent pairs

## Key Finding 2: Session-Structured KV Cache
- Prefix cache hit rate: median 98% within turn
- Trajectory within turn:
  - Call #1: ~45% (cold start)
  - Call #2: ~86%
  - Call #3+: ~92-94% plateau
- Turn boundaries: -26% average drop (time-based eviction during user idle)
- After 2 min idle: median drops to ~70%
- After 10 min: near-complete collapse (median 0-5%)

## Key Finding 3: Model Switches Destroy Cache
- 6.4% of sessions have model switches
- Predominantly reactive (36% non-success rate before switch vs 8% baseline)
- After switch: only 8% cache hit (-67% drop)
- Manual→auto: 52% downgrades; Auto→manual: 51% upgrades
- Implication: pin sessions to single model; proactive cache staging on target model

## Key Finding 4: Context Compaction
- 7.8% of sessions undergo compaction (22.6% of long-context sessions >100K)
- Compact sessions account for 44.2% of tokens, 37.1% of LLM calls
- Median drops 72.8% of prompt tokens
- Cache hit rate drop after compaction: median -66.1%
- 34.3% of compaction events erase 90%+ of cache hit rate
- Compaction duration: median 22% of turn execution time, P90 34%
- Multiple trigger thresholds observed (50%, 65-66%, 80%, ~100%) — model-specific

## Key Finding 5: Turn Boundaries as Reclamation Signal
- Intra-turn idle: median 5.8s (container), 1.2s (KV cache)
- Cross-turn idle: median 243s (container, ~4.1 min), 172s (KV cache, ~2.9 min)
- User idle between turns: median 1,512s (25.2 min), tail >1 day
- >90% of idle intervals are intra-turn (too short to reclaim)
- 8-9% are cross-turn (2+ orders of magnitude longer)

## Key Finding 6: Tool Failures Amplify Compute
- 9% of turns contain tool failures
- Failures trigger autonomous retry loops: up to 4× compute amplification
- Failed run_build: 7-8× more tokens than success (verbose diagnostics)
- Failed run_command: 48× longer at P95 than success
- Deep-loop w/failures archetype: 36 LLM calls, 34 tool batches per turn

## Key Finding 7: Five User Archetypes

| Archetype | % Users | Sessions | Turns | Tools/Turn | Tokens/Turn | Description |
|---|---|---|---|---|---|---|
| Readers | 41.7% | 2 | 6 | 4.8 | 203K | Mostly reads/searches |
| Coders | 30.4% | 5 | 50 | 6.2 | 417K | Balanced read+edit+exec |
| Terminal users | 11.0% | 2 | 7 | 4.0 | 213K | Mostly terminal calls |
| Deep-loop users | 9.2% | 2 | 6 | 20.0 | 1.1M | Long agentic loops |
| Chat-only users | 7.6% | 1 | 2 | 0.0 | 23K | 100% Q&A; no tools |

- 50× token range between Chat-only (23K) and Deep-loop (1.1M) users
- Deep-loop users more active on weekends
- Coders: most sessions (median 5) and turns (median 50)

## Six Turn-Level Workflow Archetypes

| Archetype | % Turns | LLM Calls | Description |
|---|---|---|---|
| Deep-loop read | 30.5% | 9 | 7 tool batches; read-heavy exploration |
| LLM-only | 20.2% | 1 | No tools; pure reasoning |
| Multi-cycle edit | 19.0% | 5 | Read + edit + build feedback |
| Multi-cycle other | 13.2% | 4 | Read-dominant exploration |
| Deep-loop w/failures | 9.1% | 36 | 34 batches; retry loops (4× compute) |
| Deep-loop run | 8.1% | 7 | Terminal-heavy execution |

## LLM Call Characteristics
- Input-to-output ratio: >275:1 (median 68K prompt, 247 completion)
- Token breakdown: conversation history 48%, function-call messages 28%, system prompt 14%, retrieved context 10%
- LLM inference dominates wall-clock: 87.7% of single-turn session time
- Multi-turn: user idle 80.1%, LLM 13.7%, tools 2%
- LLM execution is overwhelmingly serial: 36.7% strictly sequential, P90 concurrency 1.4

## Tool Characteristics
- Top tools: get_file 35.0%, run_command 17.0%, replace_string 9.8%
- Top 11 tools = 90% of invocations
- Median tool execution: 166ms; mean 16.7s (heavy right skew)
- Read tools: ~100% success, tens of ms
- Execution tools (run_command, run_build): ~73% success, heavy-tailed latency
- Tool parallelism: 93% of batches single-tool; parallel batches mostly 2-3 read-only ops
- LLM-tool overlap: 97% of batches run inside LLM window, but only 7.7% of aggregate tool time hidden

## Idle-Time Predictor
- Architecture: LightGBM quantile regressors (12 quantiles, 400 trees each, ~2MB)
- Training: 150K sessions (one week), eval 50K (following week)
- Features: turn-level (duration, calls, tokens, failures) + session-level (turn index, prior idle, avg idle) + temporal (weekday)
- Session-level features dominate importance (avg idle duration + turn index >50%)
- ROC-AUC 0.73 for >60s idle binary task
- Captured idle time: 86-90% across all time horizons
- Inference: <3ms per turn boundary
- Output: survival curve S(t) = Pr(idle > t), updatable in closed form as time passes

## Serving System Design Implications
1. **Session-aware scheduling**: model entire agent execution chains, not individual requests
2. **KV cache as schedulable resource**: within-turn retain, cross-turn offload, model-switch proactive stage
3. **Adaptive compaction**: incremental, prefix-preserving strategies to maintain partial cache
4. **Archetype-aware SLOs**: 50× per-miss cost disparity demands tiered policies
5. **Tool reliability as efficiency leverver**: 9% failure rate → 4× compute amplification
6. **Turn-boundary resource reclamation**: natural trigger for container sleep + KV offload
7. **Capacity planning**: 4-5× diurnal swing; weekend sessions longer but fewer

## Limitations
- No quality signals (can't correlate infrastructure efficiency with task completion)
- No server-side view (GPU utilization, queue depth, batch size)
- Client-side traces only
- Rapidly evolving workload (results consistent Jan-June 2026 but longitudinal tracking needed)
- Sanitized traces planned for public release

## A-Tech Alignment
- **Open-source AI**: applies to Aider, Cline, OpenHands, any open-source coding agent
- **Data privacy**: internal telemetry, no prompt content collected
- **Financial freedom**: idle-time prediction enables 86-90% idle capture → significant cost savings
- **Practical implementation**: production-scale (13.5M sessions), reproducible methodology, planned public trace release