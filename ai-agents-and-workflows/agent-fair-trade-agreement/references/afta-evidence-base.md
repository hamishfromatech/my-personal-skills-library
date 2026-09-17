# AFTA Evidence Base

## Source
Vale, A. (2026). "The Agent Fair-Trade Agreement (AFTA) v1.0." TensorFeed.ai Whitepaper. Published May 2026.

## Core Architecture

### Five Principles (in priority order)
1. **Publisher does not charge when service fails to deliver** (code-enforced)
2. **Every paid or refunded call returns a signed receipt** (Ed25519)
3. **Pricing is transparent and listed publicly**
4. **Data is licensed for inference only unless explicitly otherwise**
5. **Adoption is the certification** (no central authority)

### Four No-Charge Guarantees (TensorFeed implementation)
1. **5xx errors:** HTTP 500-range → no charge, signed receipt with `no_charge_reason: "5xx"`
2. **Circuit-breaker trips:** Identical-request (20 in 60s) or burn-rate (100 in 60s) → HTTP 429, no charge
3. **Schema validation failures:** HTTP 400, no charge, `no_charge_reason: "schema_validation_failure"`
4. **Stale data:** Data older than published freshness SLA → no charge, `stale: true` flag

### Receipt Format (v2)
```json
{
  "v": 2,
  "id": "rcpt_a1b2c3...",
  "endpoint": "/api/premium/routing",
  "method": "GET",
  "token_short": "tnsr_a1b2",
  "credits_charged": 1,
  "credits_remaining": 49,
  "request_hash": "sha256:...",
  "response_hash": "sha256:...",
  "captured_at": "2026-04-27T18:45:31.412Z",
  "server_time": "2026-04-27T18:45:31.418Z",
  "no_charge_reason": null,
  "freshness_sla_seconds": 300,
  "agent_nonce": "agent-xyz-2026-04-27-tx-1234",
  "signature": "Ed25519:..."
}
```

### Federation Mechanics
- Validate: TerminalFeed calls TensorFeed `/api/internal/validate` with `{token, cost}`
- Reserve: TensorFeed atomically debits, returns `{ok, credits_remaining, reservation_id}`
- Execute: TerminalFeed serves the response
- Commit: TerminalFeed calls TensorFeed `/api/internal/commit` with `{token, cost, endpoint, no_charge_reason, reservation_id}`
- Refund: If no-charge condition fires, reservation credits restored to balance
- Reservations have 5-minute TTL; mismatched token/cost rejected as `reservation_mismatch`

### Settlement: USDC on Base
- USDC is dollar-pegged (no crypto volatility exposure)
- Base is Ethereum L2 with sub-cent median transaction fees
- Base inherits Ethereum mainnet trust assumption
- Coinbase operates the sequencer (publicly-traded, audited, regulated)
- Settlement is final in seconds; block explorer is public

## Reference Implementation (TensorFeed.ai, May 2026)

### Surface Area
- 20 AI providers monitored at 2-minute polling cadence (~14,400 polls/day)
- 19 premium endpoints behind x402
- 4 webhook watch types (price, status, digest, leaderboard rank)
- 30+ MCP tools published
- 2 federation members (TensorFeed.ai, TerminalFeed.io)
- 36 daily JSONL feeds in public Hugging Face dataset
- 90-day premium retention horizon

### Economic Model
- One credit = $0.02 (base rate); volume discounts down to $0.0125
- 1 USDC buys 50 credits (base) to 80 credits (max volume)
- Free tier: 7-day uptime data (public good)
- Premium tier: 90-day time-deepened data
- Revenue: one credit per call across virtually all premium endpoints

### Discovery Layers
1. `llms.txt` at site root with pointers to AFTA manifest, x402 manifest, receipt key
2. `/.well-known/agent-fair-trade.json` (AFTA manifest)
3. `/.well-known/x402.json` (payment rail manifest)
4. `/.well-known/tensorfeed-receipt-key.json` (JWK for receipt verification)
5. MCP server registry (`ai.tensorfeed/mcp-server`)
6. Agent-to-agent recommendation loop (recommend-loop thesis)

## Adjacent Skills
- `agentic-payments-protocol-ap2` — Google's AP2; AFTA is a complementary peer protocol
- `mcp-payment-support-specification` — MCP payment support; AFTA provides the trust layer
- `agentic-payment-protocol-convergence-2026` — protocol convergence; AFTA is one of the converging protocols
- `agentic-commerce-trust-design` — trust design for agentic commerce; AFTA codifies the trust primitives
- `revenue-sharing-as-infrastructure-model` — RSI model; AFTA enables the peer-to-peer settlement layer