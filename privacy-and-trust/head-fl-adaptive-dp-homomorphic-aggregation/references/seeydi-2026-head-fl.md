# HEAD-FL: Secure and Efficient Federated Learning with Adaptive Differential Privacy and Verifiable Homomorphic Aggregation

- **Authors**: Seyedi, Rahmati, Seyedi
- **Source**: IACR ePrint 2026/1376
- **Domain**: Privacy-preserving federated learning, differential privacy, homomorphic aggregation

---

## 1. Problem Statement

Federated learning (FL) enables collaborative model training across decentralized clients without sharing raw data. However, two distinct privacy threat models have historically been addressed in isolation:

1. **Statistical privacy leakage**: Gradient/model updates can leak information about individual training samples (membership inference, reconstruction attacks). Differential privacy (DP) provides formal guarantees against these threats, but fixed-noise DP schemes suffer from suboptimal privacy-utility tradeoffs — they either over-perturb early rounds (hurting utility) or under-perturb (weakening privacy).

2. **Cryptographic aggregation risks**: A malicious or compromised server can tamper with aggregation, inject poisoned aggregates, or inspect individual client updates. Cryptographic secure aggregation (e.g., secret sharing, homomorphic encryption) addresses this but typically lacks formal statistical privacy guarantees against information leakage from the aggregates themselves.

Prior work generally addresses one or the other. HEAD-FL's contribution is a unified framework that provides **both** formal statistical privacy (via adaptive DP with RDP accounting) **and** cryptographic security (via verifiable homomorphic aggregation), while simultaneously improving communication efficiency.

## 2. Core Innovations

### 2.1 Round-Adaptive Gaussian Perturbation (RDP Framework)

Instead of applying a fixed Gaussian noise scale across all communication rounds, HEAD-FL adapts the noise perturbation per round. Key technical elements:

- **Rényi Differential Privacy (RDP)**: RDP provides a compositional privacy accounting framework that is tighter than naive (ε,δ)-DP composition. RDP is parameterized by an order α and yields a Rényi divergence bound ε(α).
- **Cumulative accounting across rounds**: Because RDP composes additively, the cumulative privacy loss across T rounds can be tightly bounded by summing per-round RDP contributions. This avoids the privacy budget blowup inherent in naive composition.
- **Adaptive noise schedule**: The per-round noise scale σ_t is adjusted based on the round index t and the remaining privacy budget. Early rounds (where the model is still converging and updates are large) can tolerate different noise levels than late rounds (where updates are small and utility-sensitive).
- **Conversion to (ε,δ)-DP**: RDP bounds are explicitly converted to (ε,δ)-DP guarantees using the standard conversion formula, giving the practitioner a concrete, interpretable privacy budget.

**Advantage over fixed-noise DP**: Fixed-noise schemes must pick a single σ that works across all rounds, leading to either excessive utility loss or insufficient privacy late in training. Adaptive perturbation allocates noise where it is least harmful to utility, yielding a strictly better privacy-utility frontier.

### 2.2 Verifiable Homomorphic Aggregation

HEAD-FL adds a cryptographic layer that ensures the server's reported aggregate is correct and that individual updates remain confidential:

- **Homomorphic aggregation**: The server computes the aggregate (model average) over protected/encrypted client updates without decrypting individual contributions. The aggregation operation is compatible with the protection scheme applied at the client.
- **Verifiability**: Clients (or a verifier) can cryptographically confirm that the server correctly performed the aggregation — i.e., the reported aggregate is the correct function of the submitted (protected) updates. This defends against a malicious server that tampers with the aggregate or fabricates results.
- **Confidentiality preservation**: Individual client updates are never exposed in plaintext to the server or to other clients. Only the perturbed, protected aggregate is revealed.
- **Compatibility with DP**: The homomorphic layer operates on the differentially private (perturbed) updates, so the two mechanisms compose cleanly. The DP noise is added before the cryptographic protection, and the homomorphic aggregation preserves the noise distribution in the aggregate.

**Advantage over secure-aggregation-only approaches**: Pure cryptographic secure aggregation prevents the server from seeing individual updates but does not bound what an adversary can learn from the aggregate itself (e.g., via repeated queries or auxiliary information). HEAD-FL's DP layer closes this gap with a formal statistical guarantee.

### 2.3 FedAvg-Based Aggregation (Communication Efficiency)

HEAD-FL aggregates via **FedAvg** (federated averaging) rather than gradient-based aggregation:

- **Model averaging**: Clients send their locally-trained model parameters (or their average), and the server averages them weighted by dataset sizes. This contrasts with schemes that transmit raw gradients per step.
- **Reduced communication**: FedAvg requires one model transmission per round per client (rather than multiple gradient transmissions), substantially reducing bandwidth — critical for bandwidth-constrained and mobile/edge deployments.
- **Preserved guarantees**: Switching to FedAvg does not weaken the DP or cryptographic guarantees. The homomorphic aggregation and RDP accounting apply to the averaged model updates.
- **Dropout robustness**: FedAvg naturally accommodates client dropouts — the server averages over whichever clients respond in a given round. The privacy accounting and verification mechanism are designed to remain valid when the participating client set changes across rounds.

**Advantage over gradient-based secure aggregation**: Gradient-based schemes transmit more data per round and often require all selected clients to complete the round (for masking/secret-sharing to reconstruct correctly). HEAD-FL's FedAvg approach is both lighter on bandwidth and more tolerant of stragglers/dropouts.

## 3. Architecture (End-to-End Flow)

```
Round t (t = 1 ... T):

  [Clients]
    1. Local training on private data → local model update w_i
    2. Apply round-adaptive Gaussian perturbation: ẇ_i = w_i + N(0, σ_t² I)
       (σ_t chosen per adaptive schedule, RDP-analyzed)
    3. Cryptographically protect ẇ_i (for homomorphic aggregation)
    4. Transmit protected update to server

  [Server]
    5. Homomorphically aggregate protected updates → protected aggregate
    6. Publish aggregate + cryptographic proof of correctness
    7. Verify proof (by clients or designated verifier)
    8. Reveal/apply perturbed aggregate as global model for round t

  [Privacy Accounting]
    9. Record per-round RDP bound ε_t(α) for chosen order(s) α
   10. Accumulate: ε_total(α) = Σ_t ε_t(α)
   11. At end (or checkpoint): convert RDP → (ε, δ)-DP final guarantee
```

### Components Summary

| Component | Role | Mechanism |
|-----------|------|-----------|
| Round-adaptive Gaussian perturbation | Statistical privacy | Per-round noise σ_t, RDP composition |
| RDP accounting | Tight privacy budget | Rényi divergence, additive composition, (ε,δ) conversion |
| Verifiable homomorphic aggregation | Cryptographic security | Homomorphic computation + verifiable proof |
| FedAvg aggregation | Communication efficiency | Model averaging, one transmission/round/client |
| Dropout handling | Robustness | Average over responding clients; accounting valid under changing sets |

## 4. Privacy-Utility Tradeoffs

HEAD-FL's central claim is a **strictly improved privacy-utility frontier** over both reference classes:

- **vs. Fixed-noise DP**: At the same final (ε, δ), adaptive perturbation achieves higher model utility because noise is allocated where it harms utility least. Empirically, the gap widens as the number of rounds T grows (because fixed noise's cumulative suboptimality compounds).
- **vs. Gradient-based secure aggregation**: HEAD-FL adds the formal DP guarantee that secure-aggregation-only schemes lack, at *lower* communication cost due to FedAvg.
- **Combined**: HEAD-FL is positioned as the first scheme to simultaneously offer (a) formal statistical privacy, (b) cryptographic verifiability, and (c) reduced communication — the prior art offers at most two of these three.

## 5. Comparison Table (Detailed)

| Property | Fixed-Noise DP | Cryptographic Secure Aggregation Only | HEAD-FL |
|----------|----------------|---------------------------------------|---------|
| Statistical privacy (DP) | Yes, but suboptimal utility | No formal guarantee | Yes, RDP-optimized |
| Cumulative accounting | Loose (naive composition) | N/A | Tight (RDP additive) |
| Cryptographic security | No | Yes | Yes (homomorphic) |
| Verifiable aggregation | No | Varies by scheme | Yes |
| Confidentiality of updates | Only via noise | Yes (encryption/masking) | Yes (both layers) |
| Communication overhead | Standard | Higher (per-round gradient + masking) | Reduced (FedAvg) |
| Client dropout tolerance | Depends on scheme | Often requires full participation | Robust (FedAvg averaging) |
| Bandwidth-constrained suitability | Moderate | Low | High |

## 6. Practical Deployment Considerations

- **Bandwidth-constrained / edge environments**: FedAvg's one-model-per-round transmission makes HEAD-FL particularly suitable for mobile, IoT, or rural deployments where uplink bandwidth is limited.
- **Privacy-sensitive domains**: Healthcare, finance, and cross-organization FL benefit from the dual (statistical + cryptographic) guarantee — useful when regulatory or contractual requirements demand formal DP *and* protection against a curious/malicious server.
- **Long-running training**: The RDP cumulative accounting is most advantageous when T is large; short training runs see less benefit from adaptive scheduling.
- **Client heterogeneity / dropouts**: FedAvg averaging tolerates variable participation; the privacy accounting remains valid as long as the per-round noise and accounting incorporate the actual participating set.

## 7. Limitations and Open Questions

- The adaptive noise schedule's optimal form depends on the learning task, model size, and round count; a practical, general-purpose schedule may require tuning or heuristics.
- Homomorphic aggregation adds computational overhead on the server (and possibly clients for protection), partially offsetting communication savings — net benefit depends on the compute-vs-bandwidth ratio of the deployment.
- RDP tightness depends on the chosen order(s) α; the framework's practical advantage assumes appropriate α selection.
- The paper's threat model and precise cryptographic assumptions (encryption scheme, proof system, trust model for the verifier) should be consulted in the source for implementation decisions.

## 8. Key Terms Glossary

- **Rényi Differential Privacy (RDP)**: A generalization of DP based on Rényi divergence, offering tighter composition for mechanisms like Gaussian perturbation. Parameterized by order α > 1 and bound ε(α).
- **(ε, δ)-DP**: The standard approximate differential privacy definition: a mechanism is (ε, δ)-private if outputs on neighboring datasets differ in probability by a factor bounded by e^ε, except with probability δ.
- **FedAvg (Federated Averaging)**: Aggregation by weighted averaging of client model parameters, rather than averaging gradients. Typically each client performs several local SGD steps before transmitting.
- **Homomorphic aggregation**: Performing aggregation (averaging) on protected/encrypted data such that the server learns only the aggregate, not individual inputs.
- **Verifiable aggregation**: A cryptographic proof that the server's reported aggregate is the correct function of the submitted (protected) updates, detectable if the server tampers.
- **Round-adaptive perturbation**: Choosing the noise scale per communication round (σ_t as a function of t) rather than a single fixed σ.

## 9. Citation

Seyedi, Rahmati, Seyedi. "HEAD-FL: Secure and Efficient Federated Learning with Adaptive Differential Privacy and Verifiable Homomorphic Aggregation." IACR ePrint 2026/1376.

---

*This reference summarizes the architectural and theoretical contributions of HEAD-FL for use as an agent skill. For full proofs, exact noise schedules, and cryptographic construction details, consult the original paper.*