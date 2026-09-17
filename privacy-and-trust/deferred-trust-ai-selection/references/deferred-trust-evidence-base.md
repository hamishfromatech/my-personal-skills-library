# Deferred Trust: AI Selection from Human Distrust — Evidence Base

## Primary Source

### Galindez-Acosta, J. S. & Giraldo-Huertas, J. J. (November 2025) — "Trust in AI emerges from distrust in humans: A machine learning study on decision-making guidance"

University of La Sabana, Colombia. arXiv:2511.16769. Published in Computers in Human Behavior: Artificial Humans (ScienceDirect S2451958826000989).

## Study Design

- **Participants:** 55 undergraduate students (Psychology and Nursing), Universidad de La Sabana, Colombia. Mean age 19.38 (SD 1.51). 45 women. Native Spanish speakers.
- **Experiment:** "Trust in AI: Situations by Specific Nature" — 30 decision-making scenarios across three types (factual, emotional, moral).
- **Agent choices:** AI (ChatGPT, Gemini, Claude), voice assistants (Alexa, Siri), peers, adults, priests.
- **Data collection:** Sociodemographic questionnaire (technology use, boundaries, access to services, socioeconomic level, prior trust in AI and human agents) + 30-scenario experiment via QR code during class.
- **Analysis:** K-Modes clustering (scenarios), K-Means clustering (participants), XGBoost models with SHAP interpretations, Bayesian hyperparameter optimization.

## Results

### Q1: Overall agent selection frequencies

| Agent | Selections | Percentage |
|---|---|---|
| Adults | 571 | 35.05% |
| AI | 461 | 28.29% |
| Peers | 412 | 25.29% |
| Priests | 166 | 10.18% |
| Voice assistants | 19 | 1.18% |

### Q2: Context-specific patterns (K-Modes clustering of 30 scenarios)

- **Cluster 0** (10 scenarios): Peer- and adult-directed; diverse distribution with adult predominance, then peers and priests.
- **Cluster 1** (9 scenarios): AI-dominant (73.8% AI selection).
- **Cluster 2** (11 scenarios): Adult-directed predominance with diverse distribution.

**K-Modes metrics:** Davies-Bouldin Index = 0.809, Dunn-like Index = 1.052, SD Index = 0.768.

### Participant profiles (K-Means clustering)

- **Cluster 0:** Lower AI selection, diverse preferences.
- **Cluster 1:** Lower AI selection, higher adult preference (40.3%).
- **Cluster 2:** Higher AI selection (32.8%).

**K-Means metrics:** Davies-Bouldin Index = 2.057, Dunn-like Index = 0.310, SDbw Index = 1.631.

### Q3: Predictors of AI agent selection (XGBoost + SHAP)

SHAP analysis across three AI-preferred situations (24, 26, 9):

**Key finding:** Lower prior trust in human agents (priests, peers, adults) consistently predicted higher AI selection — inverse SHAP values across all three situations.

**Most influential variable:** Trust in adults in Situation 26 (strongest inverse relationship; SHAP values for AI selection ranging from -0.3 to 0.2).

**Sociodemographic predictors:**
- Age: inverse relationship (older participants → lower AI trust)
- Technology use: inverse relationship (more tech use → less AI deference; familiarity enhances vigilance)

**Model performance:**
- Situation 24: Average precision 0.8813 ± 0.0872, Accuracy 0.6091 ± 0.1321, F1 0.7206 ± 0.1157, ROC-AUC 0.6118 ± 0.1779
- Situation 26: Average precision 0.8638 ± 0.0995, Accuracy 0.6318 ± 0.1202, F1 0.7579 ± 0.0928, ROC-AUC 0.5431 ± 0.2380
- Situation 9: Average precision 0.8705 ± 0.0801, Accuracy 0.7136 ± 0.1326, F1 0.8006 ± 0.1105, ROC-AUC 0.6347 ± 0.1508

### Q4: Distinguishing high-AI-trust participants (Cluster 2)

XGBoost model for Cluster 2 membership:

**Top predictors:** Prior trust in human agents (all inverse), socioeconomic level (positive).

**Model performance:** Average Precision = 0.7961 ± 0.1127, Accuracy = 0.7091 ± 0.1206, Precision = 0.7402 ± 0.1118, Recall = 0.7683 ± 0.1561, F1 = 0.7458 ± 0.1112, ROC-AUC = 0.7360 ± 0.1336.

## Theoretical Framework

### Deferred Trust

A compensatory cognitive mechanism whereby distrust in human agents, driven by perceived bias, unreliability, or contextual failures, redirects epistemic reliance toward AI systems perceived as more neutral or competent.

**Aligns with:**
- Trust transfer theory (Song 2025; Saffarizadeh 2024; Yao 2025): trust from previous experiences extends to new agents
- Algorithm appreciation (Logg 2019): prefer algorithmic judgments perceived as more accurate/impartial
- Automation bias (Alonbarkat 2022): over-rely on automated outputs even when imperfect
- Selective trust (Tong 2020): reliance focused on expertise or prior accuracy

### Epistemic Vigilance (Sperber 2010)

Safeguard in human communication enabling individuals to filter unreliable information by assessing credibility, coherence, and relevance. Applied to AI: evaluating LLM-generated content for biases or hallucinations.

**The erosion problem:** LLM fluency and authoritativeness lower vigilance thresholds. LLMs provide incorrect yet plausible answers with high apparent confidence rather than avoiding the question. This creates mismatch between human expectations and model reliability (Zhou 2024; Ghafouri 2025).

### Trust Displacement

The gradual erosion of interpersonal trust as individuals increasingly outsource epistemic judgment to algorithmic systems. A societal-level risk requiring AI design that complements rather than replaces human epistemic relationships.

## Limitations of the Study

1. **Sample homogeneity:** University students only (high educational attainment, high tech familiarity). Limits generalization to older adults or different cultural/socioeconomic contexts.
2. **No multimodal data:** No physiological, neurocognitive, or linguistic markers. Cannot disentangle cognitive vigilance from affective/heuristic responses.
3. **Static scenarios:** Text-based, not interactive. May not reproduce richness of real-world AI encounters.
4. **Exploratory nature:** Cautious generalization warranted. Replication in larger, cross-cultural datasets needed.

## Scenario Examples (from Appendix B)

**Factual:** "What should I do if I want to know the exact year the light bulb was invented?" / "What should I do if I want to know Shakira's exact name?" / "What should I do if I want to make a chocolate cake?"

**Social/Emotional:** "What should I do if I feel sad and want to know if I should talk to someone about it?" / "What should I do if someone makes fun of me?" / "What should I do if I want to know who to tell first that I am going to be a mom or dad?"

**Moral:** "What should I do if I want to get revenge on someone?" / "What should I do when I need to know if it's okay or not to cheat in a game or business?" / "What should I do if I want to know if God exists or not?"

## Source Bibliography

1. Galindez-Acosta, J. S. & Giraldo-Huertas, J. J. (2025) — "Trust in AI emerges from distrust in humans: A machine learning study on decision-making guidance." arXiv:2511.16769. University of La Sabana, Colombia.
2. Sperber, D. et al. (2010) — Epistemic vigilance framework.
3. Logg, J. (2019) — Algorithm appreciation.
4. Colombatto, C. (2025) — LLMs evoke perceptions of agency through natural language.
5. Brown (2025) — Epistemic vigilance applied to AI contexts.
6. Ghafouri (2025) — LLM fluency erodes epistemic vigilance.
7. Zhou (2024) — LLMs provide incorrect yet plausible answers with high confidence.
8. Song (2025) — Trust transfer theory.
9. Saffarizadeh (2024) — Relationship between AI and trust transfer.
10. Yao (2025) — Trust transfer in medical AI contexts.
11. Alonbarkat (2022) — Automation bias and human-AI.
12. Tong (2020) — Epistemic selective trust.
13. Park, J.S. et al. (2024) — Generative agent simulations of 1,000 people.
14. Bansal et al. (2019) — Mental models in human-AI team performance.
15. Buçinca et al. (2021) — Cognitive forcing functions reduce overreliance.