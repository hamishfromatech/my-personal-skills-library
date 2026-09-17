# UX Laws Evidence Base — AI Translation

This reference collects the origin citations, key empirical findings, and AI-specific evidence for each of the ten laws translated in `SKILL.md`. It is the backing material for design-review arguments: when a law is cited, the source and study are here.

---

## 1. Jakob's Law

**Origin:** Jakob Nielsen, Nielsen Norman Group. "Users Spend Most of Their Time on Other Sites." The law: users expect your site to work the same way as all the other sites they already know.

**Key finding:** Habit transfer dominates. Users bring expectations formed across thousands of prior interactions. Violating a convention imposes a learning cost that is rarely repaid.

**AI-specific evidence / corollaries:**
- Generative UI research (2026) found that when interfaces reinvent controls every render, users cannot form a stable mental model — task time and error rates rise. The `generative-ui-dynamic-interface-design` skill's "Predictable Dynamism" principle is Jakob's Law applied to generative surfaces.
- The `calm-technology-ai-coding` skill documents that chat-based agentic coding tools that reinvent the interaction chrome (vs. reusing inline diff/sidebar patterns) increase idle time (Becker study: idle time approximately doubled).

**AI rule:** Reuse familiar interaction patterns. Let the novelty live in the *output content*, not in the *interaction chrome*.

---

## 2. Hick's Law

**Origin:** W. E. Hick (1952), "On the rate of gain of information," Quarterly Journal of Experimental Psychology. Ray Hyman (1953) extension. The law: reaction time increases logarithmically with the number of options.

**Key finding:** Decision time ≈ a + b·log₂(n+1), where n is the number of choices. The practical implication: doubling options adds a constant (not a proportional) delay, but the delay is real and compounds when the user must choose repeatedly.

**AI-specific evidence / corollaries:**
- The `ai-explanation-ability-cue-trap` skill (Saßmannshausen et al., 2026) found that reliance was *not* moderated by task difficulty — meaning users accept AI suggestions at similar rates regardless of how hard the task is, i.e., they are not using the number of presented options as a difficulty signal. Presenting too many options invites unexamined acceptance.
- Agent frameworks that surface "all possible next actions" create decision paralysis; the `agentic-coding-trends-2026` skill notes that effective delegation requires the agent to *propose* a default, not enumerate the action space.

**AI rule:** Cap AI-presented choices at 3–4. Always offer a default. Collapse the rest behind a "more" affordance.

---

## 3. Doherty Threshold

**Origin:** Walter J. Doherty and Ahrvind J. Thadani (IBM, 1982), "The Economic Value of Rapid Response Time." The threshold: computing interaction feels instantaneous when response is under 400 ms.

**Key finding:** Below 400 ms, users perceive the system as responding "instantly" and stay in flow. Above ~1 s, attention drifts; above ~10 s, the user context-switches away. The cost is not just wait time — it is the flow disruption and the re-orientation cost on return.

**AI-specific evidence / corollaries:**
- AI generation latency routinely exceeds 400 ms (often several seconds). Streaming responses, skeleton screens, and progressive (token-by-token) rendering restore the *perceived* instantaneousness even when the full generation takes longer.
- The `cognitive-load-reduction-for-ide` and `calm-technology-ai-coding` skills both treat perceived-latency reduction (not just actual latency) as a flow-preservation technique.

**AI rule:** The first useful signal must appear within 400 ms — real or perceived via streaming, skeletons, and progressive results. Anything longer needs a perceived-progress indicator.

---

## 4. Aesthetic-Usability Effect

**Origin:** Kurosu & Kashimura (1995), Hitachi study; popularized by Don Norman. The effect: users judge more aesthetically pleasing interfaces as more usable, even when the underlying usability is identical.

**Key finding:** Aesthetic appeal increases perceived ease of use, perceived efficiency, and tolerance for minor flaws. The effect is robust and cross-cultural.

**AI-specific evidence / corollaries:**
- This is the *mechanism* behind the `ai-explanation-ability-cue-trap`: a polished AI explanation (or a polished AI surface) is judged more competent (perceived ability rises), which raises trust and reliance — *without* raising understanding. The polish is an ability cue.
- The implication is not "don't polish." It is "polish + substance + uncertainty disclosure." A polished shallow AI feature manufactures unearned trust; a polished deep AI feature earns it. The polish must not substitute for the substance.

**AI rule:** Invest in polish *and* in genuine depth. Add uncertainty disclosure so the polish does not become an unearned ability cue.

---

## 5. Miller's Law

**Origin:** George A. Miller (1956), "The Magical Number Seven, Plus or Minus Two," Psychological Review. The law: working memory holds approximately 7 ± 2 chunks.

**Key finding:** The capacity limit is on *chunks* (grouped units), not raw items. Chunking (grouping related items) expands effective capacity. Without chunking, 7+ ungrouped items overflow working memory and the user starts scanning instead of processing.

**AI-specific evidence / corollaries:**
- Chain-of-thought reasoning traces that dump 10–15 steps at full depth overflow working memory; users skim and miss the critical step. The `chain-of-thought-ux-reasoning-transparency` skill's transparency spectrum collapses reasoning to summary → steps → full, letting the user expand only what they need.
- The `cognitive-load-reduction-ai-scaffolding` skill treats chunking as a core scaffolding technique for AI-generated content.

**AI rule:** Present AI reasoning as ≤ 7 chunks. Group steps. Collapse detail by default. Let the user expand.

---

## 6. Fitts's Law

**Origin:** Paul M. Fitts (1954), "The Information Capacity of the Human Motor System," Journal of Experimental Psychology. The law: time to acquire a target is a function of the distance to the target and the target size.

**Key finding:** MT = a + b·log₂(2D/W), where D is distance and W is target width. Large, close targets are acquired fast; small, distant targets are slow and error-prone. The practical implication: primary actions should be large and adjacent to what they act on.

**AI-specific evidence / corollaries:**
- AI coding tools that place accept/reject controls in a remote toolbar (far from the inline diff) impose a Fitts penalty on every accepted suggestion — slow, and mis-clicks leak changes the user didn't intend.
- The `calm-technology-ai-coding` skill's "pass-through" pattern keeps the action adjacent to the artifact it modifies.

**AI rule:** Accept/reject/edit controls for AI suggestions must be large and adjacent to the suggestion, not floating in a remote toolbar.

---

## 7. Tesler's Law (Law of Conservation of Complexity)

**Origin:** Larry Tesler (Apple / Xerox PARC). The law: every application has an inherent amount of complexity that cannot be removed or hidden; it must be dealt with somewhere — either in the system or in the user's experience.

**Key finding:** You can move complexity, not eliminate it. Pushing it to the user (e.g., "the AI handles it, trust it") does not remove it — it relocates it to the user's later debugging and verification burden.

**AI-specific evidence / corollaries:**
- The `devex-verification-bottleneck-framework` skill is Tesler's Law made operational: AI moved the *writing* complexity to the *verification* side. The irreducible verification complexity must be relocated to the *system* (adversarial agentic test layers, static analysis, cognitive guardrails), not left to the human reviewer's unaided eyes.
- The Sonar 2026 survey (>33% of devs say reviewing AI code takes more effort than reviewing human code) is the quantitative measure of complexity relocated to the user without system support.

**AI rule:** The irreducible verification complexity cannot be eliminated — move it to the *system* (automated checks), not to the *user* (manual review).

---

## 8. Postel's Law (Robustness Principle)

**Origin:** Jon Postel (RFC 793, TCP specification, 1981). "Be liberal in what you accept, and conservative in what you send."

**Key finding (origin):** interoperability is maximized when a system accepts a wide range of inputs but produces strictly conformant outputs. The principle generalizes to UI/UX: accept messy user input, produce clean system output.

**AI-specific evidence / corollaries:**
- An AI agent that is *strict* about prompt format (rejects loose input) but *loose* in its generated output (untyped, untested, unverified code) inverts Postel's Law — it's hard to use and unreliable to trust.
- The `agent-experience-design-2026` skill's schema-first design principle is Postel's "conservative in what you send" applied to agent APIs: agents should produce strict, well-formed, verifiable output.

**AI rule:** Accept messy, ambiguous user intent liberally; produce strict, well-formed, verifiable output.

---

## 9. Parkinson's Law

**Origin:** Cyril Northcote Parkinson (1955, The Economist), "Parkinson's Law." "Work expands so as to fill the time available for its completion."

**Key finding:** Without a binding constraint, a task consumes all allotted resources. The law is empirical, observed across bureaucracies, projects, and now AI agent runs.

**AI-specific evidence / corollaries:**
- AI agents with no explicit time/token/step budget expand indefinitely — "thinking" with no deadline, consuming tokens and compute without a stopping condition.
- The `agentic-coding-trends-2026` skill's long-running-agent pattern requires checkpoints and human-approval gates — a form of binding the task to a budget.
- Visible budgets (token meter, step counter, soft deadline) let the user plan around the agent's work and prevent the unbounded expansion.

**AI rule:** Set explicit budgets (token, time, step count) for AI tasks and surface them to the user.

---

## 10. Peak-End Rule

**Origin:** Daniel Kahneman, Barbara Fredrickson, Charles Schreiber, and Donald Redelmeier (1993), "When More Pain Is Better Than Less," Psychological Science. The rule: people judge an experience largely by how it felt at its peak and at its end, not by the sum or average of every moment.

**Key finding:** The colonoscopy study showed that adding a slightly less uncomfortable period at the end improved overall retrospective ratings, despite more total discomfort. The memory is dominated by the peak and the end.

**AI-specific evidence / corollaries:**
- The `peak-end-rule-demo-design` skill applies this to demos and onboarding (engineer the insight peak; engineer the clean ending).
- For AI agent sessions, a weak ending (an ambiguous "done?" with no summary, no confirmation, no reversible commit) erases a strong middle. The user remembers the uncertainty, not the helpful generation.

**AI rule:** Engineer the peak (the moment of insight or resolution) and the ending (a clean, confirmed, reversible close). A weak ending erases a strong middle.

---

## Resolution Logic for the Four AI-Specific Tensions

### Tension 1 — Novelty vs. Familiarity (Jakob vs. Generative UI)
Generative UI's premise is novelty; Jakob's Law says users expect familiarity. Resolution: separate the *chrome* (controls, navigation, accept/reject patterns) from the *content* (the generated layout, content, suggestions). The chrome stays familiar (Jakob); the content is allowed to be novel. This is the `generative-ui-dynamic-interface-design` "Predictable Dynamism" principle.

### Tension 2 — Polish vs. Substance (Aesthetic-Usability vs. Ability-Cue Trap)
Polish builds trust (Aesthetic-Usability), but polish on a shallow feature manufactures unearned trust (the ability-cue trap). Resolution: invest in both polish and substance, and add uncertainty disclosure. The polish must be matched by depth; the depth must be signaled by the polish. Polish without disclosure is the anti-pattern.

### Tension 3 — Speed vs. Verification (Doherty vs. Tesler)
Doherty says make it feel instant; Tesler says verification is irreducible. Resolution: the speed budget applies to the *response* (stream it, make it feel instant), not to the *human's check* (make verification explicit and system-assisted). The two operate on different layers: generation is fast, verification is thorough.

### Tension 4 — Few Options vs. AI Breadth (Hick vs. Agent Capability)
Hick says few options; agents can do many things. Resolution: show ≤ 4 next-actions by default, with one highlighted as the default; gate the rest behind a "more" affordance. The agent's breadth is available; it is not imposed on the user's attention all at once.

---

## Citations

1. Nielsen, J. (n.d.). "Jakob's Law of the Internet User Experience." Nielsen Norman Group.
2. Hick, W. E. (1952). "On the rate of gain of information." Quarterly Journal of Experimental Psychology, 4(1), 11–26.
3. Hyman, R. (1953). "Stimulus information as a determinant of reaction time." Journal of Experimental Psychology, 45(3), 188–196.
4. Doherty, W. J., & Thadani, A. J. (1982). "The economic value of rapid response time." IBM Systems Journal, 21(3), 302–312.
5. Kurosu, M., & Kashimura, K. (1995). "Apparent usability vs. inherent usability: experimental analysis on the determinants of the apparent usability." CHI '95 Conference Companion.
6. Norman, D. (2002). "Emotion & Design: Attractive things work better." Interactions, 9(4), 36–42.
7. Miller, G. A. (1956). "The magical number seven, plus or minus two." Psychological Review, 63(2), 81–97.
8. Fitts, P. M. (1954). "The information capacity of the human motor system." Journal of Experimental Psychology, 47(6), 381–391.
9. Tesler, L. (circa 1980s). "The Law of Conservation of Complexity." Apple / Xerox PARC.
10. Postel, J. (1981). RFC 793: "Transmission Control Protocol." "Be liberal in what you accept, and conservative in what you send."
11. Parkinson, C. N. (1955). "Parkinson's Law." The Economist (Nov 19, 1955).
12. Kahneman, D., Fredrickson, B. L., Schreiber, C. A., & Redelmeier, D. A. (1993). "When more pain is better than less." Psychological Science, 4(6), 401–405.
13. Saßmannshausen, T., Burggräf, P., Hassenzahl, M., & Sauer, C. R. (2026). "Effects of AI explanations on trust and reliance." Ergonomics.
14. Sonar / SonarSource (2026). "2026 State of Code Developer Survey." N=1,100+.
15. Becker et al. — agentic coding idle-time study (referenced in `calm-technology-ai-coding`).
16. Domenech Burin, L. (2026). arXiv:2607.05413 — Sovereign Tech Fund causal impact (referenced in `sovereign-tech-fund-causal-impact`).