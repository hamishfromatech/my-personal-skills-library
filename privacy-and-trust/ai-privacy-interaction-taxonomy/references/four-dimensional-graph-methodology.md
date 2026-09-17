# AI-Privacy Interaction Taxonomy — Technical Deep Dive

## Source

Voloch, N. & Hirschprung, R.S. (2026). "Both ends of artificial intelligence impacting privacy: a review of violation and protection." Frontiers in Artificial Intelligence, 9:1686454. doi: 10.3389/frai.2026.1686454

## Methodology

### PRISMA 2020 Compliance

The review followed PRISMA 2020 guidelines for transparent reporting of systematic reviews:

- **Identification:** Records retrieved from Google Scholar, IEEE Xplore, ACM Digital Library, SpringerLink, PubMed, and Scopus
- **Search period:** 2000-2025
- **Screening:** Two-step — automated keyword filtering, then manual full-text review
- **Final corpus:** 94 papers
- **Cutoff date:** May 15, 2025

### Search Strategy

Boolean search template:
```
("Artificial Intelligence" OR "Machine Learning" OR "Deep Learning" OR "Large Language Model" OR NLP OR "Computer Vision" OR "Speech Recognition" OR IoT OR "Online Social Networks")
AND
(privacy OR "data protection" OR "privacy-preserving" OR "privacy attack" OR "privacy violation" OR "privacy by design" OR "privacy shell" OR PPDM)
```

Domain-specific query examples:
- LLMs: `("Large Language Model" OR "GPT" OR "ChatGPT") AND (privacy OR "data leakage")`
- Computer Vision: `("computer vision" AND (privacy OR "re-identification" OR "face recognition privacy"))`
- IoT: `(IoT AND (privacy OR "edge privacy"))`
- OSN: `("online social networks" AND (privacy OR "profiling" OR "user data misuse"))`

### Inclusion Criteria

1. Explicitly addresses the intersection of AI and privacy
2. Involves at least one technological domain from the four-dimensional framework
3. Describes concrete privacy actions (attacks, defenses, vulnerabilities, or regulations)
4. Provides sufficient methodological or conceptual detail for classification
5. English-language academic publication

### Neo4J Graph Database

- **Engine:** Neo4J (graph database management system)
- **Node types:** Paper (blue), Domain (yellow), Action (red), Approach (brown), Direction (green)
- **Relationship:** BELONGS_TO (paper → dimension value)
- **Properties:** Paper nodes include title, authors, year, publication platform, DOI/URL
- **Public dataset:** DOI: 10.5281/zenodo.17584342
- **Queryable:** Readers can inspect, filter, and extend the graph with new papers

## Detailed Domain Analysis

### Machine Learning (ML)

Most researched domain alongside OSN and IoT. Key papers:

- **Oseni et al. (2021):** Security and privacy challenges in AI — secure development, adversarial threats, defense methods
- **Dilmaghani et al. (2019):** Big data impact on privacy and security in ML/AI systems
- **Ma et al. (2023):** Privacy and security in distributed ML systems
- **Perino et al. (2022):** AI model vulnerabilities in telecom — FL, DP, trusted execution environments
- **Zhu et al. (2020):** DP benefits in ML, DL, and multi-agent systems
- **Rodriguez-Barroso et al. (2020):** Sherpa.ai — combining FL and DP in unified framework
- **Khalid et al. (2023):** PPML in healthcare — attack types and defense techniques
- **Cheng et al. (2020):** FL in finance and edge computing — real-world deployments
- **Rahman et al. (2020):** Privacy-preserving AI for edge computing using FHE
- **Toch & Birman (2018):** Behavioral privacy framework for AI's predictive capabilities

### Natural Language Processing (NLP)

- **Contissa et al. (2018):** Automating GDPR privacy policy assessment using ML — 14 policies analyzed, none fully compliant
- **Martinelli et al. (2020):** AI/NLP for identifying sensitive data in unlabeled documents — 10,000 documents, strong performance
- **Xing et al. (2023):** AI privacy concerns comparison US vs. China — Americans more wary, Chinese more optimistic
- **Zarifis et al. (2021):** Trust in AI-driven health insurance — AI visibility reduces perceived trust
- **Shahriar et al. (2023):** Privacy risks across AI lifecycle — identification, inaccuracy, non-transparency, non-compliance

### Large Language Models (LLMs)

- **Li et al. (2023):** Privacy attacks on ChatGPT and Bing — prompt-based data extraction
- **Gupta et al. (2023):** Cybersecurity implications of generative AI — jailbreaks, prompt injections, reverse psychology
- **Mylrea & Robinson (2023):** AI trust framework using "entropy lens" from information theory
- **Wei & Liu (2024):** Trust in distributed AI — robustness, privacy, fairness, governance
- **Peres et al. (2023):** ChatGPT in mental health — privacy, accuracy, ethical risks

### Computer Vision

- **Ferm et al. (2022):** AI consumer privacy impact — Clearview AI, Hello Barbie case studies
- **Harichandana et al. (2022):** Lightweight AI for detecting sensitive content in images of people with disabilities
- **Liu et al. (2019):** Privacy-preserving image method — adversarial perturbations + visual obfuscation

### Speech Recognition

- **Curzon et al. (2021):** AI privacy risks across five domains — computer vision, speech, NLP, knowledge representation, automated reasoning
- **Liu et al. (2021):** ML and privacy in speech recognition — ML as privacy target, defense, and attacker
- **Gandeeban et al. (2025):** SER-EQCNN-ESC architecture for Speech Emotion Recognition

### Internet of Things (IoT)

- **Giordano et al. (2022):** AI supporting privacy in IoT systems
- **Elhoseny et al. (2021):** Blockchain-based, AI-enabled IoT for private healthcare data transfer
- **Xiao et al. (2018):** ML approaches for IoT security — authentication, malware detection
- **Liu et al. (2022):** Smart speaker privacy concerns in China — PIPL regulation
- **Gray & Mehrnezhad (2025):** PhotonKey — lightweight key pairing for constrained IoT devices using ambient light sensors
- **Sugianto et al. (2024):** Privacy-preserving public surveillance using FL — Responsible AI Implementation Framework

### Online Social Networks (OSN)

- **Hirschprung & Alkoby (2022):** OISA framework — game theory and AI agents for privacy-risk-aware information sharing
- **Wang et al. (2021):** Privacy dynamics in AI-driven e-commerce — evolutionary game theory
- **Majeed & Hwang (2023):** AI-generated synthetic data undermining anonymization
- **Wang et al. (2022):** Metaverse security and privacy challenges

### Databases (DBs)

- **Devi (2023):** PPDM methods — anonymization, cryptography
- **Hewage et al. (2023):** Privacy-accuracy trade-off; Privacy-Preserving Data Stream Mining (PPDSM)
- **Hirschprung (2023):** PPDM technique categorization — anonymization, randomization, cryptography, result privatization

## Cross-Domain Patterns

### Privacy Violation Evolution

Three stages of privacy violation in the digital age:
1. **Information Systems era:** Large databases containing sensitive personal data could leak
2. **Internet era:** Users began publishing their own sensitive data
3. **AI era:** Sophisticated processes can access sensitive information that is apparently not directly available — AI can infer private information from seemingly innocuous data

### Defense Mechanism Comparison

| Mechanism | Impact on Accuracy | Privacy Strength | Complexity | Best Use Case |
|-----------|-------------------|-------------------|------------|---------------|
| Differential Privacy | Moderate (3-10% loss) | High (Formal) | High | Statistical queries, text tasks |
| Gradient Mixing | Negligible | Moderate (Numerical) | Low | Preventing model inversion |
| Secure Aggregation | None | High (Cryptographic) | High | Federated learning systems |
| Input Encoding (MixUp/InstaHide) | Low (<6% loss) | Moderate | Medium | Image data protection |
| Federated Learning | Varies | Moderate-High | Medium-High | Distributed/edge environments |
| Homomorphic Encryption | Varies | Very High | Very High | Computation on encrypted data |
| On-device Processing | None | Very High | Low-Medium | Personal AI, voice assistants |

### Regulatory Alignment

The review maps technical findings to regulatory frameworks:
- **GDPR:** Data minimization (Art. 5), purpose limitation (Art. 6), data protection by design (Art. 25), automated decision-making (Art. 22)
- **OECD AI Principles:** Transparency, robustness, human-centric design
- **HIPAA:** Healthcare data protection
- **PIPL (China):** Smart device and IoT privacy

### Key Limitations

1. Some emerging AI privacy challenges (generative AI, quantum computing) may not be extensively studied yet
2. Classification may be somewhat subjective across disciplines
3. Proprietary solutions are not accessible for review
4. Performance trade-offs are not empirically validated in the review
5. Database bias and publication bias may overrepresent certain domains

### Future Research Directions

- Refining privacy-preserving techniques that balance computational efficiency, fairness, and robustness
- Transparent communication about AI privacy measures influencing user trust
- User-centered design approaches empowering individuals with data control
- Explainable AI (XAI) for privacy implications
- Multi-domain attack chains and cross-modal attribute extraction
- Standardized benchmarks and reporting practices for privacy-preserving AI