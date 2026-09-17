# DDP-SA: Scalable Privacy-Preserving Federated Learning via Distributed Differential Privacy and Secure Aggregation

**Authors**: Wei, Nait-Abdesselam, Jammine
**Affiliation**: Université Paris Cité
**Year**: 2026

---

## 1. Summary

DDP-SA is a scalable privacy-preserving federated learning framework that combines two complementary privacy mechanisms — local differential privacy (LDP) and additive secret sharing (ASS) — to provide both statistical and cryptographic protection for individual client updates. The framework employs a multi-server architecture in which noisy gradients are decomposed into additive secret shares distributed across intermediate servers, achieving end-to-end (ε,δ)-differential privacy with zero additional privacy loss introduced by the cryptographic layer. The approach scales linearly with the number of participants and reduces uplink bandwidth by converting n client uplinks into m intermediate server uplinks (where m << n).

---

## 2. System Architecture

### 2.1 Participants
- **n clients**: Each holds a private local dataset and computes gradients locally.
- **m intermediate servers**: Each receives one additive secret share per client, aggregates the shares locally, and forwards the combined result.
- **1 parameter server**: Receives aggregated shares from all intermediate servers and reconstructs the aggregated noisy gradient for model updates.

### 2.2 Data Flow
1. Each client computes per-sample gradients on its local data.
2. Gradients are clipped with an ℓ₁ norm threshold Δ.
3. Calibrated Laplace noise with scale Δ/ε is added to the clipped gradients (LDP stage).
4. Noisy gradients are averaged locally per client.
5. Noisy average gradients are encoded using fixed precision.
6. Encoded gradients are decomposed into m additive secret shares.
7. Each share is sent to a distinct intermediate server (ASS stage).
8. Intermediate servers aggregate shares across clients and forward to the parameter server.
9. The parameter server reconstructs the aggregated noisy gradient and updates the global model.

---

## 3. Two-Stage Privacy Protection

### 3.1 Stage 1 — Local Differential Privacy (LDP)
- **Mechanism**: Laplace mechanism applied per-client.
- **Sensitivity**: Bounded by gradient clipping with ℓ₁ threshold Δ.
- **Noise scale**: Δ/ε, where ε is the per-client privacy budget.
- **Guarantee**: Each client's averaged noisy gradient satisfies (ε,δ)-LDP.
- **Post-processing invariance**: Any downstream transformation of the LDP output (including secret sharing and aggregation) preserves the LDP guarantee without additional privacy loss.

### 3.2 Stage 2 — Additive Secret Sharing (ASS)
- **Encoding**: Noisy gradients encoded in fixed precision to enable modular arithmetic.
- **Decomposition**: Each gradient vector is split into m shares such that their sum (mod modulus) reconstructs the original.
- **Distribution**: One share per intermediate server; no single share reveals information about the secret.
- **Aggregation**: Intermediate servers sum shares locally; only the combined result is forwarded.
- **Reconstruction**: Parameter server sums the aggregated shares to recover the sum (or average) of all client noisy gradients.
- **Privacy property**: A single compromised server or communication channel cannot reveal any individual client's update.

---

## 4. Privacy Guarantees

### 4.1 Theorem 1 — End-to-End Privacy
DDP-SA satisfies (ε,δ)-differential privacy end-to-end. The LDP guarantee at the client level is preserved through all subsequent operations (secret sharing, aggregation, reconstruction) by virtue of post-processing invariance of differential privacy.

### 4.2 Corollary 1 — No Additional Privacy Loss
The additive secret sharing layer introduces **zero additional privacy loss** beyond the LDP stage. This is a direct consequence of the post-processing invariance property of differential privacy: since ASS operates only on the already-LDP-protected output, it cannot degrade the privacy guarantee.

### 4.3 Theorem 2 — Multi-Round Composition
For T communication rounds, DDP-SA satisfies:
- **Basic composition**: (Tε, Tδ)-DP — privacy budget accumulates linearly.
- **Advanced composition**: (ε', δ')-DP where
  - ε' = ε·√(2T·ln(1/δ')) + T·ε·(e^ε − 1)
  - for an additional failure probability δ'.
- **Recommendation**: Advanced composition is preferred for long-running FL systems with many rounds, as it provides a sub-linear growth in the effective ε.

---

## 5. Privacy Budget Management

### 5.1 Uniform Allocation
- ε_per_round = ε_total / T
- Simple to implement; treats all rounds equally.
- May waste budget in later rounds when gradients become small.

### 5.2 Adaptive Allocation
- Exponential decay schedule: more budget allocated to early rounds when gradients are larger and more informative.
- Conserve budget in later rounds where noise has less impact relative to gradient magnitude.
- Better utility-privacy trade-off for long-running training.

### 5.3 Composition Strategy Selection
- Short training (small T): Basic composition is sufficient and simpler.
- Long training (large T): Advanced composition recommended to keep cumulative ε manageable.

---

## 6. Overhead Analysis

### 6.1 Computation Overhead
| Component | Share of Computation | Dominant Operation | Complexity |
|-----------|----------------------|--------------------|------------|
| LDP       | 92.12%               | Gradient clipping   | O(d)       |
| MPC (ASS) | 7.88%                | Share transmission | O(d·m)     |

- The LDP stage dominates computation due to the gradient clipping operation, which scales linearly with the gradient dimension d.
- The MPC/ASS stage contributes a small fraction of computation; its cost scales with d·m (gradient dimension times number of servers).

### 6.2 Communication Overhead
- LDP introduces **no communication overhead** (noise added locally).
- ASS introduces a **factor of m** communication overhead compared to plaintext aggregation, since each client must send m shares instead of one gradient.
- This is offset by the multi-server architecture, which converts n client uplinks into m intermediate server uplinks (m << n), providing bandwidth savings at the parameter server.

### 6.3 Scalability
- Communication complexity is **linear** in the number of participants.
- Generalizes to arbitrary m (number of intermediate servers).
- The bandwidth reduction ratio is n/m at the parameter server level.

---

## 7. Defense Against Inference Attacks

### 7.1 Membership Inference Attacks
- **Threat**: Attacker attempts to determine whether a specific record was part of a client's training data.
- **Defense**: LDP noise (Laplace mechanism) masks the contribution of any individual record within a client's gradient. The calibrated noise scale Δ/ε ensures that the presence or absence of a single record does not observably change the noisy gradient.
- **Status**: ✓ Protected

### 7.2 Property Inference Attacks
- **Threat**: Attacker attempts to infer global properties of a client's training data (e.g., class distribution) from the shared model update.
- **Defense**: Perturbed gradients hide fine-grained patterns that property inference attacks rely on. The LDP noise destroys the subtle signal needed to distinguish data properties.
- **Status**: ✓ Protected

### 7.3 Training Data / Label Inference Attacks
- **Threat**: Attacker attempts to reconstruct individual training samples or labels from gradient updates.
- **Defense**: Secure aggregation (ASS) prevents any single party — including the parameter server — from accessing individual client updates. Only the aggregated noisy gradient is reconstructed, preventing per-client gradient analysis.
- **Status**: ✓ Protected

### 7.4 Class Representative Attacks
- **Threat**: Attacker attempts to reconstruct representative examples of a class from gradient information.
- **Defense**: The combination of LDP (noise injection) and ASS (cryptographic aggregation) prevents reconstruction. LDP obscures the gradient features needed for optimization-based reconstruction, while ASS ensures no individual gradient is exposed.
- **Status**: ✓ Protected

---

## 8. Experimental Results

### 8.1 Utility Comparison
| Method                  | Test R²   |
|-------------------------|-----------|
| No-Private / MPC-only   | 0.9999    |
| DDP-SA                  | 0.9666    |
| LDP-only                | 0.9357    |

### 8.2 Key Findings
- **DDP-SA outperforms standalone LDP**: DDP-SA achieves higher accuracy (R² = 0.9666) than LDP alone (R² = 0.9357) while providing stronger protection. This is because the secure aggregation layer allows the system to benefit from averaging noise across many clients, effectively reducing the variance of the aggregate noise.
- **Privacy-utility trade-off**: DDP-SA incurs a modest accuracy cost (0.9666 vs. 0.9999 for the non-private baseline) in exchange for formal (ε,δ)-DP guarantees and cryptographic protection against server compromise.
- **Scalability**: The framework scales effectively to large numbers of clients and servers, with linear communication complexity and a small MPC overhead relative to the LDP computation.

### 8.3 Interpretation
- The accuracy gap between DDP-SA and the non-private baseline represents the cost of privacy.
- The improvement over LDP-only demonstrates that adding secure aggregation on top of LDP is not just a security add-on — it actively improves utility by enabling noise reduction through aggregation across a larger effective population, which would not be possible if clients operated independently.

---

## 9. Design Decisions and Trade-offs

### 9.1 Why Two Stages?
- LDP alone provides statistical privacy but does not protect against a curious server that sees individual noisy gradients — repeated observations can still leak information over rounds.
- ASS alone provides cryptographic privacy but no formal DP guarantee against attacks that exploit the aggregated gradient.
- Combining them: LDP provides the formal (ε,δ)-DP guarantee; ASS prevents any single server from seeing individual updates, strengthening the practical privacy and improving utility through secure aggregation.

### 9.2 Why Multiple Intermediate Servers?
- A single server performing aggregation can still observe individual client updates before aggregation.
- Distributing shares across m servers ensures that no single server has access to a complete gradient.
- The architecture generalizes to arbitrary m, allowing system designers to trade off communication overhead (factor m) against the trust assumption (fewer servers = stronger assumption that all are honest).

### 9.3 Why Laplace Noise (Not Gaussian)?
- Laplace mechanism provides pure (ε,0)-DP at the single-round level, which is stronger than the (ε,δ)-DP of the Gaussian mechanism.
- The (ε,δ) bound in DDP-SA arises from multi-round composition (Theorem 2), not from the noise distribution itself.
- ℓ₁ sensitivity clipping pairs naturally with the Laplace mechanism.

---

## 10. Practical Implementation Notes

### 10.1 Gradient Clipping
- Clip per-sample gradients using ℓ₁ norm threshold Δ.
- The choice of Δ directly determines the noise scale (Δ/ε) and thus the privacy-utility trade-off.
- Smaller Δ → less noise but more gradient information lost; larger Δ → more noise but better fidelity.

### 10.2 Fixed-Precision Encoding
- Required for modular arithmetic in additive secret sharing.
- Choose a modulus large enough to avoid overflow during aggregation.
- Precision must accommodate both the gradient values and the accumulated noise.

### 10.3 Server Selection
- m should be small relative to n (m << n) to realize bandwidth savings.
- Intermediate servers must be non-colluding (or at least, the threat model assumes not all are compromised simultaneously).
- Increasing m strengthens the privacy assumption (more servers must collude to break ASS) but increases communication overhead linearly.

### 10.4 Threat Model
- **Honest-but-curious servers**: Servers follow the protocol but may attempt to learn from the data they see.
- **Up to m−1 compromised servers**: The system remains secure as long as at least one intermediate server is honest.
- **Parameter server**: Sees only the aggregated noisy gradient — protected by LDP.

---

## 11. Comparison with Related Approaches

| Feature                    | DDP-SA              | LDP-only        | MPC/ASS-only     | Central DP         |
|----------------------------|---------------------|-----------------|------------------|--------------------|
| Statistical DP guarantee   | ✓ (ε,δ)-DP          | ✓ (ε,δ)-DP      | ✗                | ✓ (ε,δ)-DP         |
| Cryptographic protection   | ✓ (ASS)             | ✗               | ✓                | ✗                  |
| Server sees individual grads | ✗                | ✓               | ✗                | N/A                |
| Trust model                | Up to m−1 servers   | Trust server     | Up to m−1 servers| Trust curator      |
| Scalability                | Linear, m << n      | Linear           | Linear           | Centralized        |
| Utility (R²)               | 0.9666              | 0.9357           | 0.9999           | Varies             |
| Additional privacy loss   | Zero (Corollary 1)  | N/A              | N/A              | N/A                |

---

## 12. Citation

```bibtex
@article{wei2026ddpsa,
  title={DDP-SA: Scalable Privacy-Preserving Federated Learning via Distributed Differential Privacy and Secure Aggregation},
  author={Wei and Nait-Abdesselam and Jammine},
  year={2026},
  institution={Universit\'e Paris Cit\'e}
}
```

---

## 13. Glossary

- **LDP (Local Differential Privacy)**: DP applied at the data source (client) before any data leaves the device. No trusted curator required.
- **ASS (Additive Secret Sharing)**: A secret sharing scheme where the secret is split into m additive shares that sum (mod modulus) to the secret; any subset of fewer than m shares reveals nothing.
- **Post-processing invariance**: A fundamental property of DP stating that any function applied to the output of a DP mechanism does not weaken the privacy guarantee.
- **Parameter server**: The central server responsible for aggregating client updates and updating the global model.
- **Intermediate server**: A server in the multi-server architecture that receives and aggregates one secret share from each client.
- **ℓ₁ sensitivity**: The maximum ℓ₁ norm change in the output when a single record is added or removed; determines the noise scale for the Laplace mechanism.