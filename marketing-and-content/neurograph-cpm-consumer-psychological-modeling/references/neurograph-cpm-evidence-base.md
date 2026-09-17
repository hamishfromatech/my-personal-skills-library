# NeuroGraph-CPM Evidence Base

## Primary Source

**Gao, F. (2026). "Consumer Psychological Modeling and Behavior Prediction System Based on Graph Neural Network." *International Journal of Computational Intelligence Systems*, 19, 24. DOI: 10.1007/s44196-025-01093-y. Published December 14, 2025.**

---

## 1. Study Design

### Dataset
- **Amazon Electronics Reviews** dataset (publicly available).
- User-product interaction records with associated review text, ratings, and metadata.
- **Train / Validation / Test split: 70 / 15 / 15.**

### Implementation
- **Framework**: PyTorch Geometric (open-source GNN library).
- **Hardware**: NVIDIA GPU, 32 GB VRAM.
- **Embedding dimension**: 128.
- **GNN depth**: 3 layers.
- **Optimizer**: Lookahead + SAM (Sharpness-Aware Minimization).
- **Loss**: Binary Cross-Entropy (BCE) + KL-divergence attention regularization.

### Psychological Signal Extraction
- **Sentiment polarity**: extracted from review text via NLP sentiment analysis.
- **Trust score**: derived from reviewer helpfulness votes, rating consistency, and review depth.
- **Engagement intensity**: computed from interaction frequency, session depth, and review length.
- **Cognitive load estimate**: approximated from review complexity (readability scores, question density, feature comparison depth).
- All signals normalized to [0, 1] and concatenated into multidimensional edge feature vectors.

---

## 2. Architecture Details

### 2.1 Heterogeneous Graph Definition

The model operates on a heterogeneous graph **G = (V, E, X)**:

| Component | Specification |
|---|---|
| **Node types (V)** | `user`, `product`, `context` |
| **Edge types (E)** | `view`, `click`, `purchase`, `review` |
| **Node features (X)** | 128-dimensional learned embeddings per node type |
| **Edge features** | Multidimensional vector per edge: `e_{ij} = [interaction_type ∥ affective_signal ∥ context_signal]` |

**Edge feature vector construction:**

```
e_{ij} = [ one_hot(type_{ij}) ∥ affect_{ij} ∥ ctx_{ij} ]
```

Where:
- `one_hot(type_{ij})` ∈ {view, click, purchase, review} — 4-dim interaction type encoding
- `affect_{ij}` = [sentiment, trust, engagement, cognitive_load] — 4-dim affective embedding
- `ctx_{ij}` = temporal, categorical, and session-level contextual features
- Final edge feature vector dimensionality: 4 (interaction) + 4 (affective) + |ctx| (context)

### 2.2 Affect-Aware Message Passing

Psychological edge features are integrated into the GNN message-passing step so that affective signals modulate information propagation:

```
h_i^{(l+1)} = σ( Σ_{j ∈ N(i)} α_{ij} · W^{(l)} · [ h_j^{(l)} ∥ e_{ij}^{affect} ] )
```

Where:
- `h_i^{(l)}` is the hidden representation of node `i` at layer `l`
- `α_{ij}` is the attention weight from neighbor `j` to node `i`
- `W^{(l)}` is the learnable weight matrix at layer `l`
- `e_{ij}^{affect}` = [sentiment, trust, engagement, cognitive_load] — the affective portion of the edge feature
- `σ` is a non-linear activation (ReLU / GELU)
- The concatenation `[ h_j ∥ e_{ij}^{affect} ]` ensures that the message from `j` to `i` is conditioned on the psychological context of their interaction

**Attention weight computation:**

```
α_{ij} = softmax_j( LeakyReLU( a^T [ W_q h_i ∥ W_k h_j ∥ W_e e_{ij}^{affect} ] ) )
```

Where `a` is a learned attention vector and `W_q`, `W_k`, `W_e` are learned projection matrices for query, key, and edge features respectively.

**Depth**: 3 GNN layers, enabling psychological signals to propagate across 2-hop neighborhoods.

### 2.3 Psychologically Regularized Attention

The attention distribution is constrained via **KL-divergence** to align with a psychological prior `q_psych`:

```
L_attention = KL( p_attention ∥ q_psych ) = Σ_i Σ_{j ∈ N(i)} p_{ij} · log( p_{ij} / q_{ij}^{psych} )
```

Where:
- `p_{ij}` = learned attention weight from `j` to `i`
- `q_{ij}^{psych}` = psychological prior encoding domain knowledge:
  - `purchase` edges receive higher prior weight than `view` edges
  - High-trust reviews receive higher prior weight than low-trust reviews
  - High-engagement interactions receive higher prior weight than low-engagement
  - The prior is constructed per-edge-type with affective signal modulation

This regularization:
- Prevents attention from drifting toward spurious or non-psychological correlations
- Grounds model explanations in psychologically meaningful patterns
- Improves interpretability without sacrificing accuracy

### 2.4 Behavior Prediction Decoder

The decoder fuses structural, behavioral, and affective features:

```
f_fused = [ h_user^{(L)} ∥ h_product^{(L)} ∥ ( h_user^{(L)} ⊙ h_product^{(L)} ) ]

ŷ = sigmoid( MLP( f_fused ) )
```

Where:
- `h_user^{(L)}` and `h_product^{(L)}` are the final-layer GNN embeddings (128-dim each)
- `∥` denotes concatenation
- `⊙` denotes the **Hadamard (element-wise) product** — captures multiplicative user-product compatibility
- `MLP` is a multi-layer perceptron with hidden layers and ReLU activation
- `sigmoid` produces a probability in [0, 1] for the target behavior (purchase / click / engagement)

The three fused streams:
1. **Structural features**: `h_user` and `h_product` — graph-derived representations capturing neighborhood structure
2. **Behavioral features**: aggregated interaction history statistics (frequency, recency, type distribution)
3. **Affective features**: aggregated psychological edge features along user-product interaction paths

### 2.5 Total Loss Function

```
L_total = L_BCE(ŷ, y) + λ · L_attention

L_BCE = -[ y · log(ŷ) + (1 - y) · log(1 - ŷ) ]

L_attention = KL( p_attention ∥ q_psych )
```

Where:
- `L_BCE` is the binary cross-entropy loss for behavior prediction
- `L_attention` is the KL-divergence attention regularization
- `λ` is a hyperparameter controlling the strength of psychological regularization

### 2.6 Optimization Strategy: Lookahead + SAM

| Component | Mechanism | Benefit |
|---|---|---|
| **Lookahead** | Maintains "fast" (inner loop) and "slow" (outer loop) weight copies; periodically synchronizes fast weights toward slow weights | Stabilizes training on noisy, heterogeneous graph data; reduces oscillation |
| **SAM (Sharpness-Aware Minimization)** | Minimizes both loss value and loss sharpness (gradient of loss w.r.t. parameter perturbation) | Finds flatter minima → better generalization; reduces overfitting on graph topology noise |
| **Lookahead + SAM (combined)** | SAM applied to fast weights, Lookahead synchronizes to smoothed slow weights | Best AUC, fastest convergence |

**Results (optimizer ablation):**

| Optimizer | AUC | Epochs to Convergence | Notes |
|---|---|---|---|
| Adam | Baseline | Baseline | Standard optimizer |
| SAM | +0.9% over Adam | Similar | Flatter minima, better generalization |
| Lookahead + SAM | **+1.3% over Adam, +0.9% over SAM** | **15% fewer epochs** | Best overall; stable + efficient |

---

## 3. Evaluation Metrics (8 Metrics)

| # | Metric | What It Measures |
|---|---|---|
| 1 | **Accuracy** | Overall classification correctness (purchase vs. non-purchase) |
| 2 | **AUC** | Area under ROC curve — ranking quality across thresholds |
| 3 | **Precision@K** | Fraction of top-K recommended items that are relevant |
| 4 | **Recall@K** | Fraction of relevant items retrieved in top-K |
| 5 | **F1@K** | Harmonic mean of Precision@K and Recall@K |
| 6 | **CTR@K** | Estimated click-through rate for top-K recommendations |
| 7 | **Diversity Index** | Intra-list diversity of recommended items (catalog coverage) |
| 8 | **Personalization Relevance** | Degree to which recommendations are personalized to individual user preferences vs. popular items |

K is evaluated at K = 5, 10, 15, 20, 25.

---

## 4. Results

### 4.1 Accuracy Comparison Across Input Configurations

NeuroGraph-CPM was tested with varying input feature configurations to isolate the contribution of psychological signals:

| Input Configuration | Accuracy |
|---|---|
| Structural only (node features, no edge affect) | Baseline |
| Structural + interaction type | Moderate improvement |
| Structural + interaction + affective (full) | **Highest (+19.6% over baselines)** |

**Key finding**: The affective edge features (sentiment, trust, engagement, cognitive load) contribute the largest accuracy gain — removing them collapses performance toward baseline GNN behavior.

### 4.2 AUC Comparison

| Model | AUC |
|---|---|
| NeuroGraph-CPM (full) | **Highest** |
| HAN (Heterogeneous Attention Network) | Lower |
| R-GCN (Relational GCN) | Lower |
| GAT (single-type graph) | Lowerest |

NeuroGraph-CPM's AUC advantage is driven by both the affective edge features and the psychologically regularized attention.

### 4.3 Precision@K (K = 5–25)

| K | NeuroGraph-CPM | Best Baseline | Improvement |
|---|---|---|---|
| 5 | Highest | — | Significant |
| 10 | Highest | — | Significant |
| 15 | Highest | — | Sustained |
| 20 | Highest | — | Sustained |
| 25 | Highest | — | Sustained |

Precision advantage is maintained across all K values, indicating robust ranking quality, not just top-1 accuracy.

### 4.4 Recall@K

NeuroGraph-CPM achieves the highest Recall@K across all K values, confirming that the model retrieves a greater fraction of relevant items in top-K recommendations than baselines.

### 4.5 F1@K

F1@K (harmonic mean of Precision@K and Recall@K) is highest for NeuroGraph-CPM across all K values, confirming that the precision and recall improvements are balanced rather than traded off.

### 4.6 CTR@K

| Metric | NeuroGraph-CPM | Best Baseline | Improvement |
|---|---|---|---|
| **CTR@K** | Highest | — | **+7.1%** max over best baseline |
| **Overall CTR estimation** | Highest | — | **+16.3%** |

The CTR improvement demonstrates that the psychological edge features translate to practically meaningful click-through prediction gains, not just academic metric improvements.

### 4.7 Diversity Index

| Metric | NeuroGraph-CPM | Best Baseline | Improvement |
|---|---|---|---|
| **Diversity Index** | Highest | — | **+12.1%** |

The model recommends a more diverse set of products than baselines, avoiding the "popularity bubble" where recommenders converge on a few popular items.

### 4.8 Personalization Relevance

| Metric | NeuroGraph-CPM | Best Baseline | Improvement |
|---|---|---|---|
| **Personalization Relevance (overall)** | Highest | — | **+21.8%** |
| **Personalization Relevance (ranked, @K)** | Highest | — | **+9.4%** |

The largest improvement across all metrics — NeuroGraph-CPM excels at tailoring recommendations to individual user preferences rather than recommending globally popular items. This is the core value proposition of the psychological modeling approach.

---

## 5. Interpretability Analysis

### 5.1 Attention Heatmaps

NeuroGraph-CPM produces attention heatmaps over the user-product interaction graph, showing which neighbors (products, reviews, context nodes) receive the highest attention weight for a given prediction. These heatmaps are:

- **Psychologically grounded**: high-attention edges correspond to high-trust reviews, strong sentiment, or high-engagement interactions — not arbitrary structural proximity
- **Per-prediction**: each recommendation can be explained by surfacing the top-attended neighbors and their affective features
- **Visualizable**: rendered as node-link diagrams with edge thickness ∝ attention weight and edge color ∝ affective signal type

### 5.2 Case Study: User U123

A detailed case study of user **U123** demonstrates the interpretability pipeline:

1. **Prediction**: NeuroGraph-CPM predicts U123 will purchase Product P456 with probability 0.87
2. **Top attended neighbors**: the model surfaces the 5 highest-attention edges in U123's subgraph
3. **Explanation**: "U123 previously reviewed P456's category with high trust (0.91) and positive sentiment (0.78); 3 of the top-5 attended neighbors are products U123 purchased with high engagement"
4. **Faithfulness check**: removing the top-5 attended neighbors and re-running prediction drops the probability from 0.87 to 0.71 (18% reduction) — confirming the attention is genuinely predictive

### 5.3 Faithfulness: Neighbor Removal Test

| Operation | Prediction Impact |
|---|---|
| Remove top-1 attended neighbor | Moderate reduction |
| Remove top-3 attended neighbors | Significant reduction |
| Remove top-5 attended neighbors | **18% reduction** in prediction confidence |
| Remove random 5 neighbors | Minimal reduction (control) |

**Key finding**: Removing the *top attended* neighbors causes an 18% prediction drop, while removing *random* neighbors causes negligible change. This confirms the attention mechanism is faithful — it focuses on genuinely predictive interactions, not noise.

### 5.4 Sufficiency and Comprehensiveness Curves

- **Sufficiency**: using only the top-k attended features maintains high prediction accuracy even as k decreases — the most-attended features are sufficient for prediction
- **Comprehensiveness**: removing the top-k attended features causes prediction accuracy to drop sharply — the attended features are comprehensive (necessary for prediction)
- Both curves confirm that the attention mechanism identifies the causally relevant features, not just correlated ones.

### 5.5 Attention Entropy

| Model | Attention Entropy |
|---|---|
| **NeuroGraph-CPM** | **0.73** |
| HAN baseline | 1.12 |
| R-GCN baseline | 1.35 |
| GAT baseline | 1.28 |

**Key finding**: NeuroGraph-CPM has the lowest attention entropy (0.73), meaning its attention is **focused and selective** — it concentrates on a few psychologically meaningful neighbors. Baselines have higher entropy (1.12–1.35), meaning their attention is **diffuse and less interpretable** — spreading weight across many neighbors without clear psychological grounding.

The KL-divergence regularization is the mechanism driving this: it pushes attention toward the psychological prior, producing focused, explainable attention patterns.

---

## 6. User Study

### 6.1 Design

- **Participants**: 30 participants (mixed expertise: consumers, marketing professionals, data scientists)
- **Task**: participants were shown recommendations from NeuroGraph-CPM and HAN (baseline), each with their respective explanations
- **Rating dimensions**: Intelligibility (how understandable is the explanation?) and Trustworthiness (how much do you trust this recommendation?)
- **Scale**: 1–7 Likert scale
- **Statistical test**: paired t-test, significance threshold p < 0.01
- **Additional measure**: recommendation match — percentage of NeuroGraph-CPM recommendations that participants agreed they would actually engage with

### 6.2 Results

| Dimension | NeuroGraph-CPM | HAN Baseline | Significance |
|---|---|---|---|
| **Intelligibility** | **5.6** | 4.3 | p < 0.01 |
| **Trustworthiness** | **5.2** | 4.1 | p < 0.01 |
| **Recommendation Match** | **72%** | — | — |

**Key findings**:
- NeuroGraph-CPM explanations are significantly more **intelligible** (5.6 vs 4.3, p<0.01) — participants understand *why* the model recommends what it does
- NeuroGraph-CPM explanations are significantly more **trustworthy** (5.2 vs 4.1, p<0.01) — participants trust recommendations backed by psychological grounding
- **72% recommendation match** — nearly three-quarters of NeuroGraph-CPM's recommendations aligned with participant self-reported preferences, validating the model's personalization quality

---

## 7. Limitations

### 7.1 Affective Metadata Quality Dependency
- The model's performance depends on **high-quality affective metadata** — sentiment, trust, engagement, and cognitive load estimates extracted from review text and interaction data
- Domains with sparse, low-quality, or adversarial review text will produce noisy affective features, degrading the psychological edge signal
- The psychological signal extraction pipeline (NLP sentiment, trust derivation, cognitive load estimation) is a critical dependency

### 7.2 Rapidly Changing User Intents
- The model struggles with **rapidly changing user intents** — users whose preferences shift quickly (e.g., trend-driven, seasonal, or context-switching behavior) may not be well-served by a graph that encodes historical interaction patterns
- The graph structure is inherently backward-looking; sudden preference shifts are not captured until sufficient new interaction data accumulates
- Potential mitigation: temporal edge weighting or sliding-window graph construction (not implemented in the original study)

### 7.3 Lack of Psychological Ground-Truth Labels
- The Amazon Electronics Reviews dataset **does not contain psychological ground-truth labels** — there is no "true" sentiment, trust, or cognitive load measurement to validate the extracted affective features against
- The authors address this via two validation strategies:
  1. **30-participant user study**: correlation between model-extracted psychological signals and participant self-reported affective states: **r = 0.68** (moderate-to-strong correlation)
  2. **Yelp cross-domain test**: the model was tested on the Yelp dataset (different domain, different review characteristics) to confirm that the psychological signal extraction generalizes beyond Amazon Electronics — results remained robust, supporting cross-domain validity
- Nevertheless, the absence of ground-truth psychological labels remains a fundamental limitation — the affective features are *inferred* proxies, not measured constructs

### 7.4 Computational Overhead
- Heterogeneous graph construction (multiple node types, typed edges, multidimensional edge features) adds preprocessing overhead
- Affect-aware message passing with edge feature concatenation is more expensive per layer than standard GNN message passing
- Not suitable for real-time, low-latency inference without graph caching or approximation

---

## 8. Cross-References to Existing Skills

| Skill | Relationship |
|---|---|
| `ai-neuromarketing-synergy-framework` | NeuroGraph-CPM operationalizes the emotion-attention-memory triad: **emotion** → sentiment/trust edge features; **attention** → psychologically regularized attention mechanism; **memory** → interaction history encoded as graph structure. The privacy-first behavioral proxy approach is directly aligned — NeuroGraph-CPM uses review-text-derived signals, not biometrics. |
| `graph-neural-network-neuromarketing` | Companion skill using GNNs for **EEG-based** consumer choice prediction (electrode-node graphs, brain connectivity edges). NeuroGraph-CPM is the **behavioral-signal complement** — user-product graphs with affective edges. Together they cover both neural (EEG) and behavioral (review/interaction) graph modalities. |
| `neuromarketing-predictive-purchase-intent-model` | The purchase-intent prediction hierarchy (single-variable models) can be augmented with NeuroGraph-CPM's graph-structured, psychologically-augmented prediction as the top layer — moving from feature-based intent scoring to graph-based intent reasoning. |
| `predictive-neuromarketing-bayesian-framework` | NeuroGraph-CPM's KL-divergence attention regularization is conceptually aligned with the Bayesian framework's prior-constrained prediction — both use domain knowledge priors to constrain model behavior. The Bayesian framework's uncertainty quantification could complement NeuroGraph-CPM's point-estimate predictions. |
| `hierarchical-attention-neuromarketing-prediction` | NeuroGraph-CPM's psychologically regularized attention can be viewed as a domain-specific instance of hierarchical attention — where the hierarchy is over psychological signal types (trust > sentiment > engagement > cognitive load) rather than temporal or spatial levels. |
| `neuro-marketing-privacy-first-behavioral-analytics` | NeuroGraph-CPM is a concrete implementation of privacy-first behavioral analytics — all psychological signals are derived from publicly observable behavior (reviews, interactions), not biometric surveillance. |
| `customer-digital-twin-neuromarketing` | NeuroGraph-CPM's user node embeddings, enriched with affective edge features, function as a **graph-structured digital twin** of consumer psychological states — a complement to simulation-based digital twin approaches. |
| `neuro-sor-trait-impulsivity-moderation` | NeuroGraph-CPM's cognitive load and engagement edge features could serve as behavioral proxies for trait impulsivity moderation in purchase prediction. |

---

## 9. Reproducibility Checklist

| Item | Status |
|---|---|
| Dataset | Amazon Electronics Reviews (publicly available) |
| Framework | PyTorch Geometric (open-source) |
| Hardware | NVIDIA GPU, 32 GB VRAM (single GPU sufficient) |
| Embedding dimension | 128 |
| GNN layers | 3 |
| Train/Val/Test split | 70/15/15 |
| Optimizer | Lookahead + SAM |
| Loss | BCE + λ · KL(attention ∥ q_psych) |
| Metrics | 8 (Accuracy, AUC, Precision@K, Recall@K, F1@K, CTR@K, Diversity, Personalization Relevance) |
| K values | 5, 10, 15, 20, 25 |
| User study | 30 participants, 1–7 Likert, paired t-test |
| Cross-domain validation | Yelp dataset |
| Psychological signal validation | r = 0.68 (user study correlation) |
| DOI | 10.1007/s44196-025-01093-y |

---

## 10. A-Tech Value Alignment Summary

| Value | Evidence |
|---|---|
| **Open-source AI** | PyTorch Geometric (open-source GNN library); Amazon Electronics Reviews (open dataset); architecture fully described for reproduction |
| **Data privacy** | Psychological signals from review text + interaction metadata only — no EEG, eye-tracking, facial coding, or biometric collection; behavioral-signal-only approach |
| **Financial freedom** | Single 32 GB GPU sufficient; no proprietary data or hardware; +16.3% CTR estimation reduces wasted ad spend; SME-accessible consumer prediction |
| **Practical implementation** | 70/15/15 split on public dataset; 128-dim embeddings and 3-layer GNN are computationally tractable; reproducible architecture with open tooling |