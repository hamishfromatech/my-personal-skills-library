# Open-Core AI-Feature Metering Evidence Base

## Primary sources

### Free Core, Paid AI Features (ShareAI, 2026)

The source model for the three-layer separation (core product access / commercial product value / AI consumption) and the credits-caps-top-ups structure. Key points:

- Traditional open-core pricing controls product access; AI adds a layer where every answer, summary, document extraction, code review, report, image, or agent run creates variable inference cost.
- A flat subscription or enterprise license hides that cost until power users turn a useful feature into a margin problem.
- The cleaner path: keep the core product useful, package premium AI features as optional value, and meter the AI traffic separately.
- Two customers on the same plan may create very different costs; if both pay the same fixed price for unlimited AI, the heaviest user defines the economics for everyone.
- The free core paid AI features model separates three things that should not be blended: core product access, commercial product value, AI consumption.
- Open-source businesses have long shown the software itself does not have to be the only paid value (Red Hat's subscription model: support, lifecycle management, stability, enterprise operating needs).
- Most open-core teams should avoid launching premium AI features as unlimited by default. Use included allowance → visible limits → paid top-ups → routed premium usage.
- Start with one premium AI workflow where usage is clearly valuable and uneven; learn before expanding.
- Community backlash avoidance: users accept paid AI when the boundary is honest; they revolt when AI is marketed as free, cost is hidden, then access is suddenly removed.

### The AI pricing and monetization playbook (Bessemer Venture Partners)

- AI economics are fundamentally different from SaaS — COGS matter again. Every AI query incurs real compute cost. Companies see 50–60% gross margins vs. 80–90% for SaaS. If the math doesn't work at 10 customers, it won't at 1,000.
- Three charge metrics: consumption-based (per API call / token), workflow-based (per completed task), outcome-based (per successful outcome).
- Hybrid models (base subscription + usage/outcome tiers) win when uncertain — customer predictability while capturing upside as they scale.
- The renewal cliff: much of the "sexy" AI product spend today lives in soft ROI territory; as 2025 pilots hit 2026 renewals, pricing must reflect actual value, not promise.
- Pricing complexity trap: nine different pricing approaches across contracts becomes unmanageable at scale. Identify one model that works at both 10 and 1,000 customers.
- Find the sweet spot through friction: start with a price; if customers say "sold" immediately, you're too cheap; raise incrementally until you hear "we have to think about that"; stop before it becomes a blocker.

### Monetizing Open Source AI: The DenchClaw Model (Dench, 2026)

A working three-pillar open-source AI monetisation case:
- **Pillar 1 — Dench Cloud (managed hosting):** one-click deployment, automatic updates, backups, uptime monitoring. Individual ~$29/mo; team flat fee; enterprise custom. The classic open-core / hosted service model (Redis Labs, Elastic, MongoDB Atlas).
- **Pillar 2 — Team Workspaces (shared infrastructure):** multi-user sync, access control, coordination — benefits from managed infrastructure; flat per team, not per seat.
- **Pillar 3 — Model and API access:** pre-configured models billed at a reasonable markup over API cost. Convenience layer, not a monopoly — users can bring their own keys. This is the AI-consumption metering layer in practice.

What doesn't work (from the case):
- **Feature gating:** sophisticated users fork the open version, remove the gates, publish it — two competing products confuse the market and alienate the best developers.
- **Training-data upsells:** ethically suspect in the AI era; users are savvy. Don't.
- **Platform lock-in through proprietary protocols:** the lock-in open-source users chose the product to avoid. Works short-term, damages trust long-term.
- **Support as primary revenue:** works for enterprise-first open source (Red Hat) but not for developer/founder-focused tools where users expect self-service and community support.

The community → cloud funnel: discover → self-host → get value → share with team → hit self-hosting friction → move to cloud → upgrade as team grows. Conversion driven by genuine friction reduction, not feature gates. 1–5% of free users convert to paid in well-run open-source businesses.

### Open Source AI Business Models (Malpani, 2026)

Consolidates the 2026 open-source AI landscape:
- Open source is the default in 2026; closed is the exception. Three forces: DeepSeek's 545% margin on open weights, Apache 2.0 winning as the enterprise standard, closed labs losing the long tail (data residency, fine-tuning, sovereignty, air-gapped deployment).
- The Give-Away/Keep Matrix: axes are Free-to-USE × Free-to-MODIFY. Four quadrants: Q1 Free Service, Q2 True Open Source AI, Q3 Proprietary SaaS, Q4 Open Core.
- Five proven revenue models: hosted inference, managed cloud (open core), enterprise skin, custom training & consulting, tools & pickaxes. Most successful companies stack 2+.
- The 5-Layer Monetization Stack: Adoption Engine → Self-Host Loss Leader → Managed Cloud → Enterprise Skin → Network & Data Moat. Skip a layer and the stack collapses.
- The license trap: Redis (→ Valkey), Elastic (→ OpenSearch), HashiCorp (→ OpenTofu) all tried restrictive licenses, all got forked, all lost developer mindshare. The cleaner play: keep Apache 2.0, compete on ergonomics not the artifact.

### OSS Monetization Models 2026 (ZAOOS/Sparkz research)

- Four sustainable models with complete data: (1) consumption-based cloud SaaS ($50M–$170M ARR — Supabase $170M, PostHog $57.5M); (2) open-core + enterprise add-ons (HashiCorp $583.1M); (3) freemium + enterprise support (70/30 SaaS split); (4) Drips + product revenue (emerging).
- What people actually pay for, ranked: Tier 1 (60%) enterprise reliability + compliance (SLAs, SOC2/HIPAA/PCI, dedicated support); Tier 2 (40–70%) consumption & scale; Tier 3 (15–25%) team collaboration (SSO, RBAC, audit); Tier 4 (5–10%) access & inner circle; Tier 5 (0–5%, mostly failed) governance & voting rights.
- The contributor attribution gap: 60% of OSS maintainers earn zero; 50.1% of OSS labour uncompensated; 66.7% invisible (triaging, review, moderation, docs, translations).

## The metering-layer routing pattern

From the ShareAI Builder model — the implementation pattern for routing AI consumption without rebuilding billing infrastructure:

1. The product routes AI inference traffic from the existing product through a metering/marketplace layer.
2. The product configures a surcharge or margin for that routed usage.
3. The customer pays the metering layer directly for the routed usage.
4. The metering layer routes the inference through the marketplace.
5. The product is paid monthly based on generated earnings from app traffic.

The product keeps roadmap, repository, hosting, licenses, plans, customer experience, and community relationship outside the metering layer. Only the AI-usage path is metered. This is especially useful when AI usage varies by customer, workspace, team, feature, model, document volume, or workflow complexity.

## Why this is distinct from existing skills

- `monetization-and-revenue/open-source-ai-monetization-mastery-2026` covers the Give-Away/Keep Matrix and the 5-Layer Stack at the strategic level.
- `monetization-and-revenue/open-source-ai-give-away-keep-matrix` is the quadrant framework.
- `monetization-and-revenue/open-source-ai-five-layer-stack` is the layer architecture.
- `monetization-and-revenue/open-core-enterprise` is the classic open-core enterprise model (pre-AI-COGS).
- `monetization-and-revenue/hybrid-monetization-open-source-platforms` covers hybrid models generally.
- `monetization-and-revenue/bessemer-ai-pricing-playbook-2026` covers the three charge metrics and the renewal cliff.

None of these isolate the specific problem this skill solves: **when an open-core product adds AI features, the variable per-customer inference cost breaks the flat-subscription model, and the three-layer separation (core access / commercial value / metered AI consumption) is the structural fix.** This skill provides that separation, the free-core-vs-metered-AI decision matrix, the credits-caps-top-ups structure, the one-workflow-first discipline, the anti-backlash communication pattern, and the metering-layer routing implementation.

## Cross-references

- `monetization-and-revenue/open-source-ai-monetization-mastery-2026` — the strategic stack this sits inside.
- `monetization-and-revenue/open-core-enterprise` — the pre-AI open-core model this extends.
- `monetization-and-revenue/bessemer-ai-pricing-playbook-2026` — the AI COGS economics and hybrid pricing.
- `monetization-and-revenue/real-time-metering-ai-agent-revenue` — the metering infrastructure for agent revenue.
- `monetization-and-revenue/mcp-gateway-monetization` — MCP gateway monetisation (an instance of the metering pattern).
- `privacy-and-trust/privacy-preserving-ai-monetization` — privacy as a metering-tier differentiator.

## Novelty confirmation

Grep across the existing monetisation skills corpus confirms no prior skill isolates the open-core + AI-COGS margin-erosion problem and provides the three-layer separation (core access / commercial value / metered AI consumption) with the credits-caps-top-ups structure and the metering-layer routing pattern. Adjacent skills cover the strategic stack, the quadrant matrix, and the general hybrid model, but not the specific AI-consumption metering fix for open-core products.