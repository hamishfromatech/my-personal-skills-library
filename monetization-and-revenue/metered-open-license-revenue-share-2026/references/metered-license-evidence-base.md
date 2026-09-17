# Metered Open License — Full Evidence Base

## Timeline of the Revenue-Share Turn

- **July 3, 2026**: Moonshot releases Kimi K3 — 2.8T parameters (largest open-weights release), MoE with 104B active, 896 experts, native vision, 1,048,576-token context. Topped coding leaderboards (Arena.ai web-interface building; comparable to GPT-5.5 / Claude Opus 4.8 per Artificial Analysis). Launch triggered global selloff (July 17: Nasdaq −1.4%, S&P −1%, Philly Semiconductor −1.63%; Nvidia −2.21%, briefly losing top market cap to Apple).
- **Kimi K3 license**: free below **$20M total group revenue over trailing twelve months**; above that, commercial use forbidden until a paid agreement — sources put the ask at **up to 30% of revenue**.
- **~July 2026**: Moonshot signs revenue-sharing agreement with Chinasoft International (domestic channel; split undisclosed) — the domestic precedent.
- **Aug 7, 2026**: Reuters (exclusive) — Alibaba plans to ask major users of its next open-source model for a revenue share. Qwen3.8-Max: 2.4T parameters, biggest/most capable open release yet; license reported to trigger at ~**$50M group revenue**; abandons Apache 2.0 used for earlier Qwen models.
- **Aug 26, 2026**: Reuters — Moonshot in talks with Microsoft Azure, AWS, Google Cloud to host Kimi K3 for **up to 30% of hosting revenue**. No deal signed; hangups: revenue split, data access, token-usage auditing across three cloud billing systems. Would be first revenue-sharing pact between a Chinese AI lab and major US clouds.
- **Aug 27–28, 2026**: Harvey (US legal AI unicorn, $11B valuation, ~$350M ARR, 2,400 institutions) launches **Harvey Tenet** — a legal-specific model post-trained on Kimi K3. First Chinese open-weight model entering a top US vertical AI training system. Demonstrates the downstream ecosystem value the meter is designed to tax.
- **Aug 25, 2026 (context)**: Microsoft CEO Nadella warns enterprises not to rely on a single AI model; keep "token capital" (corporate knowledge, prompts, interaction logs) in-house — the multi-model posture that makes metered licenses negotiable rather than extractive.
- **Aug 7, 2026 (context)**: 70% of Microsoft's AI revenue comes from OpenAI ($24.1B FY) — incumbents' revenue concentration shapes their willingness to share with labs.

## Economics Detail

### Moonshot
- ARR trajectory: $100M (March 2026) → $200M (May) → $300M (mid-June) — pre-K3 growth.
- Valuation: ~$31.5B round → seeking $50B pre-IPO; Hong Kong listing as early as Q3 2026; dismantling offshore holding structure.
- Alibaba holds ~36% (invested ~$800M in fiscal 2024).
- Kimi K2.5 end-of-life announced (end of August 2026) — continuous model-line churn.
- K3 demand outran GPUs: Moonshot paused new subscriptions.
- Regulatory friction: US Treasury Secretary floated trade-blacklist addition; US officials allege illegal Nvidia chip acquisition and distillation from Anthropic's Fable model (Moonshot rejects distillation claim, citing original architectural changes).

### Alibaba
- Cloud external revenue +45% YoY (fastest in 5+ years); AI products ~RMB 49.5B (~$7.3B) annual run rate, ~35% of cloud external revenue, triple-digit growth for 12 straight quarters.
- Model-as-a-Service bookings: RMB 16B ARR, year-end target RMB 30B.
- But: consolidated adjusted EBITDA −30% YoY, GAAP net income −75%, quarterly capex RMB 67.7B, FCF outflow RMB 44.7B (net cash RMB 46.5B / $30.7B funds the buildout).
- The meter's function: protect cloud MaaS pricing + convert previously-unpaid deployments (Qwen in customer DCs / third-party clouds) into a future claim. Qwen: 2B+ downloads on Hugging Face.

### The DeepSeek counterfactual
- DeepSeek V4 ships under **unmodified MIT** — no gates, no meters. It set the price floor for everything Chinese. The open question: is free MIT a structural weapon (commoditize the token, monetize elsewhere) or a passing gesture? If DeepSeek follows into metering, the metered license becomes the new default; if it holds, resellers have permanent leverage.

## The Three Watch-Questions (from the primary analysis)

1. **Do Moonshot's US cloud talks close — and at what split?** First real market price for a frontier open model's meter; first proof the toll produces cash at scale.
2. **Does Alibaba's MaaS ARR hit RMB 30B** while cloud margins keep climbing at 40%+ growth? Whether the honest-money part of the strategy compounds.
3. **Does DeepSeek hold the MIT line?** Whether "free open weights" was structural or tactical.

## Structural Reading

- The license is a **meter, not a paywall**: it taxes demonstrable conversion of free weights into revenue (resellers, cloud providers, scaled deployments) and spares the recruiting funnel (developers, hobbyists). Design intent, not accident.
- The trigger is the **licensee's group revenue** (trailing 12 months), not model-attributable revenue — a deliberately wide base.
- At ~86× revenue, comparable distribution-layer deals (Stripe–OpenRouter) show the market prices positions, not P&Ls. The metered license is a lab-side attempt to claim a position over downstream revenue without closing the weights.
- Failure mode is competitive: free-substitute substitution by resellers (DeepSeek MIT). The exchange rate between ecosystem and claim is unknown; the earlier Apache Qwen licensing existed precisely because free distribution was the moat.

## License Comparison Detail

| Dimension | Permissive (MIT/Apache) | BSL/FCL (source-available) | Open-core | Metered open license (2026) |
|---|---|---|---|---|
| Boundary | None | Use-case restricted fields | Feature boundary | Licensee revenue threshold (group-wide) |
| Who pays | Nobody | Restricted-field users | Paid-tier users | Scaled commercial resellers/deployers |
| Revenue shape | $0 (unless API business) | License fees | Support/hosting | Negotiated % of downstream revenue (up to 30% reported) |
| Ecosystem effect | Maximum | Fork/community friction | Feature-split maintenance | Recruiting funnel untouched; reseller substitution risk |
| Enforcement | Copyright | Contract | Contract | Contract + audit (telemetry-dependent) |
| Precedent | DeepSeek V4 (MIT) | 2023–2025 BSL trend | Classic OSS | Kimi K3 ($20M/30%), Qwen3.8-Max (~$50M) |

## Practical Checklists

**For a startup building on open-weight models:**
- Inventory every model in the stack and its license class (permissive / source-available / metered).
- Model group-revenue trajectory against each metered threshold; set an internal alarm at 50% of the lowest threshold in the stack.
- Prefer permissive-licensed models where capability parity exists, for pricing predictability.
- If building on a metered model is strategically necessary, document the license chain: who is the licensee (you or your host?), what counts as "Model-as-a-Service" resale.

**For a lab choosing a license:**
- Metered licensing makes sense only with a cloud/API business to protect (Alibaba logic); without one, permissive (DeepSeek logic) maximizes distribution.
- Threshold placement: above the ecosystem-building zone, below demonstrable commercial conversion. Group-revenue triggers are wider but more defensible than model-attributable revenue.
- Budget for negotiation infrastructure: revenue-share deals (Chinasoft precedent, US cloud talks) require auditing, usage tracking, and data-access agreements — each with privacy implications.

**For enterprises:**
- Ask vendors which entity trips the threshold and whether your usage could retroactively create liability.
- Maintain multi-model posture (Nadella's "token capital" guidance) so no single license becomes extractive.

## A-Tech Synthesis

The metered license turn is the moment open-weight monetization grew a second axis: not what you may do with the model (BSL's question), but how much you make from it. A-Tech's practical stance: open weights remain the strategic default for sovereignty and financial freedom; the metered license is a warning to build revenue models license-aware from day one — and the DeepSeek MIT line is the benchmark against which any lab's "openness" should be measured.
