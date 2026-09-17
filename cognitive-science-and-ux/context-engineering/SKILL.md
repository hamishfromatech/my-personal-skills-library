---
name: context-engineering
description: Apply context engineering — the practice of curating what fills the context window rather than relying on prompt engineering — to build higher-quality AI outputs. Covers the four context failures (poisoning, distraction, confusion, clash), the lost-in-the-middle phenomenon, just-in-time retrieval, and the context curation protocol. Use when designing agent systems, MCP servers, RAG pipelines, or any workflow where LLM output quality depends on what information the model sees.
---

# Context Engineering

## Overview

Prompt engineering is dead. Context engineering is what actually moves models now.

In 2026, the frontier has shifted from crafting clever prompts to **curating the minimum set of high-signal tokens** that enable correct, verifiable outputs. Models like Claude Opus 4.6 advertise million-token context windows, but research proves that dumping everything in degrades performance. A Databricks study found model correctness drops around 32,000 tokens — long before million-token limits. Stanford researchers documented the "lost in the middle" phenomenon: information buried mid-context gets ignored, regardless of relevance.

Context engineering is the discipline of structuring, filtering, and delivering context so that models use it effectively. It is not about bigger windows — it is about better curation.

## The Four Context Failures

### 1. Poisoning
A hallucination enters context and gets repeatedly referenced. This happens when AI agents use outdated facts instead of live web data, then build reasoning chains on false premises.

**Mitigation:**
- Validate every fact before it enters persistent context
- Use live web data APIs for time-sensitive claims
- Maintain a "fact ledger" that tracks provenance for each context element

### 2. Distraction
Context grows so large the model over-focuses on conversation history and ignores training knowledge. The model becomes "trapped" in its own prior outputs.

**Mitigation:**
- Summarize and compress conversation history at regular intervals
- Distinguish static context (coding standards, API specs) from dynamic context (current state, real-time data)
- Use prompt caching for static context to reduce token burn and attention dilution

### 3. Confusion
Irrelevant content influences responses. Models perform worse when more tools are available in context, even if those tools are never invoked. Availability creates noise.

**Mitigation:**
- Register only the tools relevant to the current task phase
- Use just-in-time retrieval instead of pre-loading everything
- Filter retrieved documents by relevance score before adding to context

### 4. Clash
Different parts of context directly contradict each other. The model sees conflicting instructions, outdated and updated API docs, or contradictory examples.

**Mitigation:**
- Maintain a single source of truth for each context category
- Version-stamp all context elements and expire outdated ones
- When contradictions are unavoidable, explicitly rank sources by recency and authority

## The Lost-in-the-Middle Phenomenon

Stanford researchers proved that LLMs systematically fail to use information positioned in the middle of long contexts, regardless of relevance. Critical facts placed at the beginning or end of context are recalled accurately; identical facts placed in the middle are ignored.

**Practical implication:** Structure context like a newspaper lead, not a novel.

| Position | Recall Rate | Best Use |
|----------|-------------|----------|
| Beginning (top 10%) | ~95% | Task instructions, critical constraints, user intent |
| Middle (40–60%) | ~40–60% | Supporting documentation, reference material, examples |
| End (bottom 10%) | ~90% | Summary, next steps, confirmation of constraints |

**The Inverted Pyramid Protocol:**
1. Lead with the task and constraints (what must be done, what must not be done)
2. Provide essential reference material (API specs, coding standards)
3. Place examples and elaboration in the middle
4. End with a concise restatement of the task and success criteria

## Just-in-Time Retrieval vs. Pre-Loading

### Pre-Loading (Anti-Pattern)
Load all potentially relevant documents, schemas, and history into context before the model begins.
**Cost:** 32,000–82,000 tokens per operation for MCP, versus ~200 tokens for equivalent CLI calls.
**Result:** Context pollution, degraded reasoning, higher latency, higher cost.

### Just-in-Time Retrieval (Best Practice)
Retrieve only what is needed, only when it is needed.
**Implementation:**
1. Decompose the task into phases (gather → diagnose → design → execute → review)
2. For each phase, retrieve only the context relevant to that phase
3. After each phase, summarize the output and discard the phase-specific context
4. Feed the summary forward as the only context for the next phase

**Example: Code Review Agent**
```
Phase 1: Gather
  Retrieve: PR diff, related files, commit history
  Output: Summary of changes and intent

Phase 2: Diagnose
  Retrieve: Relevant coding standards, security checklist
  Output: Identified issues with severity ratings

Phase 3: Design
  Retrieve: Similar fixed issues from codebase
  Output: Recommended fix patterns

Phase 4: Execute
  Retrieve: Specific API docs for suggested changes
  Output: Suggested code changes

Phase 5: Review
  Retrieve: Original task summary + all phase summaries
  Output: Final review with consolidated feedback
```

## The Context Curation Protocol

### Step 1: Define the Signal-to-Noise Threshold
For every context element, ask: does this information change the output meaningfully? If removing it would not degrade quality, exclude it.

### Step 2: Apply the 40–60% Window Rule
Keep active context at 40–60% of the model's usable capacity. This prevents attention dilution while leaving headroom for the model's own reasoning tokens.

### Step 3: Separate Static from Dynamic
| Static Context | Dynamic Context |
|---------------|-----------------|
| Coding standards | Current task state |
| API specifications | Real-time data |
| Project conventions | Conversation history |
| Testing requirements | User preferences for this session |

Static context should be cached. Dynamic context should be refreshed and summarized.

### Step 4: Position Strategically
- Critical instructions: beginning
- Supporting evidence: middle (but keep minimal)
- Success criteria and next-step prompts: end

### Step 5: Validate Before Propagation
Before any context element is passed to the next phase or the next agent, verify that it is accurate, relevant, and non-contradictory.

## Context Engineering for Agent Systems

### Single-Agent Systems
- Maintain a compact "working memory" of key facts and decisions
- Summarize and compress after every major step
- Never let context grow beyond 60% of window capacity

### Multi-Agent Systems
- Each agent receives only the context relevant to its specialization
- Orchestrator agents synthesize outputs, not raw inputs
- Use a shared "context bus" for verified facts only; each agent builds its own reasoning context

### MCP Server Context
- Register tools selectively per task phase
- Unregister tools when not needed to prevent confusion
- Prefer direct CLI calls for token-efficient production pipelines (200 tokens vs. 32,000–82,000 for MCP)

## A-Tech Applications

### A-Coder (IDE)
- **Context Window Monitor:** Display current context usage as a percentage of model capacity
- **Smart Context Pruning:** Automatically compress conversation history and remove stale references
- **Phase-Aware Tool Registration:** Only load tools relevant to the current coding phase
- **Inverted Pyramid Prompts:** Structure all agent instructions with critical constraints at top and bottom

### Be Practical (Playbooks)
- **"Context Engineering for Agent Builders"** — playbook covering the four failures, retrieval patterns, and window management
- **Just-in-Time Retrieval recipes** for common workflows (code review, debugging, documentation)

### Builder's Club
- **Open-source context management toolkit:** Libraries for token-efficient context curation
- **Context benchmark leaderboard:** Community benchmarks measuring output quality vs. context size

## Measurement Framework

| Metric | Target | Measurement |
|--------|--------|-------------|
| Context window utilization | 40–60% | Token counter per request |
| Poisoned context incidents | 0 per 1000 requests | Fact-ledger audit |
| Mid-context recall accuracy | ≥ 80% | Controlled test with placed facts |
| Token cost per task | Minimized | Cost tracking per workflow |
| Output correctness | ≥ 95% | Human + automated evaluation |
| Tool confusion rate | < 5% | A/B test with full vs. selective tool registration |

## Cross-References
- See `cognitive-science-and-ux/context-maxxing-cognitive-agency` for the Brookings framework on user-controlled context and cognitive agency
- See `ai-agents-and-workflows/agentic-coding-trends-2026` for multi-agent orchestration patterns
- See `ai-agents-and-workflows/mcp-security-trust` for MCP server security and context isolation
- See `cognitive-science-and-ux/cognitive-load` for cognitive load theory applied to AI interfaces

## Sources
- Firecrawl — "Context Engineering vs Prompt Engineering for AI Agents" (Feb 2026): Context failures, lost-in-the-middle, just-in-time retrieval
- Databricks — "Context Rot in Long-Context Models" (2026): Performance degradation at 32K tokens
- Stanford — "Lost in the Middle: How Language Models Use Long Contexts" (2024): Foundational research on positional recall
- Anthropic — "2026 Agentic Coding Trends Report": Context engineering as a core skill shift
- Subramanya.ai — "Context Engineering: Why Prompt Engineering Was Never Enough" (Apr 2026): MSR paper on context engineering in open-source software
- Stackademic — "Prompt Engineering Is Dead. Context Engineering Is What Actually Moves Models Now" (2026): Practical framework synthesis
