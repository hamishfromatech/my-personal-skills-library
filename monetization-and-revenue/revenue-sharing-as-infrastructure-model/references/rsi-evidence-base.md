# Revenue-Sharing as Infrastructure (RSI) Evidence Base

## Source
Mondjo, G. D. T. (2026). "Revenue-Sharing as Infrastructure: A Distributed Business Model for Generative AI Platforms." arXiv:2603.20533. Published March 2026.

## Three Generations of GenAI Business Models

### Generation 1: Pay-per-Use (like gasoline)
- First API offerings (late 2022/early 2023) adopted cloud computing models
- OpenAI: pay-per-token, patterned after AWS/Azure
- Advantage: simplicity and predictability for provider
- Disadvantage: transfers all adoption risk to developers; barrier for innovators without capital

### Generation 2: Diversification (freemium/subscriptions, like Spotify)
- From 2024 onward: vertical differentiation (more powerful models at higher prices)
- Subscription plans for intensive developers
- Free tier with quotas; paid tiers with higher caps
- Yu et al. finding: "introducing a marketplace can sometimes hurt profitability"

### Generation 3: Revenue Sharing (like YouTube)
- Platform hosts content/infrastructure for free
- Takes percentage of generated revenue (YouTube: ~45%)
- Ai et al.: three-layer markets with data intermediaries can lead to lose-lose outcomes
- RSI simplifies to two-layer (platform ↔ developers) to preserve efficiency

## Formal Model

### Actors and Parameters
- Platform offers free AI infrastructure to developers D
- Each developer i generates revenue R_i from end users
- Platform takes commission α ∈ [0, 1]
- Developer receives (1-α)R_i
- Platform marginal cost per API request: c
- Total requests by application i: q_i, linked to revenue by R_i = f_i(q_i)
- f_i is increasing (more usage → more revenue) and concave (diminishing returns)

### Developer Decision
Developer chooses effort e_i and price p_i to maximize:
$$\pi_i = (1 - \alpha) R_i(e_i, p_i) - \varphi(e_i)$$
where φ(e_i) is the cost of effort (strictly increasing and convex).

### Platform Decision
Platform chooses α to maximize:
$$\Pi = \sum_{i \in D} (\alpha R_i - c q_i)$$
Platform anticipates developers' reactions to α.

### Illustrative Example
If R = e, φ(e) = ½e², q = e, N(α) = 1:
- Developer: maximizes (1-α)e - ½e² → e* = 1-α
- Platform: Π(α) = (1-α)(α - c)
- Optimal: α* = (1 + c) / 2

**Examples:**
- c = 0.2 → α* = 0.6 (60% commission)
- c = 0.4 → α* = 0.7 (70% commission)

### Equilibrium Properties
- Effort e* is decreasing in α (higher commission reduces innovation)
- Developer participation N(α) decreases with α
- RSI preferred by developers with low capital; may be less advantageous for those with low effort costs

## Co-Creation Mechanisms (Heimburg et al., 2025)
Value is co-created through four developer mechanisms:
1. **System instructions:** Fine-tune AI behavior with precise instructions
2. **Contextual data:** Provide domain-specific information (legal docs, medical data, product catalogs)
3. **User input curation:** Organize, filter, or reformulate user requests
4. **Output revision:** Check, correct, or enrich AI responses

RSI maximally aligns platform incentives with co-creation because platform revenue depends on application success.

## Incentive Equilibrium (Keinan, 2025)
- Game-theoretic model analyzing platform-creator interactions with GenAI
- "Full-sharing equilibrium profiles" where all creators voluntarily share content
- Revenue allocation mechanisms affect creator utility and platform revenues
- Well-designed revenue-sharing leads to cooperation equilibria

## Societal Impact

### Emerging Markets
- 84% mobile penetration in low/middle-income countries (World Bank 2025)
- 90% of internet users access via mobile
- Smartphone is the primary gateway to income-generating digital services
- RSI could unlock the "latent jobs dividend" (World Economic Forum 2025)

### Sector Applications
- **Health:** 50%+ of people in low-income countries lack access to essential health services
- **Agriculture:** Sub-Saharan Africa loses $4B+ annually to preventable crop/livestock diseases
- **Legal services:** 90%+ of African small businesses operate informally without proper contracts
- **Business services:** Platforms like Jobop (Morocco) demonstrate digital labor market structuring

### Key Figures
| Indicator | Value |
|---|---|
| Mobile penetration (low/middle-income) | 84% |
| Adults earning money online (East Asia/Pacific) | 10%+ |
| African temporary work market | $100B (65% of employment) |
| Startups failing due to cash flow (first 2 years) | 60%+ |

## Challenges and Risks
- **Technical:** Reliable transaction tracking; fraud detection (underreporting); payment integration
- **Economic:** Optimal commission rate; managing off-platform revenue; balancing with existing offers
- **Legal:** Complex partnership contracts; DMA/DSA compliance; revenue calculation disputes
- **Regulatory:** Token classification risk; revenue-share may be subject to marketplace regulations

## Adjacent Skills
- `open-source-ai-monetization-mastery-2026` — strategic stack; RSI is a potential model within the stack
- `open-source-ai-give-away-keep-matrix` — give-away/keep decision; RSI gives away infrastructure, keeps revenue share
- `agentic-commerce-pricing-consolidation-2026` — market validation; RSI is emerging as a third-generation model
- `agent-fair-trade-agreement` — peer-to-peer settlement; AFTA enables the trust layer for RSI transactions
- `community-monetization-ladder` — community monetization; RSI is the platform-level equivalent