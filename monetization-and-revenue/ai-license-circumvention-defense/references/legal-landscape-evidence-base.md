# AI License Circumvention: Legal Landscape Evidence Base

## Source
Mat Cyb3rF0x Fuchs (Feb 22, 2026) — "Is AI breaking open source's business model." Medium. 11 min read.

## The Dual-Licensing Model Under Siege

### Pre-AI Pressure: Hyperscaler Commoditization
The cloud provider threat drove the 2018-2024 license change wave:
- **MongoDB** (Oct 2018): AGPL → SSPL. OSI rejected SSPL as non-open-source. Debian and Red Hat dropped MongoDB.
- **Elasticsearch** (Jan 2021): Apache 2.0 → SSPL/Elastic License. Amazon forked OpenSearch (496 contributors, 100M+ downloads in first year). Elastic added AGPL as third option (Aug 2024) — effectively conceding the fork couldn't be reversed.
- **HashiCorp** (Aug 2023): All products to BSL. OpenTofu fork backed by Linux Foundation (140+ corporate supporters). IBM acquired HashiCorp for $6.4B (April 2024).
- **Redis** (March 2024): BSD → RSAL/SSPL. Valkey fork (Linux Foundation) adopted or tested by 83% of large Redis-using companies within a year. Redis added AGPL back (May 2025) — ecosystem already fragmented.

RedMonk (Aug 2024): No clear evidence that license changes improved revenue for the companies involved.

### The AI Vector: A Different Attack Surface
AI attacks the license itself, not the deployment model. Before AI, clean-room reimplementation was prohibitively expensive. AI collapses this calculus.

## The "Copyright Laundering" Mechanism

Jamie Tanna coined the term: AI ingests copyleft-licensed code, strips provenance/attribution/license, produces output that appears unencumbered.

Sean O'Brien (Yale Privacy Lab): "The LLMs are copyright removal devices — copy open source data into it, and you get copyright-free data on the other side that you are free to plagiarize into newly copyrighted works."

Heather Meeker (March 2025): AI can replace one or both teams in a clean room process. "An AI that writes code has probably been exposed to almost all the open source code ever written."

## Productivity Data Supporting the Threat

- GitHub: Copilot accounts for ~40% of code in files where enabled
- AI code generation market: $4.91B (2024) → $30.1B projected (2032)
- GitClear (153M lines): 4x more code cloning with AI-assisted coding — copy/paste now more common than code reuse for first time in history

## The Seven Unresolved Legal Questions

### 1. Does training on copyleft code create derivative works?
- Prevailing view: model weights (statistical abstractions) don't preserve original expression in recognizable form → not derivative
- GEMA v. OpenAI (German court, Nov 2025): When ChatGPT "memorized" content and could reproduce it, encoding in model weights constituted copyright reproduction
- No U.S. court has directly addressed this for code

### 2. Is AI-generated functionally equivalent code a license violation?
- Non-literal copying doctrine (Computer Associates v. Altai, 1992): protects structure, sequence, organization even without literal copying
- Abstraction-Filtration-Comparison test: filter functional/necessary elements, compare only creative expression
- If AI replicates distinctive design choices (unusual architectures, distinctive organizational patterns) → could constitute infringement
- Purely functional reimplementation likely not protectable

### 3. Does AI compromise the clean room defense?
- Almost certainly yes
- Clean room requires implementing team to have NO access to original code
- AI trained on GitHub has accessed millions of copyrighted works by definition
- Thomson Reuters v. ROSS Intelligence: court rejected clean-room-like defense for AI training (not fair use when output competes with original)
- Nordstrom v. M&S Technologies: information through "dirty" intermediary to "clean" team vitiates defense entirely

### 4. Can AI-generated code be copyrighted?
- Thaler v. Perlmutter (D.C. Circuit, March 2025): human authorship is "bedrock requirement"
- U.S. Copyright Office (Jan 2025): mere prompting insufficient for authorship; "gaps between prompts and resulting outputs demonstrate that the user lacks control over the conversion of their ideas into fixed expression"
- Devastating paradox: if AI-generated code can't be copyrighted, open-source licenses (which depend on copyright) may not attach to it at all → copyleft obligations don't apply

### 5. Is training on copyrighted code fair use?
- U.S. Copyright Office (May 2025 Part 3 Report): NOT categorically fair use; case-by-case evaluation
- "Commercial use of vast troves of copyrighted works to produce expressive content that competes with them goes beyond established fair use boundaries"
- Register of Copyrights was fired the day after this report released

### 6. Doe v. GitHub (Nov 2022, on appeal to 9th Circuit)
- Most consequential pending case
- Most DMCA/copyright claims dismissed; breach of contract and open-source license violation claims survived
- Court treating open-source licenses as enforceable contractual agreements — not merely suggestions
- Could set critical precedent

### 7. Does API reimplementation constitute fair use?
- Google v. Oracle (Supreme Court, 2021): reimplementing Java APIs in different computing environment = fair use
- Court declined to rule on whether APIs are copyrightable
- If wholesale API copying by humans is fair use, AI-generated reimplementations almost certainly are too

## Countermeasure Analysis

| Countermeasure | Effectiveness | Limitation |
|---|---|---|
| Code watermarking (SynthID) | Low for code | Fewer ways to write correct code; harder to watermark than prose; trivially removable; open-source models can disable |
| Snippet detection (FossID, Black Duck, Threatrix) | Moderate for direct copying | Catches reproduction but not functional reimplementation with different syntax |
| API design patents | Weak | Google v. Oracle weakened; if human API copying is fair use, AI reimplementation is too |
| RAIL licenses | Low for code | Target AI model distribution, not code protection |
| Contextual Copyleft AI (CCAI) | Theoretical | Collective action problem — only works if widely adopted |
| Post-Open Zero Cost License (Perens) | Radical draft | Companies >$5M revenue pay 1% of revenue, administered like ASCAP royalties |

## Survival Strategies: Moats Beyond Licensing

### 1. Accountability and Support Services
Petabridge: best year ever in 2025, 19% subscriber growth. "Support subscriptions don't sell information — they sell accountability and availability." Aaron Stannard: AI-generated production assessments range from "half right" to "wildly insane."

### 2. Managed Services and SaaS
MongoDB Atlas, Confluent Cloud, Elastic Cloud: managing distributed systems in production is genuinely complex. Operational expertise, SLA guarantees, integration ecosystems matter more than code.

### 3. Data Moats and Network Effects
WordPress: dominance from plugin ecosystem and developer community, not PHP difficulty. Projects with proprietary data, user-generated content, or community effects create value AI cannot reimplement.

### 4. The Tailwind CSS Case
Downloads doubled (up 126.5%) while revenue fell 80%. Layoffs of 75% of engineering team. Adam Wathan rejected llms.txt: "the docs are the only way people find out about our commercial products."

## Community Response

- Mitchell Hashimoto (HashiCorp): considering closing external PRs due to AI "slop PRs"
- Armin Ronacher (Flask): describes "agent psychosis" — developer addiction to agentic coding
- QEMU: formally banned AI-generated code contributions over copyright provenance
- Linux Foundation: Agentic AI Foundation (Dec 2025, Anthropic/OpenAI founders), OpenMDW license (May 2025), generative AI policies for contributed code
- Linux kernel: proposed 'Co-developed-by' tag for AI-assisted patches (Torvalds dismisses special treatment)
- Bruce Perens (OSI co-founder): "Our licenses aren't working anymore. Businesses have found all the loopholes." Post-Open proposal: contractual revenue-sharing model administered like ASCAP royalties.

## Systemic Context
- 60% of open source maintainers unpaid (Tidelift 2024)
- Harvard Business School: commercial dependency on open source valued at $8.8 trillion
- Open letter (Sept 2025, 10 open-source foundations): open source "operates under a dangerously fragile premise" reliant on goodwill

## A-Tech Relevance

This source directly informs the `ai-license-circumvention-defense` skill. Key A-Tech implications:

1. **Code-access licensing is now the weakest moat layer** — AI can circumvent it
2. **Privacy-first architecture is an AI-resistant moat** — trust commitment cannot be regenerated
3. **Community network effects are AI-resistant** — cannot be regenerated
4. **Accountability/support is AI-resistant** — AI cannot provide 2 AM human expertise
5. **The Tailwind lesson applies directly** — structure documentation so AI discovery serves open-source adoption while commercial offerings remain human-discoverable
6. **Accept AI-generated contributions cautiously** — provenance risk, slop PR burden, copyright contamination

## Bibliography
- Fuchs, M. (Feb 22, 2026) — "Is AI breaking open source's business model." Medium.
- Meeker, H. (March 2025) — "AI Could Be Your Next Team for Clean Room Development."
- Tanna, J. — "Worries about Open Source in the age of LLMs."
- O'Brien, S. (Yale Privacy Lab) — quoted in Fuchs.
- GitClear — Analysis of 153M lines of code, 4x code cloning with AI.
- Thaler v. Perlmutter (D.C. Circuit, March 2025) — human authorship requirement.
- U.S. Copyright Office (Jan 2025) — AI authorship report.
- U.S. Copyright Office (May 2025) — Part 3 Report on fair use and AI training.
- GEMA v. OpenAI (German court, Nov 2025) — model weights as copyright reproduction.
- Thomson Reuters v. ROSS Intelligence — clean-room defense rejected for AI.
- Nordstrom v. M&S Technologies — dirty intermediary vitiates clean room.
- Computer Associates v. Altai (1992) — non-literal copying doctrine.
- Google v. Oracle (Supreme Court, 2021) — API reimplementation as fair use.
- Doe v. GitHub (Nov 2022, 9th Circuit appeal pending) — most consequential pending case.
- Perens, B. — Post-Open Zero Cost License draft proposal.
- Tidelift (2024) — 60% of maintainers unpaid.
- Harvard Business School — $8.8 trillion commercial dependency on open source.
- RedMonk (Aug 2024) — no clear revenue improvement from license changes.
- Petabridge / Stannard, A. — accountability as AI-resistant moat.
- Wathan, A. (Tailwind CSS) — llms.txt rejection, revenue collapse case.