# Zendesk Outcome Pricing Case Study

## Source
Futurum Group — "Zendesk Bets on Autonomous AI Agents & Outcome Pricing" (Relate 2026)

## Model
Zendesk launched its Autonomous Service Workforce and Resolution Platform at Relate 2026, shifting to outcome-based pricing.

### Structure
- Base platform fee for core ticketing and support infrastructure
- Outcome component: charge per AI-resolved ticket
- If the human agent resolves the ticket, no additional AI charge
- If the AI resolves autonomously, charge per resolution

### Rationale
- Aligns Zendesk revenue with customer value (fewer support costs)
- Customer only pays when AI actually delivers
- Removes risk of paying for AI that doesn't perform
- Incentivizes Zendesk to continuously improve AI resolution quality

### Implementation
- AI resolution confidence threshold (e.g., 95% confidence = autonomous resolution)
- Human handoff trigger when confidence drops
- Clear reporting: "AI resolved X tickets this month = $Y charge"
- Monthly cap on AI resolution charges for predictability

## A-Tech Lessons
1. Outcome pricing works when the outcome is unambiguous (ticket resolved = yes/no)
2. Base platform fee ensures predictable revenue even in low-AI-usage months
3. Cap on outcome charges prevents customer anxiety
4. Confidence thresholds prevent false autonomous resolutions

## Date of Extraction
2026-05-31
