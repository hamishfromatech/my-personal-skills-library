# Proxy-Freedom Evidence Base

This file holds source extracts supporting the `ai-agents-make-free-software-matter-again` skill. Material is synthesized; short factual quotes are attributed to their speakers.

## Source 1 — George London (March 28, 2026)

**Title:** "AI Agents Could Make Free Software Matter Again"
**URL:** https://www.gjlondon.com/blog/ai-agents-could-make-free-software-matter-again/
**Author:** George London (CTO of Upwave; software writer)

### Core thesis

AI coding agents may be about to make free software (Stallman's sense — software that gives users the freedom to run, study, modify, and share) matter more than it ever has. Not open source in the bland corporate sense — free software in Stallman's sense. If an agent can read a codebase, understand it, and modify it on your behalf, access to source code stops being a symbolic right for programmers and becomes a practical capability for far more people. Suddenly the difference between software you can change and software you can only beg starts to really matter.

### Historical scaffolding (why free software faded)

- **Stallman's printer (1980):** The Xerox laser printer at MIT's AI Lab kept jamming; Stallman wanted the source to add a notify-on-jam feature; Xerox refused. Crystallized the founding of the Free Software Foundation and the four freedoms (Freedom 0: run; Freedom 1: study and change; Freedom 2: redistribute; Freedom 3: distribute modified versions). "Free as in speech, not free as in beer."
- **The 1998 rebrand:** Feb 3, 1998 at the Foresight Institute (a nanotechnology think tank, not a software org), Christine Peterson proposed "open source" over "free software." April 1998 Freeware Summit at Tim O'Reilly's, attendees voted 9-6 for "open source." Eric Raymond published "Goodbye 'free software'; hello, 'open source'" — key argument that the old terminology made corporate types nervous. Stallman was not invited to the summit. "Open source" kept the code-sharing practices but surgically removed the ethical claim about what users deserve. Stallman: "Open source is a development methodology; free software is a social movement."
- **The SaaS loophole:** The GPL required sharing source with anyone you *distributed* software to. If you never distributed — you just ran it on your servers and let people access it over the web — the license didn't apply. AWS offering managed Elasticsearch is the canonical example. The AGPL was designed to close the network-use loophole; it was powerful enough that Google maintains a broad public policy banning AGPL code inside Google.
- **The license-change cascade:** MongoDB → SSPL; Redis modules → Commons Clause (2018), then dual source-available (2024), then AGPL in Redis 8 (2025); HashiCorp → BSL (2023); Elastic → SSPL/ELv2 (2021) then back to AGPL (2024). Each validated the problem while failing to fully solve it.
- **Why users stopped caring:** When software runs on someone else's servers, having the source doesn't help. You can't run your own modified version because you don't run any version. The four freedoms became theoretical.

### The Sunsama case study (concrete demonstration)

The author wanted an iOS share-sheet button that saves a tweet to Sunsama (task manager) with an LLM-generated smart title and auto-categorization. Conceptually a 20-minute project. On closed software it became a six-layer Rube Goldberg machine:

1. Sunsama has no official API (feature request open since Dec 2019, ignored ~6 years). Only works because a user (Robert Niimi) reverse-engineered the internal API and published sunsama-relay + mcp-sunsama as open source.
2. Authentication is your actual Sunsama email and password (no API keys, no OAuth) — the serverless function must store real credentials.
3. The iOS wall: Apple does not document a public file format for programmatically generating arbitrary end-user Shortcuts, so the iOS Shortcut had to be built by hand (visual builder) with opaque error messages and no accessible logs. The agent can write a 200-line TypeScript server in seconds but cannot automate a five-step iOS Shortcut.
4. Twitter/X oEmbed endpoint for tweet metadata.
5. Self-hosted serverless function to deploy and maintain.
6. Anthropic API key for the LLM title generation.

Result: six layers of workarounds, three authentication mechanisms, a dependency on a stranger's reverse-engineering project, infrastructure now the author's responsibility, a manually-built iOS Shortcut that can't be version-controlled or shared. Compare to free-software alternative: the agent reads the source, understands the data model, modifies the share-sheet behavior — ten minutes, no reverse engineering, no gray-zone APIs.

### The proxy-freedom bridge

The four freedoms presuppose the ability to read and modify source code, which the vast majority of users lack (cited: Protesilaos Stavrou, Mahmoud Mazouz). An AI coding agent is an intermediary that can exercise technical freedom on behalf of a non-technical user. "Make my task manager auto-categorize tweets" exercises Freedom 1 through a proxy. This bridges the gap between software freedom as an abstract right and software freedom as a practical capability.

### Supporting thinkers

- **Nawaz Dhandala (OneUptime, Jan 2026):** AI agents give open source an "insurmountable advantage" — the question isn't "do we have the expertise to customize this?" but "do we want full control over our software stack?"
- **Martin Alderson:** Many things he'd previously seek a paid SaaS for, he now has an agent solve in a few minutes exactly the way he wants it. Agents lower maintenance costs dramatically, and unlike the one engineer who built your internal tool and left, "agents don't leave."
- **John Loeber (Feb 2026):** Predicted a "great repatriation of user data from lots of fragmented services into just one place," because having data locally makes AI dramatically more useful. Called it "tremendously hopeful for open-source values" and "bearish for proprietary software."
- **Vitalik Buterin (July 2025):** Shifted from favoring permissive licenses to copyleft, arguing "nonzero openness is the only way that the world does not eventually converge to one actor controlling everything."

### The maintainer-economics tension

- **"Vibe Coding Kills Open Source"** (CEU-affiliated 2026 working paper, arXiv:2601.15494v1): argues vibe-coding severs the user-maintainer feedback loop.
- **Adam Wathan (Tailwind CSS):** documentation traffic down ~40% from early 2023 even as usage grew; revenue down ~80%; 75% of engineering team laid off.
- **Mitchell Hashimoto (Terraform/Ghostty):** publicly said he was considering closing external PRs; moved Ghostty to a vouch-based contribution model in response to low-quality AI-generated contribution flood.
- **Stallman's gap:** The four freedoms tell us what users deserve. They say nothing about what maintainers deserve. That gap might end up mattering more than the freedoms themselves.

### The author's honest position

Not "everyone should self-host" (the author has self-hosted and knows the cost — security updates, backups, SSL, DNS, operational overhead). The SaaS model solved real problems. The demand for agent-customizable software is about to get loud; the industry needs new models (radically more open SaaS with real plugin systems and full API coverage, or agents that can host and operate software). The next buying criterion will be whether your agent can actually change the software to fit your needs. A CTO of a legacy SaaS living off switching-cost assumptions is in trouble. The author is steering Upwave toward building capabilities as easy as possible for AI agents to integrate.

### John Gilmore quote (the framing metaphor)

"The Net interprets censorship as damage and routes around it." The author's prediction: agents will interpret unfree software as damage — an obstacle between the user and what the user wants — and route around it.

## Source 2 — The open-vs-invocable counterargument (Leo, comment on the essay)

Two reframes from a commenter:
1. Are agents really expanding software freedom, or just shifting control? Users don't need to read code anymore, but now depend on models, APIs, and platforms — delegated execution inside controlled systems.
2. If agents become the main users, does "free software" still matter the same way? Agents don't care about source code. They care about what's callable, reliable, and composable. The axis may shift from open-vs-closed to **invocable-vs-non-invocable**. The deeper shift: software as a product → software as a network of capabilities.

## Source 3 — Mike Hearn counterargument (comment on the essay)

SaaS vendors will implement plugin APIs and sell a chatbot that lets you request customizations to your tenancy. Advantages: it's an integrated feature; sandbox plugins limit blast radius; users run nothing; CI on new plugins; adapt interfaces on the fly; study where agents get stuck and iterate internal skills. Retains core SaaS advantages (someone else administers, low price via amortized/timesliced compute). Stallmanism became irrelevant not due to SaaS (you can have free software SaaS; support was one of Stallman's funding ideas) but because free software had poor usability + high maintainer abandonment, and the volunteering model was unfixable (anarcho-communism). Remaining activity is Apache 2.0 from VC-funded startups, tiny one-person modules, or inertial maintenance. "One Man And His Agent aren't going to change the fundamentals."

## Source 4 — Edward J. Yoon counterargument (comment)

Users don't want to re-write software for customizing their AI model. If the application is AI-based, they just set a persona prompt in human language — the model understands it. The scenario of AI reading/compiling/distributing source code in text form is inefficient (excessive intermediate steps through human language). Rather than modifying software, exchanging the model's embedding data or activation tensors directly is more efficient.

## Cross-references to existing skills

- **open-source-agency-argument** — the four agency properties (verifiability, forkability, jurisdiction independence, permanent availability). This skill is the *demand-side* complement: why users will want those properties now that agents can exercise them.
- **open-source-license-economics-2026** — the BSL decision framework and fork economics. This skill is the *why-now* that makes the license choice strategically consequential again.
- **open-source-licensing-landscape-2026** — the quantitative 73% permissive / AGPL-resurgence data. This skill explains the demand driver behind the AGPL resurgence.
- **open-source-funding-crisis-defense** — the maintainer-economics tension (Tailwind traffic drop, Ghostty vouch model) connects here.
- **ai-license-circumvention-defense** — AI-enabled copyleft circumvention. The proxy-freedom bridge makes circumvention less necessary (agents can work with the source directly), but the maintainer-economics tension (vibe-coding kills open source) is the inverse risk.