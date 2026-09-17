# Be.FM — Open Behavioral Foundation Model: Evidence Base

## Source

Xie, J., Li, C., Wang, S., et al. (May 28, 2025). "Be.FM: An Open Foundation Model for Human Behavior." arXiv:2505.23058.

---

## 1. Model Architecture & Training

### Base Models
- **Llama 3.1 8B** (Meta, open weights)
- **Llama 3.1 70B** (Meta, open weights)

### Fine-Tuning Method
- **LoRA (Low-Rank Adaptation)** applied to **all layers** of the base models
- Fine-tuning framework: **LlamaFactory**
- Prompt template: **Alpaca**

### Training Configuration

| Parameter | Value |
|---|---|
| Fine-tuning method | LoRA (all layers) |
| Training framework | LlamaFactory |
| Prompt template | Alpaca |
| Epochs | 3 |
| Precision | bf16 |
| Learning rate | 1e-4 |
| LR scheduler | Cosine |

### Resulting Model Variants
- **Be.FM-8B** — Llama 3.1 8B + behavioral LoRA adapters
- **Be.FM-70B** — Llama 3.1 70B + behavioral LoRA adapters

Models are available on request from the authors.

---

## 2. Training Data — Four Categories

### Category 1: Literature Data
- **Source:** American Economic Review (AER) publications
- **Volume:** 2,703 papers
- **Tokens:** 3.1 million tokens of behavioral-science text
- **Purpose:** Encodes behavioral knowledge `K` — published empirical regularities, theoretical frameworks, and experimental findings from economics

### Category 2: Experimental Data
- **Source:** MobLab economic-game platform (incentivized, real-stakes experiments)
- **Subjects:** 68,780
- **Observations:** 82,057
- **Games:** 5 economic games (see Section 3 below)
- **Purpose:** Teaches the model `F` to map `(x, c) → y` — subject characteristics and game context to behavioral choice distributions

### Category 3: Survey Data
- **Source:** Big Five personality inventory responses
- **Subjects:** 17,667
- **Purpose:** Enables `x → traits` inference (demographics to personality) and provides trait labels for simulation conditioning

### Category 4: Observational Data
- **Status:** Planned for future incorporation
- **Purpose:** Real-world behavioral traces (not yet in the current model)

---

## 3. Economic Games (Experimental Corpus)

### Game 1: Dictator Game
- **Setup:** An allocator (dictator) receives an endowment and decides how much (if any) to send to a passive recipient.
- **Behavioral construct:** Prosocial preference, altruism, fairness
- **Typical finding:** Modal offers of 0% or 50%; mean offer ~20–30% of endowment

### Game 2: Ultimatum Game
- **Setup:** Proposer offers a split of an endowment. Responder accepts (both get the proposed split) or rejects (both get nothing).
- **Behavioral construct:** Fairness norms, strategic reasoning, punishment of unfair offers
- **Typical finding:** Offers around 40–50%; low offers (<20%) frequently rejected

### Game 3: Trust Game
- **Setup:** Sender receives an endowment and sends some amount to a receiver. The sent amount is multiplied (often ×3). Receiver decides how much to return.
- **Behavioral construct:** Trust, trustworthiness, reciprocity
- **Typical finding:** Senders send ~50% on average; receivers return slightly less than sent

### Game 4: Public Goods Game
- **Setup:** Each player privately contributes to a public pool. The pool is multiplied (e.g., ×2) and split equally among all players regardless of contribution.
- **Behavioral construct:** Cooperation, free-riding, conditional cooperation
- **Typical finding:** Contributions start ~40–60% and decay over rounds

### Game 5: Bomb Risk Elicitation Task (BRET)
- **Setup:** Player collects boxes one at a time. Each collected box adds money. One box contains a bomb that destroys all collected earnings. Player decides when to stop.
- **Behavioral construct:** Risk attitudes, risk aversion
- **Typical finding:** Average collection of ~33–50% of safe boxes; substantial heterogeneity in risk preferences

---

## 4. Experimental Results

### 4.1 Behavior Prediction & Simulation (Wasserstein Distance)

The Wasserstein distance (earth-mover's distance) measures the difference between the predicted choice distribution and the empirical human choice distribution. **Lower is better.** Be.FM variants are compared against their base Llama 3.1 counterparts and other LLMs.

**Models compared (7 total):**
1. Llama 3.1 8B (base)
2. Be.FM-8B
3. Llama 3.1 70B (base)
4. Be.FM-70B
5. GPT-4o
6. Claude 3.5 Sonnet
7. GPT-4o-mini

**Wasserstein distance across 5 economic games:**

| Model | Dictator | Ultimatum | Trust | Public Goods | Bomb |
|---|---|---|---|---|---|
| Llama 3.1 8B (base) | — | — | — | — | — |
| **Be.FM-8B** | ↓ improved | ↓ improved | ↓ improved | ↓ improved | ↓ improved |
| Llama 3.1 70B (base) | — | — | — | — | — |
| **Be.FM-70B** | ↓ improved | ↓ improved | ↓ improved | ↓ improved | ↓ improved |
| GPT-4o | (no advantage over Be.FM) | | | | |
| Claude 3.5 Sonnet | (comparable or worse) | | | | |
| GPT-4o-mini | (worse) | | | | |

**Key finding:** Be.FM-8B and Be.FM-70B both **improve Wasserstein distance over their base Llama counterparts across all 5 games**. The fine-tuning shifts the models toward human-like behavioral distributions.

**Headline result:** **GPT-4o fails to outperform the smaller Be.FM models** on these behavioral tasks despite having hundreds of times more parameters. This demonstrates that task-specific behavioral fine-tuning matters more than raw scale for behavioral prediction.

> **Note:** The original paper reports full numeric Wasserstein values per game per model. The above table summarizes the directional finding (improvement over base) that holds across all five games. For exact numeric values, consult Table 1 in arXiv:2505.23058.

### 4.2 Subject Characteristic Inference (Big Five from Demographics)

**Task:** Given a subject's demographic profile (age, gender, income, education), predict their Big Five personality trait scores (Openness, Conscientiousness, Extraversion, Agreeableness, Neuroticism) on a 1–100 scale.

**Metrics:**

| Metric | Value | Interpretation |
|---|---|---|
| **MAE** (Mean Absolute Error) | 7.27 | On average, predictions are ~7 points off on the 100-point scale |
| **Spearman ρ** | 0.101–0.128 | Weak but statistically significant rank correlation with true trait scores |

**Interpretation:** Personality prediction from demographics alone is inherently difficult — demographics explain only a small fraction of personality variance. The MAE of 7.27 and Spearman of 0.10–0.13 confirm a weak but real signal. This is useful for **coarse segmentation or ranking**, not individual-level clinical assessment.

### 4.3 Demographics Inference (Reverse Direction)

Be.FM can also infer demographics from behavioral signatures, though this direction is less developed in the current paper. The model demonstrates bidirectional reasoning between `x` (characteristics) and `y` (behavior), consistent with the `y = F(K, x, c)` framework.

### 4.4 Contextual Insight Generation (Dictator Game Treatments)

**Task:** Ask Be.FM to suggest novel treatment conditions for the Dictator Game that would be interesting to test experimentally.

**Result:** Be.FM generated treatment suggestions that **aligned with published behavioral-economics literature** — the model proposed variations grounded in known behavioral mechanisms (e.g., social-distance framing, identifiability of recipient, endowment-source manipulation). This demonstrates that the literature-data fine-tuning (2,703 AER papers, 3.1M tokens) gives the model usable knowledge of the experimental design space.

### 4.5 Applying Behavioral Knowledge (IEO Contest)

**Task:** Individualized Educational Outcomes (IEO) contest — predict which educational intervention will improve outcomes for a specific student.

**Accuracy results:**

| Model | Base Accuracy | Be.FM Accuracy | Gain |
|---|---|---|---|
| Llama 3.1 8B | 48.4% | 51.3% | **+2.9 pp** |
| Llama 3.1 70B | 68.8% | 73.3% | **+4.5 pp** |

**Key finding:** The 70B variant shows a larger gain from behavioral fine-tuning (+4.5 pp) than the 8B variant (+2.9 pp), suggesting that larger base models benefit more from the behavioral knowledge injection — the richer representation space can better absorb and apply the fine-tuning signal.

### 4.6 Research Workflow Quality (BLEURT / ROUGE)

Be.FM was also evaluated on research-workflow tasks (generating research summaries, experimental designs, literature-grounded hypotheses). Quality was assessed using:

- **BLEURT** (learned metric for text generation quality)
- **ROUGE** (overlap-based metric for summarization)

Be.FM-generated research artifacts scored competitively against base-LLM outputs, with improvements attributable to the literature-data fine-tuning providing better grounding in behavioral-science conventions and terminology.

> **Note:** Exact BLEURT and ROUGE numeric scores are reported in the paper's research-workflow evaluation section. The directional finding is that Be.FM produces more literature-grounded and conventionally-formatted research artifacts than base Llama models.

---

## 5. The GPT-4o Comparison in Detail

The most striking finding for the open-source community:

| Dimension | GPT-4o | Be.FM-8B / Be.FM-70B |
|---|---|---|
| Parameter count | ~1.8 trillion (est.) | 8B / 70B |
| Open weights | No (API only) | Yes (Llama-based, available on request) |
| Behavioral fine-tuning | None (general-purpose) | LoRA on literature + experiments + surveys |
| Economic-game Wasserstein | Does not outperform Be.FM | Improved over base across all 5 games |
| IEO contest | Not reported as superior | 51.3% (8B) / 73.3% (70B) |
| Cost to run | Per-token API fee | Self-hosted (8B on consumer GPU) |
| Data privacy | Data sent to OpenAI | Runs locally, no data leaves your infrastructure |

**Implication:** For behavioral-science applications, a task-specific open-weight model can match or exceed a general-purpose frontier model that is orders of magnitude larger. This is a strong argument for **specialized open foundation models** over generic proprietary APIs in domain-specific workflows.

---

## 6. The Be.FM Framework (Formal)

```
y = F(K, x, c)
```

| Symbol | Definition | Role in Training |
|---|---|---|
| **x** | Subject characteristics (demographics, traits, history) | Input — from survey + experimental data |
| **c** | Context (game rules, framing, incentives, environment) | Input — from experimental data |
| **K** | Behavioral knowledge (literature, empirical regularities, theory) | Encoded via literature-data fine-tuning (2,703 AER papers) |
| **F** | Latent behavioral function | Learned via LoRA fine-tuning on all layers |
| **y** | Behavioral choice / distribution of choices | Output — evaluated via Wasserstein distance to empirical data |

The framework separates **knowledge** (K, what the literature says about behavior) from the **function** (F, how to apply that knowledge to a specific person in a specific context). This separation is what enables Be.FM to:
1. **Simulate** new scenarios (novel `c`) using accumulated `K`
2. **Infer** characteristics (reverse `x → y` to `y → x`)
3. **Generate** insights (propose new `c` variants grounded in `K`)
4. **Apply** knowledge (use `K` to improve prediction accuracy)

---

## 7. Limitations

1. **Modest personality inference:** MAE 7.27 and Spearman 0.10–0.13 mean Big Five prediction from demographics is a weak signal — useful for segmentation, not individual diagnosis.
2. **Literature scope:** Training literature is drawn from AER (economics). Behavioral findings from psychology, marketing, or HCI journals may be underrepresented.
3. **No observational data yet:** The observational data category is planned but not incorporated, limiting real-world longitudinal prediction.
4. **Five games only:** Experimental data covers 5 economic games. Generalization to non-economic behavioral domains (health behavior, environmental behavior, political behavior) is untested.
5. **Model availability:** Models are "available on request" rather than published as a direct download — not fully open-weight in the HuggingFace sense, though the base (Llama 3.1) is open-weight and the fine-tuning recipe is fully documented.
6. **GPT-4o comparison scope:** The finding that GPT-4o doesn't outperform Be.FM is specific to the behavioral tasks tested (economic games, IEO). GPT-4o will outperform on general reasoning, coding, and knowledge tasks outside the behavioral domain.

---

## 8. Practical Implementation Notes

### Running Be.FM

| Variant | Hardware | Notes |
|---|---|---|
| Be.FM-8B | Single consumer GPU (e.g., 16–24 GB VRAM) | LoRA adapters load on top of Llama 3.1 8B base |
| Be.FM-70B | Multi-GPU or cloud instance (e.g., 2× A100 80GB) | LoRA adapters load on top of Llama 3.1 70B base |

### Reproducing the Fine-Tuning

Using the documented configuration (LlamaFactory, Alpaca template, LoRA all layers, 3 epochs, bf16, lr 1e-4 cosine), a practitioner with the training data could reproduce or extend Be.FM on new behavioral data — e.g., domain-specific survey or experimental data from marketing, health, or finance.

### Evaluation Protocol

When evaluating Be.FM (or any behavioral simulation model):
1. **Use Wasserstein distance**, not point-estimate accuracy — behavioral heterogeneity is the signal
2. Compare against the **base Llama model** (without LoRA) to isolate the fine-tuning effect
3. Benchmark against **GPT-4o / Claude** to test whether open specialized models match frontier general models
4. Evaluate on **held-out games or contexts** to test generalization, not just interpolation

---

## 9. A-Tech Application Framework

### When to Use Be.FM

| Scenario | Be.FM Role |
|---|---|
| Designing a nudge / intervention | Simulate expected choice distribution before fielding |
| Pricing or messaging A/B test design | Predict distribution of responses to narrow the variant space |
| Behavioral experiment design | Generate literature-grounded treatment suggestions |
| Lightweight personalization | Infer Big Five from signup demographics for segmentation |
| Behavioral research prototyping | Simulate game outcomes to calibrate expectations and pre-register hypotheses |

### Step-by-Step Implementation

1. **Frame the question** in `(x, c)` terms — who are the subjects, what is the context?
2. **Select model variant** — 8B for speed/iteration, 70B for fidelity
3. **Prompt for a distribution** — request the full choice distribution, not a single answer
4. **Evaluate with Wasserstein** — compare predicted vs. empirical (if available) distributions
5. **Infer characteristics** (optional) — Big Five from demographics for segmentation
6. **Generate treatment suggestions** — ask Be.FM for novel experimental conditions
7. **Validate with a real sample** — Be.FM informs design; empirical confirmation is still required

### A-Tech Alignment

- **Open-source AI:** Built on Llama 3.1 (open weights); fine-tuning recipe fully documented; models available on request
- **Data privacy:** Behavioral prediction without centralized data collection; infer traits from demographics; simulate populations without surveilling individuals; runs locally
- **Financial freedom:** Democratizes behavioral-science tooling; solo researchers and small teams can prototype experiments that previously required university labs and thousands of subjects
- **Practical implementation:** Benchmarks provided (Wasserstein, MAE, Spearman, IEO accuracy); concrete training config; runs on consumer GPU (8B) or multi-GPU (70B)

---

## Cross-References

- `scaled-behavioral-measurement-targeting` — complements Be.FM by grounding simulations in incentivized measurement
- `llm-iterative-personalized-nudging` — LLM-based personalization that Be.FM could power as the behavioral-simulation engine
- `nudge-effectiveness-reality-check` — evidence on nudge heterogeneity that Be.FM's simulations should respect
- `cross-cultural-llm-personalized-nudge-design` — cultural adaptation layer for Be.FM simulations
- `digital-nudging-ethical-persuasion` — ethical framing for deploying behavioral predictions
- `behavioral-design-practical-playbooks` — intervention patterns Be.FM can help select and parameterize

---

*Evidence base compiled from arXiv:2505.23058 (Xie, Li, Wang et al., May 2025) | A-Tech Research Division*