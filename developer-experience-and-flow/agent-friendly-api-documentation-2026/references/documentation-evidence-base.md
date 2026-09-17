# Agent-Friendly API Documentation — Evidence Base

## Primary Source

**LogRocket Blog — Joseph, Frank. "How to write agent-friendly API documentation." May 1, 2026.**
URL: https://blog.logrocket.com/how-write-agent-friendly-api-documentation/

Author: software engineer and technical writer focused on the developer community and internet-scale applications.

## The Core Thesis

API documentation has always been the source of truth for developers integrating with a product. As AI agents become part of the development workflow, that source of truth now needs to serve a second reader: software that does not browse, infer, or troubleshoot the way a human does. Agent-friendly API documentation is documentation that a model or agent can reliably retrieve, parse, and use to take the right action.

## Why It Matters

Agents increasingly operate across real product workflows. A support agent handling a refund may need to authenticate, retrieve an order, check eligibility, and call a `createRefund` endpoint with the correct `orderId`, `reason`, and `amount`. If the docs do not clearly define the required fields, valid states, error responses, and sequencing rules, the agent may call the wrong endpoint, omit required data, or invent behavior the API does not support.

AI agents depend on reliable context. In a developer workflow, that context might come from a package README, an API reference, a framework guide, or a tool definition exposed through Model Context Protocol (MCP). MCP standardizes how applications expose tools, prompts, and resources to LLM-powered clients, which makes documentation and tool metadata more important, not less.

Human developers can work around incomplete docs (infer intent from naming, inspect network requests, search GitHub issues, ask another engineer). Agents are much weaker at filling in those gaps safely. When the contract is ambiguous, the model may guess — and those guesses turn into real API failures: invalid requests, broken multi-step flows, unnecessary retries, bad error handling, unsafe actions.

## Human-Focused vs. Agent-Focused Documentation

| Area | Human-Focused | Agent-Focused |
|---|---|---|
| Context | Assumes readers infer missing details | Makes assumptions, constraints, dependencies explicit |
| Error handling | Expects developers to debug interactively | Provides structured errors, causes, recovery steps |
| Workflow | Explains endpoints, often one page at a time | Defines ordered workflows across endpoints |
| Terminology | Uses natural variation | Uses consistent names for the same concept everywhere |
| Format | Relies on visual pages, navigation, examples | Prioritizes OpenAPI, JSON Schema, Markdown, tool definitions, metadata |
| Goal | Help developers understand and integrate | Help humans and agents retrieve, plan, execute correctly |

The biggest difference is tolerance for ambiguity. A developer can infer `is_active` is boolean. An agent performs better when the schema explicitly defines type, default behavior, valid transitions, and what the field controls.

## OpenAPI as the Source of Truth

A useful agent-facing OpenAPI spec should include:
- Fully typed request and response bodies
- Required and optional fields
- Enum values and constraints
- Authentication requirements
- Status-specific error responses
- Preconditions for state-changing actions
- Idempotency and retry guidance where relevant

Example: a refund endpoint should not only say it creates a refund. It should explain when a refund is valid, which order states are eligible, what happens if the amount exceeds the captured payment, and how the agent should respond to each error state.

## Document Workflows, Not Just Endpoints

Agents often need to complete multi-step tasks. If docs only explain endpoints in isolation, the agent has to infer the workflow. Instead, provide explicit workflow pages for common tasks.

Refund workflow example:
1. Call GET /orders/{orderId}
2. Confirm status is paid or delivered
3. Confirm refundableAmount is greater than 0
4. Call POST /refunds with orderId, reason, and amount
5. If POST /refunds returns 409, tell the user the order is not eligible
6. If POST /refunds returns 201, return the refund ID and status

Especially useful for MCP tools and agentic interfaces where the agent chooses which tool to call and in what order.

## Terminology Consistency

Terminology drift is a common documentation problem. Humans understand that "dashboard," "workspace," and "control panel" might refer to the same thing. Agents may treat them as separate concepts.

Rules:
- One canonical term for each resource and field
- If a field is `orderId`, do not call it `order_id`, "order number," and "transaction ID" unless genuinely different concepts
- When synonyms are unavoidable, define them clearly
- A simple glossary helps

## Writing Descriptions That Work for LLMs

LLMs use natural-language `description` fields inside schemas to decide when and how to call tools. The description field is not decorative — it is part of the execution surface.

A weak description tells the model what an endpoint does. A strong description tells the model when to use it, what must be true before use, and what the expected outcome is.

| Weak | Better |
|---|---|
| Creates a refund | Creates a refund for a paid order. Use only after confirming the order is eligible and the refund amount does not exceed the captured payment. |
| Deletes a user | Permanently deletes a user account after the user explicitly requests account closure or data removal. Do not use for temporary deactivation. |
| Updates status | Updates the order status. Valid transitions are pending to paid, paid to shipped, and shipped to delivered. |
| Gets customer data | Retrieves customer profile data by customer ID. Requires a valid access token with `customers:read` scope. |

To make schema descriptions more useful for agents:
- Be concise but explicit
- Describe intent, not just function
- Include preconditions and constraints
- Use consistent field and resource names
- Explain parameter relationships
- Document valid state transitions
- Include recovery guidance for expected errors

Example: Instead of "Deletes a user," write "Permanently deletes a user account when the user explicitly requests account closure. Do not use this endpoint to suspend, deactivate, or hide a user." That wording gives the agent a boundary.

## The Role of llms.txt

The `llms.txt` proposal defines a Markdown file served from the root of a website (usually at `/llms.txt`) that points AI systems to the most important documentation on the site. It is a discovery and prioritization layer, not a replacement for OpenAPI, Markdown docs, or structured schemas.

Distinction:
- `robots.txt` tells crawlers what they may or may not access
- `sitemap.xml` lists URLs
- `llms.txt` helps LLMs find high-value, context-rich documentation without parsing full HTML pages, navigation menus, ads, or client-side rendering

Example `llms.txt`:
```
# Example API docs

> Documentation for integrating with the Example payments API.

## Start here

- [Authentication](https://example.com/docs/auth): How to authenticate API requests
- [Refund workflow](https://example.com/docs/refunds): End-to-end refund flow and eligibility rules
- [OpenAPI spec](https://example.com/openapi.yaml): Machine-readable API contract
- [Error handling](https://example.com/docs/errors): Error codes, retry behavior, and recovery steps

## Agent guidance

- Use the OpenAPI spec as the source of truth for request and response schemas.
- Confirm resource state before calling state-changing endpoints.
- Do not call destructive endpoints unless the user explicitly requests the action.
```

For agent-facing docs, `llms.txt` can:
- Point agents to canonical docs instead of outdated pages
- Highlight workflow guides, API specs, error references
- Provide a lightweight map of documentation structure
- Reduce noise from HTML, navigation, duplicate pages
- Make documentation easier to retrieve in LLM-assisted development tools

`llms.txt` is still an emerging convention, not a formal web standard enforced across all AI providers. Treat it as a helpful addition to strong docs, not the whole strategy.

## Serve Markdown and Raw Specs

Many documentation sites are built for visual browsing. That is fine for humans but can make ingestion harder for agents if content depends on JavaScript, tabs, hidden panels, or complex navigation.

Where possible, provide:
- Clean Markdown versions of important pages
- Raw machine-readable files for specs
- `llms.txt` and `llms-full.txt` where documentation platforms support them
- Simplified content served to AI crawlers

The goal is not to abandon the visual docs site. The goal is to make the same information available in formats agents can retrieve and parse reliably.

## Examples in Practice (2026)

| Platform | Pattern |
|---|---|
| Cash App | Exposes documentation indexes for AI agents via `/llms.txt` and `/llms-full.txt`; supports Markdown versions of pages |
| Stripe | AI integration docs include guidance for using LLMs in Stripe integration workflows and installing Stripe's MCP server |
| Rightbrain | Provides machine-readable documentation endpoints for agents, LLMs, automated systems |
| Fern | Documentation platform that generates and maintains `llms.txt` and `llms-full.txt` |
| n8n | AI Agent node documentation explains how agents connect to tools and execute workflows inside n8n |

Broader trend: documentation is becoming part of the agent runtime. Docs no longer only explain how a developer should use a product; they also shape how automated systems discover tools, choose actions, and recover from errors.

## The Agent-Ready Documentation Checklist

- [ ] Complete OpenAPI or equivalent machine-readable API contract
- [ ] Typed request and response bodies
- [ ] Required and optional fields
- [ ] Enum values, constraints, and defaults
- [ ] Authentication and authorization requirements
- [ ] Workflow guides for common multi-step tasks
- [ ] Preconditions for state-changing endpoints
- [ ] Structured error responses and recovery guidance
- [ ] Consistent terminology across docs, schemas, and SDKs
- [ ] Markdown or raw-text versions of important pages
- [ ] A maintained `llms.txt` or equivalent documentation index
- [ ] Versioning and freshness signals for agent-facing resources

The highest-impact improvements are usually the ones that reduce ambiguity around workflows, destructive actions, and required fields.

## Why This Is Novel (Cross-Reference Confirmation)

Grep across `/home/user/.skills` confirmed:
- `agent-experience-design-2026` covers six AX *design* principles (schema-first, idempotency, machine-readable context, batch/streaming, reversible actions, negotiation protocols) — API *architecture* for agents, not documentation *writing craft*
- `devex-verification-bottleneck-framework` mentions the 3 D's (Design/Documentation/Discovery) in one section as part of the DevEx verification framework — gestures at the documentation dimension but does not provide the writing craft
- `generative-engine-optimization-2026` covers `llms.txt` for *brand/content* discoverability in AI search (+32% coverage lift) — not for *API documentation* discovery
- `ai-license-circumvention-defense` covers the Tailwind CSS `llms.txt` rejection case (the boundary caution) — not the writing pattern
- No existing skill provides: (a) the human-vs-agent documentation difference (tolerance for ambiguity); (b) the seven questions every endpoint must answer; (c) the workflow-page pattern with explicit sequencing; (d) the intent-vs-function description technique with weak/better comparison; (e) the `llms.txt` discovery/prioritization layer as applied to API docs; (f) the Markdown/raw-spec serving pattern; (g) the 12-item agent-ready documentation checklist; (h) the five platform examples (Cash App, Stripe, Rightbrain, Fern, n8n)

This skill is the *writing craft* complement to `agent-experience-design-2026`'s architecture and `devex-verification-bottleneck-framework`'s measurement framework.

## Cross-References to Adjacent Skills

- `agent-experience-design-2026` — AX design principles (this skill executes the Documentation and Discovery principles)
- `devex-verification-bottleneck-framework` — the 3 D's of agent-friendly APIs (this skill operationalizes Documentation and Discovery)
- `generative-engine-optimization-2026` — `llms.txt` for AI search discoverability (this skill covers `llms.txt` for API docs)
- `ai-license-circumvention-defense` — the `llms.txt` paid-tier boundary caution
- `spec-driven-development-framework` — machine-readable intent (this skill documents the API consumption side)
- `mcp-server-monetization-2026` — MCP tool definitions (this skill's intent-rich description pattern applies)
- `spec-driven-cognitive-partnership` — spec-driven development (this skill provides the documentation surface)