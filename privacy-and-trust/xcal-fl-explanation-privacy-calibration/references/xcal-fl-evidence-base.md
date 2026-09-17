# XCal-FL Evidence Base

**Paper:** Khavkin, M., Lee, K., Jin, J., Ko, J., & Toch, E. (2026). *Pushing the (Decision) Boundaries: Dynamically Calibrating Differentially Private Noise to Explainability in Federated Learning.* arXiv:2609.03851 (submitted Sept 3, 2026; 21 pages, 9 figures). Tel Aviv University (Toch lab) with Yonsei University (Ko lab).

## Abstract (verbatim key claims)

> "Federated Learning (FL) with Differential Privacy (DP) is increasingly adopted to preserve data confidentiality in distributed machine learning. However, DP noise distorts learned representations and degrades explanation fidelity, limiting differentially private FL where trustworthy explanations are required, such as assistive clinical diagnosis. Prior work adapted DP noise with static feature-importance signals, restricting explainability to post hoc analysis and precluding noise calibration to explanation quality during training. We propose XCal-FL, a closed-loop, explainability-driven local training algorithm for image classification in cross-silo FL that dynamically calibrates DP noise from three complementary signals: (1) prediction logit variations, measuring causal influence on model confidence, (2) counterfactual margins, capturing decision-boundary sensitivity, and (3) saliency concentration, quantifying spatial coherence of model attention, while enforcing formal DP guarantees via adaptive privacy accounting. Experiments on three medical imaging datasets across varying FL configurations show that XCal-FL yields more accurate and interpretable global models, improving predictive performance by over 10% and explanation fidelity by up to 5× over static-noise FL, and outperforming state-of-the-art adaptive DP methods in fidelity. XCal-FL also achieves higher privacy-budget efficiency, turning each unit of cumulative privacy loss into larger gains in both accuracy and explanation fidelity. Our analysis further reveals that, unlike predictive performance, which scales roughly linearly with privacy loss, explanation fidelity exhibits non-linear dynamics. These findings suggest explainability is a distinct dimension of the privacy trade-off that cannot be inferred from utility alone, with implications for training and privacy-budget allocation in decision-critical applications."

## The three calibration signals — why these three

| Signal | What it measures | Why it matters for DP noise |
|---|---|---|
| Prediction logit variations | Causal influence on model confidence | Regions where logits swing widely are where noise hurts confidence most → allocate less noise |
| Counterfactual margins | Decision-boundary sensitivity | Thin margins are where explanations mislead → protect these regions |
| Saliency concentration | Spatial coherence of model attention | Concentrated saliency = interpretable explanation; diffuse saliency = degraded fidelity → calibrate toward concentration |

Prior work (static feature-importance DP noise, e.g., FedSVA's Shapley-guided injection) restricted explainability to **post hoc** analysis — XCal-FL closes the loop **during training**.

## The three-axis privacy budget

The paper's core conceptual contribution is reframing the DP trade-off:

- **Accuracy axis:** scales roughly linearly with privacy loss (ε) — well-understood, budgetable.
- **Explanation-fidelity axis:** **non-linear dynamics** — small ε changes can produce disproportionate fidelity swings.
- **Safety/regulatory axis:** untouched by XCal-FL but implied — a budget allocation must respect all three.

**A-Tech rule:** any DP-FL deployment in decision-critical domains (clinical, legal, financial audit) must report all three metrics independently. Accuracy-only reporting hides fidelity collapse.

## Positioning against the library's DP-FL stack

| Existing skill | What it covers | XCal-FL adds |
|---|---|---|
| `adaptive-dp-fl-concept-drift-edge` | DP-FL under distribution drift at the edge | The explanation-quality axis (orthogonal to drift) |
| `veridp-verifiable-dp-sgd` | Verifiable DP integrity | Fidelity calibration (complementary — XCal-FL is accuracy/fidelity, VeriDP is auditability) |
| `privacy-preserving-ai-attribution-framework` | Attribution methods under privacy constraints | The *training-time* fix (XCal-FL calibrates during training, not post hoc) |
| `safelm-unified-privacy-llm-framework` | LLM-side privacy | Image-domain counterpart; text generalization is open |
| `dptrainer-drop-in-differential-privacy` | Drop-in DP-SGD tooling | XCal-FL extends such stacks with a fidelity-aware allocation layer |

## Deployment checklist

1. **Confirm the explanation method first.** XCal-FL calibrates for a specific explanation target (saliency maps in the paper). If your product explains via a different method (e.g., Grad-CAM, LIME, counterfactuals), the three signals must be adapted.
2. **Instrument per-layer noise allocation.** The calibration loop is client-local — each client adjusts its own noise before sending updates.
3. **Track cumulative ε with an adaptive accountant.** Closed-form (Rényi-DP style) accounting keeps guarantees auditable.
4. **Report the three axes separately in every release.** Accuracy, explanation fidelity, ε-consumption — never collapse them into one score.

## Open questions for follow-up

- **Text/LLM generalization:** the three signals are spatial for imaging; attention-concentration analogues for text are untested.
- **Federated aggregation compatibility:** the method is FedAvg-compatible by design, but interaction with secure aggregation or TEE layers is unexamined.
- **Multi-modality:** medical imaging is the test bed; whether the non-linear fidelity dynamics hold for video or audio is open.

## Content angles for A-Tech

- **Video:** "The privacy trade-off just gained a third axis — and most DP deployments are ignoring it."
- **Slide:** the three-axis budget diagram (accuracy / explanation fidelity / safety).
- **Counter-intuitive hook:** "Your DP model can hit its accuracy target and still be unusable in a hospital."

## Citation

Khavkin, M., Lee, K., Jin, J., Ko, J., & Toch, E. (2026). Pushing the (Decision) Boundaries: Dynamically Calibrating Differentially Private Noise to Explainability in Federated Learning. arXiv:2609.03851. https://doi.org/10.48550/arXiv.2609.03851