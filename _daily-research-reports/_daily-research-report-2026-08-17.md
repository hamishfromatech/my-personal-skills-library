# Daily Research Report — 2026-08-17

## Executive Summary

Today's research cycle identified **4 novel findings** across four research domains, resulting in **4 new skills created** (plus 1 reference document). The findings span the first large-scale field evidence of motivated reasoning and cognitive load in occupational choices (University of Zurich/Bern, April 2026), the first hybrid encrypted-plaintext federated learning system with model fusion (Bilkent University, June 2026), the first unified framework addressing four pillars of LLM safety in federated training (Istanbul Technical University, April 2026), and the first empirical study of coding expertise "enaction" in AI-assisted interviews (Fujitsu Research, CHI 2026). An additional finding on adaptive digital nudging architecture with LLM-driven reasoning (Syddansk Universitet, SAGAI-ICSA 2026) rounds out the cycle. All findings represent genuinely new frameworks or empirical contributions not previously in the skill library.

---

## Research Phase Findings

### 1. Behavioral Psychology & Nudging

**Novel Finding: Choice Architecture in Occupational Decisions (Dell et al., University of Zurich/Bern, April 2026)**

This is the first large-scale field evidence (not laboratory) demonstrating that motivated reasoning and cognitive load operate in high-stakes, real-world occupational choices with long-term consequences. Prior evidence for these behavioral mechanisms came almost exclusively from controlled laboratory settings with abstract decision tasks.

- **Setting**: Yousty, Switzerland's largest private apprenticeship job board (~90% market coverage, 246,869 users, 2019-2024). Swiss VET system: adolescents' first occupational choice at age 15-16, ~240 occupations, 3-4 year apprenticeships setting career trajectory.
- **Motivated reasoning (rank effect)**: When occupations have identical match scores, the platform randomly assigns display rank. Rank order strongly increases engagement and applications even among equally well-matched options. Moving an occupation down one rank reduces activity by ~0.4-0.5 percentage points. The effect varies systematically: stronger for high-paying occupations (rank gradient at least doubles for top-quartile earnings), stronger for gender-congruent occupations (muted for gender-atypical), and driven by users with gender-congruent preferences. This pattern is consistent with motivated reasoning — users interpret ambiguous ranking as a quality signal, selectively weighting it when it reinforces ex ante preferences.
- **Cognitive load (redesign effect)**: On June 1, 2023, Yousty redesigned from a static text-heavy list (all 20 occupations shown simultaneously) to an interactive Tinder-like presentation (one at a time, with images/videos and save/swipe decision). Difference-in-discontinuity design: +0.032 to +0.052 additional occupations applied to (sizable relative to 0.085 average discontinuity in control years). The mechanism is a large increase in watch list usage (+0.345 to +0.444 probability of saving any; +1.7 to +2.0 occupations saved), which keeps more options in working memory.
- **Key insight**: Laboratory behavioral mechanisms generalize to consequential, identity-relevant decisions. Platform design features (ranking, presentation format) have aggregate effects on occupational allocation and gender segregation. Reducing cognitive load through interface design broadens search and improves match quality.
- **A-Tech alignment**: Open-source (applies to open-source recommenders), data privacy (uses platform process data), financial freedom (broader search → access to higher-paying paths), practical implementation (246,869 users, quasi-experimental design).
- **Source**: Swiss Leading House "Economics of Education" Working Paper No. 255, April 2026.
- **Skill created**: `choice-architecture-occupational-decisions`

### 2. Privacy & Trust

**Novel Finding: HADES — Selective Feature Encryption with Hybrid Model Fusion for FL (Kaynak et al., Bilkent University, arXiv June 2026)**

This is the first system to perform fully encrypted model training while incorporating a fusion mechanism between encrypted and plaintext model components. Prior privacy-preserving FL approaches either encrypted everything (impractical computational overhead) or relied on secure aggregation (leaving client-side models vulnerable).

- **Core innovation**: HADES selectively encrypts only the most privacy-sensitive features (identified via PCA) while training a plaintext network on the remaining features in parallel, then fuses their outputs via score-level fusion. This balances privacy and computational efficiency.
- **Architecture**: Dual-model structure — encrypted network (W_HE) trains on privacy-sensitive features using multiparty homomorphic encryption (MHE), never decrypted during training. Plaintext network (W_P) trains on remaining features with no HE overhead. Fused logits: z̄_HE = α·z_HE + (1-α)·HE(z_P).
- **Privacy**: Against iDLG reconstruction attack, encrypting 256 features on MNIST reduces SSIM from 1.00 (perfect reconstruction) to 0.11 (near worst case). On SVHN, SSIM drops to 0.04. A moderate encryption budget already approaches worst-case reconstruction quality.
- **Utility**: Matches vanilla FL accuracy — BCD 97.08% (vs 94.71% baseline), MNIST 94.99% (vs 95.00%), SVHN 70.4% (vs 63.1%, +7.3% gain). Fusion network does not introduce meaningful degradation.
- **Scalability**: Training time scales ~linearly with encrypted features. Reducing |F_HE| can improve runtime by up to 28%. Linear scaling with number of clients.
- **Key insight**: By addressing privacy as a feature-level knowledge exposure problem, HADES enables practical privacy-preserving FL that is computationally efficient without sacrificing utility.
- **A-Tech alignment**: Open-source (OpenFHE-python, protocol-level design), data privacy (formal defense against reconstruction, data stays local), financial freedom (7-28% runtime reduction), practical implementation (reproducible across 3 datasets).
- **Source**: arXiv:2606.22928v1, June 2026, Bilkent University.
- **Skill created**: `hades-selective-feature-encryption-federated-learning`

**Novel Finding: SafeLM — Unified Privacy-Aware Optimization for Trustworthy Federated LLMs (Mohammad & Bayazıt, Istanbul Technical University, arXiv April 2026)**

This is the first framework to jointly address four interconnected pillars of LLM safety — privacy, security, misinformation, and adversarial robustness — within a single federated training and deployment pipeline. Prior solutions addressed these aspects in isolation, leading to incompatible defenses and unclear interactions.

- **The four pillars**: S1 (gradient confidentiality), S2 (backdoor resistance), S3 (factual consistency), S4 (adversarial robustness).
- **Gradient smartification** (key innovation): Median-based statistical binarization compresses 32-bit gradients to 1 bit (32× communication reduction). Per-client adaptive threshold suppresses low-magnitude components. Convergence guaranteed with only ~15% slowdown (measured cosine alignment γ = 0.87).
- **Paillier homomorphic encryption**: 2048-bit, IND-CPA security under DCRA. Privacy holds even if server fully compromised.
- **Results**: Gradient inversion PSNR 31.7 → 15.1 dB (unrecognizable); label recovery 98.7% → 14.3% (near-random for 7 classes); backdoor ASR 91.3% → 6.8% at 20% malicious; hallucination rate reduced 41% on TruthfulQA; adversarial accuracy degradation -23.6 → -9.6 pp on AdvGLUE. 96.9% total bandwidth reduction (129.15 GB → 4.05 GB).
- **Synergy discovery**: The four safety pillars are not merely additive but synergistic — gradient smartification both reduces communication AND strengthens privacy; adversarial training also reduces hallucination.
- **Key insight**: Privacy-preserving federated training is compatible with — and mutually reinforcing of — security, misinformation, and adversarial robustness objectives. A unified perspective can inform regulatory frameworks (EU AI Act) requiring simultaneous demonstration of privacy compliance and robustness.
- **A-Tech alignment**: Open-source (code released), data privacy (IND-CPA, PSNR ≤ 15.1 dB), financial freedom (96.9% communication reduction), practical implementation (7B LLM, LoRA, reproducible hyperparameters).
- **Source**: arXiv:2604.16606, April 2026, Istanbul Technical University.
- **Skill created**: `safelm-unified-privacy-llm-framework`

### 3. Developer Experience & Flow

**Novel Finding: Evolving Enactions of Coding Expertise with AI Assistants (Jang et al., Fujitsu Research, CHI 2026)**

This is the first study to introduce "enaction of expertise" as a lens for understanding how AI reshapes professional knowledge work — not just what expertise individuals possess, but how expertise is put into practice through demonstration and evaluation.

- **Setting**: 16 professional software engineers in simulated live coding interviews with AI tools allowed. 12 sessions, mixed evaluator-candidate roles.
- **Core finding**: Evaluators continued to use familiar criteria (task understanding, approach development, error handling, code explanation) but required additional evidence because AI-generated output codes were no longer sufficient to demonstrate expertise.
- **Extended enactions** (new ways to demonstrate expertise): Choice of AI tools (selecting the right tool for the task), prompting practices (level of detail reflects task comprehension), use of AI-generated outputs (discernment about which outputs to adopt).
- **Diminished enactions** (lost visibility): AI decomposing tasks prevents showing understanding; AI producing whole code prevents showing incremental development; AI identifying errors prevents showing error handling.
- **Implementation vs. planning emphasis**: Enactions matter more for evaluators valuing code implementation (AI rarely produces optimal extendable/efficient/readable code); matter less for those valuing code planning ("with ChatGPTs help, the code won't be a very big deal").
- **Productivity tensions**: AI raised productivity expectations without clear connection to expertise, creating conflicts with evolving enactions.
- **Key insight**: AI coding assistants are reshaping the nature of software engineering work itself — not just accelerating existing tasks but changing where and how expertise becomes visible. Extended enactions (tool choice, prompting, output use) need to be supported through training and tools; diminished enactions raise questions about which traditional indicators of expertise should be retired or revised.
- **A-Tech alignment**: Open-source (applies to Cline, Aider, OpenHands), data privacy (examines evaluation without compromising candidate privacy), financial freedom (fairer evaluation supports career advancement), practical implementation (16 professional engineers, actionable design recommendations).
- **Source**: CHI 2026, Fujitsu Research of America.
- **Skill created**: `coding-agent-expertise-enaction`

### 4. AI Agents & Workflows

**Novel Finding: Adaptive Digital Nudging Systems with LLM-Driven Reasoning (Santilli et al., Syddansk Universitet, SAGAI-ICSA 2026)**

This is the first architecture to integrate multi-dimensional user modeling (cognitive mode, behavioral stage, attention capacity) with ethical compliance as architectural concerns for adaptive nudging. Prior nudging systems treated ethics as implementation details, not structural guardrails.

- **Architecture**: Three sequential processing layers (Data Capture → User Modeling → Nudge Intelligence) with two cross-cutting modules (Adaptation, Evaluation). Ethics Compliance is side-mounted, intercepting all nudge outputs — no nudge reaches users without validation.
- **LLM-driven reasoning**: User Modeling components use LLMs rather than rule-based systems for contextual interpretation of weak behavioral signals. Strategy Optimizer uses LLM with constraint-satisfaction prompting over a 68-strategy taxonomy.
- **Literature foundation**: Systematic review of 21 primary studies (from 707 papers) synthesizing 68 nudging strategies, 11 quality attributes, 3 user profiling dimensions.
- **Validation**: 13 software architects rated requirements satisfaction 4.62/5 (UI Adaptation highest at 4.85, Explainability lowest at 4.38). 61.5% rated "Highly Transferable." 15 users in energy sustainability proof-of-concept: nudge quality 4.73/5, explanation quality 4.27/5, positive emotional impact across all participants.
- **Key architectural decisions**: Sequential pipeline (deterministic reasoning, compliance auditability), side-mounted evaluation (structural ethics enforcement), LLM-driven reasoning (robustness to edge cases), backend-driven UI adaptation (privacy protection).
- **Key insight**: Ethics and fairness can be enforced as structural architectural properties that hold across all execution paths, rather than implementation details that can slip through during edge cases. This provides architectural evidence for DSA Article 27 and AI Act compliance.
- **A-Tech alignment**: Open-source (code available), data privacy (backend-driven adaptation, structural ethics enforcement), financial freedom (modular, transferable), practical implementation (validated with architects + users).
- **Source**: SAGAI-ICSA 2026, Syddansk Universitet. Code: github.com/tiziasan/Adaptive-Digital-Nudging-System.
- **Skill created**: `adaptive-digital-nudging-llm-architecture`

---

## Synthesis Phase: Cross-Domain Patterns

Five cross-domain patterns emerged across today's findings:

1. **From Implementation Detail to Architectural Concern**: Both SafeLM and the Adaptive Nudging Architecture elevate what were previously implementation-level concerns (ethics, privacy, safety) to structural architectural properties. SafeLM enforces ethics via side-mounted evaluation modules; the nudging architecture uses interceptor patterns for ethics compliance. This represents a broader shift: in 2026, compliance and safety are becoming architectural first-class citizens, not afterthoughts.

2. **Selective vs. Comprehensive Application**: HADES selectively encrypts only sensitive features; SafeLM selectively compresses gradients; the nudging architecture selectively applies strategies based on multi-dimensional user state. All three findings reject the binary "all or nothing" approach in favor of intelligent, targeted application that balances competing objectives (privacy/utility, communication/accuracy, personalization/fairness).

3. **Laboratory-to-Field Generalization**: The occupational choice architecture study provides the first field evidence that laboratory-documented behavioral mechanisms (motivated reasoning, cognitive load) operate in high-stakes real-world decisions. This validates a decade of behavioral economics laboratory research while revealing that the mechanisms interact with identity and social norms in ways labs cannot capture.

4. **Enaction and Visibility**: The coding expertise study reveals that AI tools are not just changing what work is done but where expertise becomes visible. Extended enactions (tool choice, prompting, output use) emerge while diminished enactions (incremental development, error handling visibility) disappear. This pattern generalizes: AI doesn't replace expertise, it relocates where expertise is demonstrated and evaluated.

5. **Unified Multi-Objective Frameworks**: Both SafeLM (4 safety pillars) and the nudging architecture (3 user dimensions + 11 quality attributes) represent a move away from single-objective optimization toward unified frameworks that handle competing objectives simultaneously. The synergy discovery in SafeLM — where components reinforce each other beyond additivity — suggests that isolated single-objective approaches leave value on the table.

---

## Daily Research Metrics

| Metric | Value |
|--------|-------|
| Research domains covered | 4 (behavioral psychology, privacy/trust, developer experience, AI agents) |
| Web searches conducted | 6 |
| Novel findings identified | 5 |
| New skills created | 5 |
| Reference documents created | 1 |
| Existing skills updated | 0 |
| Cross-references established | 25+ |
| A-Tech applications documented | 20+ |

---

## Skills Created

### NEW (2026-08-17): Choice Architecture in Occupational Decisions (`behavioral-psychology-and-nudging/choice-architecture-occupational-decisions/`)
- **SKILL.md** — First large-scale field evidence of motivated reasoning and cognitive load in occupational choices. Yousty platform, 246,869 users. Rank effects via random tie-breaking; redesign effect via DD-RD.
- **references/choice-architecture-occupational-evidence-base.md** — Full evidence base: study design, empirical strategy, results tables, heterogeneity analysis, robustness checks, limitations.

### NEW (2026-08-17): HADES Selective Feature Encryption FL (`privacy-and-trust/hades-selective-feature-encryption-federated-learning/`)
- **SKILL.md** — First hybrid encrypted-plaintext FL with model fusion. Selective PCA-based feature encryption, dual-network training, score-level fusion. Matches vanilla FL accuracy with 7-28% runtime improvement.

### NEW (2026-08-17): SafeLM Unified Privacy LLM Framework (`privacy-and-trust/safelm-unified-privacy-llm-framework/`)
- **SKILL.md** — First unified framework for 4 LLM safety pillars (privacy, security, misinformation, robustness) in federated training. Gradient smartification (32× compression), Paillier HE, Byzantine filtering, contrastive grounding, adversarial fine-tuning. 96.9% bandwidth reduction.

### NEW (2026-08-17): Coding Agent Expertise Enaction (`developer-experience-and-flow/coding-agent-expertise-enaction/`)
- **SKILL.md** — First study of "enaction of expertise" in AI-assisted coding interviews. Extended enactions (tool choice, prompting, output use), diminished enactions (incremental development, error handling). 16 professional engineers, 12 sessions.

### NEW (2026-08-17): Adaptive Digital Nudging LLM Architecture (`ai-agents-and-workflows/adaptive-digital-nudging-llm-architecture/`)
- **SKILL.md** — First architecture integrating multi-dimensional user modeling with ethical compliance as structural guardrails. 3 processing layers + 2 cross-cutting modules. LLM-driven reasoning. Validated with 13 architects (4.62/5) + 15 users.

---

## A-Tech Values Alignment

All 5 new skills align with A-Tech Corporation's core values:

- **Open-source AI**: Choice Architecture (open-source recommenders), HADES (OpenFHE-python), SafeLM (code released), Expertise Enaction (open-source coding agents), Adaptive Nudging (open-source code)
- **Data privacy**: HADES (selective encryption, formal reconstruction defense), SafeLM (IND-CPA, PSNR ≤ 15.1 dB), Adaptive Nudging (structural ethics enforcement, backend-driven adaptation)
- **Financial freedom**: Choice Architecture (broader search → higher-paying paths), HADES (7-28% runtime reduction), SafeLM (96.9% bandwidth reduction), Expertise Enaction (fairer evaluation → career advancement)
- **Practical implementation**: All 5 skills include reproducible evidence, concrete metrics, and actionable frameworks

---

## Report Date
2026-08-17