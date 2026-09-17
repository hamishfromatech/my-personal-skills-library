# FLiPD Evidence Base

## Source
Chandran, Önen, Schneider. "FLiPD: Privacy-Preserving Federated Learning via Multi-Party Computation and Differential Privacy." IACR ePrint 2026/324, February 2026.

## Key Properties

1. **Combined inference + backdoor defense**: Most SA protocols protect only against inference. FLiPD adds DP noise to also defend against backdoor/poisoning attacks.
2. **Majority collusion resistance**: Distributed DP noise generation means even if majority of clients collude with the server, they cannot strip the noise.
3. **Communication efficiency**: Client-server cost ≈ unprotected FL. Server-server cost 11% lower than Prio+.

## Comparison with Prio+ (Addanki et al., SCN'22)

Prio+ is the state-of-the-art in SA protocols. FLiPD improves on it:
- Server-server communication: 11% lower
- Adds DP defense (Prio+ has no DP)
- Maintains same client-server efficiency

## Accuracy

- Linear Regression on HAR dataset: 87%
- CNN on MNIST: 90%

These are with DP noise applied — the privacy-utility trade-off is acceptable for practical use.

## DP Noise Distribution

The distributed noise generation is the critical innovation:
- Standard DP-FL: server or single party adds noise → can be removed by collusion
- FLiPD: noise generated across multiple parties via MPC → no single coalition can remove it
- This is what enables the majority-collusion guarantee

## Threat Model Details

- Honest-but-curious server(s)
- Up to majority of clients can collude with the server
- Protocol remains secure: DP noise cannot be stripped, individual updates cannot be inspected
- Defends against both gradient inversion (inference) and backdoor/poisoning simultaneously

## Practical Implications

1. For environments with realistic collusion threats (multi-organization FL), FLiPD provides stronger guarantees
2. Communication efficiency matching unprotected FL removes a key adoption barrier
3. Combined defense (inference + backdoor) reduces need for separate filtering mechanisms
4. Requires at least 2 non-colluding MPC servers for the protocol to work