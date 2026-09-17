# Trust Economy & AEO Framework — Evidence Base

## Primary Source

Eric Yanfei Zhao and Yinuo Tang, "Competing in the Trust Economy: How AI Agents are Rewriting the Rules of Digital Strategy," *California Management Review* Insight, June 2, 2026. Published at UC Berkeley Haas School of Business.

- Zhao is Associate Dean for Research and Professor of Strategy at Oxford's Saïd Business School; dual recipient of top early career awards in strategy and entrepreneurship; previously at Stanford and Indiana University.
- Tang is Assistant Professor of Strategy and International Business at Peking University HSBC Business School.

## The Signal Event: Amazon v. Perplexity (November 4, 2025)

Amazon sued Perplexity AI alleging its "Comet" agent accessed Amazon's systems without authorization — evaluating products, comparing prices, completing purchases on behalf of users. Amazon's legal team invoked the Computer Fraud and Abuse Act (CFAA).

**Strategic interpretation (not the legal dispute):**

- No banner ads were seen. No sponsored listings were clicked. No traditional purchase funnel was navigated.
- Amazon's true fear is not web scraping; it is "Headless Commerce." Amazon's Retail Media Network (sponsored listings and banner ads brands pay billions for) depends on users seeing search-result pages. If an AI agent handles purchasing, users never see those pages, evaporating billions in high-margin ad revenue overnight.
- Amazon represents the incumbent "Platform Economy" (built on Aggregation). Perplexity represents the challenger "Agentic AI" economy (built on Agency and disintermediation). The lawsuit signals the end of the Attention Economy era.

## The Attention Economy — Structural Limit

The Attention Economy was built on a single insight: human attention, unlike physical goods, could be manufactured at near-zero marginal cost. Platforms kept users scrolling indefinitely, then auctioned that time to advertisers. The model created immense value through recommendation algorithms, personalized feeds, and infinite-scroll interfaces.

Fatal structural limitation: **human attention is finite**. As more firms competed for the same eyeballs, engagement-based models saturated. Tactics became increasingly aggressive — clickbait headlines, addictive interface designs — with diminishing returns.

## Why AI Agents Break the Attention Economy

Quantitative signals of the behavioral shift (as of late 2025):
- "Zero-click" searches (where a query is answered directly by AI without clicking a blue link) surpassed **65% of all search traffic**.
- Adoption rate of autonomous and semi-autonomous AI agents has outpaced the early adoption curves of both the smartphone and social media.

In the traditional Attention Economy, a consumer moves through a predictable funnel: enter query → scan results → click → compare → decide → buy. At every step, companies insert influence via ads, pop-ups, brand colors, notifications.

AI agents eliminate the funnel. When users delegate goals to an agent, they initiate a **B2A (Business-to-Algorithm)** transaction. The agent does not scroll, click, watch video ads, or respond to persuasive content. It is the ultimate form of "banner blindness."

## AI Rationality vs. Human Perception

- Humans are emotionally influenced by brand prestige, five-star badge placements, clever copywriting. Brands paid massive premiums for this traffic.
- AI agents are purely rational. They evaluate options using strict, structured criteria: reliability, performance, unit price, delivery time, warranties, historical outcomes. They ignore brand premiums unless explicitly instructed by the user to factor them in.
- When Perplexity's Comet evaluated vendors on Amazon, it was not persuaded by the engineered interface or sponsored placements. It queried price, delivery time, return rates, review authenticity. The true customer was the algorithm — and the algorithm has zero patience for marketing.

## The Trust Economy Framework — Three Pillars

### Pillar 1: Machine Readability (Structured Data and APIs)

Can an AI agent actually interact with your product, understand your pricing, and execute a transaction without ever touching a human interface? Your website is secondary; your API is your storefront.

**Criteria:**
- Standardized data schemas (JSON-LD)
- Headless commerce architectures
- Semantic clarity
- Open APIs
- Agents must instantly parse inventory, technical specifications, compatibility requirements without scraping messy HTML

**Actionable Steps:**
- Transition from web-first to API-first architecture
- Map all product catalogs to globally recognized ontologies so an LLM understands exactly what you are selling

**Example:** Travel giants Expedia and Kayak adapted early to OpenAI. Rather than relying solely on users visiting their websites, they built comprehensive, structured plugins and APIs specifically for LLMs. When a user tells an agent "Book me a flight to Tokyo under $800," the agent bypasses the Expedia homepage entirely, querying Expedia's API for structured pricing and availability, and executing the booking in the background.

### Pillar 2: Outcome Reliability (Performance Metrics and Guarantees)

Does your service deliver a measurable, repeatable result? An AI agent evaluating a vendor does not read the "About Us" page. It calculates expected value based on historical data.

**Criteria:**
- Agents query strict performance metrics: on-time delivery rates, SLAs, return rates, claims resolution speeds, historical defect rates

**Actionable Steps:**
- Make operational metrics machine-accessible and continuously verifiable
- Marketing claims must be replaced with programmatic guarantees
- If a product fails, the refund process should be executable via API, reducing the agent's calculated risk of choosing your brand

**Example:** In B2B logistics, digital freight forwarders like Flexport are positioned for the agentic era. An AI agent optimizing a supply chain won't be swayed by a sales brochure; it will ping APIs to compare real-time on-time delivery rates, customs clearance speeds, and dynamic pricing. The vendor with the highest statistically proven reliability wins automatically.

### Pillar 3: Verification Infrastructure (Auditable Processes and Certifications)

In the Trust Economy, unverifiable claims are invisible claims. How does an AI know your data is real and not hallucinated or manipulated?

**Criteria:**
- Agents require independent confirmation
- Cryptographic proofs
- Third-party API certifications
- Blockchain-based provenance records
- Verified, bot-resistant review systems

**Actionable Steps:**
- Integrate with established third-party verification networks
- If your product is "sustainably sourced," that claim must be backed by a digital, auditable trail an agent can instantly trace back to the supplier

**Example:** Apparel brands like Patagonia increasingly utilize supply-chain transparency tools (blockchain ledgers) to track materials. If a consumer instructs an AI shopping agent to "only buy a winter jacket from genuinely fair-trade, carbon-neutral sources," the agent will filter out brands with only PDF sustainability reports. It will select the brand whose supply-chain claims can be mathematically verified via third-party auditing APIs.

## SEO vs. AEO

- **SEO** was fundamentally about persuasion — crafting content, tweaking keywords, building backlinks to rank high on a human-reviewed search engine.
- **AEO** (Agent Engine Optimization) is entirely about accuracy and legibility. An AI agent does not reward compelling copywriting, A/B-tested headlines, personalized retargeting, or emotional brand narratives. To an AI making a procurement decision, these tactics are functionally invisible.
- AEO rewards verified data quality, structured information, and mathematically reliable performance records.

## Value Migration Context

Value migrated from physical infrastructure (telecoms) → hardware (PCs/Mobile) → software platforms (the Attention Economy) between 1995 and 2022. Now it is migrating again — from the interface layer to the trust layer. The companies that anticipated past migrations early (Google in search, Amazon in e-commerce) did so by understanding where value was flowing, not where it had always been.

## The Success-to-Interaction Ratio

Under the Attention Economy, success was measured through engagement metrics: click-through rates (CTR), dwell time, Daily Active Users (DAUs). These assumed more time spent with a brand equaled more value created.

In the Trust Economy, time spent is friction. Executives must measure the efficiency with which their systems solve user problems:

**Success-to-Interaction Ratio = (Number of user goals successfully achieved) / (Number of human interactions — clicks, scrolls, minutes — required to achieve them)**

In an agent-driven world, the perfect transaction requires zero clicks and zero seconds of human dwell time. The companies that win will be those that deliver the highest success rate with the absolute minimum human interaction.

## Navigating the Messy Middle: The Dual-Track Allocation Strategy

The transition will not happen overnight. The Attention Economy is deeply entrenched; for the next several years, most firms operate in a hybrid environment where both systems coexist. A consumer goods brand still needs Instagram reach to drive human awareness today; a marketplace still relies on ad-funded traffic to meet quarterly earnings.

The strategic mistake is treating legacy channels as permanent foundations for future growth, rather than as transitional cash cows.

**Track 1: Harvesting Attention (The Cash Cow)**
- Maintain investments in traditional SEO, paid media, engagement-driven product design
- Mentally reclassify these as declining assets
- Goal: maximize current revenue extraction to fund future transformation
- Optimize for short-term human conversion while accepting CAC in these channels will continue to rise

**Track 2: Building Trust Infrastructure (The Future Engine)**
- Simultaneously ring-fence a growing percentage of R&D and marketing budgets
- Build the machine-readable infrastructure autonomous agents will query
- Invest heavily in verified data systems, structured APIs, headless commerce, outcome tracking
- Build this infrastructure long before agent traffic becomes a material percentage of revenue — because by the time it matters, the window for establishing algorithmic trust will have closed

## What Leaders Need to Do Next (Three Immediate Actions)

1. **Audit Machine-Readability:** Ask technical and marketing teams: Could an AI agent evaluate, verify, and purchase our product without a human ever visiting our website? If no, bridging that gap is the most urgent strategic priority.
2. **Redesign Core KPIs:** Alongside CTR and dwell time, begin tracking the Success-to-Interaction Ratio. That metric will predict Trust Economy performance long before agent-driven revenue is directly measurable.
3. **Treat Data Infrastructure as a Strategic Marketing Asset:** The marketing department of the future will look more like an engineering team. The companies best positioned are not those with the largest human audiences or cleverest brand narratives, but those with the most verifiable, structured, machine-accessible performance records.

The window for building machine trust is open now. It will not be open indefinitely.

## Related Articles Referenced

- Sandeep Saini, "Governing the Agentic Enterprise: A New Operating Model for Autonomous AI at Scale," California Management Review Insight, March 20, 2026.
- Mohammad Hossein Jarrahi and Paavo Ritala, "Rethinking AI Agents: A Principal-Agent Perspective," California Management Review Insight, July 23, 2025.

## A-Tech-Specific Extensions (Not in the Source)

These extensions are A-Tech's own application of the framework, grounded in the source but extended for A-Tech's open-source + privacy-first + financial-freedom values:

- **Open-source as structural Trust Economy advantage:** Inherent verifiability (auditable code), reproducible builds, signed releases, public benchmarks — these are exactly the Pillar 3 verification infrastructure the Trust Economy rewards. Closed-source competitors make claims agents must trust on faith; open-source products provide proof.
- **Privacy-first as machine-readable trust signal:** Zero data retention, local-first processing, no surveillance — these are procurement-grade trust signals that enterprise buying agents increasingly filter on. Privacy is not just ethics; it is Trust Economy infrastructure.
- **The feedback-loop-severed risk:** Extends the `machine-mediated-market-strategy` "machine memory" risk — you will not see a ranking drop when a model stops including you; you will just stop being mentioned. Prevention (building trust infrastructure now) is dramatically cheaper than cure (repairing a damaged machine reputation).
- **Community signals as bot-resistant review systems:** GitHub stars, contributor activity, public issue resolution are the Pillar 3 "verified, bot-resistant review systems" that cannot be faked at scale the way paid reviews can.