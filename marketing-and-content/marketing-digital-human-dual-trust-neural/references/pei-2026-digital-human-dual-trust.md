# Pei et al. (2026) — Digital Human Dual Trust Neural Mechanisms

## Full Citation
Pei, L., Dong, Y., Jin, X., Meng, Y., & Zhang, W. (2026). Research on consumer behavior and neural mechanisms of dual trust in marketing digital humans. *Advances in Psychological Science*, 34(2), 227–238.

**Affiliations**: Zhejiang Lab; Shanghai International Studies University; Zhejiang University.

---

## 1. Background and Motivation

### 1.1 Marketing Digital Humans
Marketing digital humans are a new-generation human-computer interaction (HCI) interface that uses conversational intelligence systems as the crucial engine for driving consumption upgrades. The rise of large language model (LLM) technology creates new opportunities for digital humans to reshape HCI patterns in sales scenarios — moving beyond static avatars or rule-based chatbots toward LLM-powered agents capable of fluent, context-aware, multi-turn conversation.

### 1.2 The Trust Problem
Consumer trust is the central bottleneck for adoption of marketing digital humans. Prior trust research has predominantly been:
- **Static**: measured at a single point in time, ignoring how trust evolves
- **Single-turn**: focused on one-shot interactions rather than ongoing dialogue
- **Unidimensional**: treating trust as a single construct rather than distinct cognitive and affective components

This framework addresses all three limitations.

---

## 2. Three Research Dimensions

The framework is organized around three interlocking dimensions of investigation:

### 2.1 Behavioral Phenomena
How multidimensional conversational intelligence features (accuracy, empathy, warmth, consistency, responsiveness, contextual awareness) and external factors (brand, product category, consumer traits) influence consumer behavior outcomes — including purchase intention, engagement, and satisfaction.

### 2.2 Dynamic Cognitive Process
How conversational intelligence features impact dual trust (cognitive + affective) across multi-turn interactions. Trust is treated as a continuously evolving state rather than a fixed disposition.

### 2.3 Neural Mechanisms
fMRI characterization of the neural mechanisms underlying dual trust in the marketing digital human context — distinguishing cognitive from affective trust at the brain level.

---

## 3. Cognitive-Affective Trust Theory

### 3.1 Dual Trust Dimensions
Consumer trust in digital humans comprises two distinct but interacting dimensions:

**Cognitive Trust**:
- Grounded in competence, reliability, and capability assessment
- The consumer evaluates: "Does this digital human know what it's talking about? Is it accurate, consistent, and competent?"
- Built through: accurate information, consistent performance, demonstrated professional knowledge, logical reasoning, factual correctness
- Fragile under: errors, contradictions, hallucinations, inconsistency across turns

**Affective Trust**:
- Grounded in emotional connection, interpersonal warmth, and relationship quality
- The consumer evaluates: "Do I feel comfortable with this digital human? Does it seem to care about me?"
- Built through: empathetic responses, conversational warmth, personalization, active listening signals, relationship-building behaviors
- Fragile under: cold/robotic responses, ignored emotional cues, dismissive language

### 3.2 Differentiated Effects of Conversational Features
A central claim of the framework: different conversational intelligence features exert **differentiated effects** on each trust dimension. Features are not monolithic — they must be mapped to the specific trust dimension they primarily influence.

| Conversational Feature | Primary Trust Dimension | Mechanism |
|---|---|---|
| Information accuracy / relevance | Cognitive | Signals competence and reliability |
| Professional knowledge depth | Cognitive | Demonstrates capability |
| Contextual awareness | Cognitive | Competence signal — remembers and uses context |
| Consistency across turns | Cognitive + Affective | Reliability (cognitive) + dependability (affective) |
| Empathetic responses | Affective | Emotional connection, perceived care |
| Conversational warmth / tone | Affective | Interpersonal warmth, likability |
| Response speed / responsiveness | Affective | Perceived attentiveness, engagement |
| Personalization | Affective | Relationship quality, feeling known |

Both dimensions are needed to drive purchase intention — cognitive trust alone may produce respect without willingness to engage; affective trust alone may produce liking without confidence to purchase.

---

## 4. Dynamic Trust Processing Framework (Novel Contribution)

### 4.1 Limitation of Prior Approaches
Previous trust research has used static, single-turn measurement paradigms — asking "how much do you trust this agent?" once, after one interaction. This misses the dynamic, evolving nature of trust in real conversational commerce.

### 4.2 Process-Tracing Paradigm
The framework adopts a **process-tracing paradigm**: trust is measured and modeled continuously across the full sequence of conversational turns.

Key properties:
- **Multi-turn**: Trust is assessed at each exchange, not just at the end
- **Bidirectional**: Each party's responses influence the other's trust state; trust is co-constructed
- **Continuous calibration**: Trust is continuously adjusted up or down based on each conversational exchange
- **Event-driven**: Specific exchanges (an error, a moment of empathy, a contradiction) trigger trust recalibration events
- **Stabilization**: After sufficient interaction, trust levels tend to stabilize around an equilibrium — but can be disrupted by negative events

### 4.3 Bayesian Decision Modeling
Trust levels are modeled using **Bayesian decision modeling**:
- Prior trust state is updated with each new conversational observation
- Each exchange provides evidence that shifts the posterior trust distribution
- A psychological process coding model is constructed to represent trust level trajectories
- This allows both prediction (forecasting future trust) and diagnosis (identifying which exchanges caused trust shifts)

### 4.4 Trust Trajectory Phases
The framework implies trust evolves through identifiable phases over a conversation:
1. **Initial formation**: Early exchanges establish baseline trust from priors (brand reputation, digital human appearance, first response quality)
2. **Active calibration**: Middle exchanges continuously adjust trust up/down based on feature performance
3. **Stabilization**: Trust converges to an equilibrium reflecting accumulated evidence
4. **Disruption susceptibility**: Stabilized trust can still be disrupted by salient negative events (error, contradiction, emotional misstep)

---

## 5. Neural Mechanisms (fMRI)

### 5.1 Brain Imaging Approach
The framework proposes using fMRI to characterize the neural mechanisms of dual trust in marketing digital human contexts.

### 5.2 Disentangling Dual Trust at the Neural Level
- Cognitive and affective trust are disentangled via brain imaging — not just self-report
- Assess differences in **activation intensity** between cognitive and affective trust conditions
- Distinguish **effective functional pathways** — which brain networks are engaged for each trust dimension
- A **brain network model of dual trust** is constructed, mapping cognitive and affective trust to distinct (but interacting) neural systems

### 5.3 Implications
- Provides neural validation that cognitive and affective trust are genuinely distinct constructs, not just two labels for the same thing
- Identifies biomarkers that could theoretically be used to monitor trust formation in real time
- Supports the dual-trust decoupling claim (Challenge 3, below) with physiological evidence

---

## 6. Predictive Model

### 6.1 Multi-Input Prediction Architecture
The framework proposes a deep learning predictive model for consumer behavior in marketing digital human conversational contexts.

**Inputs**:
- Neural data (fMRI-derived features)
- Behavioral data (interaction logs, response patterns, conversation metrics)
- Historical consumption data (past purchases, preferences, browsing history)

**Outputs**:
- Trust levels (cognitive and affective, over time)
- Purchase intention

### 6.2 Architecture: CNN + LSTM
- **CNN (Convolutional Neural Network)**: extracts spatial patterns from neural and behavioral feature representations
- **LSTM (Long Short-Term Memory)**: captures temporal/sequential patterns across the multi-turn conversation — models how trust evolves over time
- The combination enables both feature extraction (CNN) and temporal dynamics (LSTM)

### 6.3 Applications
- Predict whether a given conversation trajectory will lead to purchase
- Identify which conversational turns are trust-critical (high-leverage moments)
- Diagnose trust failures — pinpoint the exchange that caused trust collapse
- Optimize digital human response strategies to maximize trust calibration

---

## 7. Three Key Challenges

### 7.1 Multidimensional Conversational Intelligence Complexity
Conversational intelligence is not a single trait — it comprises multiple features (accuracy, empathy, warmth, consistency, responsiveness, contextual awareness, personalization) that interact with each other. Improving one feature may help one trust dimension while hurting another (e.g., more empathetic language may reduce perceived professionalism). The combinatorial space of feature configurations is large.

### 7.2 Multi-Turn Interaction Pattern Dynamics
Trust trajectories depend on the *sequence* of exchanges, not just their aggregate content. The same set of responses delivered in different orders may produce different trust outcomes. This temporal dependency makes prediction harder and requires sequential models (LSTM), not bag-of-features approaches.

### 7.3 Dual-Trust Effects Decoupling
Cognitive and affective trust are **distinct but interacting**:
- They can move independently (a highly competent but cold response raises cognitive trust while lowering affective trust)
- They can reinforce each other (a warm, accurate response raises both)
- They can substitute for each other partially (high affective trust may buffer against a minor cognitive trust dip)
- They must be measured separately — collapsing them into a single "trust score" loses critical information
- Decoupling them enables targeted intervention: fix the specific trust dimension that is deficient

---

## 8. Practical Design Implications

### 8.1 For Digital Human System Designers
- Instrument both trust dimensions separately in evaluation, not a single trust score
- Map each conversational feature to the trust dimension it primarily drives
- Design response strategies that attend to both dimensions — competence-focused responses for cognitive trust, warmth-focused responses for affective trust
- Monitor trust trajectories over multi-turn conversations, not just endpoint satisfaction

### 8.2 For Trust Calibration
- Treat trust as a dynamic state to be actively managed across the conversation
- Identify high-leverage turns where trust is most likely to shift (early exchanges, error recovery moments, emotional disclosure moments)
- Use Bayesian updating to track trust state and trigger interventions when trust dips

### 8.3 For Evaluation and Research
- Use process-tracing measures (trust at each turn) not just endpoint measures
- Where feasible, use neural data to validate that cognitive and affective trust are genuinely distinct constructs
- Use CNN+LSTM architectures to model the temporal dynamics of trust and predict purchase intention

---

## 9. Key Terms Glossary

| Term | Definition |
|---|---|
| Marketing Digital Human | LLM-powered conversational agent designed for sales/marketing scenarios; new-generation HCI interface |
| Cognitive Trust | Trust based on competence, reliability, capability assessment |
| Affective Trust | Trust based on emotional connection, warmth, relationship quality |
| Dual Trust | The combined cognitive + affective trust framework; two distinct but interacting dimensions |
| Dynamic Trust Calibration | Continuous adjustment of trust across multi-turn interactions |
| Process-Tracing Paradigm | Measuring trust at each step of a process, not just endpoints |
| Bayesian Decision Modeling | Probabilistic updating of trust state based on each new conversational observation |
| Conversational Intelligence Features | Multidimensional capabilities of the digital human (accuracy, empathy, warmth, consistency, responsiveness, contextual awareness) |
| Trust Stabilization | Convergence of trust to equilibrium after sufficient interaction |
| Dual-Trust Decoupling | The principle that cognitive and affective trust are distinct and can move independently |
| Brain Network Model of Dual Trust | fMRI-derived mapping of cognitive and affective trust to distinct neural systems |

---

## 10. Source

Pei, L., Dong, Y., Jin, X., Meng, Y., & Zhang, W. (2026). Research on consumer behavior and neural mechanisms of dual trust in marketing digital humans. *Advances in Psychological Science* (心理科学进展), 34(2), 227–238. Authors affiliated with Zhejiang Lab, Shanghai International Studies University, and Zhejiang University.