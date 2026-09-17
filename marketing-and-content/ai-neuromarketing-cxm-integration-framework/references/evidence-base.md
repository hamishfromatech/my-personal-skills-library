# Evidence Base: AI-Neuromarketing-CXM Integration Framework

## Primary Source

**Citation:** Topcugil & Hiziroglu (2026). AI-Neuromarketing-CXM Integration Framework. *Future Business Journal*, Springer Nature. Published August 14, 2026.

**Author Affiliation:** Izmir Bakircay University, Izmir, Turkey.

**Publication Type:** Peer-reviewed conceptual framework paper. *Future Business Journal* is an open-access journal published by Springer Nature focusing on emerging business models, technology-management integration, and future-oriented business research.

---

## Theoretical Basis

The framework synthesizes three distinct research streams into an integrated conceptual architecture. Each stream contributes a dimension of the framework.

### Stream 1: Customer Experience Management (CXM)

CXM research provides the customer journey dimension and the touchpoint-centric logic of the framework.

**Key concepts drawn from CXM literature:**

- **Customer journey as multi-stage**: The pre-purchase → purchase → post-purchase model is grounded in Lemon & Verhoef (2016) and subsequent CXM frameworks that conceptualize the customer experience as a longitudinal journey rather than a transactional moment.
- **Touchpoints as the unit of analysis**: CXM frameworks position touchpoints — any point of contact between customer and firm — as the atomic unit where experience is created and measured. The framework adopts this touchpoint-centric view, extending it to include biometric measurement at each touchpoint.
- **Experience as cumulative**: CXM research emphasizes that customer experience is the cumulative perception across all touchpoints, not the sum of individual interactions. The framework's firm journey dimension (data → analytics → insights → actions) reflects this by processing biometric data across the entire journey, not per-touchpoint in isolation.
- **CXM vs. CRM distinction**: The framework explicitly positions itself within CXM (managing the experience) rather than CRM (managing the relationship/data). CXM is experience-centric and journey-oriented; CRM is data-centric and transaction-oriented. Biometric data enriches CXM by adding an emotional layer that CRM's transactional data cannot provide.

**Theoretical grounding**: The CXM stream draws on:
- Lemon, K.N., & Verhoef, P.C. (2016). Understanding customer experience throughout the customer journey. *Journal of Marketing*.
- Verhoef, P.C., et al. (2009). Customer experience creation: Determinants, dimensions and outcomes. *Journal of Retailing*.
- Berry, L.L., Carbone, L.P., & Haeckel, S.H. (2002). Managing the total customer experience. *Sloan Management Review*.

### Stream 2: Artificial Intelligence in Marketing

AI-in-marketing research provides the firm journey dimension, the analytics typology, and the three firm types.

**Key concepts drawn from AI-in-marketing literature:**

- **AI analytics typology**: The four-type analytics progression (descriptive → predictive → diagnostic → prescriptive) is adapted from the broader business analytics literature (Davenport & Harris, 2017) and applied specifically to AI-driven marketing analytics. The framework extends this typology by specifying biometric data as an input to each analytics type.
- **Three AI types (mechanical, thinking, feeling)**: This typology originates from Huang & Rust (2021), who categorize AI capabilities along a continuum from mechanical (routine automation) to thinking (reasoning and prediction) to feeling (emotional intelligence and empathy). The framework adopts this typology and maps it to CXM functions:
  - Mechanical AI → data collection automation, descriptive analytics, dashboard generation
  - Thinking AI → predictive and diagnostic analytics, pattern recognition in biometric data
  - Feeling AI → emotion-responsive CXM, real-time touchpoint adaptation, empathetic AI agents
- **AI as augmentation, not replacement**: The framework positions AI as augmenting human CXM decision-making, not replacing it. The firm journey's "Insights" and "Actions" stages retain human interpretation, even when analytics is fully AI-driven.

**Theoretical grounding**: The AI-in-marketing stream draws on:
- Huang, M.H., & Rust, R.T. (2021). A strategic framework for artificial intelligence in marketing. *Journal of the Academy of Marketing Science*.
- Davenport, T., & Harris, J. (2017). Competing on analytics: The new science of winning. *Harvard Business Review Press*.
- Mustak, M., et al. (2021). Artificial intelligence in marketing: Topic modeling, scientometric analysis, and research agenda. *Journal of Business Research*.
- Vlačić, B., et al. (2021). The evolving role of artificial intelligence in marketing. *Journal of Business Research*.

### Stream 3: Neuromarketing

Neuromarketing research provides the biometric data class and the measurement modalities.

**Key concepts drawn from neuromarketing literature:**

- **Biometric measurement modalities**: The four modalities (EEG, GSR, facial coding, eye-tracking) are the most established tools in consumer neuroscience. The framework selects these based on their demonstrated validity in measuring marketing-relevant constructs:
  - EEG → attention, engagement, approach/withdrawal motivation (frontal asymmetry)
  - GSR → emotional arousal (sympathetic nervous system activation)
  - Facial coding → emotion categorization via FACS (Ekman, 1992)
  - Eye-tracking → visual attention, cognitive effort (pupillometry)
- **Subconscious measurement advantage**: Neuromarketing's core value proposition is measuring reactions that self-report cannot capture — either because they are too fast (millisecond-level), too subtle (micro-expressions), or subconscious (brainwave patterns). The framework positions this as the key advantage of biometric data over behavioral and transactional data.
- **Biometric data as a distinct class**: The framework's novel contribution is explicitly classifying neuro/biometric data as a third data class (alongside behavioral and transactional). Prior literature treats biometric data as an add-on to behavioral data or as a standalone research tool. The framework argues it should be a first-class citizen in CXM data architectures.

**Theoretical grounding**: The neuromarketing stream draws on:
- Plassmann, H., et al. (2015). Consumer neuroscience: applications, challenges, and solutions. *Journal of Consumer Psychology*.
- Ariely, D., & Berns, G.S. (2010). Neuromarketing: the gap between the promise and the reality. *Nature Reviews Neuroscience*.
- Ekman, P. (1992). Facial expressions of emotion: new findings, new questions. *American Psychologist*.
- Poels, K., & Dewitte, S. (2019). How own and comparative advertising neutralize consumer scepticism. *Journal of Advertising*.
- Wedel, M., & Pieters, R. (2008). A review of eye-tracking research in marketing. *Review of Marketing Research*.

---

## Three AI Types: Detailed Specification

The framework's three AI types (adapted from Huang & Rust, 2021) are not merely capability levels but represent distinct architectural approaches to integrating biometric data into CXM.

### Mechanical AI

**Definition**: AI that performs routine, repetitive, rule-based tasks without reasoning or learning. In the CXM context, mechanical AI automates the data collection and basic processing stages of the firm journey.

**CXM function**: 
- Automates biometric data collection from sensors and APIs
- Performs data cleaning, normalization, and structuring (e.g., removing EEG artifacts, normalizing GSR baselines)
- Generates standard visualizations (heatmaps, arousal timelines, emotion frequency charts)
- Routes data to downstream analytics pipelines

**Analytics depth**: Descriptive analytics only. Mechanical AI answers "what happened?" with aggregated, visualized biometric data.

**Biometric data handling**: 
- EEG: artifact removal (blinks, muscle noise), band-power extraction (alpha, beta, theta, gamma)
- GSR: tonic/phasic decomposition, arousal event detection
- Facial coding: frame-by-frame FACS action unit detection, emotion classification
- Eye-tracking: fixation identification, scan-path generation, heatmap aggregation

**CXM maturity implication**: Firms at the mechanical AI level can collect and visualize biometric data but cannot predict, diagnose, or prescribe. They know *what* customers feel at touchpoints but not *why* or *what to do about it*.

**Open-source implementation**: EEGlab (EEG processing), OpenFace (facial coding), standard eye-tracking analysis libraries. These are well-established open-source tools that make mechanical AI-level biometric processing accessible to SMEs.

### Thinking AI

**Definition**: AI that reasons from data — classifying, predicting, and diagnosing. Thinking AI applies machine learning to identify patterns in biometric data and relate them to CXM outcomes.

**CXM function**:
- Predicts customer behavior from biometric patterns (e.g., churn prediction from support-call GSR + facial coding)
- Classifies emotional states and segments customers by biometric response profiles
- Diagnoses root causes of experience friction by correlating biometric signals with journey events
- Identifies which touchpoints have the highest emotional impact

**Analytics depth**: Predictive + diagnostic analytics. Thinking AI answers "what will happen?" and "why did it happen?"

**Biometric data handling**:
- ML models (random forests, gradient boosting, neural networks) trained on biometric → behavior/outcome mappings
- Time-series models for biometric trajectory prediction
- Causal inference methods (Granger causality, structural equation modeling) for diagnostic insights
- Feature importance and SHAP values to explain which biometric features drive predictions

**CXM maturity implication**: Firms at the thinking AI level can predict which customers are at risk and diagnose which touchpoints cause friction. They cannot yet automatically adapt the experience in real-time.

**Open-source implementation**: scikit-learn, XGBoost, PyTorch/TensorFlow for model training; SHAP for explainability; open biometric datasets (DEAP, MAHNOB-HCI) for pre-training.

### Feeling AI

**Definition**: AI that recognizes and responds to human emotion — affective computing and empathetic AI. Feeling AI closes the loop by not only detecting emotion but generating appropriate emotional responses and adapting the experience in real-time.

**CXM function**:
- Detects customer emotional state in real-time from biometric signals
- Generates empathetic responses (e.g., chatbot adjusts tone when frustration detected)
- Adapts touchpoint content dynamically based on emotional state
- Prescribes and executes touchpoint interventions autonomously (with human oversight)

**Analytics depth**: Prescriptive analytics + real-time adaptation. Feeling AI answers "what should we do?" and does it.

**Biometric data handling**:
- Real-time emotion recognition models (streaming EEG/GSR/facial coding inference)
- Affective computing models that map emotional state → optimal CXM response
- Reinforcement learning agents that learn which touchpoint adaptations produce desired emotional outcomes
- Generative models for empathetic response generation (tone, content, timing)

**CXM maturity implication**: Firms at the feeling AI level can detect emotion, decide on a response, and adapt the experience in real-time. This is the frontier — few firms operate here today, and significant ethical and technical challenges remain.

**Open-source implementation**: Open-source emotion recognition models, ONNX Runtime / TensorFlow Lite for real-time edge inference, reinforcement learning frameworks (Stable Baselines3, Ray RLlib). Open-weight LLMs for empathetic response generation.

### Progression Path

The framework implies a maturity progression: **Mechanical → Thinking → Feeling**. Most organizations today operate at the mechanical level (basic biometric dashboards) or early thinking level (predictive models). Feeling AI is aspirational and represents the competitive frontier for CXM differentiation.

Importantly, the progression is not strictly linear — a firm may have feeling AI at one touchpoint (e.g., an empathetic chatbot) while operating at mechanical AI at others (e.g., automated eye-tracking heatmaps). The framework accommodates this heterogeneity.

---

## Vignette Details

The three illustrative vignettes demonstrate the framework across different industries, biometric modalities, and AI types. They are **illustrative scenarios** constructed to demonstrate framework application, not case studies of fully deployed systems.

### Vignette 1: Liberty Group (Insurance) — Detailed

**Context**: Liberty Group is an insurance provider seeking to optimize its AI chatbot for claims and policy inquiries. Insurance claims are inherently high-stress — customers filing claims are often in distress, making emotional measurement particularly valuable.

**Touchpoint**: AI chatbot (purchase/post-purchase stage — claims filing and policy inquiry)

**Biometric stack and application**:

| Modality | Sensor | What's Measured | CXM Question Answered |
|----------|--------|-----------------|----------------------|
| EEG | Consumer-grade headset (e.g., open-source OpenBCI) | Cognitive load (theta power), attention (alpha suppression), approach/withdrawal (frontal asymmetry) | Is the chatbot conversation too cognitively demanding? |
| GSR | Wristband or finger sensor | Emotional arousal intensity | How stressful is the claims discussion? |
| Facial coding | Webcam + facial coding software | Emotion valence (frustration, confusion, relief, satisfaction) | What emotions does the chatbot evoke? |
| Eye-tracking | Webcam-based or eye-tracker | Gaze patterns, fixation on policy text vs. chatbot | Are customers reading policy details or skipping? |

**AI analytics application**:
- **Descriptive**: Aggregate biometric data across chatbot sessions → "Average GSR arousal is 2.1× higher during claims discussions vs. policy inquiries"
- **Diagnostic**: Correlate biometric signals with chatbot conversation logs → "Medical terminology in claims triggers GSR spike + facial 'confusion' + low fixation on text → indicates comprehension failure"
- **Predictive**: Train model on biometric + conversation data → "Sessions with frontal asymmetry (withdrawal) + rising GSR have 3.4× higher likelihood of chatbot abandonment"
- **Prescriptive**: RL agent learns optimal intervention → "When arousal > threshold + confusion detected → simplify language + offer human handoff"

**Touchpoint actions**:
- **Monitoring**: Continuous biometric tracking on chatbot interactions (opt-in panel of customers)
- **Prioritization**: Claims flow identified as highest-emotional-impact touchpoint → budget allocated here first
- **Adaptation**: Real-time chatbot tone and language adjustment based on detected emotional state
- **Journey design**: Restructure claims flow to front-load plain-language summaries before technical details

**Firm type**: Transitioning from Thinking AI (diagnostic + predictive) to Feeling AI (real-time empathetic adaptation)

**Ethical considerations**: Insurance claims involve vulnerable customers. Informed consent is critical — customers must understand biometric monitoring during a stressful life event. Data minimization is essential — biometric data should be purged after the interaction. Consumer choice: biometric monitoring must be optional, with non-biometric chatbot experience as default.

### Vignette 2: TUI (Travel) — Detailed

**Context**: TUI is a travel/tourism company seeking to personalize holiday recommendations based on customers' emotional responses to destination imagery.

**Touchpoint**: Holiday browsing platform (pre-purchase stage — destination discovery and consideration)

**Biometric stack and application**:

| Modality | Sensor | What's Measured | CXM Question Answered |
|----------|--------|-----------------|----------------------|
| Facial coding | Webcam + facial coding software | Emotional reactions to destination imagery (joy, surprise, excitement, neutral) | Which destinations evoke positive emotional responses? |

Note: TUI vignette uses facial coding as the primary modality — lower barrier to entry (webcam-based) than EEG/GSR, more feasible for a broad customer base during browsing.

**AI analytics application**:
- **Descriptive**: Aggregate facial coding data across browsing sessions → "Beach destinations evoke 40% more 'joy' expressions than city destinations"
- **Predictive**: Model predicts destination category preference from facial response patterns → "Customers showing 'surprise' + 'joy' to adventure imagery are 2.8× more likely to book adventure holidays"
- **Prescriptive**: Recommend personalized holiday packages based on emotional response profile → "Recommend Destination X (predicted emotional match: 87%)"

**Touchpoint actions**:
- **Monitoring**: Track emotional responses to destination content across sessions
- **Adaptation**: Homepage dynamically reorders destinations based on cumulative emotional response profile
- **Journey design**: Restructure browsing experience to surface emotionally resonant content earlier

**Firm type**: Feeling AI (emotion-driven personalization)

**Ethical considerations**: Facial coding via webcam raises significant privacy concerns. On-device processing (A-Tech adaptation) is critical — facial coding inference should run locally, transmitting only derived emotion labels, not video frames. Consent must be explicit and granular.

### Vignette 3: Zyro (Software/SaaS) — Detailed

**Context**: Zyro is a software/SaaS company seeking to optimize its website and landing pages using eye-tracking data.

**Touchpoint**: Website and landing pages (pre-purchase stage — awareness and consideration)

**Biometric stack and application**:

| Modality | Sensor | What's Measured | CXM Question Answered |
|----------|--------|-----------------|----------------------|
| Eye-tracking | Eye-tracker or webcam-based | Gaze point, fixation duration, scan-path, pupil dilation | Which page elements attract attention? Where is cognitive effort spent? |

Note: Zyro vignette uses eye-tracking only — the most established and commercially mature biometric modality in web optimization. This represents the lowest-friction entry point for biometric-informed CXM.

**AI analytics application**:
- **Descriptive**: AI-generated heatmaps from gaze data → "70% of fixations are above the fold; CTA button receives 3% of gaze time"
- **Diagnostic**: Correlate fixation patterns with conversion data → "Pages where CTA receives <5% gaze time have 45% lower conversion"
- **Predictive**: Model predicts conversion likelihood from scan-path features → "F-shaped scan pattern + low CTA fixation → 78% predicted bounce probability"

**Touchpoint actions**:
- **Monitoring**: Regular eye-tracking studies on key landing pages
- **Prioritization**: Redesign elements with high cognitive effort (pupil dilation) and low conversion first
- **Journey design**: Restructure page layout based on natural scan-path patterns

**Firm type**: Mechanical → Thinking AI (automated heatmap generation + diagnostic insight)

**Ethical considerations**: Eye-tracking is less invasive than EEG/GSR/facial coding but still reveals attention patterns. Webcam-based eye-tracking requires consent. Pupil dilation data can inadvertently indicate cognitive load and emotional state — treat as sensitive.

---

## Ethical Framework: Detailed Specification

The framework specifies five ethical principles. Each is grounded in both marketing ethics literature and data protection regulation.

### Principle 1: Informed Consent

**Requirement**: Participants must fully understand what biometric data is collected, how it is processed, what it is used for, and who has access to it — before data collection begins.

**Implementation details**:
- Consent forms in plain language (not legal jargon) describing each biometric modality
- Explicit, active opt-in (not opt-out or pre-ticked boxes)
- Consent is modality-specific — a user can consent to eye-tracking but decline EEG
- Real-time consent status indicators (e.g., "Eye-tracking: ON | Facial coding: OFF")
- Consent is withdrawable at any time, with immediate data cessation

**Regulatory alignment**: GDPR Article 9 (explicit consent for special category data including biometric data), CCPA/CPRA (opt-in for sensitive data).

### Principle 2: Transparency

**Requirement**: Firms must be open about the AI analytics processes applied to biometric data and how biometric-derived insights influence CXM decisions.

**Implementation details**:
- Published data use policies specifying which AI models process biometric data and for what purposes
- Explainable AI (XAI) outputs — customers (or regulators) can request an explanation of how biometric data influenced a specific CXM decision
- Customer-facing emotion dashboards where appropriate (e.g., "Your browsing session showed elevated interest in adventure travel — we used this to personalize recommendations")
- Third-party audits of AI analytics pipelines, with audit summaries publicly available

**Regulatory alignment**: GDPR Article 22 (right to explanation for automated decisions), EU AI Act (transparency obligations for emotion recognition systems).

### Principle 3: Data Minimization

**Requirement**: Collect only the biometric data necessary for the specified CXM purpose. Do not collect "just in case" or for unspecified future uses.

**Implementation details**:
- Purpose-bound data collection — each biometric data stream must have a specified CXM purpose documented before collection
- Automatic deletion after the analysis window (e.g., raw EEG purged 30 days after session analysis)
- No secondary use without re-consent (e.g., biometric data collected for chatbot optimization cannot be reused for advertising targeting without new consent)
- Data retention schedules published and enforced

**Regulatory alignment**: GDPR Article 5(1)(c) (data minimization principle), Article 5(1)(e) (storage limitation).

### Principle 4: Consumer Choice

**Requirement**: Customers must be able to opt out of biometric data collection without losing access to the core service or experiencing degraded service quality.

**Implementation details**:
- Non-biometric fallback experiences — the CXM system functions (possibly with reduced personalization) without biometric input
- Granular consent toggles — per-modality, per-touchpoint consent management
- Easy withdrawal — one-click consent withdrawal with immediate effect
- No penalty for non-participation — customers who decline biometric monitoring are not charged more, offered fewer features, or otherwise disadvantaged

**Regulatory alignment**: GDPR Article 7(3) (right to withdraw consent at any time), CCPA/CPRA (right to opt out of sale/sharing of sensitive data).

### Principle 5: Algorithmic Accountability

**Requirement**: AI systems processing biometric data must be auditable, fair, and free from discriminatory outcomes. Firms are responsible for the decisions their AI makes.

**Implementation details**:
- Bias testing on emotion recognition models — facial coding systems are known to perform unevenly across demographics (skin tone, age, gender). Models must be tested and certified for equitable performance.
- Third-party algorithmic audits — independent audits of AI analytics pipelines, with remediation requirements for identified biases or risks.
- Human-in-the-loop for high-stakes CXM decisions — automated touchpoint adaptations that significantly affect the customer (e.g., insurance claim routing) require human review.
- Model cards / data sheets documenting training data, performance metrics, limitations, and intended use of each AI model.

**Regulatory alignment**: EU AI Act (risk management obligations for high-risk AI systems), NIST AI Risk Management Framework, GDPR Article 22 (human intervention in automated decisions).

### Ethical Framework Summary

The five principles are interdependent — informed consent is meaningless without transparency; data minimization is unenforceable without algorithmic accountability; consumer choice requires all four other principles to be meaningful. The framework presents them as an integrated ethical architecture, not a checklist.

---

## Comparison with Existing CXM Models

The AI-neuromarketing-CXM integration framework can be positioned against three categories of existing models:

### vs. Traditional CXM Frameworks (Lemon & Verhoef, 2016; Verhoef et al., 2009)

| Dimension | Traditional CXM | This Framework |
|-----------|----------------|----------------|
| Data inputs | Behavioral, transactional, attitudinal (survey) | Behavioral, transactional, **+ neuro/biometric** |
| Emotional measurement | Self-report surveys, sentiment analysis | **Direct biometric measurement** (EEG, GSR, facial coding, eye-tracking) |
| Analytics | Primarily descriptive + predictive | **Four-type AI analytics** (descriptive, predictive, diagnostic, prescriptive) |
| Touchpoint action | General optimization | **Specific strategies** (monitoring, prioritization, adaptation, journey design) |
| AI integration | Optional / peripheral | **Structural** — AI is the processing engine for all data classes |
| Firm capability | Not specified | **Three AI types** (mechanical, thinking, feeling) as maturity model |

**Key difference**: Traditional CXM frameworks treat emotion as something to be inferred from surveys or behavioral proxies. This framework treats emotion as something that can be directly measured via biometric signals and processed by AI into actionable CXM strategy.

### vs. AI-in-Marketing Frameworks (Huang & Rust, 2021; Mustak et al., 2021)

| Dimension | AI-in-Marketing Frameworks | This Framework |
|-----------|---------------------------|----------------|
| Data scope | Behavioral, transactional, text, social | **+ neuro/biometric** data class |
| Application domain | Marketing broadly (advertising, pricing, product, etc.) | **CXM specifically** — customer experience management |
| AI typology | Mechanical, thinking, feeling | **Adopted and mapped to CXM functions** |
| Analytics types | Varies by framework | **Four-type analytics** explicitly linked to biometric data |
| Touchpoint focus | Not central | **Touchpoints are the structural intersection** of customer and firm journeys |
| Neuromarketing | Not integrated | **Structurally integrated** as third data class |

**Key difference**: AI-in-marketing frameworks address AI's role in marketing broadly but do not structurally integrate neuromarketing data. This framework makes biometric data a first-class input to AI-driven CXM.

### vs. Neuromarketing Frameworks (Plassmann et al., 2015; Ariely & Berns, 2010)

| Dimension | Neuromarketing Frameworks | This Framework |
|-----------|--------------------------|----------------|
| Purpose | Understand consumer brain responses | **Integrate biometric data into operational CXM systems** |
| Data use | Research insight | **Operational CXM data** — feeding AI analytics pipelines |
| Scale | Lab studies, small samples | **Framework designed for operational deployment** (with privacy-first adaptation for scale) |
| Actionability | Academic/strategic insight | **Touchpoint-level actions** (monitoring, prioritization, adaptation, journey design) |
| AI integration | Minimal | **AI is the core processing engine** |
| CXM integration | Not addressed | **CXM is the application domain** |

**Key difference**: Neuromarketing frameworks focus on understanding consumer responses in research settings. This framework operationalizes biometric measurement within a CXM system, using AI to translate signals into touchpoint-level actions.

### Synthesis: What This Framework Adds

The AI-neuromarketing-CXM integration framework's unique contribution is the **structural integration** of three previously separate domains:

1. **From CXM**: The customer journey structure and touchpoint-centric logic
2. **From AI-in-marketing**: The analytics typology, firm-type maturity model, and AI as processing engine
3. **From neuromarketing**: The biometric data class and measurement modalities

No prior framework combines all three into a single, structured, actionable architecture with explicit touchpoint strategies, analytics types, firm maturity levels, and ethical principles.

---

## Limitations

The framework has several acknowledged and observable limitations:

### 1. Conceptual, Not Empirical
The framework is a **conceptual architecture**, not an empirically validated model. The vignettes are illustrative scenarios, not case studies of deployed systems. There is no empirical evidence that integrating biometric data into CXM AI pipelines produces superior customer experience outcomes compared to behavioral-data-only approaches. The framework proposes a plausible architecture but does not prove its effectiveness.

### 2. Biometric Hardware Accessibility
The framework assumes access to biometric measurement hardware (EEG headsets, GSR sensors, eye-trackers). While consumer-grade options exist (OpenBCI, webcam-based eye-tracking), the quality of consumer-grade biometric data is significantly lower than research-grade equipment. This limits the real-world fidelity of biometric-informed CXM, particularly for SMEs.

**A-Tech mitigation**: The privacy-first adaptation (behavioral proxies) partially addresses this — firms can approximate biometric insights without hardware. However, proxy models are less accurate than direct measurement.

### 3. Consumer Adoption and Consent Friction
Asking customers to wear EEG headsets or consent to webcam facial coding during routine shopping or service interactions is a significant friction point. Adoption rates for biometric monitoring in non-research CXM contexts are unknown but likely low. The framework's viability depends on consumer willingness to share biometric data — a major unresolved question.

**A-Tech mitigation**: Zero-party biometric data (voluntary, user-initiated sharing) and on-device processing (data stays local) reduce the privacy friction but do not eliminate the hardware/adoption barrier.

### 4. Emotion Recognition Validity
The framework assumes that biometric signals (especially facial coding and EEG) reliably map to marketing-relevant emotional states. This assumption is contested:
- **Facial coding**: Ekman's basic emotion theory (universal discrete emotions) is debated. Constructed emotion theories (Barrett, 2017) argue that facial expressions do not map cleanly to emotion categories.
- **EEG**: Frontal asymmetry as an approach/withdrawal indicator has mixed replication results.
- **GSR**: Measures arousal intensity but not valence — cannot distinguish excitement from anxiety without additional data.

The framework does not address these validity debates. CXM decisions based on potentially unreliable emotion classifications carry risk.

### 5. AI Analytics Causal Claims
The diagnostic and prescriptive analytics types make implicit causal claims — "this touchpoint causes frustration" or "this adaptation will reduce abandonment." Establishing causation from biometric data is methodologically challenging. Correlational patterns in biometric-behavioral data do not guarantee causal relationships. The framework does not specify causal inference methodologies in detail.

### 6. Regulatory Uncertainty
Biometric data processing for marketing purposes faces increasing regulatory scrutiny. The EU AI Act classifies emotion recognition systems as high-risk in certain contexts. Several jurisdictions are considering bans on biometric processing in commercial settings. The framework's ethical principles are necessary but may not be sufficient for compliance in all jurisdictions, and the regulatory landscape is evolving rapidly.

### 7. Scalability of On-Device Processing
The A-Tech adaptation (on-device biometric processing) is technically feasible for simple models (e.g., basic facial coding) but challenging for complex models (e.g., real-time EEG interpretation, multi-modal fusion). Edge device computational constraints may limit the sophistication of on-device biometric analytics, creating a trade-off between privacy and analytical depth.

### 8. Single Framework Source
The framework originates from a single paper (Topcugil & Hiziroglu, 2026). While the theoretical grounding draws on established literature, the specific integration architecture has not been independently validated or tested by other research groups. The framework should be treated as a promising proposal, not an established paradigm.

---

## References (Theoretical Grounding)

- Ariely, D., & Berns, G.S. (2010). Neuromarketing: the gap between the promise and the reality. *Nature Reviews Neuroscience*, 11(4), 284-292.
- Barrett, L.F. (2017). *How Emotions Are Made: The Secret Life of the Brain*. Houghton Mifflin Harcourt.
- Berry, L.L., Carbone, L.P., & Haeckel, S.H. (2002). Managing the total customer experience. *MIT Sloan Management Review*, 43(3), 85-89.
- Davenport, T., & Harris, J. (2017). *Competing on Analytics: The New Science of Winning*. Harvard Business Review Press.
- Ekman, P. (1992). Facial expressions of emotion: new findings, new questions. *American Psychologist*, 47(3), 372-379.
- Huang, M.H., & Rust, R.T. (2021). A strategic framework for artificial intelligence in marketing. *Journal of the Academy of Marketing Science*, 49(1), 30-50.
- Lemon, K.N., & Verhoef, P.C. (2016). Understanding customer experience throughout the customer journey. *Journal of Marketing*, 80(6), 69-96.
- Mustak, M., Salminen, J., Plé, L., & Cooper, J. (2021). Artificial intelligence in marketing: Topic modeling, scientometric analysis, and research agenda. *Journal of Business Research*, 124, 389-404.
- Plassmann, H., Venkatraman, V., Huettel, S., & Yoon, C. (2015). Consumer neuroscience: applications, challenges, and solutions. *Journal of Consumer Psychology*, 25(3), 427-435.
- Poels, K., & Dewitte, S. (2019). How own and comparative advertising neutralize consumer scepticism. *Journal of Advertising*, 48(2), 163-179.
- Topcugil, & Hiziroglu (2026). AI-Neuromarketing-CXM Integration Framework. *Future Business Journal*, Springer Nature. August 14, 2026. Izmir Bakircay University.
- Verhoef, P.C., Lemon, K.N., Parasuraman, A., Roggeveen, A., Tsiros, M., & Schlesinger, L.A. (2009). Customer experience creation: Determinants, dimensions and outcomes. *Journal of Retailing*, 85(1), 31-41.
- Vlačić, B., Corbo, L., e Silva, S.C., & Dabić, M. (2021). The evolving role of artificial intelligence in marketing. *Journal of Business Research*, 128, 187-203.
- Wedel, M., & Pieters, R. (2008). A review of eye-tracking research in marketing. *Review of Marketing Research*, 4, 123-147.

---

## Open-Source Tools Reference

| Tool | Function | Modality | A-Tech Relevance |
|------|----------|----------|------------------|
| EEGlab | EEG processing, artifact removal, band-power extraction | EEG | Open-source MATLAB/Python; standard in neuroscience research |
| OpenFace | Facial action unit detection, facial coding | Facial coding | Open-source; enables facial coding without proprietary APIs |
| OpenBCI | Open-source EEG hardware + software platform | EEG | Open-source hardware; consumer-grade EEG accessibility |
| DEAP dataset | Open EEG + self-report emotion dataset | EEG | Open dataset for pre-training emotion models without proprietary data |
| MAHNOB-HCI dataset | Open multimodal biometric emotion dataset | EEG, GSR, facial, eye-tracking | Open dataset for multimodal model training |
| TensorFlow Lite / ONNX Runtime | Edge AI inference | All modalities | On-device biometric processing for privacy-first CXM |
| scikit-learn / XGBoost | Predictive + diagnostic analytics | All modalities | Open-source ML for thinking-AI-level CXM analytics |
| SHAP | Explainable AI | All modalities | Transparency and algorithmic accountability compliance |
| Stable Baselines3 / Ray RLlib | Reinforcement learning | All modalities | Prescriptive analytics and real-time touchpoint adaptation |

---

*Evidence base compiled from: Topcugil & Hiziroglu (2026), Future Business Journal, Springer Nature. Supplementary theoretical sources as cited.*