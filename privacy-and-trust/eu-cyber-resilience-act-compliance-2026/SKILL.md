---
name: eu-cyber-resilience-act-compliance-2026
description: Navigate the EU Cyber Resilience Act (CRA) for software products, including open-source projects with commercial activity. Covers risk classification, timeline, SBOM requirements, vulnerability disclosure, incident reporting, CE marking, and the 90-day readiness plan. Use when shipping software to EU users, evaluating open-source stewardship obligations, or designing security-by-default products for European markets. NOT for purely internal tools with no EU users.
---

# EU Cyber Resilience Act Compliance 2026

## Overview

The EU Cyber Resilience Act (CRA) entered into force on December 11, 2024. Partial requirements become mandatory on **September 11, 2026** — just months away. Full compliance is required by **December 12, 2027**. The CRA applies to all "products with digital elements" (PDEs) placed on the EU market, including software, regardless of whether the developer is based in the EU. For open-source projects, the distinction between hobbyist and commercial stewardship determines whether full manufacturer obligations apply.

This skill provides the practical compliance path: risk classification, documentation requirements, timeline, open-source exemptions, and the 90-day preparation plan.

## When to Use

- Shipping software that EU users can download, purchase, or access
- Determining whether an open-source project qualifies for CRA exemption or stewardship obligations
- Designing security-by-default product architecture for EU markets
- Preparing SBOMs, vulnerability disclosure policies, and incident response plans
- Advising Builder's Club members on EU distribution strategy

NOT for:
- Purely internal tools with no EU users
- Hobby projects with zero commercial activity and no EU distribution intent
- Assuming open-source automatically exempts from all CRA requirements

## The Timeline

| Date | Milestone | Action Required |
|------|-----------|-----------------|
| **Dec 11, 2024** | CRA enters into force | Start compliance planning |
| **Sep 11, 2026** | Partial requirements mandatory | Vulnerability handling, SBOM, secure-by-design |
| **Jun 12, 2026** | Conformity Assessment Bodies operational | Begin third-party assessments for Class I/II |
| **Oct 12, 2027** | Incident reporting begins | 24–72 hour reporting to EU authorities |
| **Dec 12, 2027** | Full compliance required | Non-compliant products cannot be placed on EU market |

## Does the CRA Apply to You?

### Trigger Conditions
- EU users can download, purchase, or use your software
- You have any commercial activity (paid support, premium versions, SaaS, donations in some cases)
- You are a legal entity (company, foundation) maintaining the software

### Open Source Exemptions
| Status | CRA Obligation | Condition |
|--------|----------------|-----------|
| **Hobby project** | Exempt | Zero commercial activity; individual developer; no paid services |
| **Open Source Steward** | Reduced obligations | Legal entity maintaining FOSS; must facilitate compliance for downstream manufacturers |
| **Commercial manufacturer** | Full obligations | Monetizes software; distributes through marketplaces; offers paid support |

**Critical nuance:** WordPress.org plugins combined with any commercial activity trigger full manufacturer obligations, even if the plugin itself is free.

## Risk Classification

| Category | Products | Assessment |
|----------|----------|------------|
| **Default** | Most software, WordPress plugins/themes | Self-assessment; internal documentation |
| **Class I** | Browsers, password managers, network management | Third-party assessment required |
| **Class II** | Hardware Security Modules, industrial control, firewalls | Independent third-party conformity assessment |

## Core Requirements

### 1. Security-by-Design
- Secure defaults enabled on activation
- Reset capability to known secure state
- Update mechanism separate from feature releases
- Rollback capability if updates fail
- No known exploitable vulnerabilities at release

### 2. Software Bill of Materials (SBOM)
- Inventory all third-party libraries and components
- Version numbers documented
- Known CVEs tracked
- Update schedules recorded
- Kept current and accessible

### 3. Vulnerability Disclosure Policy (VDP)
- Publicly published process for reporting security issues
- Expected response times stated
- Coordinated disclosure commitments
- Contact method clearly documented

### 4. Incident Response & Reporting
- 24-hour window: report actively exploited vulnerabilities to authorities
- 72-hour window: report severe security incidents
- User notification when exploits are active
- Documented incident response plan with post-incident review

### 5. CE Marking & Declaration of Conformity
Required by December 12, 2027:
- Visible digital CE marking (readme, admin interface, documentation)
- EU Declaration of Conformity with:
  - Company details, product identification, applicable legislation
  - Harmonized standards used
  - Risk category classification
  - Authorized representative (if outside EU)
- Version-specific declarations for each major security release
- Retain declarations for **10 years** after product withdrawal

## Penalties for Non-Compliance

- Fines: **€5,000,000 to €15,000,000**
- Or: **1% to 2.5%** of total worldwide annual turnover
- Immediate removal from EU-accessible platforms
- Mandatory user notifications and assistance with removal

## The 90-Day CRA Preparation Plan

### Weeks 1–2: Immediate Actions
- [ ] List all dependencies and their versions (initial SBOM)
- [ ] Publish a basic Vulnerability Disclosure Policy on your website
- [ ] Register for a managed VDP if you lack bandwidth to handle reports
- [ ] Identify which risk category applies to each product

### Weeks 3–4: Assessment
- [ ] Complete risk assessment for each main product
- [ ] Scan dependencies for known CVEs
- [ ] Review default settings for secure-by-design compliance
- [ ] Document threat scenarios (injection, privilege escalation, file uploads)

### Weeks 5–8: Documentation
- [ ] Create `/CRA-Compliance/` folder with subfolders:
  - `Risk-Assessment/`
  - `SBOM-Dependencies/`
  - `Security-Testing/`
  - `CE-Declaration/`
  - `Incident-Response/`
  - `Update-Procedures/`
- [ ] Draft incident response plan (24h impact assessment, 72h authority reporting)
- [ ] Prepare CE marking materials for each product

### Weeks 9–12: Implementation
- [ ] Test rollback procedures on staging
- [ ] Separate security update pipeline from feature releases
- [ ] Add security information to product descriptions
- [ ] Schedule quarterly compliance review calendar

## A-Tech Applications

### A-Coder (IDE)
- Classify as **Default risk** (development tool, not network/security infrastructure)
- SBOM: all bundled libraries, language servers, AI model weights
- VDP: publish through A-Tech website with coordinated disclosure timeline
- Secure defaults: no telemetry without explicit opt-in; no outbound connections without user approval

### Be Practical (Playbooks)
- Default risk; self-assessment sufficient
- No network-facing attack surface reduces compliance burden
- Focus on dependency scanning and clear update policy

### Builder's Club
- **Open Source Steward track:** If A-Tech maintains tools as a foundation/entity, adopt steward obligations
- Offer voluntary security attestation (Article 25 pathway) to help downstream manufacturers comply
- Contribute to OpenSSF CRA readiness resources and baseline security guides

## Open Source Specific Considerations

| Challenge | CRA Solution |
|-----------|--------------|
| Who is the "manufacturer" for a community project? | Legal entity maintaining the project; otherwise, each commercial downstream user is responsible |
| Can I stay exempt? | Only if truly zero commercial activity and individual-maintained |
| What if I accept donations? | Grey area — get legal advice; structured donations may trigger stewardship |
| What about dual-licensed projects? | Commercial license path triggers manufacturer obligations |
| Do I need to pay for third-party assessment? | Default risk: no. Class I/II: yes, unless voluntary attestation program covers it |

## Cross-References
- `developer-experience-and-flow/vibe-coding-security-defense` — AI-specific security vulnerability management
- `developer-experience-and-flow/ai-code-provenance-generative-authorship` — SBOM governance and provenance tracking
- `developer-experience-and-flow/supply-chain-agentic-security` — Dependency scanning and vulnerability disclosure automation
- `community-and-growth/open-source-agency-argument` — Open-source positioning and jurisdiction independence
- `privacy-and-trust/pets-ai-collaboration-framework` — Privacy-enhancing technologies for regulated AI deployment

## Sources
- Regulation (EU) 2024/2847 — Cyber Resilience Act (Official Journal, Dec 10, 2024)
- OpenSSF — "EU Cyber Resilience Act" public policy portal (2026): CRA resources, steward playbook, readiness report
- Patchstack — "EU's Cyber Resilience Act — Complete Guide for Open Source Vendors" (2026): whitepaper, timeline, risk categories, 90-day plan, WordPress checklist
- European Commission — CRA Implementation Website and FAQ (2026)
- ENISA — Single Reporting Platform FAQ (2026)
- OpenSSF — "Unaware and Uncertain: The Stark Realities of Cyber Resilience Act Readiness in Open Source" (2026)
- OpenSSF — "Taking Stock of the State of European Cyber Resilience Act Compliance" (May 2026)

## Date Researched
2026-06-19 | Daily Research Process | A-Tech Research Division
