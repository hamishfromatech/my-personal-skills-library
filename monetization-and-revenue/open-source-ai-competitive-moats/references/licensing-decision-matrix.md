# Licensing Decision Matrix for Open Source AI

## License Categories

### Permissive Licenses (Maximum Adoption)
- **MIT:** Minimal restrictions, maximum adoption, suitable for tools and libraries
- **Apache 2.0:** Patent protection, enterprise-friendly, suitable for frameworks and runtimes
- **BSD:** Simple, minimal attribution, suitable for reference implementations

### Reciprocal Licenses (Ensure Contributions Flow Back)
- **GPL:** Strong copyleft, all derivatives must be open-source
- **AGPL:** Extends GPL to networked services (cloud use triggers copyleft)
- **MPL:** File-level copyleft, more permissive than GPL

### Custom / Hybrid Licenses
- **Business Source License (BSL):** Open source with delayed proprietary period (e.g., 4 years)
- **AI-specific licenses:** Address model weights, training data, and inference rights
- **Ethical use licenses:** Restrict harmful applications (e.g., Hippocratic License)

## Decision Matrix

| Goal | Recommended License | Examples |
|------|---------------------|----------|
| Maximum ecosystem growth | Apache 2.0 or MIT | Hugging Face Transformers, TensorFlow |
| Protect against cloud providers | AGPL or BSL | MongoDB (SSPL), MariaDB (BSL) |
| Enterprise adoption with patent safety | Apache 2.0 | Kubernetes, React |
| Delayed open-sourcing for revenue window | BSL | CockroachDB, Sentry |
| Prevent harmful uses | Ethical license + Apache 2.0 | Some AI safety projects |

## A-Tech Recommendations
- **A-Coder core runtime:** Apache 2.0 — enterprise standard, enables ecosystem
- **Be Practical curriculum framework:** MIT — maximum educational reuse
- **Builder's Club tooling:** Apache 2.0 — community contributions with patent protection
- **Enterprise orchestration layer:** Proprietary — strategic differentiator
- **Federated learning components:** Apache 2.0 — encourages adoption and contribution

## Sources
- Craddock, M. — "Open Source AI as a Competitive Advantage" (Medium, January 2025)
- OSI — Open Source Initiative license definitions
- ChooseALicense.com — GitHub's license selection guide
