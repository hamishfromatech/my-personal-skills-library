# EU AI Act Text — Key Articles for Developers

## Article 6 — Classification of AI Systems

### Article 6(1)
An AI system shall be considered high-risk where both of the following conditions are met:
(a) the AI system is intended to be used as a safety component of a product covered by the Union harmonisation legislation listed in Annex I;
(b) the product whose safety component is the AI system is required to undergo a third-party conformity assessment.

### Article 6(2)
An AI system shall be considered high-risk where it is intended to be used in any of the areas listed in Annex III.

### Article 6(3) — Carve-Out
An AI system referred to in Annex III shall not be considered high-risk if it does not pose a significant risk of harm to the health, safety or fundamental rights of natural persons, including not materially influencing the outcome of decision-making. This applies where the AI system:
- performs a narrow procedural task;
- improves the result of a previously completed human activity;
- detects decision-making patterns or deviations from prior decision-making patterns, without replacing or influencing human assessment;
- performs a preparatory task to an assessment relevant for the purposes of the use cases listed in Annex III.
Exception: profiling natural persons always stays high-risk regardless.

---

## Article 11 — Technical Documentation

1. Technical documentation shall be drawn up in respect of high-risk AI systems before those systems are placed on the market or put into service, and shall be kept up-to-date.
2. The technical documentation shall be such as to demonstrate that the high-risk AI system complies with the requirements set out in Chapter III.
3. The Commission shall be empowered to adopt delegated acts to specify the elements to be included in the technical documentation.

### Annex IV — Mandatory Documentation Categories
1. General description of the AI system
2. Description of the development process
3. Description of the monitoring and control mechanisms
4. Description of risk management (per Article 9)
5. Description of changes made to the system during its lifecycle
6. List of the harmonised standards applied
7. EU Declaration of Conformity
8. Description of validation and testing processes
9. Source code access procedures (Article 74 fallback)

---

## Article 12 — Record-Keeping

1. High-risk AI systems shall technically allow for the automatic recording of events (logs) over the duration of the lifetime of the system.
2. The logging shall facilitate tracing of the functioning of the AI system throughout its lifetime, in particular for:
(a) facilitating the post-market monitoring of the AI system;
(b) enabling the identification of the reasons for any malfunction or deterioration in performance.
3. Logs shall be retained for a minimum of 6 months.

### Recommended Schema for Multi-Agent Coding Pipelines
- Invoking user identity
- Governing specification version
- Model identifier and provider
- Input context
- Output artifact
- Human reviewer identity
- Disposition (accepted, modified, rejected)

---

## Article 14 — Human Oversight

### Article 14(4) — Five Oversight Measures
1. Understand the capabilities and limitations of the AI system
2. Remain aware of the tendency to automatically rely on or trust the output (automation bias)
3. Correctly interpret the output of the AI system
4. Decide not to use the system or to disregard its output
5. Intervene or stop the system via a "kill switch"

---

## Article 25 — Requalification

A deployer shall be considered a provider for the purposes of this Regulation and shall be subject to the obligations of a provider where:
(a) it places on the market or puts into service a high-risk AI system under its name or trademark;
(b) it modifies a high-risk AI system already placed on the market or put into service.

### "Substantial Modification" Triggers
- Fine-tuning a third-party model on proprietary code and deploying under your brand
- Wrapping a GPAI model in your own product for client distribution
- Changing the intended purpose of an AI system
- Rebranding without changing functionality (low risk)

---

## Article 50 — Transparency Obligations

Providers of AI systems shall ensure that AI systems are designed and developed in such a way that natural persons are informed that they are interacting with an AI system, unless this is obvious from the circumstances and the context of use.

---

## Penalty Framework (Articles 71–72)

| Violation | Maximum Fine |
|-----------|-------------|
| Prohibited practices (Art. 5) | €35M or 7% global turnover |
| High-risk system breach | €15M or 3% global turnover |
| GPAI provider violation | €15M or 3% global turnover |
| Misleading authorities | €7.5M or 1% global turnover |

---

## Annex III — Relevant Points for Developer Tools

| Point | Domain | Developer-Tool Trigger |
|-------|--------|----------------------|
| Point 2 | Critical infrastructure | AI embedded in safety systems |
| Point 4 | Employment / worker management | AI evaluating, screening, monitoring developers |
| Point 5(b) | Essential services | AI determining access to services |

---

*Compiled from Regulation (EU) 2024/1689 — Artificial Intelligence Act, Official Journal L 2024/1689*
