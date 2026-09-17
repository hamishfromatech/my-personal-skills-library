# Sovereign Tech Standards Network — Evidence Base

## Program Card

| Field | Value |
|---|---|
| Operator | Sovereign Tech Agency (sovereign.tech), Berlin |
| Partner | DIN — German Institute for Standardization (MoU signed June 8, 2026) |
| Instrument | Sovereign Tech Standards network (pilot) |
| Cohort size | Up to 10 open-source maintainers |
| SDOs covered | IETF, W3C, ISO |
| Funding | €4,800–5,200/month fixed (≈10 h/week) + SDO participation fees + travel support |
| Pilot window | Mid-June 2026 → June 2027; evaluation published after |
| Application call | April 28 – May 19, 2026; notifications June 2026; cohort announced July 27, 2026 |
| Meetings | IETF 126 Vienna (July 17–24, 2026); TPAC 2026 Dublin (Oct 26–30); IETF 129 & TPAC 2027 in Europe |
| Payment mechanics | Euro bank/wire; participants invoice Sovereign Tech Agency GmbH; worldwide eligibility |
| Contact | standards@sovereign.tech |

## The Justifying Survey
Of maintainers the STA has worked with: **three-quarters actively rely on standards, yet very few can afford long-term participation in developing them.** Framing from the program page: "Maintainers bring something that's frequently missing from standards processes: direct open source implementation experience. They build, test, and operate software that has to conform to these specifications, and they encounter in practice what looks clean on paper. Standards developed without that perspective risk being less robust, less interoperable, and less reflective of how modern software actually works."

## Selection Rubric (two independent reviewers)
1. **Public-interest relevance** — how foundational is the standard to digital infrastructure? How widely used? Is the process at a stage where maintainer input can still meaningfully shape the outcome?
2. **Proposed work** — a clear, realistic plan: which working group, which draft or gap; what outputs within the timeframe
3. **Representation gap** — is the open-source/implementer perspective currently missing or underrepresented in the WG?
4. **Expertise** — track record as maintainer and technical authority in the domain

## Requirements & Anti-Capture Rules
- Active maintainer of ≥1 open-source digital-infrastructure project
- Project implements/depends on/relates to IETF/W3C/ISO technologies (or plans substantial new work at one)
- ~10 h/week average availability + in-person meeting attendance
- Strong preference for individual contracts (freelance/sole proprietor); organizations that "do not require public support for this work are likely to be excluded"; STA's relationship with an organization ends if the individual's relationship ends; standard legal agreements non-negotiable

## The 2026 Cohort (ten maintainers, three SDOs)

**IETF:**
- **Job Snijders** — internet routing security, RPKI next-generation protocols; RFC author; OpenBSD developer
- **Marten Seemann** — QUIC/HTTP-3/WebTransport/MASQUE; creator-lead of quic-go; MASQUE WG co-chair; 10+ years at IETF
- **Martine Lenders** — RIOT OS core maintainer (10+ yrs), embedded network stack; co-author RFC 9953 (DNS over CoAP); encrypted DNS for constrained IoT
- **Heiko Schäfer** — OpenPGP ecosystem (rPGP, Stateless OpenPGP); OpenPGP WG

**W3C:**
- **Lola Odelola** — elected W3C Technical Architecture Group co-chair; Accessibility Compatibility Data project (screen-reader support data into MDN/Baseline)
- **Mike Pennisi** — AT Driver protocol (automated testing + accessibility tooling)
- **Stephen Curran** — digital identity/privacy; co-created did:webvh (W3C DID method adopted by UN, Switzerland, Canada); maintains ACA-Py, AnonCreds; DID WG invited expert/co-editor; plans DID standardization + post-quantum prep

**ISO:**
- **Antoine d'Aligny** — GNU Taler (free payment system) banking + crypto integrations; core banking/payment standards
- **Kacper Dalach** — EVerest EV-charging platform; DIN70121/ISO 15118-2 protocols

**Fediverse (W3C):**
- **Daniel Supernault** — ActivityPub since 2018; Pixelfed, Loops, FediDB; Fediverse Enhancement Proposals

## DIN Partnership (June 8, 2026 MoU)
- DIN joins the selection process and accompanies participants into ISO work — "navigating the structures of international standardization requires institutional knowledge and relationships that take years to build"
- Christoph Winterhalter (DIN Executive Board): "Open, vendor-neutral standards create fair conditions for competition. Open source puts those standards into practice quickly. Standards and open source are not opposites, they reinforce each other"
- Adriana Groh (STA Managing Director): open-source implementation experience should be "a consistent presence in the processes that shape digital infrastructure, not an exception"

## Strategic Context (from the Möller interview, Open Source Security podcast, Aug 31 2026)
- STA invests in critical open-source infrastructure "on behalf of the German taxpayer" — roads-and-bridges framing, not grant-making
- Criteria: criticality ("what happens when it breaks"), how widely used, whether an existing funder ecosystem already covers it
- **Sovereignty accelerant:** governments woke up to dependency risk — "the recent example of access to Anthropic models being restricted through export control mechanisms" made every government ask what else can be cut off
- **EU scaling:** Digital Commons EDIC (France/Netherlands/Germany/Luxembourg/Italy) pooling resources; pilot EU Sovereign Tech Fund instrument
- **Möller on AI:** open weights "are basically just blobs you cannot reproduce" — "until you have reproducibility, you do not have [openness]"; the standards instinct applies: reproducibility, verifiability, vendor-neutrality
- **Sovereign Tech Resilience program** (adjacent): security audits, memory safety, post-quantum encryption readiness, CRA compliance; hiring Program Manager (deadline Sept 7, 2026)
- **IGF Policy Network on Open Digital Infrastructure** — STA + German Federal Ministry for Digital Transformation proposal accepted; open participation for maintainers, researchers, policymakers, civil society

## The OSS-Funding Quintet (library mapping)

| Leg | Case | Control | Money shape | Failure mode targeted |
|---|---|---|---|---|
| Endowment | Open Source Endowment ($752K → $100M target, ~5% perpetual income) | Community board | Invested principal | Time horizon (perpetual vs project grants) |
| Co-op | Maintainer co-ops (3–7 maintainers, one-page governance) | Members | Shared treasury | Bus-factor-1 + burnout |
| Employment | Rust MiR ($350K RFMF; $10K/mo FT flat; Google/AWS/OpenAI sponsors) | Project funding team | Salaried contracts | Sustained presence, not scoped projects |
| Patron | Omacom ($12.6M, 14 patrons, upstream deployment, paywall buyout) | Single founder | Multi-year pledges | Speed + upstream dependency |
| **Standards** | **STA Standards (€4.8–5.2K/mo × 10 maintainers)** | **Funder + standards-body partnership** | **Function stipend** | **Governance capture / representation gap** |

## Usage Notes
- Cite the program as: the first production-grade *standards-participation* funding mechanism in open source (the library's sovereign-tech-fund-causal-impact covers the Fund's velocity effects; this is a distinct instrument).
- Watch items: pilot evaluation (post-June 2027); whether other regions replicate (EU EDIC pilot is the near-term candidate); whether AI-standards seats (post-quantum, identity) expand in cohort 2.