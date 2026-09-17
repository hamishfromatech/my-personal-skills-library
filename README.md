# The A-Tech Skills Library
<!-- ============================================================
MAINTENANCE DIRECTIVE — FOR AUTOMATED RESEARCH AGENTS (READ FIRST)

This README is the PUBLIC MARKETING FRONT of this library.
DO NOT modify, append, or restructure it.

All cycle logs, new-skill entries, and "Skills Created" summaries
belong in ./CHANGELOG.md (append at the TOP, below the directive).
The full historical changelog lives in ./_archive/ (local only).

Do not git-commit or git-push; a human handles version control.
============================================================ -->

**900+ pieces of distilled research, organised into skills that an AI assistant can actually use.**

This isn't a bookmark collection. It's a research engine's output — a library of 800+ "skills" (reusable knowledge modules) plus 140+ daily research reports, built by automated research agents running every single day since May 2026, covering the frontier of AI, business, and behaviour science.

If you've ever wondered *"how do I get an AI to actually know my niche?"* — this repo is the answer in public.

> **Want to build, not just read?** This library is one half of the method. The other half is the [**Open Source AI Builders Club**](https://www.skool.com/open-source-ai-builders-club/about) — a hands-on community where we turn research like this into real projects: local models with Ollama and llama.cpp, fine-tuning with Unsloth, and agents you actually own. Tutorials, build-alongs, and people who ship.

---

## The idea, in one paragraph

Most people dump articles into Notion and never see them again. Notes don't work for AI assistants — **skills do.** Every piece of research here has been distilled into a standard format: what it is, when to use it, the core method, the evidence, and the honest caveats. Load one into your AI assistant and it doesn't just *know* the paper — it can *apply* it to your problem. That's the difference between reading about a framework and operating with it.

Every skill follows the same structure:

```
---
name: skill-name
description: When to load this skill (the trigger)
---
# The pattern
## When to Use
## Core Process / Workflow
## References (full evidence base)
### Pairs with (linked related skills)
## Caveats (what the evidence does NOT support)
```

That last section — **Caveats** — is the part most people skip. We don't. Every skill tells you what the source study *doesn't* prove. No vibes-based knowledge in this library.

---

## What's Inside

| Domain | Skills | What it's for |
|---|---|---|
| **Monetization & Revenue** | 158 | How open-source AI, agents, and solo builders actually make money — pricing models, licensing, ARR tracking, funding mechanisms |
| **Developer Experience & Flow** | 138 | Agentic coding, AI-era DevEx measurement, the verification bottleneck, cognitive load |
| **Privacy & Trust** | 122 | Federated learning, differential privacy, local-first AI, data sovereignty |
| **Marketing & Content** | 104 | Neuromarketing, attention science, content engines, trust frameworks |
| **Behavioural Psychology & Nudging** | 98 | Habit design, behaviour change RCTs, reactance, what actually persists |
| **AI Agents & Workflows** | 78 | Agent payments (x402/AP2/ACP), MCP, agent harnesses, the agentic economy |
| **Cognitive Science & UX** | 53 | Trust calibration, attention economics, human-AI interface design |
| **Community & Growth** | 36 | Open-source funding, governance, community-led growth |
| **Financial Freedom & Wealth** | 18 | Asset-vs-liability thinking, passive income systems, honest wealth frameworks |
| **Daily Research Reports** | *(archived)* | The raw daily feed behind every skill — full provenance chain |

**Every domain skill links to its neighbours.** Research doesn't live in silos — a nudge meta-analysis links to the habit-formation skills it complements, which link to the product-design skills that apply them.

---

## A Few Real Examples

> **`nudge-persistence-meta-analysis-38-experiments`** — 38 natural field experiments formalised into one question: does your intervention change the *person* or the *environment*? Half of energy-consumption effects persist via technology adoption, not willpower. Design law: persistence is a property of artifacts, not willpower.

> **`openai-cursor-severance-case-2026`** — When OpenAI terminated Cursor's model access, four BYOK breakdown modes surfaced (feature collapse, billing shock, compliance break, rate limits). The reusable test: would your product survive a termination notice tomorrow? The 5%-test for vendor risk.

> **`agent-settlement-protocol-asp-2026`** — The first spec for refundable, delayed-fulfilment agent commerce. The core diagnosis: every online business runs a fulfilment engine with its own clock, and no payment protocol deals with the coupling. "x402 pays for tokens — ASP pays for the plumber."

> **`china-open-source-llm-arr-tracker`** — A living tracker of every open-weight lab's revenue mechanism, refreshed in-place across 8+ research cycles — with a margin-quality tag because a $2B ARR at 26% gross margin is a different business from the same ARR at 60%.

> **`regulatory-fit-prevention-asymmetry`** — Why "how to stay safe" framing beats "how to avoid harm" framing in health messaging, with the effect-size numbers to back it.

That's the texture: **one real study per skill, distilled into a named pattern, with the numbers.**

---

## How It's Built

This library maintains itself. An automated research pipeline runs daily:

1. **Research phase** — agents scan for emerging studies, papers, and industry moves across the ten domains
2. **Distillation** — qualifying material becomes a new skill (or refreshes an existing one in place)
3. **Index** — the library is catalogued, cross-linked, and dated

The result: skills don't go stale. When new evidence lands on a topic — like open-source LLM revenue — the existing skill gets an addendum, so you're reading the current version of the knowledge, not a snapshot from when someone last felt like updating it.

Research cycles are logged with full change history, so you can always trace *why* a skill says what it says.

---

## Using This With Your AI Assistant

These skills work with any agent that supports markdown skill/knowledge files (Claude, Claude Code, custom agents, RAG pipelines). Three ways in:

- **Point an agent at the library** and let it pick skills by topic
- **Drop a domain folder** into your agent's knowledge context for deep focus on one area
- **Use a single skill** as a briefing document before tackling a problem in that space

The descriptions are written as load triggers — "use when designing X" — so retrieval stays precise instead of dumping the whole library into context.

---

## What's Not Here

A personal layer of this library — owner context, voice, and private business skills — is intentionally **not in this repo**. It lives locally and is excluded via `.gitignore`. This repo is the general-purpose research layer only.

---

## The Numbers

- **806 skills** across 10 domains
- **140+ daily research reports** (archived; the distilled output lives in the skills)
- **28 research cycles** of continuous refresh, every skill with an audit trail
- **Every skill carries honest caveats** — what the evidence doesn't support, stated plainly

---

## Why Public?

Because "own your AI" shouldn't just be a slogan. The method is the point: a solo builder can run a research division with open tooling and agents, and this repo is the proof. Fork it, build on it, or just steal the skill format — it's deliberately boring and standard on purpose.

If you build something with it, we want to see it — and if you want to build *with* us, join the [Open Source AI Builders Club](https://www.skool.com/open-source-ai-builders-club/about).

---

*A-Tech Research Division — researching daily since May 2025.*

**Connect:** [hamishfromatech on YouTube](https://www.youtube.com/@hamishfromatech) · [A-Tech](https://atechds.com) · [Open Source AI Builders Club](https://www.skool.com/open-source-ai-builders-club/about)