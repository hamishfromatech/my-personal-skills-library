# Plan-and-Execute Patterns for Agent Cost Optimization

Detailed implementation templates for the Plan-and-Execute pattern and related FinOps techniques.

## The Core Pattern: Plan → Execute → Verify

The Plan-and-Execute pattern decomposes a complex task into sub-tasks, uses an expensive model only for planning and verification, and delegates execution to cheaper models.

### Implementation Template

```python
class PlanAndExecuteAgent:
    def __init__(self):
        self.planner = FrontierModel()      # GPT-5, Claude Opus
        self.executor = MidTierModel()      # Claude Sonnet, GPT-4.1
        self.fast_executor = LocalSLM()      # Phi-4, Qwen-2.5-7B
        self.verifier = FrontierModel()

    async def run(self, task: str) -> Result:
        # Phase 1: Plan (1 frontier call)
        plan = await self.planner.decompose(task)
        # plan = [
        #   {"step": 1, "action": "gather_context", "complexity": "low"},
        #   {"step": 2, "action": "analyze_data", "complexity": "medium"},
        #   {"step": 3, "action": "generate_code", "complexity": "medium"},
        #   {"step": 4, "action": "run_tests", "complexity": "low"},
        #   {"step": 5, "action": "synthesize_report", "complexity": "high"}
        # ]

        # Phase 2: Execute (N cheap calls)
        results = []
        for step in plan:
            model = self._route_model(step["complexity"])
            result = await model.execute(step)
            results.append(result)

        # Phase 3: Verify (1 frontier call)
        final = await self.verifier.synthesize(task, plan, results)
        return final

    def _route_model(self, complexity: str):
        if complexity == "low":
            return self.fast_executor   # ~$0.0001/call
        elif complexity == "medium":
            return self.executor          # ~$0.01/call
        else:
            return self.planner           # ~$0.10/call
```

### Cost Comparison

| Approach | Model | Calls | Cost per Call | Total Cost |
|----------|-------|-------|---------------|------------|
| All-frontier | GPT-5 | 12 | $0.10 | $1.20 |
| Plan-and-Execute | Mixed | 2 frontier + 10 SLM | $0.10 + $0.0001 | $0.201 |
| **Savings** | | | | **~83%** |

## Semantic Caching Implementation

```python
import hashlib
from sklearn.metrics.pairwise import cosine_similarity

class SemanticCache:
    def __init__(self, embedding_model, threshold=0.92, ttl=3600):
        self.cache = {}  # {embedding_hash: (response, timestamp)}
        self.embedding_model = embedding_model
        self.threshold = threshold
        self.ttl = ttl

    async def get_or_compute(self, query: str, compute_fn):
        query_embedding = await self.embedding_model.embed(query)

        # Check for semantic match
        for key, (response, timestamp) in self.cache.items():
            if self._is_expired(timestamp):
                continue
            cached_embedding = self._load_embedding(key)
            similarity = cosine_similarity(query_embedding, cached_embedding)
            if similarity > self.threshold:
                return response  # Cache hit — zero LLM cost

        # Cache miss — compute and store
        response = await compute_fn(query)
        self.cache[self._hash(query_embedding)] = (response, time.time())
        return response
```

## Structured Output Token Reduction

Using JSON mode instead of free-form text reduces output tokens by 30–60%.

```
# Free-form (verbose):
"I think the function should return a list of user objects. 
 Let me explain why... [200 tokens of explanation]...
 The result is: [{'id': 1, 'name': 'Alice'}, {'id': 2, 'name': 'Bob'}]"

# Structured (compact):
{"users": [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]}
# 30 tokens vs 230 tokens = 87% reduction
```

## Per-Agent Spend Limit Pattern

```python
class AgentSpendGuard:
    def __init__(self, daily_limit=10.00, alert_threshold=0.80):
        self.daily_limit = daily_limit
        self.alert_threshold = alert_threshold
        self.spend = 0.0

    async def check_and_approve(self, estimated_cost: float, task_risk: str):
        if self.spend + estimated_cost > self.daily_limit:
            if task_risk == "high":
                return await self._request_human_approval(
                    f"Spend limit reached: ${self.spend:.2f} + ${estimated_cost:.2f} "
                    f"exceeds daily limit of ${self.daily_limit:.2f}"
                )
            else:
                raise SpendLimitExceeded(
                    f"Agent daily spend limit (${self.daily_limit}) exceeded. "
                    f"Current: ${self.spend:.2f}"
                )

        if self.spend > self.daily_limit * self.alert_threshold:
            self._send_alert(
                f"Agent at {self.spend/self.daily_limit:.0%} of daily limit"
            )

        return True

    def record_spend(self, actual_cost: float, model: str, task: str):
        self.spend += actual_cost
        # Log to cost attribution system
        self.cost_log.append({
            "model": model, "task": task,
            "cost": actual_cost, "timestamp": time.time()
        })
```

## Token Efficiency Techniques (from Claude Code benchmarks)

| Technique | Token Savings | Implementation |
|-----------|--------------|---------------|
| Path-scoped rules | 30–40% | Only load `.clinerules` for directories in current scope |
| CLAUDE.md trimming | 20–30% | Remove stale entries; keep only active conventions |
| Model routing | 50–80% | Route simple tasks to SLM, reserve frontier for reasoning |
| Structured output | 30–60% | JSON mode for all extractable data |
| Semantic caching | 40%+ for repeated patterns | Cache by intent similarity |
| Plan-and-Execute | 80–90% | Frontier plans, SLM executes |

**Combined effect:** 77–91% total cost reduction when all techniques applied systematically.