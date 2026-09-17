# Human Behavior Atlas & OmniSapiens 2.0 — Evidence Base

## Sources
- **Human Behavior Atlas (HBA):** Ong, Dai, Li et al., MIT. "Human Behavior Atlas: A Large-Scale Benchmark for Unified Psychological and Social Behavior Understanding." ICLR 2026.
- **OmniSapiens 2.0:** "OmniSapiens 2.0: A Foundation Model for General Social Behavior Processing." ICML 2026.
- **Code:** GitHub — `MIT-MI/human_behavior_atlas` (MIT license implied by academic release)
- **Models:** HuggingFace (Apache 2.0)
- **Benchmark data:** HuggingFace (parquet + tar versions)

---

## 1. Released Models — Comparison Table

All six models are released on HuggingFace under Apache 2.0.

| # | Model | HF ID (namespace) | Description | Stage | Use Case |
|---|---|---|---|---|---|
| 1 | **OmniSapiens-7B-RL** | `MIT-MI/OmniSapiens-7B-RL` | RL-refined model trained with HARPO reasoning-RL on top of SFT backbone | RL (HARPO) | Researching reasoning-RL effects; ablation of HARPO contribution |
| 2 | **OmniSapiens SFT** | `MIT-MI/OmniSapiens-SFT` | Supervised fine-tuned model after the full 3-stage SFT pipeline (classification → QA → BAM) | SFT (post Stage 3) | General-purpose behavioral inference; base for BAM adapters |
| 3 | **BAM — humour adapter** | `MIT-MI/OmniSapiens-BAM-humour` | Lightweight per-dataset residual adapter for humor detection | BAM (Stage 3 adapter) | Plug into OmniSapiens SFT for humor-specific tasks |
| 4 | **BAM — sentiment adapter** | `MIT-MI/OmniSapiens-BAM-sentiment` | Lightweight per-dataset residual adapter for sentiment understanding | BAM (Stage 3 adapter) | Plug into OmniSapiens SFT for sentiment-specific tasks |
| 5 | **BAM — sarcasm adapter** | `MIT-MI/OmniSapiens-BAM-sarcasm` | Lightweight per-dataset residual adapter for sarcasm detection | BAM (Stage 3 adapter) | Plug into OmniSapiens SFT for sarcasm-specific tasks |
| 6 | **OmniSapiens 2.0** | `MIT-MI/OmniSapiens-2.0` | Full SOTA model: SFT (3-stage) + BAM + HARPO RL | Full pipeline | Best cross-domain performance; production use |

> **Note:** The HuggingFace namespace `MIT-MI` reflects the MIT Media Lab / MIT initiative origin. Confirm exact repo IDs at the GitHub README (`MIT-MI/human_behavior_atlas`) before programmatic download, as naming may evolve.

---

## 2. Training Pipeline Details

### Base Architecture
- **Backbone:** Qwen2.5-Omni-7B — a natively multimodal model supporting text, audio, image, and video inputs in a single architecture
- **Why Qwen2.5-Omni-7B:** Native multimodality avoids bolt-on encoders; the same backbone processes all HBA modality signatures without architectural changes

### Training Framework
- **VERL (Volcano Engine Reinforcement Learning):** Used for the RL stage (HARPO). VERL provides scalable RL infrastructure for LLM training.
- **VERL version:** v0.5.0.dev (development pre-release at time of training)
- **FlashAttention:** Built from source for optimized attention computation on the multimodal inputs
- **vLLM:** v0.10.2 with audio support — used for efficient inference and roll-out generation during RL training

### Three-Stage SFT Pipeline

#### Stage 1: Classification Training
- Per-task classification heads attached to the Qwen2.5-Omni-7B backbone
- Each HBA domain (emotion, sentiment, humor/sarcasm, intent, non-verbal, mental health) gets a dedicated head
- **Goal:** Teach the backbone domain-specific behavioral discrimination features
- Backbone weights are updated during this stage

#### Stage 2: QA Training
- Backbone is **frozen** — only the `lm_head` (language modeling head) is trained
- Converts the classification knowledge from Stage 1 into natural-language reasoning and question-answering format
- **Goal:** Enable the model to articulate behavioral reasoning in text, not just produce classification labels
- This decoupling preserves the behavioral features learned in Stage 1 while teaching the model to express them

#### Stage 3: BAM (Behavioral Adapter Module)
- Lightweight **per-dataset residual adapters** are trained on top of the frozen backbone
- Each adapter specializes in one dataset/domain (e.g., humour, sentiment, sarcasm)
- **Goal:** Enable cheap, fast specialization without full model retraining
- Multiple BAM adapters can be swapped in/out at inference time depending on the target domain

### RL Stage: HARPO
- Applied after the 3-stage SFT pipeline
- Uses VERL v0.5.0.dev infrastructure for scalable RL training
- Roll-outs generated with vLLM 0.10.2 (audio support enabled)

---

## 3. Benchmark Dataset Structure (HBA)

### Format
- **File format:** JSONL (JSON Lines) splits — one JSON object per line
- **Distribution:** HuggingFace, available in both **parquet** and **tar** versions
  - Parquet: efficient columnar storage for tabular/structured access
  - Tar: efficient for bulk media (audio/video/image) download

### Per-Sample Fields

| Field | Type | Description |
|---|---|---|
| `problem` | string | The behavioral task prompt/question |
| `answer` | string (or label) | Ground-truth answer or classification |
| `images` | list[ref] | Image file references (may be empty) |
| `videos` | list[ref] | Video file references (may be empty) |
| `audios` | list[ref] | Audio file references (may be empty) |
| `texts` | list[string] | Text transcripts or supplementary text |
| `modality` (signature) | string | Modality signature, e.g. `text_audio`, `video`, `text_video` |

### Modality Signatures
Each sample carries a modality signature indicating which input modalities are present and required:
- `text_audio` — text + audio (e.g., spoken sentiment with transcript)
- `video` — video only (e.g., non-verbal communication from body language)
- `text_video` — text + video (e.g., humor detection from video + subtitles)
- Other combinations as applicable per domain

This design enables:
- **Modality-conditional evaluation** — subset the benchmark by available modalities
- **Fair comparison** — models are evaluated only on modalities they support
- **Missing-modality robustness** — test how models degrade when modalities are dropped

### Pre-Extracted Features
To enable reproducible evaluation without re-running feature extractors, HBA includes pre-extracted features:
- **Pose features:** body pose estimates (for non-verbal communication domain)
- **OpenSMILE audio features:** standardized acoustic feature set (e.g., Geneva Minimalistic Acoustic Feature Set, emobase, etc.) — widely used in speech emotion recognition

Pre-extracted features allow researchers to benchmark the reasoning layer without depending on specific feature extractor versions.

### Behavioral Domains Covered

| Domain | Description | Example Modalities |
|---|---|---|
| Emotion recognition | Identifying emotional states from multimodal signals | text_audio, video, text_video |
| Sentiment understanding | Polarity and nuanced sentiment beyond positive/negative | text_audio, text |
| Humor/sarcasm detection | Detecting humor and sarcasm (hardest domain; cross-modal cues) | text_audio, text_video |
| Intent recognition | Inferring speaker/actor goals | text_audio, video |
| Non-verbal communication | Pose, gesture, facial expression, prosody | video |
| Mental health indicators | Behavioral markers relevant to wellbeing | text_audio, text |

---

## 4. BAM (Behavioral Adapter Module) Architecture

### Concept
BAM is a **lightweight per-dataset residual adapter** that augments the frozen OmniSapiens SFT backbone for a specific behavioral domain.

### Design Principles
- **Residual:** BAM adds a residual signal to the backbone's representations rather than modifying backbone weights — the backbone stays frozen
- **Per-dataset:** Each adapter is trained on one HBA dataset/domain, producing specialized behavioral features
- **Lightweight:** Far fewer parameters than the backbone; training is cheap and fast
- **Composable:** Multiple BAM adapters can be loaded and swapped at inference time

### Architecture (Conceptual)
```
Input (multimodal)
    │
    ▼
[Frozen Qwen2.5-Omni-7B Backbone]  ──►  backbone_representation
    │                                       │
    │                                       ▼
    └──► [BAM Adapter (humour|sentiment|sarcasm|...)]
                    │
                    ▼
            adapter_residual  (lightweight transform)
                    │
                    ▼
    output = backbone_representation + adapter_residual
                    │
                    ▼
            [lm_head]  ──►  prediction / reasoning
```

### Why BAM?
- **Decoupling:** Broad behavioral knowledge (backbone) is separated from dataset-specific specialization (adapter)
- **Efficiency:** New domains require only a small adapter, not full retraining
- **Modularity:** Deploy only the adapters you need; swap at runtime
- **Released adapters:** humour, sentiment, sarcasm (on HuggingFace); new adapters can be trained for custom domains

---

## 5. HARPO Algorithm — Heterogeneity-Aware Relative Policy Optimization

### Problem It Solves
Standard RLHF assumes relatively homogeneous reward signals across tasks. But HBA's behavioral domains are highly heterogeneous:
- **Difficulty varies massively:** sentiment (easier) vs. sarcasm/humor (much harder)
- **Label structure differs:** some domains are multi-class, others are binary or ordinal
- **Modality availability differs:** some samples have text+audio, others have video only

Under standard RLHF, the model tends to over-optimize easy tasks (where reward is dense and achievable) and under-invest in hard tasks (where reward is sparse and noisy).

### HARPO Approach
HARPO (**Heterogeneity-Aware Relative Policy Optimization**) is a reasoning-RL algorithm that:

1. **Accounts for task heterogeneity** during reward shaping — rewards are normalized relative to the difficulty/structure of each behavioral domain
2. **Uses relative policy optimization** — compares the policy's performance relative to domain-appropriate baselines rather than applying a uniform reward scale
3. **Balances easy vs. hard tasks** — prevents reward hacking on easy domains at the expense of hard ones (e.g., sarcasm detection where standard RLHF often fails to improve)

### Position in the Pipeline
```
Stage 1: Classification SFT  →  Stage 2: QA SFT (frozen backbone)  →  Stage 3: BAM adapters
    ↓
[RL Stage: HARPO on top of SFT + BAM]  →  OmniSapiens 2.0 (final SOTA)
```

HARPO is applied as the final refinement step, producing `OmniSapiens-7B-RL` and, in combination with BAM, the full `OmniSapiens 2.0`.

### Key Insight
The heterogeneity-aware design is specifically motivated by the observation that behavioral AI's hardest problems (sarcasm, humor, nuanced intent) are exactly where standard RLHF provides the weakest training signal. HARPO re-weights the optimization landscape to push improvement where it matters most.

---

## 6. Software Stack Summary

| Component | Version / Detail | Purpose |
|---|---|---|
| Qwen2.5-Omni-7B | Base backbone | Natively multimodal (text/audio/image/video) |
| VERL | v0.5.0.dev | RL training infrastructure (HARPO stage) |
| FlashAttention | Built from source | Optimized attention for multimodal inputs |
| vLLM | 0.10.2 (audio support) | Efficient inference & RL roll-out generation |
| HuggingFace | — | Model + benchmark data distribution (parquet + tar) |
| GitHub | `MIT-MI/human_behavior_atlas` | Code repository |

---

## 7. Open-Source & Reproducibility

| Resource | License / Access | Location |
|---|---|---|
| OmniSapiens models (all 6) | Apache 2.0 | HuggingFace |
| HBA benchmark data | Public (parquet + tar) | HuggingFace |
| Code | Open source | GitHub `MIT-MI/human_behavior_atlas` |
| Pre-extracted features | Included in benchmark | HuggingFace |

This makes HBA + OmniSapiens 2.0 one of the most reproducible behavioral AI releases: models, data, features, and code are all openly available.

---

## 8. A-Tech Value Alignment

| A-Tech Value | Alignment Detail |
|---|---|
| **Open-source AI** | Apache 2.0 models, public benchmark, open code on GitHub — fully reproducible |
| **Data privacy** | 7B open-weight model enables on-device behavioral processing; no cloud dependency for sensitive audio/video/text behavioral data |
| **Practical implementation** | VERL framework integration, vLLM inference, HuggingFace deployment, BAM adapters for cheap domain customization |

---

## 9. Limitations & Open Questions

- **Research-grade:** Not validated for clinical mental health diagnosis; HBA mental health indicators are behavioral markers, not diagnostic tools
- **Cultural context:** Sarcasm and humor are highly culture-dependent; HBA coverage of cultural variation is limited
- **HARPO novelty:** HARPO is a newly proposed algorithm; limited independent replication at time of writing — treat SOTA claims with appropriate caution
- **Feature extraction dependency:** Pre-extracted features (pose, OpenSMILE) are dataset-specific; new data requires running the same extractors for fair comparison
- **Domain coverage:** HBA covers six domains — not exhaustive of human social behavior (e.g., deception detection, persuasion strategy, social dominance not explicitly covered)
- **Ethical use:** Social behavior models can be misused for surveillance, manipulation, or emotional exploitation — deployment requires ethical guardrails

---

## 10. Adjacent Skills

- `concept2brain-predictive-neural-response-model` — predicts neural responses to stimuli; OmniSapiens reads behavioral signals, Concept2Brain predicts neural responses — complementary layers of behavioral AI
- `ai-neuromarketing-synergy-framework` — emotion-attention-memory triad; OmniSapiens provides the emotion recognition engine
- `cognitive-load` / `cognitive-load-reduction-ai-scaffolding` — mental health indicators in HBA overlap with cognitive load research
- `neuroadaptive-attention-engineering` — adaptive systems using behavioral signals; OmniSapiens is a candidate recognition engine
- `embodied-ai-interface-design` — non-verbal communication domain in HBA directly informs embodied AI design