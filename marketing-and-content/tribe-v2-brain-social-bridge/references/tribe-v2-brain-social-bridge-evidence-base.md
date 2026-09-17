# TRIBE v2 Brain–Social Bridge Evidence Base

## Primary Source

**Musayev, O., Bhatia, P., Karamnchandi, V., Kedia, A., Dua, P., & Shenai, R. (2026). "Adneural: Brain-Grounded Campaign Intelligence." GitHub: OmarMusayev/Adneural. MIT License. Built at Purdue Catapult Hackathon.**

### Platform Overview
Adneural is a three-engine platform for pre-launch advertising analysis:
1. **Brain Encoding**: Meta TRIBE v2 predicts cortical brain response to any video/audio/image.
2. **Video Summarizer**: LLM with frame extraction generates scene-by-scene descriptions.
3. **Social Simulation**: MiroFish/OASIS models 200 personality-diverse AI agents reacting and interacting.
4. **Diagnostic Agents**: LangGraph + Neo4j GraphRAG over 22 neuroscience papers analyses brain + social + video data.

### Novel Contribution: The Seeding Bridge
The bridge between brain encoding and social simulation:
1. **Network aggregation**: 148 cortical parcels → 8 functional brain networks.
2. **Emotional computation**: network activations → 9D emotional state vector (anxiety, trust, excitement, discomfort, memorability, etc.).
3. **Personality modulation**: base emotional state × Big Five personality traits → individualised response.
   - Neurotic viewer → anxiety
   - Extraverted viewer → curiosity
   - Same brain data, different reactions.
4. **Population distribution**: 200 agents across 6 archetypes (enthusiastic sharer, empathetic worrier, skeptical analyst, hostile critic, average viewer, emotional storyteller).

### TRIBE v2 (Meta FAIR)
- Open-source: github.com/facebookresearch/tribev2
- Trained on 450 hours of fMRI data.
- Predicts cortical surface activation (~20,484 vertices) from any media stimulus.
- Outputs Destrieux atlas (aparc.a2009s) parcellation into 148 cortical regions.

### Subcortical Inference (Honest Limitation)
- TRIBE v2 outputs cortical surface data only.
- Subcortical structures (amygdala, hippocampus, nucleus accumbens) are inferred from cortical proxy patterns with explicit confidence tagging, not directly measured.
- This limitation is documented honestly throughout the codebase and reports.

### Knowledge Graph (GraphRAG)
- Neo4j + sentence-transformers.
- 22 neuroscience papers, 2,684 findings, 9,921 relationships.
- Papers include: Corbetta & Shulman (2002) attention networks; Kanwisher et al. (1997) face processing; Klucharev et al. (2008) persuasion and memory; Genevsky et al. (2025) neuroforecasting.

### Reports
Four audience-specific reports from the same analysis:
| Report | Audience | Focus |
|---|---|---|
| Executive | C-suite | Go/no-go with risk dashboard |
| Marketing | Creative team | Timestamped edit recommendations with neural rationale |
| Compliance | Legal/ethics | Mental health impact, regulatory flags |
| Full Research | Technical | Complete neural-social bridge analysis with paper citations |

### Guardrail
- No manipulation: the platform is diagnostic, not persuasive.
- The system analyses; it does not influence.

### Case Study: Jaguar "Copy Nothing" Rebrand (Retrospective)
Ran the Jaguar rebrand ad through Adneural without knowing the real-world outcome:

| Adneural Predicted | What Actually Happened |
|---|---|
| Weak emotional tagging, low memorability | Positive sentiment collapsed 23% → 8% |
| No narrative processing (pure visual stimulus) | "Where are the cars?" became the meme |
| Narrative vacuum vulnerable to hostile reframing | The void got filled with ridicule |
| Minimal trust formation | No brand equity built; CEO resigned |

**Launch recommendation: NO GO** (predicted blind, validated by real outcome).

### Technology Stack
| Component | Technology |
|---|---|
| Brain encoding | Meta TRIBE v2 |
| Video summarizer | LLM + ffmpeg frame extraction |
| Atlas mapping | Destrieux (aparc.a2009s) |
| Knowledge graph | Neo4j + sentence-transformers |
| Emotional bridge | Custom Python |
| Social simulation | MiroFish/OASIS |
| Diagnostic agents | LangGraph |
| LLM backend | OpenRouter (Qwen 3) |
| Frontend | Next.js 15 / React 19 / Three.js |

### Scalability
- Global advertising spend: $1 trillion/year.
- Neuromarketing market: $1.8 billion (0.2% of ad spend).
- A single fMRI study: $50-200K, 6 months, 20-30 people.
- Adneural: near-zero marginal cost per ad analysis (TRIBE v2 inference is seconds).

## Cross-References

- `in-silico-neuromarketing-platform-pattern` — TRIBE v2 open-source platform; this skill is the social-simulation extension.
- `concept2brain-predictive-neural-response-model` — CLIP-to-EEG synthesis; complementary neural-response prediction (EEG vs fMRI).
- `multimodal-eeg-eye-tracking-consumer-choice` — EEG+ET consumer choice; this skill adds the social-cascade layer.
- `forward-prediction-neuromarketing-framework` — direction-of-inference and vendor evaluation; Adneural is a forward-prediction platform.
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad; Adneural implements all three layers.
- `neuro-rights-data-sovereignty-monetization` — neuro-rights; Adneural's guardrail (no manipulation) aligns.
- `machine-mediated-market-strategy` — machine-mediated marketing; Adneural is a machine-mediated pre-launch tool.

## A-Tech Value Alignment

- **Open-source AI**: MIT-licensed; TRIBE v2 is open-source (Meta FAIR); OASIS/CAMEL-AI is open-source; Neo4j community edition is open-source.
- **Data privacy**: no human subjects needed (in-silico); no biometric data collected; privacy-by-design.
- **Financial freedom**: eliminates $50-200K fMRI study costs; near-zero marginal cost per analysis; SMEs can access pre-launch neuromarketing.
- **Practical implementation**: Docker + Python 3.10+; reproducible pipeline; TRIBE v2 inference in seconds; GraphRAG build is first-time-only.