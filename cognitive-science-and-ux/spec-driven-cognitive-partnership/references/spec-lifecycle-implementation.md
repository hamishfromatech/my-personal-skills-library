# Spec-Driven Cognitive Partnership: Implementation Patterns

## OpenSpec Integration

[OpenSpec](https://github.com/openspec-dev/openspec) is an open-source spec-driven development framework that aligns with the cognitive partnership pattern.

### Directory Architecture

```
project/
├── specs/
│   ├── add-retry-logic/
│   │   ├── spec.md          # The intent specification
│   │   ├── design.md        # Design proposal (AI-generated, human-reviewed)
│   │   └── tasks.md         # Implementation tasks
│   └── current/
│       └── ...
├── changes/
│   └── add-retry-logic/
│       ├── proposal.md      # AI's implementation proposal
│       └── delta.md         # What changed (token-efficient delta format)
└── AGENTS.md                # Multi-agent configuration
```

The `specs/` directory is the cognitive engagement layer — each spec is a durable, reviewable artifact. The `changes/` directory is the AI execution layer — proposals and deltas are generated against specs.

### AGENTS.md Configuration for Cognitive Partnership

```yaml
# AGENTS.md
project_name: payment-service
spec_workflow: openspec

interaction_policy:
  require_spec_before_implementation: true
  explanation_gate: before_accept
  teaching_questions: after_accept
  manual_mode_schedule: "one_task_per_day"

agent_roles:
  - name: spec-drafter
    model: frontier  # helps surface context and constraints
    authority: draft_only
    description: "Helps developer draft specs by surfacing relevant patterns, existing code, constraints"

  - name: proposal-generator
    model: frontier  # generates implementation against spec
    authority: propose_only
    description: "Generates implementation proposals against finalized specs"

  - name: verifier
    model: mid-tier  # runs verification criteria
    authority: verify_only
    description: "Runs verification criteria from spec against implementation"

  - name: comprehension-checker
    model: mid-tier  # asks explanation gate questions
    authority: gate
    description: "Asks developer comprehension questions before acceptance"

comprehension_gate:
  questions:
    - "What does this code do in one sentence?"
    - "What's the one edge case most likely to break this?"
    - "What would you change if the constraint on max_duration were relaxed?"
  pass_threshold: 2  # must answer 2 of 3
  retry_on_fail: true
  fallback: "Ask AI to explain, then re-attempt gate"

teaching_questions:
  schedule: "after_accept, periodic"
  questions:
    - "If you had to write this without me, what's the first thing you'd do?"
    - "Which part of this are you least sure about?"
  log_to: "erosion-detection-dashboard"
```

### CLI Lifecycle Commands (OpenSpec-compatible)

```bash
# Explore: AI helps draft spec from natural language description
openspec explore "Add retry logic to payment client" --draft

# Propose: AI generates implementation against finalized spec
openspec propose add-retry-logic

# Refine: iterate on spec or proposal
openspec refine add-retry-logic --feedback "increase max retries to 5"

# Apply: AI executes the accepted proposal
openspec apply add-retry-logic

# Archive: spec + proposal + code stored as reusable AKU
openspec archive add-retry-logic
```

## UI Design Specifications

### The Spec Editor

The spec editor is where cognitive engagement happens. Design principles:

1. **Structured, not freeform:** Five fields (objective, outcomes, constraints, edge cases, verification). Developer must fill at least 4 of 5 to proceed.
2. **AI-assisted, not AI-authored:** AI can suggest constraints, surface edge cases, propose verification criteria — but the developer must explicitly accept each.
3. **Reviewable history:** Every spec edit is versioned. Developers can see how their thinking evolved.
4. **Reusability:** Specs can be templated and reused. "Add retry logic" becomes a template for "Add retry logic to [service]" across the codebase.

### The Explanation Gate UI

```
┌─────────────────────────────────────────────────────┐
│  Before accepting this implementation, answer:       │
│                                                     │
│  Q1: What does this code do in one sentence?        │
│  ┌─────────────────────────────────────────────────┐ │
│  │ [developer types answer]                      │ │
│  └─────────────────────────────────────────────────┘ │
│                                                     │
│  Q2: What's the one edge case most likely to break? │
│  ┌─────────────────────────────────────────────────┐ │
│  │ [developer types answer]                      │ │
│  └─────────────────────────────────────────────────┘ │
│                                                     │
│  Q3: What would you change if max_duration relaxed? │
│  ┌─────────────────────────────────────────────────┐ │
│  │ [developer types answer]                      │ │
│  └─────────────────────────────────────────────────┘ │
│                                                     │
│  [Ask AI to explain]  [Submit answers]               │
└─────────────────────────────────────────────────────┘
```

If the developer selects "Ask AI to explain," the AI provides an explanation, and the gate re-appears. The developer cannot bypass the gate — this is the structural safeguard.

### The Erosion Detection Dashboard

Aggregates signals across the team:

```
┌─────────────────────────────────────────────────────┐
│  Cognitive Partnership Health Dashboard              │
│                                                     │
│  Spec completeness:        82%  ████████░░  ▲ +5%   │
│  Gate pass rate (1st):     71%  ███████░░░  ▼ -3%   │
│  Manual-mode success:      63%  ██████░░░░  → 0%    │
│  Spec reuse rate:          45%  ████░░░░░░  ▲ +8%   │
│  AI acceptance rate:       76%  ████████░░  ▲ +2%   │
│                                                     │
│  ⚠ Gate pass rate declining — investigate team X   │
│  ⚠ AI acceptance very high on team Y — low engage? │
└─────────────────────────────────────────────────────┘
```

## When NOT to Use Full Spec-Driven Partnership

The full five-element spec + explanation gate is not appropriate for every task:

| Task type | Recommended mode |
|---|---|
| High-stakes (security, payment, core logic) | Full spec-driven partnership |
| Medium-stakes (feature implementation, bug fix) | Abbreviated spec (objective + verification only) + lightweight gate |
| Low-stakes (formatting, docs, boilerplate) | Ambient prompting acceptable; no gate |
| Learning / onboarding | Full partnership mandatory; gate + manual mode |

The mistake is applying the full pattern to low-stakes tasks (creates friction without benefit) or ambient prompting to high-stakes tasks (creates speed without comprehension). Match the mode to the stakes.

## A-Tech Application Matrix

### A-Coder
- Default interaction mode: spec-driven partnership for all code changes above a complexity threshold
- The IDE embeds the spec editor, explanation gate, and erosion dashboard
- AGENTS.md configurable per project
- Manual mode: one task per day from the developer's queue is flagged "no-AI"

### Be Practical
- Curriculum: "How to partner with AI without losing your edge" — teaches the six interaction patterns, the cognitive engagement spectrum, and the spec lifecycle
- Exercise: write a spec for a feature, generate with AI, pass the explanation gate, then implement manually from the spec alone — compare results
- Case study: comprehension debt in a team that used ambient prompting vs. a team that used spec-driven partnership

### Builder's Club
- Community spec library: reusable specs contributed by members (the open-source knowledge commons per `knowledge-activation-atomic-knowledge-units`)
- Spec review as a community practice: members review each other's specs before AI execution
- Open-source erosion detection toolkit: community-contributed metrics and dashboards