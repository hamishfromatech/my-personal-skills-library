# Customer Digital Twin Neuromarketing Evidence Base

## Primary Source

**Okyere Sefa, A.A., Rezaei, M.R., & Valilai, O.F. (2026). "An analytics-driven method for building ethical customer digital twins using neuromarketing and social media data." Decision Analytics Journal. DOI: 10.1016/j.dajour.2026.100689.**

### Research Design
- Setting: fast-fashion e-commerce.
- Participants: monitored via EEG during an online shopping task.
- Ethical priming intervention: ethically framed information (sustainability, fair labour) introduced mid-task.
- Post-task: sentiment-based analysis of participant responses (VADER sentiment).
- Goal: build a Customer Digital Twin (CDT) integrating neuromarketing + social-media analytics.

### EEG-Derived Indicators
- Engagement
- Workload
- Emotional valence

### Key Findings

1. **Complementarity**: EEG-derived indicators and text-based sentiment measures provide partially complementary views of consumer states.
2. **Fusion challenge**: simple fusion strategies exhibit limited predictive power under real-world, asynchronous conditions (EEG is task-synchronous; sentiment is post-task or historical).
3. **Ethical priming**: the intervention reshaped cognitive and emotional states, detectable in both EEG and sentiment shifts.
4. **Emotional inertia**: modelling the persistence of emotional states is key to bridging the synchronous-asynchronous gap.
5. **Diagnostic, not fully realised**: the proof-of-concept modelling exercise explores feasibility of combined neuro-sentiment features to predict valence-related indices — an initial diagnostic decision analytics layer rather than a fully realised digital twin.

### Ethical and Technical Challenges Identified
- Temporal alignment of neural and social signals.
- Dynamic adaptation of the CDT as consumer states evolve.
- Privacy of neural data (cannot be anonymised in the traditional sense; contains identity, preference, cognitive-trait signatures).
- Consent for priming interventions.
- Risk of affect manipulation.

## Supporting Literature

### Emotional Inertia
- Kuppens, P., Allen, N.B., & Sheeber, L.B. (2010). "Emotional Inertia and Psychological Maladjustment." Psychological Science, 21(7), 984-991. — Foundational emotional-inertia paper; inertia = autocorrelation of emotional states.
- Koval, P., Sütterlin, S., & Kuppens, P. (2016). "Emotional Inertia is Associated with Lower Well-Being when Controlling for Differences in Emotional Context." Frontiers in Psychology, 6, 1997. — Inertia as a marker of psychological rigidity.
- Wenzel, M., & Brose, A. (2022). Emotion, 23(2), 412-424. — Measurement reliability of inertia; improves predictive validity for depressive symptoms.
- Rzeszutek, M., Gruszczyńska, E., & Firląg‐Burkacka, A. (2021). Health and Quality of Life Outcomes, 19(1), 105. — Daily emotional inertia and long-term well-being (HIV context; method generalises).

### Customer Digital Twins
- Tao, F., Zhang, H., Liu, A., & Nee, A.Y.C. (2019). IEEE Transactions on Industrial Informatics, 15(4), 2405-2415. — Digital Twin in Industry (foundational).
- Fuller, A., Fan, Z., Day, C., & Barlow, C. (2020). IEEE Access, 8, 108952-108971. — Digital Twin enabling technologies, challenges, open research.
- Mihai, S., Yaqoob, M., et al. (2022). IEEE Communications Surveys & Tutorials, 24(4), 2255-2291. — Digital Twins survey.
- Abdelrahman, M., Biljecki, F., et al. (2025). Building and Environment, 274, 112748. — "What is a Digital Twin anyway?" — definition from 15,000+ publications.
- Toubia, O., Gui, G., Peng, T., et al. (2025). Marketing Science, 44(6), 1446-1455. — Twin-2K-500 dataset for digital twins of 2,000+ people.

### Neuromarketing + Sentiment
- Ramsøy, T.Z., Michael, N., & Michael, I. (2019). Scientific Reports, 9(1), 15102. — Consumer neuroscience of conscious and subconscious destination preference.
- Najafabadi, A.J., Skryzhadlovska, A., & Valilai, O.F. (2024). Procedia Computer Science, 232, 1683-1693. — Agile product development via neurobehavioral + social-media sentiment.
- Almashaleh, O., & Valilai, O.F. (2025). IEEE Transactions on Engineering Management, 73, 495-509. — Causal drivers of sustainable social-media engagement (double ML).

### Neuroprivacy and Ethics
- Ienca, M., & Andorno, R. (2017). Life Sciences, Society and Policy, 13(1), 5. — New human rights in the age of neuroscience and neurotechnology.
- Yuste, R., Goering, S., et al. (2017). Nature, 551(7679), 159-163. — Four ethical priorities for neurotechnologies and AI.

## Cross-References

- `large-behavior-model-retail-customer` — transaction-based CDT; the behavioural complement to this neural-social CDT.
- `ai-consumer-behavior-brand-relationship` — 5-cluster/5-pillar consumer-brand relationship model; CDT operationalises it.
- `neuro-rights-data-sovereignty-monetization` — neuro-rights framework; CDT must comply.
- `cognitive-privacy-neuromarketing-paradox` — privacy paradox in neuromarketing; CDT inherits the tension.
- `provenance-preserving-chronicles-minimum-disclosure` — minimum-disclosure architecture; CDT should adopt.
- `digital-twin-memory-architecture` — cognitive-science digital-twin memory architecture; complementary.

## A-Tech Value Alignment

- **Open-source AI**: VADER is open-source; transformer sentiment models are open-weight; the CDT framework is reproducible.
- **Data privacy**: bounded, consented EEG sessions; no continuous biometric streaming; social-media data is consented user-generated content.
- **Financial freedom**: ethical CDTs enable SMEs to access consumer insights without $50K-$200K fMRI studies.
- **Practical implementation**: proof-of-concept is a diagnostic analytics layer; temporally aligned architectures are the next research step.