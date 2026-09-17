# Owned Stack Migration Guide

## From HubSpot to Owned CRM

1. **Export**: HubSpot → Settings → Export data (contacts, companies, deals, notes)
2. **Import**: ERPNext CRM or custom PostgreSQL schema
3. **Rebuild workflows**: N8N visual workflow builder replicates HubSpot automation
4. **Rebuild forms**: Static HTML forms → HTTP POST to owned endpoint
5. **Redirect**: Update website forms to point to new endpoints
6. **Decommission**: Cancel HubSpot after 30-day parallel validation

## From Mailchimp to Listmonk

1. **Export**: Mailchimp → Audience → Export CSV
2. **Deploy**: Self-host Listmonk (Docker, single container)
3. **Import**: Listmonk admin UI → Import subscribers
4. **Rebuild templates**: HTML email templates transferred to Listmonk editor
5. **Rebuild sequences**: Listmonk campaigns for drip sequences
6. **DNS**: Update SPF/DKIM records for owned sending domain

## From Zapier to N8N

1. **Audit**: Document every Zap (trigger, action, frequency)
2. **Deploy**: Self-host N8N (Docker or binary)
3. **Rebuild**: Create N8N workflows mirroring Zapier logic
4. **Test**: Run both in parallel; validate output matching
5. **Migrate**: Update triggers to fire only in N8N
6. **Decommission**: Cancel Zapier subscription

## Cost Comparison

| Tool | Monthly SaaS Cost | Owned Replacement Cost (amortized) | Annual Savings |
|---|---|---|---|
| HubSpot Starter | $45 | Self-hosted: ~$15/mo (server) | $360 |
| Mailchimp Standard | $20 | Listmonk: ~$5/mo (server) | $180 |
| Zapier Professional | $50 | N8N: ~$10/mo (server) | $480 |
| Jasper / Copy.ai | $49 | Local model: ~$0 (hardware sunk) | $588 |
| **Total** | **$164/mo** | **~$30/mo** | **$1,608/year** |

Note: Savings assume existing server/infrastructure. Additional hardware may be required for LLM inference.
