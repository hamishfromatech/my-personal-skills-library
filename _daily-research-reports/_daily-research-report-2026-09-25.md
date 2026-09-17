# Daily Research Report — 2026-09-25

**Date:** September 25, 2026 (Auckland time)
**Research Focus:** SE agent building practices, junior-senior agency allocation with agentic AI, buyer-side payment decision-making for autonomous agents, selective feature encryption for privacy-preserving federated learning

---

## Executive Summary

Today's research cycle identified **four high-impact novel developments** across the A-Tech domains, resulting in **four new skills** created. The most significant findings include: (1) the first mixed-methods study of how practitioners actually build SE agents — a seven-stage workflow, five process shifts, and six challenges with twelve practices, revealing that implementation is becoming cheaper while evaluation and comprehension become the new bottlenecks; (2) the first qualitative study of how junior and senior engineers allocate agency with agentic AI — agency is preconfigured by company policies, familiarity drives divergent patterns, and the talent pipeline must evolve from gradual mastery to "earning judgment through deliberate restraint"; (3) the first buyer-side decision layer for autonomous agent micropayments — solving the gap between payment execution protocols (x402/AP2) and spending policy via payment-aware Thompson sampling that uses only 39-43% of wallet while adapting to market changes; (4) the first hybrid encrypted-plaintext FL system — selectively encrypting only privacy-sensitive features via PCA while training plaintext on the rest, achieving reconstruction attack mitigation comparable to full encryption at a fraction of the cost.

---

## Skills Created (4 new skills)

### 1. SE Agent Building Practice
**Category:** developer-experience-and-flow
**Source:** Lyu, Williams, Shi, Sun, Peng, Yang, Sarro, Lo (SMU/UCL/Tencent/University of Alberta, arXiv:2607.10856v2, 2026)

**Key Findings:**
- First mixed-methods study: 20 interviews across 12 organizations + 80-practitioner survey
- Seven-stage workflow: requirements→evaluation→data→construction→testing/deployment→feedback→maintenance (91% agreement)
- Five process shifts (71-95% agreement):
  - S1: Implementation becomes cheaper (84%) — agents build agents
  - S2: Effort unmasked and created (95%) — coding shrinks, reviewing/evaluating become central
  - S3: Evaluation-driven development (83%) — evaluation steers iteration, not just final check
  - S4: Role boundaries shrink (71%) — research-engineering fusion
  - S5: Specifications as first-class artifacts (77%) — prompts, skills, context version-controlled
- Six challenges with twelve practices:
  - C1: Evaluation lacks trustworthy signal (73%) — four practices (70.5-78.2% effective)
  - C2: Change nothing, change everything (80%) — model updates break scaffolding
  - C3: Safety lags performance (74%) — enforce below prompt layer (80.8% effective)
  - C4: Agents retrieve written, not unsaid (76%) — reusable skills (83.8% effective)
  - C5: Comprehension debt accumulates (82%) — regenerative software (67.1% effective)
  - C6: Productivity metrics break down (85%) — code volume as diagnostic, not target
- 18 of 20 interviewees use SE agents to build SE agents (recursive dynamic)
- "Regenerative software" concept: preserve specs/tests/constraints to regenerate, not the implementation

**A-Tech Alignment:** Open-source (applies to Cline/OpenCode), data privacy (regenerative software), financial freedom (cheaper implementation, EDD prevents waste), practical implementation (immediately actionable with effectiveness ratings)

### 2. Junior-Senior Agency Allocation
**Category:** developer-experience-and-flow
**Source:** Feng, Yun, Wang (ETH Zurich, arXiv:2602.00496v2, Feb 2026)

**Key Findings:**
- Three-phase qualitative study: 20 engineers (10 juniors ≤1yr, 10 seniors ≥5yr)
- Agency preconfigured by company policies before individual preferences matter
- Task familiarity drives divergent patterns:
  - High familiarity: seniors maintain control via detailed delegation; juniors use constraint-based approaches
  - Low familiarity: seniors use strategic oversight; juniors oscillate between over-reliance and defensive resistance
- Five-stage interaction taxonomy: active collaboration→guided generation→supervised generation→passive supervision→full autonomy
- Three evolving practices:
  1. Preserving Individual Agency (interruptibility, legible provenance, small test-bounded diffs)
  2. Evolving Mentorship Pipeline (seniors as "Socratic guides and organizational anchors")
  3. Prompt & Code Reviews (PCRs — juniors document 2-3 key prompts, seniors ensure reasoning ownership)
- Junior imposter syndrome: "It has my name on it, but I have no idea why it works" (J8)
- Senior foundational instincts remain critical: "at no point can you hand over your expertise. You're just handing over the workload" (S4)
- Confidence in coding without AI dropped from 3.90 to 3.30 post-task (Cohen's d=0.71)
- AI as mentor for basic questions; human mentorship irreplaceable for context-specific knowledge

**A-Tech Alignment:** Open-source (applies to Cline/OpenCode), data privacy (local-first AI preserves agency + privacy), financial freedom (61.7% time savings, lightweight PCR), practical implementation (PCR immediately implementable)

### 3. 402Pilot Buyer-Side Payment Decision
**Category:** ai-agents-and-workflows
**Source:** Li et al. (arXiv:2608.01341, 2026, code: github.com/MCCodeAI/402Pilot)

**Key Findings:**
- First buyer-side decision layer for autonomous agent micropayments
- Solves gap: payment protocols (x402, AP2, ACP) handle execution but don't determine what to buy
- PA-DCT (Payment-Aware Discounted Contextual Thompson Sampling):
  - Discounted Bayesian posteriors over provider utility and cost
  - Wallet pressure shifts selection from quality to cost as budget depletes
  - Adapts to reliability and price changes via discounting (γ=0.999) and learned realized costs
- 402Pilot-Bench: 823 tasks × 5 providers × 3 scenarios × 30 paired seeds
- Results: uses only 39-43% of wallet while maintaining competitive quality
- Best non-oracle PA-gap/T in price-shock scenario: 0.121 vs 0.195 (next best)
- Ablation: payment-aware ranking preserves solvency; discounting enables adaptation; context accelerates recovery; Thompson sampling improves robustness
- Protocol-agnostic (works with x402/AP2/ACP)
- Theoretical analysis: regret bounds, adaptation rate theorems

**A-Tech Alignment:** Open-source (code open-source), data privacy (buyer-side decisions local), financial freedom (cost-effective agent operation, prevents wallet exhaustion), practical implementation (locked hyperparameters, reproducible benchmark, x402 integration witness)

### 4. HADES Selective Feature Encryption FL
**Category:** privacy-and-trust
**Source:** Kaynak, Bayramoglu, Sav (Bilkent University, arXiv:2606.22928v1, 2026)

**Key Findings:**
- First hybrid encrypted-plaintext FL system
- Selectively encrypts only most privacy-sensitive features (via PCA) using MHE/CKKS
- Trains plaintext network on remaining features
- Score-level fusion: z̄_HE = α·z_HE + (1-α)·HE(z_P)
- iDLG reconstruction attack mitigation:
  - Encrypting 256 of 784 MNIST features (33%) reduces SSIM from 1.00 to 0.11
  - Approaches near-worst-case (0.04) without encrypting all features
- Utility preserved or improved:
  - BCD: 94.71% → 97.08%
  - MNIST: 95.00% → 94.99%
  - SVHN: 63.1% → 70.4%
- 2-16x reduction in encrypted parameters
- Up to 28% runtime reduction
- Single-ciphertext mini-batching
- First system to perform fully encrypted training with fusion between encrypted and plaintext components
- Built on OpenFHE (open-source)

**A-Tech Alignment:** Open-source (OpenFHE), data privacy (selective encryption, MHE prevents single-party decryption), financial freedom (2-16x fewer encrypted parameters reduces compute cost), practical implementation (concrete algorithm, 3-dataset validation, configurable tradeoff)

---

## Cross-Domain Synthesis

### Theme 1: The Implementation-Evaluation-Comprehension Shift

Two of today's four skills converge on a fundamental restructuring of software engineering work:

- **SE Agent Building Practice** (S2: 95% agreement): As implementation becomes cheaper, effort shifts from coding to reviewing and evaluating agent behavior. The bottleneck moves upstream (requirements) and downstream (evaluation/comprehension).
- **Junior-Senior Agency Allocation**: Juniors gain speed but struggle with ownership and comprehension. "It has my name on it, but I have no idea why it works" is the human face of the comprehension debt that SE agent builders identify as their fifth challenge (82% agreement).

Together, these skills describe a unified shift: the value in AI-mediated software engineering is moving from producing code to evaluating it, understanding it, and deciding when not to use AI at all.

### Theme 2: The Decision Layer Above Execution

402Pilot fills a gap that the existing agentic payment protocol skills (Agent Economy Payment Protocols, Agentic Payment Protocol Convergence, Agent-Ready API Monetization) identified but didn't solve: payment protocols handle execution, but someone needs to decide *what* to pay for. This is the buyer-side mirror of the seller-side monetization skills already in the directory. The combination creates a complete picture: sellers set prices (existing skills), protocols execute payments (existing skills), and buyers make spending decisions (402Pilot).

### Theme 3: Selective Privacy as a Design Pattern

HADES introduces a design pattern that could apply beyond FL: instead of encrypting everything (expensive) or nothing (insecure), selectively encrypt only the most sensitive subset. This parallels the "progressive disclosure" pattern in cognitive science and the "progressive context" practice in SE Agent Building (Practice 2 of C4: "provide context progressively — smallest sufficient context"). The principle is the same: protect/reveal only what matters, not everything.

### Theme 4: The Preconfiguration of Agency

The Junior-Senior skill reveals that agency is preconfigured before individual choices matter — company policies, tool defaults, and "use AI now" mandates set the boundaries. This connects to the SE Agent Building skill's finding that safety constraints must be enforced "below the prompt layer" (C3, 80.8% effective) — both findings point to the same conclusion: the most important decisions about AI use are architectural and organizational, not individual.

---

## Incremental Updates (Not New Skills)

### Developer Experience Research Wave (Reinforces Existing Skills)
Multiple search results reinforced existing DevEx skills without meeting novelty threshold for new skills:
- **SWE-Together** (Meta, Aug 2026): Multi-turn benchmark with user simulators — reinforces `swe-chat-real-world-coding-agent-dataset` and `agentic-cognitive-engagement-decline`
- **Personalized Coding Agent Skills** (Huang et al., UMass/UMass, Aug 2026): Personalized skills provide limited gains; generic skills work better — reinforces `personalized-coding-agent-skills`
- **Agentic Coding Production Characterization** (Liu et al., Microsoft/UIUC, June 2026): First production-scale GitHub Copilot characterization (13M sessions, 3.2M users) — reinforces `agentic-coding-production-characterization` and `devex-ai-augmented-sdlc`

### Open-Source AI Monetization (Reinforces Existing Skills)
- **Malpani Give-Away/Keep Matrix** — comprehensively covered by existing `give-away-keep-matrix-oss-ai` and `open-source-ai-monetization-stack` skills
- **Minbook AI Framework Monetization** (LangChain/LlamaIndex/CrewAI) — reinforces existing `open-source-ai-five-layer-stack` and `framework-complement-open-source-model` skills
- **ShareAI Open-Source AI Monetization** — covered by existing `shareai-open-source-ai-metering-pattern` skill
- **RSI Model** (Mondjo, arXiv:2603.20533) — covered by existing `revenue-sharing-as-infrastructure-model` skill

### Agentic Payment Protocols (Reinforces Existing Skills)
- **x402 multi-chain** (QBT-Labs) — reinforces `agent-economy-payment-protocols` and `agentic-payment-protocol-convergence-2026`
- **Amazon Bedrock AgentCore Payments GA** — reinforces `agent-economy-payment-protocols`
- **ANP Agent Negotiation Protocol** — reinforces `agent-economy-payment-protocols`
- **Galaxy x402 Research** — reinforces `agent-economy-payment-protocols`

### Privacy-Preserving FL (Reinforces Existing Skills)
- **FLiPD** — already covered by existing `flipd-majority-collusion-resistant-secure-aggregation` skill
- **FIRMA Fibonacci Ring** — decentralized FL, different approach but covered by existing FL skills
- **FedHENet** — one-shot frugal FL, covered by existing FL skills
- **PrivFedTalk** — federated talking-head generation, niche application

### Community Growth (Reinforces Existing Skills)
- Multiple open-source marketing and community-led growth guides — all covered by existing `community-led-growth-for-open-source-ai` and `open-source-community-flywheel` skills

---

## Research Methodology

Today's research used web search across six domains:
1. Open source AI monetization and business models
2. Agentic AI coding developer experience research
3. Neuromarketing and behavioral psychology AI research
4. Privacy-preserving AI and federated learning
5. AI agent MCP payment protocols
6. Open source community growth

Searches returned 40+ web results across 6 queries. Results were evaluated for:
- **Novelty**: Genuinely new development not covered by existing 310+ skills
- **Open Source Focus**: Alignment with A-Tech's open-source ethos
- **Audience Interest**: Relevance to A-Tech's developer/tech audience
- **Depth Potential**: Ability to sustain well-researched skill content
- **Practical Implementation**: Actionable frameworks, not just theoretical observations

Four developments met all novelty criteria and were synthesized into Agent Skills with proper YAML frontmatter, evidence-base references, and A-Tech alignment analysis. Additional findings were classified as incremental updates to existing skills.

---

## Statistics

| Metric | Value |
|--------|-------|
| Skills created (total today) | 4 |
| Reference files created | 4 |
| Categories touched | 3 (developer-experience-and-flow, ai-agents-and-workflows, privacy-and-trust) |
| Total SKILL.md lines | ~330 |
| Total reference lines | ~2,200 |
| Research sources evaluated | 40+ web results across 6 queries |
| Skills directory total | ~315+ skills across 9 categories |

---

## A-Tech Values Alignment

All 4 new skills align with A-Tech Corporation's core values:

- **Open-source AI**: SE Agent Building (applies to Cline/OpenCode, cheapest-first strategy supports OSS models), Junior-Senior Agency (open-source agents), 402Pilot (code open-source, supports open-weight providers), HADES (built on OpenFHE)
- **Data privacy**: SE Agent Building (regenerative software preserves specs without exposing implementations), Junior-Senior Agency (local-first AI preserves agency + privacy), 402Pilot (buyer-side decisions local, x402 privacy-preserving), HADES (selective encryption, MHE prevents single-party decryption)
- **Financial freedom**: SE Agent Building (cheaper implementation, EDD prevents wasted investment), Junior-Senior Agency (61.7% time savings, lightweight PCR), 402Pilot (cost-effective agent operation, prevents wallet exhaustion), HADES (2-16x fewer encrypted parameters reduces compute cost)
- **Practical implementation**: SE Agent Building (immediately actionable workflow with effectiveness ratings), Junior-Senior Agency (PCR immediately implementable), 402Pilot (locked hyperparameters, reproducible benchmark, x402 integration witness), HADES (concrete algorithm, 3-dataset validation, configurable tradeoff)

---

## Report Date
2026-09-25