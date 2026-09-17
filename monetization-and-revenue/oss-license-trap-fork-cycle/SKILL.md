---
name: oss-license-trap-fork-cycle
description: Framework for understanding and avoiding the open-source license change trap that leads to community forks and brand loss. Use when [making open-source licensing decisions, evaluating license changes, planning OSS business strategy, assessing cloud provider competitive threats, deciding between Apache 2.0 and restrictive licenses].
---

# OSS License Trap Fork Cycle

## The Core Pattern

When an open-source company changes its license to block cloud providers from offering managed services, a predictable 7-step cycle unfolds that consistently damages the company more than the threat it was trying to block.

## The 7-Step License Trap Cycle

1. **Cloud provider starts offering managed version** of your OSS as a hosted service
2. **You change the license** to block the cloud provider (SSPL, BSL, ELv2, RSALv2)
3. **Cloud provider forks** the last open version under Apache 2.0
4. **Linux Foundation or CNCF** picks up governance of the fork
5. **Enterprises migrate to the fork** because procurement requires real OSS
6. **You lose the developer brand** you spent a decade building
7. **You eventually relicense back** to something more permissive (AGPLv3 or Apache 2.0)

## Documented Cases

### Redis (March 2024)
- **Change**: BSD → dual SSPL/RSALv2 source-available license
- **Reason**: AWS ElastiCache offering managed Redis
- **Result**: AWS forked Redis 7.2.4 → **Valkey** under Linux Foundation
- **Damage**: 83% of large enterprises testing or running Valkey by 2025
- **Reversal**: May 2025 - Redis added AGPLv3 back alongside source-available licenses
- **Lesson**: Commercial damage exceeded the protection

### Elastic (January 2021)
- **Change**: Apache 2.0 → dual SSPL and Elastic License v2
- **Reason**: AWS Elasticsearch managed service
- **Result**: AWS forked Elasticsearch → **OpenSearch** under Apache 2.0
- **Damage**: Lost decade of developer mindshare
- **Reversal**: August 2024 - Elastic added AGPLv3 as third licensing option
- **Lesson**: Partial reversal, community largely already migrated

### HashiCorp (August 2023)
- **Change**: MPL 2.0 → BSL 1.1 for Terraform
- **Result**: OpenTF Foundation forked Terraform → **OpenTofu** within weeks
- **Damage**: OpenTofu reached 10M+ downloads by 2025, still growing
- **Acquisition**: IBM acquired HashiCorp for $6.4B in February 2025
- **Status**: BSL not reversed; mid-market growth slowed as customers migrated
- **Lesson**: Even a $6.4B acquisition didn't reverse the damage

## The Strategic Lesson

**If your business needs a license restriction to survive, you do not have an open source business. You have a proprietary business with open source marketing.**

Decide upfront:
- If you cannot live with AWS hosting your model → do not pretend to open source it
- If you can compete on experience around the model → keep Apache 2.0

## The Cleaner Play

Companies that avoided the trap:
- **Mistral**: Apache 2.0 on weights, competes on hosted inference, enterprise contracts, Le Chat subscriptions
- **HuggingFace**: Apache 2.0 on libraries, moat is the Hub network effect (1.5M models, 500K orgs)
- Both left alone by cloud providers because differentiation is in ergonomics, not the artifact

## License Decision Framework

### Default to Apache 2.0 when:
- Enterprise legal teams need fast approval (30 minutes vs 30 days)
- You want maximum developer adoption
- Your moat is above the artifact (brand, network effects, enterprise relationships, data flywheel)

### Consider AGPLv3 when:
- You want copyleft protection without blocking commercial use
- Some enterprises still block it, but it's recognized as true OSS

### Never use as first move:
- SSPL (Server Side Public License)
- BSL (Business Source License)
- Elastic License v2
- RSALv2

These are last-resort defenses for companies losing to cloud providers, not first moves.

## Assessment Questions

Before changing your license, ask:
1. Can a customer self-host the production version end-to-end without paying you? If no, you're not open source.
2. Could AWS/GCP/Azure spin up a competing managed service tomorrow from your code? If no, you're not open source.
3. If you raised prices 5x tomorrow, would the OSS community fork you within a month? If no, the lock-in is real and the open source is decorative.

## Practical Guidance for Open-Source AI Companies

1. **Keep Apache 2.0 on model weights** - this is now the enterprise standard (Gemma, Qwen, Mistral, Yi)
2. **Build the moat above the model** - brand, network effects, ergonomics, enterprise relationships, data flywheel
3. **Compete on experience, not artifact** - the hosted inference, the enterprise compliance skin, the custom training
4. **If AWS becomes a threat, don't change the license** - change the experience
5. **The cost of pretending to be open source when you're not**: engineering burden of public codebase, support burden of free users, marketing burden of fork wars, with none of the network-effect benefits

## Related Skills
- `open-source-license-economics-2026` - License physics and monetization
- `open-source-licensing-landscape-2026` - Current license landscape
- `open-source-ai-competitive-moats` - Building defensible moats above the artifact