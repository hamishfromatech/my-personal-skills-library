# AI-Neuromarketing Synergy Framework — Evidence Base

## Primary Source

**Alsharif, A.H., Wang, J., Isa, S.M., Salleh, N.Z.M., Dawas, H.A., Alsharif, M.H.** (2025). "The synergy of neuromarketing and artificial intelligence: A comprehensive literature review in the last decade." *Future Business Journal*, 11, 170. Open access. DOI: 10.1186/s43093-025-00591-x. Published July 21, 2025.

## Methodology

- Systematic literature review following PRISMA methodology
- Scopus database, 2013–November 2023
- Initial pool: 1,672 publications → refined to 1,577 (date range) → 642 relevant articles after title/abstract/keyword screening
- English-language peer-reviewed articles only
- Five research questions guiding the analysis

## Emotion Models — Comparison

### VA (Valence-Arousal) — Russell 1980
- Two fundamental neurophysiological systems: valence (pleasure–displeasure) and arousal (high–low)
- EEG estimates valence; EDA gages arousal
- Vecchiato et al. (2010): neurophysiological measures of consumer responses to TV commercials, EEG + biometric data aligned with VA framework
- Cartocci et al. (2017): EEG + autonomic measures for public service announcements
- **Strength:** Simple, widely used; **Limitation:** Cannot capture complex emotional variation or individual differences

### AVA (Appraisal-Valence-Arousal) — Smith & Ellsworth 1985
- Adds cognitive appraisal (threat, benefit, fairness) to understand relevance of emotions
- Kim (2023): cognitive appraisals of music align with emotional responses
- **Strength:** Incorporates cognitive evaluation; **Limitation:** No individual variability

### VAD (Valence-Arousal-Dominance) — Russell & Mehrabian 1977
- Three dimensions: positive/negative valence, high/low arousal, dominance (degree of control/power)
- Applied in online advertising, dialog systems
- Chang et al. (2018): materialism moderates relationship between advertorial attributes and consumer emotional states
- **Strength:** Comprehensive 3D profile; **Limitation:** Cross-cultural applicability of dominance varies

## Emotion Recognition — Physiological Channels

### Face recognition
- Ekman's six basic emotions expressed through universal facial expressions
- CNN/DNN for image recognition; error rates approaching human performance for static images (Krizhevsky et al. 2017)

### Speech recognition
- DNNs enhance accuracy and efficiency for acoustic modeling and emotion recognition
- Anvarjon et al. (2020): Deep-Net lightweight CNN-based speech emotion recognition

### Heart rate
- ECG measures HRV → insights into autonomic nervous system
- Integration with other biosensors enhances relevance

### Skin conductance activity (EDA/GSR)
- Associated with emotional arousal across stimuli
- Tracks emotions, memory, marketing ad effectiveness, consumer preferences

## Attention Models — Comparison

### Bottom-up (automatic attention)
- Automatic capture by salient environmental stimuli (color, brightness, novelty, motion)
- Feed-forward sensory processing driven by external factors
- Mo et al. (2021), Simonetti & Bigne (2022): bottom-up visual stimulation in online contexts (clothing, social media cues)
- **Strength:** Explains environment-driven perception; **Limitation:** No emphasis on cognitive control

### Top-down (controlled attention)
- Goal-directed process influenced by expectations, knowledge, objectives
- Intraparietal cortex and superior frontal cortex involved
- Bressler et al. (2008): top-down control of visual cortex by frontal and parietal cortex
- **Strength:** Explains voluntary attention; **Limitation:** Complexity of interaction with bottom-up

## Attention Recognition — Physiological Channels

### Eye movement
- Linked to executive function, visual attention, working memory
- Hidden Markov models unveil relationship between patterns and recognition performance
- Hsiao et al. (2021): EMHMM with co-clustering

### Gaze tracking
- Pivotal for understanding consumer behavior and cognitive processes
- Attention mechanisms integrated into gaze-tracking systems enhance accuracy

### Pupil dilation
- Associated with cognitive control, attention, memory strength
- Discriminates between subjective and objective familiarity/novelty
- Lisi et al. (2015): pupil dilation reveals top-down attentional load during spatial monitoring

### Fixation point
- Fixation profiles reflect attention to diagnostic facial regions for each emotion
- Fixation duration and frequency shed light on visual attention and locations of interest in branding/packaging evaluation

## Memory Models — Comparison

### Atkinson-Shiffrin Memory (ASM) — Atkinson & Shiffrin 1968
- Sensory → short-term → long-term sequential flow
- Aligns with Miller's information processing limits (7±2)
- Emphasizes control processes: encoding, storing, retrieval
- **Strength:** Structured understanding; **Limitation:** Oversimplifies cognitive interactions

### Levels of Processing Memory (LOPM) — Craik & Lockhart 1972
- Deeper cognitive processing → enhanced memory retention
- Three processes: structural (shallow), phonemic, semantic (deep)
- Ovalle-Fresa et al. (2021): depth of processing during encoding influences subsequent retrieval
- **Strength:** Impact of deep processing on retention; **Limitation:** Defining/measuring depth consistently

### Working Memory (WM) — Baddeley & Hitch 1974
- Central executive + phonological loop + visuospatial sketchpad
- Guides hierarchical nature of memory representations
- **Strength:** Clinically relevant; **Limitation:** Limited long-term memory interaction

### Constructive Memory (CM)
- Memory is reconstructed, not exact replay
- Emotional valence enhances recall probability (Yonelinas & Ritchey 2015: slow forgetting of emotional episodic memories)
- Schacter & Addis (2007): episodic memory as constructive process for remembering past and imagining future
- **Strength:** Acknowledges adaptability; **Limitation:** Influence of biases and inaccuracies

### Associative Memory (AM) — McClelland & Rumelhart 1985
- Memory organized through associations between concepts
- Brain connects stimuli and emotions → memories impacting consumer preferences
- Mohanty et al. (2016): memory for item information (brand names, logos) essential for brand awareness
- Hernandez & Minor (2015): advergames enhance brand recall through repeated exposure
- **Strength:** Insights into learning; **Limitation:** Simplifies associative complexity

## Memory Key Components

### Encoding
- Sensory encoding converts input to brain-compatible format
- Brain prioritization based on sensory richness
- Sensory neurons central to memory formation

### Storing (Retention)
- Retaining encoded information over time
- Encoding-retrieval matches crucial for understanding retention variability
- Hippocampus plays key role in memory generation and processing

### Retrieval
- Prefrontal cortex (PFC), hippocampus (HC), dorsolateral PFC (dlPFC) key regions
- Memory influences preferences and decision-making
- Brand memory influences willingness to buy

## Neuroscientific Techniques

| Technique | Type | Resolution | Cost | Neuromarketing Use |
|---|---|---|---|---|
| fMRI | Metabolic | High spatial, low temporal | Very high | Predicting financial decisions, brand judgments, attention control |
| EEG | Electrical | High temporal, low spatial | Moderate | Predicts consumer choices, evaluates media, assesses emotional responses |
| Eye-tracking | Behavioral | High temporal | Moderate | Advertising effectiveness, brand attention, visual complexity |
| GSR | Autonomic | Moderate | Low | Visual influence on impulse buying, consumer choice prediction |
| ECG | Autonomic | Moderate | Moderate | HRV for ANS insights, distinguishing emotions, purchase intentions |
| EMG | Muscular | High temporal | Moderate | Subconscious reactions to advertisements, health habit impact |
| VOPAN | Voice | Moderate | Low | Voice pitch, social perceptions, aesthetics, cognitive processes |

## AI Models in Neuromarketing

### Brain-Computer Interface (BCI)
- Direct brain-device communication
- Consumer-grade BCIs broaden brainwave data applications
- Panda et al. (2024): EEG-based neuro-recommendation system for consumer purchase experience
- Congedo et al. (2017): Riemannian EEG analysis framework
- BCI + ML monitors cognitive states (attention, memorization) relevant to marketing

### Deep Learning (DL)
- DL decodes EEG signals to predict WTP and product preferences
- DeePay (Hakim et al. 2023): DL decodes EEG to predict consumer's willingness to pay
- DL outperforms traditional classifiers in recognizing consumer preferences from EEG
- Aldayel et al. (2020): Deep learning for EEG-based preference classification
- DRL: learns from behavioral patterns and reward-based systems

### Machine Learning (ML)
- ML predicts consumer preferences using EEG measures (Byrne et al. 2022)
- SVM: robustness in classification/regression; 98.96% accuracy in anxiety/depression classification
- SVM for sentiment analysis of reviews, preference classification from EEG
- Hassabis et al. (2017): ML impact in analyzing neuroimaging datasets

### Deep Neural Networks (DNNs)
- Increasingly recognized for consumer preference recognition and decision-making
- Outperform traditional ML in accuracy and recall
- Panda et al. (2024): robust EEG-based framework with DNNs + spatial attention for consumer choice profiling

#### NLP
- Sentiment analysis of consumer reviews and social media
- Conway et al. (2019): NLP for public health research using social media
- Decodes neurobiology of consumer decision-making

#### Speech Recognition
- DNNs enhance accuracy for acoustic modeling and emotion recognition
- Atmaja et al. (2022): multitask and single-task learnings for speech emotion recognition

#### Image Recognition
- CNNs play pivotal role in predicting/understanding consumer behavior
- Krizhevsky et al. (2017): ImageNet classification with deep CNNs
- Predicts consumer responses, provides personalized product recommendations

## Ethical and Bias Considerations

### Ethical Issues
1. **Privacy of consumer neuro-data** — AI analyzes brain activity and biometric responses revealing subconscious preferences without explicit knowledge. Data protection regulations and transparency protocols essential.
2. **Manipulation** — If NM techniques reach a level where they effectively manipulate consumer choices undetected, it challenges free will and autonomy (Fisher et al. 2010; Murphy et al. 2008).
3. **Informed consent** — Many consumers don't fully understand how neuro-data is collected and used. Complexity of NM research makes informed consent difficult.
4. **Consumer protection** — Protecting individuals from harm or exploitation; preserving free will in decision-making.
5. **Explainable AI (XAI)** — Interpretability of AI models becomes vital ethical concern. XAI allows interpretation of results, facilitating human understanding.

### Bias Issues
1. **Unrepresentative training data** — If training datasets aren't representative, insights reflect narrow viewpoint; marketing strategies fail to resonate with diverse segments.
2. **Cultural/contextual differences** — Cultural backgrounds alter perception of marketing stimuli; emotional responses moderated by cultural biases (uncertainty avoidance, individualism vs. collectivism).
3. **AI model bias** — AI systems with flawed assumptions about consumer behavior may misinterpret neural signals → inaccurate predictions.
4. **Cross-cultural validity** — AI algorithms trained on one demographic may not perform well on different cultural groups; eye-tracking studies show visual attention differs across cultures.

## The Privacy-First Translation Table

The core A-Tech principle: replace biometric surveillance with behavioral-signal proxies. This avoids the reverse-inference trap (Poldrack 2006/2011) because behavioral signals are observable and the model makes forward predictions.

| Lab Neurometric | AI Model | Privacy-First Behavioral Proxy | A-Tech Application |
|---|---|---|---|
| EEG frontal alpha (approach motivation) | ML classification | Session length, completion rate, return rate | A-Coder flow-state inference |
| EEG frontal beta (cognitive arousal) | DL regression | Session complexity, task branching | A-Coder engagement depth |
| Eye-tracking fixation duration | ML + CNN gaze | Dwell time, scroll depth, element interaction | Be Practical content engagement |
| Eye-tracking number of fixations | ML counting | Element interaction count, navigation depth | Be Practical information processing |
| GSR arousal | ML arousal classification | Scroll velocity, click-pattern regularity | Builder's Club community excitement |
| Facial expression valence | CNN facial recognition | Hesitation patterns, replay rate, abandonment | Product experience sentiment |
| HR/HRV autonomic state | ML time-series | Interaction rhythm, response latency | Stress/engagement inference |
| Voice pitch analysis | DNN speech recognition | Input length, response latency | Voice/video tool engagement |
| EEG-based WTP prediction | DL (DeePay) | Feature adoption rate, upgrade trigger events | A-Tech pricing research |
| fMRI reward prediction | DL classification | Product page time, comparison behavior | Purchase intent inference |

## Implications for A-Tech

### Theoretical
- AI integration redefines understanding of consumer behavior by analyzing vast neural/physiological datasets
- Emotion, attention, and memory are the three foundational constructs
- AI algorithms identify subtle patterns and correlations that elude human researchers
- The synergy fosters comprehensive, data-driven understanding of consumer behavior

### Practical
- AI-powered NM tools gain insights at conscious and subconscious levels
- Predictive analytics refines understanding of consumer preferences
- User-friendly platforms and robust data integration systems empower marketing teams
- Ethical considerations are crucial: regulatory bodies must develop clear guidelines on data privacy, informed consent, ethical applications of neuro-data

### Social
- Fosters more informed consumer base through emotionally resonant campaigns
- Raises ethical concerns about manipulation and subconscious exploitation
- Increases focus on transparency and data usage disclosure
- Companies prioritizing ethical considerations build stronger relationships and trust

## Future Directions (from the review)

- Refining AI model interpretability and fairness
- Exploring novel neuroscientific techniques
- Addressing ethical implications of AI-NM integration
- Balancing technological advancement with ethical responsibility
- Trend toward more transparent AI, regulation, equitable marketing prioritizing consumer health and cognitive privacy
- Generative AI for better content, offers, and experiences tailored to individual users
- Computer vision and NLP for omnichannel marketing understanding
- Scalability and feasibility for SMEs remains debated

## Cross-References

- `neuromarketing-consumer-journey-3x3-framework` — stage-specific framework
- `hybrid-eeg-gaze-decoding-scheme` — multimodal decoding implementation
- `predictive-neuromarketing-bayesian-framework` — predictive-coding mathematical framework
- `neuromarketing-predictive-purchase-intent-model` — single-variable predictor hierarchy
- `neuro-marketing-privacy-first-behavioral-analytics` — privacy architecture
- `dark-psychology-neuromarketing-autonomy-defense` — ethical defense
- `neuromarketing-2026-practical-operating-model` — campaign execution
- `cognitive-load-reduction-ai-scaffolding` — attention/cognitive load
- `algorithmic-transparency-accountability` — XAI implementation
- `consent-fatigue-progressive-permissioning` — consent design