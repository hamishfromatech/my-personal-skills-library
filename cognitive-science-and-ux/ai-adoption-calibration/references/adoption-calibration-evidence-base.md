# AI Adoption Calibration Evidence Base

This reference collects the literature and cross-skill evidence backing the `ai-adoption-calibration` skill: the automation-bias and algorithmic-aversion research, the verifiability taxonomy, the loss-aversion asymmetry, the confidence-gap pattern, and the links to each cross-referenced A-Tech skill.

---

## 1. Automation Bias

**Definition:** the systematic tendency to over-rely on automated systems, accepting their output as correct without adequate verification, often even when the output is wrong.

**Foundational sources:**
- Mosier, K. L., & Skitka, L. J. (1996). "Human decision makers and automated decision aids: Made we not be in need." In *Human factors in aviation.* The original aviation-CREW-study framing of automation bias as "commission errors" (acting on incorrect automation output) and "omission errors" (failing to act when automation fails).
- Parasuraman, R., & Riley, V. (1997). "Humans and automation: Use, misuse, disuse, abuse." *Human Factors*, 39(2), 230–253. The "misuse–disuse–abuse" framework that maps directly onto the bias-aversion spectrum in this skill (misuse = automation bias; disuse = algorithmic aversion).

**AI-specific corollaries:**
- The `ai-explanation-ability-cue-trap` skill (Saßmannshausen et al., 2026) isolates the *mechanism*: brief AI rationales raise reliance via perceived ability, NOT understanding — the user accepts because the AI *seems* competent, not because the user *verified*. This is the engine of AI-specific automation bias.
- The `devex-verification-bottleneck-framework` skill (Sonar 2026 survey: 96% don't fully trust AI code, but only 48% always verify) is the quantitative measure of the bias-verification gap — high adoption, low verification.

---

## 2. Algorithmic Aversion

**Definition:** the systematic tendency to reject or under-use algorithms, even when the algorithm is demonstrably more accurate than the human, often triggered by a single observed error.

**Foundational sources:**
- Dietvorst, B. J., Simmons, J. P., & Massey, C. (2015). "Algorithm aversion: People erroneously avoid algorithms after seeing them err." *Journal of Experimental Psychology: General*, 144(1), 114–126. The key finding: people abandon an algorithm after seeing a single error, even when the algorithm outperforms the human overall. The aversion is triggered by *observed* error, not by the algorithm's actual accuracy.
- Logg, J. M., Minson, J. A., & Moore, D. A. (2019). "Algorithm appreciation: People prefer algorithmic to human judgment." *Organizational Behavior and Human Decision Processes*. The complement — in some domains (estimation, numerical tasks) people *over-trust* algorithms. The bias-aversion direction depends on task type and the user's domain confidence.

**The loss-aversion asymmetry:** one visible AI error weighs more heavily on trust than many successes do positively (prospect-theory loss aversion applied to algorithm trust). This is why algorithmic aversion is so sticky: the user remembers the one failure, not the ninety-nine successes. The `trust-calibration-ux-pattern` skill's "trust builds slowly and breaks quickly" asymmetry is this mechanism.

---

## 3. The Verifiability Taxonomy (Task-Side Variable)

The verifiability dimension — whether the user can detect the AI's error — is the variable that bounds safe delegation. It is distinct from the AI's accuracy (a model can be highly accurate on a task the user cannot verify, and that delegation is still unsafe).

**Related frameworks in the skill library:**
- `ai-agents-and-workflows/verifiability-driven-automation` — the skill that matches automation to verifiable domains; this skill adds the *user-ability* dimension (verifiability is not just a property of the task, but of the task × the user's error-detection ability).
- `cognitive-science-and-ux/cognitive-offloading-ladder` — Rung 1 (raw replacement, epistemic atrophy) is what happens when the user delegates an unverifiable task and cannot climb back; the ladder is the capability-retention complement to the verifiability gate.
- `cognitive-science-and-ux/proof-first-ux-accountability` — evidence-before-output as a structural verifiability aid.

---

## 4. The Confidence Gap (User-Side Variable)

The user's confidence in their own error-detection ability is the other half of the matrix. Two patterns distort it:

- **Dunning-Kruger pattern (low ability, high confidence):** the user believes they can verify AI output they actually cannot — the ability-cue trap makes this worse by inflating perceived ability. This user over-delegates (automation bias on unverifiable tasks).
- **Imposter pattern (high ability, low confidence):** the user *can* verify but doubts they can — the user under-delegates on verifiable tasks (algorithmic aversion on tasks they could safely delegate).

**Calibration principle:** delegation is safe when the user's confidence in their error-detection ability *matches* their actual ability, AND the task is verifiable for someone at that ability level. Mismatch in either direction produces a failure mode.

**Related skill:** `behavioral-psychology-and-nudging/boosting-empowering-behavior-change` — the boosting approach builds the competence that closes the gap (the "acquire the expertise to check it" path for effectively-unverifiable tasks).

---

## 5. The Onboarding Sequence Principle

The "sequence from verifiable to unverifiable" onboarding principle is drawn from two converging ideas in the library:

- `cognitive-science-and-ux/ai-ux-laws-translation` (Tesler's Law) — verification complexity is irreducible; onboarding must teach the user *where* that complexity lives and *how* to handle it, not pretend it has been eliminated.
- `developer-experience-and-flow/onboarding-acceleration-protocol` and `cognitive-science-and-ux/micro-habit-progressive-disclosure-ide` — progressive disclosure applied to onboarding: start with the simplest, most-verifiable interaction and add complexity as the user's calibration improves.

The principle: a user's *first* AI experience should never be a high-stakes, unverifiable task. A vivid success on an unverifiable task produces blind trust (automation bias); a vivid error produces permanent aversion. Sequencing from verifiable to unverifiable builds calibrated trust — trust backed by the user's own verified-success experience.

---

## 6. Cross-Skill Evidence Map

| Cross-referenced skill | What it contributes to adoption calibration |
|------------------------|----------------------------------------------|
| `ai-explanation-ability-cue-trap` | The mechanism behind automation bias (perceived ability cue → unearned trust) |
| `trust-calibration-ux-pattern` | The UI-level trust-escalation mechanics (per-domain track records, trust repair, under-trust as failure) and the trust-builds-slowly/breaks-quickly asymmetry |
| `cognitive-offloading-ladder` | The capability-retention framework (climb the ladder to avoid skill erosion from over-delegation) |
| `chain-of-thought-ux-reasoning-transparency` | Reasoning visibility reduces algorithmic aversion (the user can trace *why*) |
| `algorithmic-aversion-defense` (community-and-growth) | The community/growth-side treatment of aversion |
| `devex-verification-bottleneck-framework` | The verification complexity this skill relocates to the system; the Sonar 96%/48% adoption-verification gap |
| `boosting-empowering-behavior-change` | Building the competence that lets the user detect errors (the "acquire the expertise" path) |
| `verifiability-driven-automation` | The task-side verifiability framework this skill extends with the user-ability dimension |
| `ai-ux-laws-translation` | Tesler's Law (verification is irreducible) and Doherty/Parkinson (budgets) |

---

## Sources

1. Mosier, K. L., & Skitka, L. J. (1996). "Human decision makers and automated decision aids." In *Human factors in aviation.*
2. Parasuraman, R., & Riley, V. (1997). "Humans and automation: Use, misuse, disuse, abuse." *Human Factors*, 39(2), 230–253.
3. Dietvorst, B. J., Simmons, J. P., & Massey, C. (2015). "Algorithm aversion: People erroneously avoid algorithms after seeing them err." *Journal of Experimental Psychology: General*, 144(1), 114–126.
4. Logg, J. M., Minson, J. A., & Moore, D. A. (2019). "Algorithm appreciation." *Organizational Behavior and Human Decision Processes.*
5. Saßmannshausen, T., Burggräf, P., Hassenzahl, M., & Sauer, C. R. (2026). "Effects of AI explanations on trust and reliance." *Ergonomics.* (The ability-cue mechanism behind AI-specific automation bias.)
6. Sonar / SonarSource (2026). "2026 State of Code Developer Survey." N=1,100+. (The 96% don't trust / 48% always verify adoption-verification gap.)
7. Buçinca, Z. et al. (2021). "To trust or to think: cognitive forcing functions." (Explanations can increase reliance on wrong AI.)