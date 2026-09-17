# Dual Licensing Playbook

## License Selection Matrix

| Goal | Open License | Commercial License | Notes |
|------|-------------|-------------------|-------|
| Maximize adoption | MIT / Apache-2.0 | Proprietary | Permissive licenses reduce friction |
| Ensure reciprocity | AGPL / GPL-3.0 | Proprietary | Copyleft ensures contributions flow back |
| Enterprise safety | AGPL | Custom commercial | AGPL scares enterprises into buying license |
| SaaS protection | AGPL | Proprietary | Prevents SaaS providers from freeloading |
| Patent protection | Apache-2.0 | Proprietary | Explicit patent grant in Apache-2.0 |

### A-Tech Recommendation
- **A-Coder IDE core:** AGPL-3.0
  - Ensures all distributed and network-interactive versions remain open
  - Enterprise customers needing proprietary integration buy commercial license
- **Builder's Club tools:** MIT or Apache-2.0
  - Maximizes adoption and contribution
  - Commercial licensing for organizations needing warranty/indemnification
- **Be Practical content:** CC-BY-SA
  - Content remains free and remixable
  - Institutional licensing for training programs and universities

## Commercial Tier Architecture

### Three-Layer Model
```
Layer 1: Open Core (AGPL/MIT)
├── Source code
├── Community documentation
├── Issue tracker (public)
└── CI/CD (public)

Layer 2: Commercial License
├── Proprietary-use rights (no copyleft obligation)
├── Indemnification (up to $X per claim)
├── Warranty (SLA: 99.9% uptime, 4-hour response)
├── Custom terms (negotiable)
└── Dedicated support channel

Layer 3: Enterprise Services
├── Professional services / consulting
├── Custom development
├── Training and certification
├── Priority feature requests
└── White-label licensing
```

### Pricing Architecture
| Tier | Price Point | Target | Includes |
|------|------------|--------|----------|
| **Community** | Free | Individual developers, students | Open core, community support |
| **Professional** | $29-99/mo | Small teams, startups | Commercial license, email support |
| **Business** | $499-1999/mo | Mid-size companies | SLA, indemnification, priority support |
| **Enterprise** | Custom | Large organizations | Custom terms, professional services, white-label |

## Compliance Framework

### For Users of Open-Core Software
1. **License Audit:** Quarterly review of all dependencies against license obligations
2. **Distribution Check:** If distributing the software (including SaaS for AGPL), ensure source availability
3. **Commercial License Path:** If proprietary use is needed, purchase commercial license before deployment
4. **Attribution Maintenance:** Preserve copyright notices and license texts

### For A-Tech as Licensor
1. **Contributor License Agreement (CLA):** All external contributors sign CLA granting A-Tech rights to dual-license their contributions
2. **License Compatibility Check:** Ensure all dependencies are compatible with the chosen open license
3. **Commercial License Enforcement:** Monitor for unauthorized proprietary use; gentle-first enforcement (contact before legal action)
4. **Revenue Attribution:** Publish annual report showing what percentage of commercial revenue funds open-core maintenance

## Risk Transfer as Product: Messaging

### Enterprise Buyer Concerns
| Concern | Open-Source Risk | Commercial License Solution |
|---------|-----------------|------------------------------|
| Legal liability | AGPL obligation unclear | Proprietary-use rights, clear terms |
| Security accountability | No warranty on open version | SLA, security response guarantee |
| IP contamination | Unknown contributor IP | Indemnification, IP warranty |
| Support continuity | Maintainer may abandon | Dedicated support, succession plan |
| Compliance audit | License unclear to auditors | Signed commercial agreement |

### Sales Messaging
"The open-source core is our public proof of quality. The commercial license is your legal and operational safety net. You are not buying features — you are buying the confidence to build on our infrastructure without legal or operational risk."

## Anti-Patterns in Dual Licensing

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| Open-core delay | Hold features back from open version | Open core gets all features; commercial gets legal + support |
| Bait-and-switch | Start permissive, switch to restrictive later | Choose license at inception, never change without community consent |
| CLA hostility | Aggressive CLA drives contributors away | Lightweight CLA, clear explanation, optional for small patches |
| Enforcement theater | Sue users for minor violations | Education-first enforcement, commercial outreach |
| Revenue opacity | Community sees no benefit from commercial sales | Publish annual sustainability report with revenue allocation |
