# Forward-Prediction Neuromarketing — Evidence Base

## Source

OpenAffect Insights — "What is neuromarketing in 2026? (And why most of what you have read is wrong)" (2026). Fetched full text. The OpenAffect framing: neuromarketing is one signal of four (neural, linguistic, cultural, historical); anyone predicting with one family hits a ceiling; anyone who integrates all four crosses it.

## A Short History of Neuromarketing

- **McClure et al. (Neuron 2004)** — Participants in fMRI scanner shown Coke and Pepsi, with and without brand cues. Medial prefrontal cortex engaged when brand visible; ventromedial prefrontal cortex correlated with blind preference. Finding was real. What followed was hype.
- **Martin Lindstrom, Buyology (2008)** — Popularized "buy button in the brain" framing. Reviewers (Roger Dooley, Sentient Decision Science) published detailed takedowns for missing controls and reverse-inference leaps. Book sold regardless.
- **A.K. Pradeep / NeuroFocus (founded 2005, sold to Nielsen 2011)** — Enterprise validation phase.
- **Innerscope Research (MIT Media Lab spinout, Carl Marci & Brian Levine, founded 2006, acquired by Nielsen May 2015)** — Formed Nielsen Consumer Neuroscience unit.
- **2020 consolidation** — Nielsen shut 17 ex-US Consumer Neuroscience labs, cut ~80% of headcount. Unit absorbed into NielsenIQ without formal shutdown announcement. The collapse was a slow unwind of claims the category had never calibrated in public.

## The Reverse Inference Problem (Poldrack)

- **Poldrack (Trends in Cognitive Sciences 2006)** — Four-page paper that should have ended half the neuromarketing industry's sales pitch. Cannot infer a cognitive process from a regional activation without knowing how specific that region is to that process. Formally: P(process | activation) depends on the selectivity of the region. mPFC and insula activate across hundreds of cognitive tasks. "mPFC lit up, therefore emotional engagement with the brand" is a Bayesian mistake.
- **Poldrack (Neuron 2011)** — Formalized the correction: forward prediction (stimulus → neural response) routes around the problem. Reverse inference (neural response → cognitive state) does not.

## What the Evidence Actually Says About Legacy Neuromarketing

- **Venkatraman et al. (JMR 2015)** — Tested neural and self-report measures against market-level advertising outcomes across 30 TV ads. Ventral striatum fMRI signal uniquely predicted sales, incremental R² ~0.10–0.14 over traditional measures. Real effect, strongest single piece of evidence for neural data in advertising research.
- **Varan, Lang, Barwise, Weber, Bellman (JAR 2015)** — ARF Neuro 1 and Neuro 2 comparison studies. Opaque vendor constructs, weak inter-vendor agreement. Different vendors scoring the same ads produced different rank orders. Field had not earned the accuracy claims in its sales decks.
- **Ariely & Berns (Nature Reviews Neuroscience 2010)** — Balanced starting point; named the hope and the hype; critique largely held up.

## The New Category: Forward Encoding Models

- **TRIBE v2 (Meta FAIR, 2026)** — Brain predictive foundation model. Predicts fMRI BOLD response across ~70,000 cortical voxels from video input, trained on 1,000+ hours of fMRI. First production-grade, open-sourced foundation model for predicting brain response to video. Not a decoder, not a neuromarketing product — the start of commodity behavioral prediction infrastructure. Forward direction (stimulus → brain), falsifiable on held-out content.
- **MindEye / MindEye2 (Scotti et al., ICML 2024)** — Decode images from within-subject fMRI. MindEye2: shared-subject models enable fMRI-to-image with one hour of data.
- **Huth Lab semantic maps (Tang, LeBel, Jain, Huth — Nature Neuroscience 2023)** — Semantic reconstruction of continuous language from non-invasive brain recordings. Distributed semantic tuning across cortex.

## Notable Exception: System1 Group

System1 Group (UK, LSE:SYS1, founded by John Kearon) quietly outperformed the EEG-cap vendors by focusing on emotion and attention with minimal neuro-dressing. The playbook that worked was not the one with brain imaging — it was rigorous consumer response measurement with transparent methodology. Clue about what actually generalizes.

## The Four Questions for a Sophisticated Buyer (2026)

1. Do you publish calibration studies against field outcomes like sales, CTR, or retention? If not, why not.
2. Are your claims forward-prediction or reverse-inference in structure? Can you articulate the difference.
3. What happens to your model on out-of-distribution creative? Short-form vertical video. Non-English language. Novel formats.
4. Can you show correlation to market outcomes, not just correlation to self-report.

## Validated vs Hype Technique Matrix

| Technique | What it measures | Peer-reviewed validity | Cost | Recommended use |
|---|---|---|---|---|
| fMRI | Localized brain activity (blood flow) | High in lab, low in field | €€€€ | Academic research, R&D |
| EEG | Cortical electrical activity (ms) | Medium-high (with interpretive limits) | €€€ | Video/ad pre-testing |
| Eye-tracking | Visual fixation, saccades | High (for UX/packaging), low (as purchase predictor) | €€ | UX, landing pages, shelf |
| GSR | Autonomic arousal (sweating) | Medium (measures arousal, not valence) | €€ | Complement to EEG/eye-tracking |
| Facial coding | Facial expressions → inferred emotions | Low (Barrett 2019 dismantles generalization) | €€ | Auxiliary data, never oracle |
| IAT | Implicit associations (reaction times) | Controversial (weak replicability on brand preference) | € | Exploratory research |
| Forward encoding models | Predicted neural response from stimulus | Emerging — falsifiable on held-out content | TBD | Infrastructure layer |

## Corroborating Sources

- **Deep Marketing — "Neuromarketing Worth $3.8B in 2026: What Works, What's Hype" (May 2026)** — Market size $3.7–4.0B (Research and Markets $3.71B; Morgan Reed Insights $3.99B). Lisa Feldman Barrett (Psychological Science in the Public Interest, 2019) review of 1,000+ studies: facial expressions are not reliable indicators of internal emotional states. Byron Sharp / Ehrenberg-Bass: brand growth comes from mental availability and physical availability, not "mind reading." Documented sales increases of 5–20% in FMCG categories from eye-tracking on adequate samples (n>40).
- **Alsharif et al. (Future Business Journal, Springer, July 2025)** — Systematic literature review of neuromarketing + AI synergy (2013–2023, Scopus). Emotion, attention, memory as vital constructs. AI algorithms analyze neural/physiological datasets. Ethical concerns: privacy, data security, consent. Emphasis on transparency and responsible neural data use.
- **Sarin et al. (Cogent Business & Management, 2026)** — Dual-method (PRISMA 2020 + VOSviewer bibliometric) review. Maps intellectual, thematic, methodological development from lab settings to technology-enabled scalable practices. Future directions: ethical governance, AI integration, practical applications.
- **AiDatalizer — "Neuromarketing in 2026: The Practical Guide for Brands" (Jan 2026)** — The neuromarketing operating model (Attention → Emotion → Memory → Trust → Action → Measurement). Core brain/behavior principles table. 90-day roadmap.

## Grep-Confirmation of Novelty

Grep across `/home/user/.skills` for "forward encoding", "direction of inference", "TRIBE", "MindEye", "OpenAffect", "Poldrack" returned no matches. The direction-of-inference distinction and the forward-encoding infrastructure category are not captured by any existing skill. Adjacent skills cover different angles:
- `neuromarketing-research-landscape-2026` — market landscape (no direction-of-inference distinction)
- `neuromarketing-2026-practical-operating-model` — operating model (no methodological foundation)
- `neuromarketing-market-evidence-2026` — market evidence (no vendor-evaluation framework)
- `neuromarketing-predictive-purchase-intent-model` — predictive model (reverse-inference EEG approach, not forward-encoding)
- `dark-psychology-neuromarketing-autonomy-defense` — autonomy defense (no forward-prediction alternative)
- `neuro-marketing-privacy-first-behavioral-analytics` — privacy-first analytics (no methodological basis for why forward-prediction is valid)

This skill provides the methodological foundation (forward encoding routes around Poldrack; reverse inference does not) that sits above all of them.