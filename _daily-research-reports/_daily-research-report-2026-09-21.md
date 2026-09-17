# Daily Research Report — 2026-09-21

**Date:** September 21, 2026 (Auckland time)
**Research Focus:** Open-weight royalty licensing, self-proposed task RL, assistant-to-agent brownfield onboarding, AI-generated content labeling effects, empathy-mediated marketing content engagement, AI character design with neurophysiological measurement, developer-agent misalignment, open-source AI monetization, privacy-preserving federated learning

---

## Executive Summary

Today's research cycle identified **six high-impact novel developments** across the A-Tech domains, resulting in **six new skills** created across two sub-cycles. The most significant findings include: (1) the OMLA open-weight royalty license — the first model-level licensing framework that compensates upstream creators through recursive lineage splits while keeping weights open; (2) Ornith-1.5's self-proposed task RL method — the model generates its own training tasks, eliminating the human-written task bottleneck but introducing a solvability-optimization failure mode; (3) the first controlled comparison of assistants vs agents during brownfield onboarding — 61.7% faster but shifts developers from active collaboration to passive supervision; (4) the first systematic study of AI model labels as emphasis-framing cues that reduce consumer self-expression for symbolic products; (5) the AI disclosure paradox — disclosure amplifies cognitive empathy for functional content while attenuating affective empathy for hedonic content; (6) the first integrated fNIRS + eye-tracking study for AI character design.

---

## Skills Created (6 new skills across two sub-cycles)

### Sub-cycle 1: Earlier today (3 skills)

#### 1. AI Model Label Framing Effect on Consumer Self-Expression
**Category:** marketing-and-content
**Source:** Cheng & Nam (Chung-Ang University, Frontiers in Psychology, July 2026)

**Key Findings:**
- Labeling a product display model as "AI-generated" functions as an emphasis-framing cue that triggers eeriness
- Serial mediation: AI Label → Eeriness → Perceived Psychological Risk → Reduced Self-Expression (CI: -0.874, -0.299)
- **Critical boundary condition:** ALL negative effects emerge ONLY for symbolic products (dress); functional products (T-shirt) show NO significant differences
- For symbolic products: self-expression drops from 4.37 (human) to 1.97 (AI), p<.001
- 2×2 between-subjects design, N=200, PROCESS Model 81 serial mediation

**A-Tech Alignment:** Open-source (open-weight image models), data privacy (behavioral proxies), financial freedom (SME cost reduction), practical implementation (clear product-type decision rule)

#### 2. Value-Dependent Empathy-Mediated AIGMC with AI Disclosure Paradox
**Category:** marketing-and-content
**Source:** Gao, Li & Zhao (Harbin University of Commerce, Frontiers in Psychology, January 2026)

**Key Findings:**
- First framework demonstrating AI disclosure has a paradoxical dual role depending on content value type
- **AI disclosure paradox:** Disclosure AMPLIFIES cognitive empathy for functional content (β=+0.306, p<.01) while ATTENUATING affective empathy for hedonic content (β=-0.245, p<.01)
- Two experiments (N=152, N=186), ELM + CAPS framework, PROCESS Model 7

#### 3. AI-Generated Character fNIRS + Eye-Tracking Design
**Category:** cognitive-science-and-ux
**Source:** Cha, Lee & Kim (Sejong/Hongik University, Frontiers in Human Neuroscience, April 2026)

**Key Findings:**
- First integrated fNIRS + eye-tracking study of AI-generated character design
- 24 participants, 18 stimuli (6 age conditions × 3 image types)
- Pupil diameter (emotional arousal) + DLPFC HbO (cognitive engagement) reveal distinct processing modes

### Sub-cycle 2: Current cycle (3 new skills)

#### 4. OMLA Open-Weight Royalty License
**Category:** monetization-and-revenue
**Source:** OMLA (Open Model License Agreement), omla-ai.org, 2026

**Key Findings:**
- First model-level royalty license for open-weight AI: 30% of commercial revenue (or run cost, whichever is greater), self-assessed
- **Recursive lineage split:** fine-tunes retain at most 5%, remainder flows upstream through signed manifests with payment pointers
- **Privacy architecture:** OMLA keeps NO records of usage, payers, or payments — commercial users self-meter and pay creators directly
- Four-step workflow: creators publish & sign → OMLA serves registry → users meter & resolve → users pay wallets directly
- License comparison: OMLA vs Apache 2.0 vs MIT vs RSI vs Qwen3.8-Max custom
- **Decision framework:** Choose Apache 2.0 for maximum adoption; OMLA for direct creator compensation with maximum privacy

**A-Tech Alignment:** Open-source (keeps weights open while enabling compensation), data privacy (no central usage database — strongest privacy design among monetization models), financial freedom (recursive splits mean small upstream contributors get paid), practical implementation (four-step workflow, signed manifest format)

#### 5. Ornith Self-Proposed Task RL
**Category:** ai-agents-and-workflows
**Source:** Ornith-1.5 (DeepReinforce), August 19, 2026, MIT license, 397B/35B/9B sizes

**Key Findings:**
- Post-training method where the model generates its own training tasks (scaffold + solution rollouts) — eliminates the human-written task bottleneck
- **Core insight:** The bottleneck for RL post-training is environment scaling, not model scaling
- Task reward = validity × novelty × difficulty (targets 0.2 success rate for optimal challenge)
- Reward hacking defense: verifiers synthesized without reference solution, three-check validation (oracle/no-op/unsolved-state)
- Benchmarks: 86.1 Terminal-Bench 2.1, 86.0 SWE-bench Verified (397B)
- **Critical caveat:** Model trained on self-proposed tasks optimizes for solvability not real-world impact — trails on Frontier-Bench (13.5 vs Opus 21.1) and NL2Repo (59.5 vs 69.7)
- All sizes MIT license; 35B MoE (3B active) delivers near-flagship coding on affordable hardware

**A-Tech Alignment:** Open-source (MIT all sizes), data privacy (self-hostable, GGUF builds for local deployment), financial freedom (35B on affordable hardware eliminates API costs), practical implementation (concrete RL pipeline with reward decomposition)

#### 6. Assistant-to-Agent Brownfield Onboarding
**Category:** developer-experience-and-flow
**Source:** Appelt & Glauben (Technical University Darmstadt, PACIS 2026, N=24 developers)

**Key Findings:**
- First controlled comparison of IDE-integrated assistant (Copilot Ask) vs LLM-based agent (Copilot Agent) during brownfield onboarding
- **Headline:** Agent reduced task completion time by 61.7% and NASA-TLX workload by 57.4%, but code correctness did NOT improve
- **Core finding:** Shift from active collaboration to passive supervision — raising over-reliance and skill-erosion concerns
- SPACE framework measurement across five dimensions
- **Brownfield-specific risk:** Agents may prevent mental model formation for new hires — creating comprehension debt that surfaces during debugging
- Interaction pattern taxonomy: active collaboration → guided generation → supervised generation → passive supervision → full autonomy
- Five guardrails: maintain writing requirement, comprehension checkpoints, progressive agent autonomy, alternate agent/assistant use, skill maintenance protocol

**A-Tech Alignment:** Open-source (findings apply to Cline/OpenCode agents), data privacy (on-device models like Muse Glimmer enable privacy-preserving agent use), financial freedom (61.7% time savings reduces onboarding cost), practical implementation (guardrails immediately implementable)

---

## Cross-Domain Synthesis

### Theme 1: The Open-Weight Monetization Spectrum Is Expanding

Today's research identified three distinct points on the open-weight monetization spectrum that were not previously covered:

- **Apache 2.0 (free commercial use):** Maximum adoption, zero direct creator compensation (existing coverage: Mistral, Qwen3.8-27B)
- **OMLA (30% royalty, recursive lineage):** Direct creator compensation, maximum privacy, self-metered (NEW)
- **Qwen3.8-Max custom (revenue share at scale):** Triggered at $50M MAU, via Alibaba contract (existing coverage)

OMLA fills the gap between "free for everyone" (Apache 2.0) and "pay the platform at scale" (Qwen3.8-Max). It is the first model-level mechanism where the model creators — not a platform — receive direct payment from commercial users.

### Theme 2: Self-Generation Is the 2026 Post-Training Frontier

Ornith-1.5's self-proposed task RL and GLM-5.3's environment-scaling approach represent two solutions to the same bottleneck: human-written task environments do not scale. Both use automated verifier synthesis and shortcut-closing, but differ on task source:

- **GLM-5.3:** Research agents collect task patterns from real professional work (more realistic, less scalable, human-in-the-loop)
- **Ornith-1.5:** Model generates its own tasks (more scalable, less grounded, minimal human involvement)

The Ornith failure mode (optimizing for solvability rather than real-world impact) is the key lesson: self-generation trades grounding for scale.

### Theme 3: The Productivity-Agency Tradeoff Is Now Measured

The Appelt & Glauben study is the first to quantify the full tradeoff: 61.7% productivity gain, 57.4% workload reduction, but a qualitative shift from active collaboration to passive supervision with no correctness improvement. This connects to a growing evidence base:

- **(Im)Paired Programming:** 28% lower comprehension with agents (Balepour et al.)
- **Cognitive engagement decline:** Engagement drops across task phases (Catalan et al.)
- **Comprehension debt framework:** Agent users score lower on code comprehension (multiple studies)

The pattern is consistent: agents deliver speed and reduced effort, but the shift from writing to supervising risks comprehension and skill retention. The guardrails (progressive autonomy, comprehension checkpoints, alternating use) are the practical response.

---

## Incremental Updates (Not New Skills)

### Open-Weight Model Wave (Reinforces Existing Skill)
**Source:** Multiple (Miraflow Qwen3.8-27B explainer, DataNorth Ornith-1.5 coverage, GLM-5.3 blog)

The August 2026 open-weight model wave continues to generate analysis. Ornith-1.5 (MIT, self-proposed task RL) is a genuine new contribution not covered by the existing `open-weight-agentic-model-wave-august-2026` skill, which focused on GLM-5.3, Muse Glimmer, Qwen3.8-27B, DeepSeek-V4-Pro, and Nemotron 3.5 Lightning. Ornith-1.5's self-proposed task RL method is now captured in the new `ornith-self-proposed-task-rl` skill.

### Developer-Agent Misalignment at Scale (Reinforces Existing Skills)
**Source:** Tang et al. (Notre Dame/Vanderbilt/Google, arXiv:2605.29442, May 2026)

Reinforces existing skills: `coding-agent-misalignment-large-scale`, `agentic-cognitive-engagement-decline`, `developer-agent-misalignment-taxonomy`. The Appelt & Glauben study adds the brownfield onboarding context and the interaction pattern shift analysis.

### Open-Source AI Monetization Trends (Reinforces Existing Skills)
**Source:** Malpani (Give-Away/Keep Matrix, comprehensive playbook); ShareAI (open-source AI monetization); OMLA (new royalty model)

Reinforces existing skills: `give-away-keep-matrix-oss-ai`, `open-source-ai-revenue-share-trend`, `open-source-ai-monetization-mastery-2026`. OMLA is a genuinely new licensing mechanism captured in the new `omla-open-weight-royalty-license` skill.

### Privacy-Preserving Federated Learning (Reinforces Existing Skills)
**Source:** Multiple new DP-FL methods

Incremental to the extensive existing privacy-and-trust skills. Notable patterns: adaptive clipping and layer-wise noise injection continue to outperform uniform approaches; post-processing invariance remains the key mechanism for zero-additional-privacy-loss cryptographic composition.

---

## Research Methodology

Today's research used web search across eight domains:
1. Open source AI model releases and weights (August 2026)
2. Neuromarketing and AI behavioral psychology research (2026)
3. Open source AI monetization and business models
4. Privacy-preserving AI and federated learning
5. Developer experience and AI coding agents
6. AI-generated content and consumer response
7. Neurophysiological measurement for design
8. Open-weight licensing and royalty frameworks

Searches returned 40+ web results across 4 queries. Results were evaluated for:
- **Novelty**: Does this represent a genuinely new development not covered by existing 310+ skills?
- **Open Source Focus**: Alignment with A-Tech's open-source ethos
- **Audience Interest**: Relevance to A-Tech's developer/tech audience
- **Depth Potential**: Ability to sustain well-researched skill content
- **Practical Implementation**: Actionable frameworks, not just theoretical observations

Six developments met all novelty criteria and were synthesized into Agent Skills with proper YAML frontmatter, evidence-base references, and A-Tech alignment analysis. Additional findings were classified as incremental updates to existing skills.

---

## Statistics

| Metric | Value |
|--------|-------|
| Skills created (total today) | 6 |
| Skills created (this sub-cycle) | 3 |
| Reference files created (this sub-cycle) | 3 |
| Categories touched (this sub-cycle) | 3 (monetization-and-revenue, ai-agents-and-workflows, developer-experience-and-flow) |
| Total SKILL.md lines (this sub-cycle) | ~450 |
| Total reference lines (this sub-cycle) | ~700 |
| Research sources evaluated | 40+ web results across 4 queries |
| Skills directory total | ~311+ skills across 9 categories |

---

## A-Tech Values Alignment

All 6 new skills align with A-Tech Corporation's core values:

- **Open-source AI**: OMLA (keeps weights open while enabling compensation), Ornith (MIT all sizes), Assistant-to-Agent (findings apply to open-source agents Cline/OpenCode), AI Model Label (open-weight image models), Value-Dependent Empathy (open-weight models), AI Character (NIRSIT Quest, standard libraries)
- **Data privacy**: OMLA (no central usage database — strongest privacy design), Ornith (self-hostable, GGUF builds), Assistant-to-Agent (on-device models enable privacy-preserving agent use), all three marketing skills (behavioral proxies or controlled measurement)
- **Financial freedom**: OMLA (recursive splits mean small upstream contributors get paid), Ornith (35B on affordable hardware eliminates API costs), Assistant-to-Agent (61.7% time savings reduces onboarding cost), all three marketing skills (SME cost reduction)
- **Practical implementation**: OMLA (four-step workflow, signed manifest format), Ornith (concrete RL pipeline with reward decomposition), Assistant-to-Agent (five guardrails immediately implementable), all three marketing skills (clear decision rules)

---

## Report Date
2026-09-21