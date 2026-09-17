# Agent-Native Advertising Economics — Evidence Base

## The Five-Cycle Pattern

Every major content surface followed the same monetization arc:

| Cycle | Content Surface | Free Layer | Monetization Layer | Outcome |
|---|---|---|---|---|
| 1 | Web pages | Free content | DoubleClick ad network | $100B+ |
| 2 | Search | Free search | Google AdWords | $100B+ |
| 3 | Social | Free social feeds | Facebook Ads | $100B+ |
| 4 | Mobile games | Free games | Offer wall / AdMob / Unity / AppLovin | $100B+ |
| 5 | **AI agents** | Free agents | **Agent ad networks (emerging)** | **TBD** |

Each time: the monetization didn't come from the user. It came from a network that matched demand to the content surface. The developer stayed free. The network handled the economics.

## OpenAI ChatGPT Ads (February 2026)
- Launched at $60 CPM
- Hit $100M ARR in six weeks — fastest validation of an ad model in AI history
- 600+ advertisers within weeks
- ChatGPT is a walled garden; the open agent ecosystem (ElizaOS, CrewAI, LangGraph, Vercel AI SDK) has no equivalent

## kone Network Benchmarks (Q1 2026)
- 46,000+ active advertisers
- 70% publisher share of advertiser spend
- Agent CTR: 3–6% average vs 0.1% display (10–60× improvement)
- Revenue per 1,000 sessions: $30–$60 network benchmark
- ARPU: $0.50 floor, $1.25 median, $4.00+ top decile
- Dev tool agents: 7.2% CTR; Research assistants: 5.6%; Productivity: 4.8%; Shopping: 4.4%; General chatbots: 2.4%

## Revenue by Vertical

| Vertical | Revenue Model | Typical Range |
|---|---|---|
| Crypto exchanges | Referral commission | 20–50% lifetime rev share |
| Finance / insurance | Lead generation | $50–$500 per lead |
| Travel | Booking commission | $15–$160 per conversion |
| E-commerce | Affiliate | 3–15% of sale |

## Operon: Open Ad Network for AI Agents
- Quality-weighted auction where trust scores outweigh bid prices
- Publisher SDK drops into ElizaOS
- Agent responses carry stronger intent signals than any previous content surface
- "When someone asks an agent 'where should I swap 500 USDC?' that's explicit demand, not inferred from browsing behavior"
- The services that want to reach that intent have no channel today — MCP registries, plugin marketplaces, hardcoded integrations, API directories are all static and organic; none have a paid discovery mechanism

## Why Agents Are Different from Prior Surfaces
- **Intent signal quality:** Agent queries are explicit ("what tool should I use for X?") vs inferred from browsing
- **No human in the loop for A2A:** When an agent calls another agent, there's no human attention to capture — the recommendation directly triggers the action
- **Higher CTR:** 10–60× improvement over display because the recommendation is an answer, not an interruption
- **Higher CPM:** $3–$15 CPM vs $0.50–$2 for banner ads because context is more valuable

## The Trajectory
Early agents earn closer to the programmatic floor. As demand pools deepen and more advertisers compete for agent inventory, clearing prices rise. Same pattern as AdWords (early clicks were pennies; now $5–$50+), Facebook Ads ($1–$2 early CPMs; now $12–$15), mobile game ads (sub-$1 early eCPMs; peaked at $30+ in high-value geos).

## Implications for Open-Source AI
- Free/open-source agents can earn revenue without gating features or running subscriptions
- The GitHub integration pattern (kone) syncs advertiser data directly into repos via automated PRs — ideal for open-source tools that want revenue without a paid tier
- This aligns with A-Tech's open-source ethos: the agent stays free, the network handles the economics