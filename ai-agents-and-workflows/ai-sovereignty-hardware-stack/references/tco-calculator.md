# Total Cost of Ownership Calculator

## Methodology

### Cloud API Costs (Monthly)
```
Monthly tokens (input + output) × blended price per 1K tokens
+ API subscription fees (ChatGPT Plus, Claude Pro, etc.)
+ Bandwidth / egress charges
= Total cloud monthly cost
```

### Local Hardware Costs (2-Year Horizon)
```
Hardware purchase price
+ Electricity (GPU wattage × hours/day × 30 × $0.15/kWh)
+ Maintenance / replacement reserve (10% of purchase price/year)
+ Labor for setup and maintenance (estimate hours × hourly rate)
= Total local 2-year cost

Break-even = Hardware purchase price / (Monthly cloud cost - Monthly local cost)
```

### Example: Single Developer
- Cloud: ChatGPT Plus ($20) + API for coding ($200) + embeddings ($30) = $250/month
- Local: RTX 4090 ($1,800) + electricity ($15/month) + maintenance ($180/year)
- Break-even: ~9 months

### Example: 5-Person Team
- Cloud: 5× ChatGPT Plus ($100) + shared API ($800) = $900/month
- Local: Dual A6000 ($7,000) + electricity ($80/month) + maintenance ($700/year)
- Break-even: ~11 months

### Hidden Costs to Include
- Downtime risk (local hardware failure vs. cloud SLA)
- Model update labor (pulling new weights, re-quantizing)
- Security hardening (local network, VPN access for remote team)
- Data backup (model weights are large; backup strategy required)
