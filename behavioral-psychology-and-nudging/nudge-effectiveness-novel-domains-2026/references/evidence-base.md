# Nudge Effectiveness in Novel Domains (2026) — Evidence Base

This file contains the full detail for three 2026 studies synthesized in the `nudge-effectiveness-novel-domains-2026` skill. Each entry covers study design, sample, interventions, results, implications, and A-Tech alignment.

---

## Study 1: Agricultural Policy — ILVO Soil Passport Eco-Scheme RCT (NULL)

**Authors:** ILVO (Instituut voor Landbouw-, Visserij- en Voedingsonderzoek / Flanders Research Institute for Agriculture, Fisheries and Food)
**Title:** Digital behavior change interventions for farmer enrollment in the Soil Passport eco-scheme
**Journal:** Journal of Behavioral and Experimental Economics, Vol. 123, July 2026
**DOI:** 10.1016/j.socec.2026.102580

### Study Design
- Large-scale preregistered randomized controlled trial
- Multiple treatment arms testing distinct digital behavior change interventions (BCIs) against control conditions
- Preregistered analysis plan (gold-standard transparency for a field RCT in agricultural policy)
- Field setting: real-world farmer enrollment decisions, not a lab analogue
- Outcome: enrollment in the Soil Passport eco-scheme (a Flemish agri-environmental scheme)

### Sample
- N = 14,285 farmers in Flanders (Belgium)
- One of the largest nudge RCTs conducted in an agricultural policy context
- Power sufficient to detect small effects — the null is not attributable to underpowering

### Interventions
Three digital behavior change interventions tested, plus an information-provision arm:

1. **Demonstration** — showing farmers how the eco-scheme works and what enrollment involves (procedural transparency / modeling)
2. **Dynamic social norms** — communicating evolving/updated social-norm information about peer farmer participation (dynamic norm feedback, a variant of social-proof nudging)
3. **Loss framing** — framing the consequences of non-enrollment as a loss rather than framing enrollment as a gain (prospect-theory-informed framing nudge)
4. **Information provision alone** — plain factual information about the eco-scheme without any behavioral-design layer (a "pure information" control to separate information effects from nudge effects)

All interventions were delivered digitally (cold-channel, no relational intermediary).

### Results
- **ALL NULL RESULTS.** No statistically significant effect of any intervention on eco-scheme enrollment.
- No effect of demonstration on enrollment.
- No effect of dynamic social norms on enrollment.
- No effect of loss framing on enrollment.
- No effect of information provision alone on enrollment.
- The nulls held across all treatment arms — not a single intervention moved enrollment.

### Implications
- **Digital BCIs are insufficient for eco-scheme adoption.** The authors conclude that cold digital behavior change interventions — even well-designed ones using established techniques (demonstration, social norms, loss framing) — do not move farmer enrollment in a high-friction agri-environmental scheme.
- **Information is not the binding constraint.** The null for the information-provision arm indicates that farmers are not enrolling because they lack information; the barrier is elsewhere (bureaucratic cost, opportunity cost, distrust, structural friction).
- **Need for complementary structural and relational policy approaches.** The authors recommend layering structural interventions (default enrollment, payment design, simplified compliance) and relational interventions (extension officers, peer networks, trusted advisory services) on top of any digital BCI.
- **Consistency with the bias-corrected nudge literature.** The null aligns with the Beermann et al. (2024) green-nudge zero-effect-after-correction finding and the Hu et al. (2025) bias-corrected d ≈ 0.004 average (see `nudge-effectiveness-reality-check`). Environmental nudges in high-friction policy contexts appear to have near-zero true effects when delivered digitally without structural or relational support.
- **Implication for nudge design in novel high-friction domains.** When the target behavior is an ongoing bureaucratic commitment with real opportunity costs, and the channel is cold-digital, expect nulls. Do not deploy a digital BCI alone; design the structural-relational-nudge stack (see SKILL.md Step 4).

### A-Tech Alignment
- **A-Coder (sustainability/green compute):** Do not expect a portal banner or email nudge to move developers toward a sustainability program (carbon-aware scheduling, green compute defaults). The ILVO null is direct evidence that digital environmental nudges fail in high-friction enrollment. Pair any nudge with a structural default (opt-out green compute) and relational support (team champion, office hours).
- **Be Practical (policy advisory):** When advising businesses on adopting open-source governance or AI-safety policies that resemble eco-scheme enrollment (ongoing, bureaucratic, opportunity-cost-laden), do not recommend a nudge-only approach. Recommend default-enrollment structures plus a consultant-led workshop (the OPPBTP model from Study 2).
- **General:** The ILVO null is the cautionary benchmark for any novel-domain nudge proposal that relies on a cold-digital channel for a high-friction behavior. Use it to challenge proposals that assume a digital nudge will suffice.

---

## Study 2: Construction Safety — OPPBTP DUERP Workshop Action Research (POSITIVE)

**Authors:** Clapier, Herrbach, Lérat-Pytlak & Lombardot
**Title:** Integrating nudges into a DUERP Workshop consultancy service for construction-sector prevention
**Journal:** European Management Journal, May 2026
**DOI:** 10.1016/j.emj.2026.05.005

### Study Design
- Action research (not a blinded RCT) conducted with OPPBTP — France's national prevention body for the construction industry
- The nudge intervention was integrated into an existing consultancy service called the "DUERP Workshop" (DUERP = Document Unique d'Évaluation des Risques Professionnels, the mandatory workplace risk assessment document in France)
- Design logic: **diagnosis-mechanism-intervention** — the consultants first diagnose the firm's prevention gaps, then identify the behavioral mechanism that explains the gap, then select and deliver the nudge matched to that mechanism
- Reproducible approach: the design logic is documented and transferable to other prevention bodies or consultancy contexts

### Sample
- Construction firms participating in the OPPBTP DUERP Workshop consultancy service
- Firm-level unit of analysis (the behavior is the firm formalizing/updating its prevention plan, not an individual worker behavior)
- Exact sample size not specified in the available summary; treated as action research with a practical sample rather than a powered RCT

### Interventions
- **Nudged prevention training** embedded within the DUERP Workshop consultancy service
- The nudges were not generic messages but were selected via the diagnosis-mechanism-intervention logic: each firm received nudges matched to its diagnosed behavioral gap
- The consultancy relationship (OPPBTP consultant working with the firm) was the delivery channel — a high-relational-proximity channel, in contrast to the ILVO cold-digital channel

### Results
- **POSITIVE.** Nudged prevention training significantly increased the likelihood that firms:
  - **Formalized** their prevention plans (moved from informal/absent to documented)
  - **Updated** existing prevention plans (moved from stale to current)
  - **Specified preventive actions** (moved from vague risk listing to concrete, actionable preventive measures)
- The effects were statistically significant (the study reports significance, though the exact effect sizes and confidence intervals are not specified in the available summary)
- The diagnosis-mechanism-intervention design logic produced a reproducible approach — other prevention bodies could adopt the same logic

### Implications
- **Relational embedding makes nudges work for formalization behavior.** The contrast with the ILVO null is instructive: when the same general class of behavior change intervention is delivered through a trusted relational channel (consultant-led workshop) rather than a cold digital channel, it can produce positive effects. The channel is a critical moderator of nudge effectiveness.
- **Diagnosis-mechanism-intervention logic is a transferable design pattern.** Rather than applying a fixed nudge (e.g., "always use loss framing"), the OPPBTP approach diagnoses the specific gap, identifies the mechanism, and matches the nudge to the mechanism. This is more principled than off-the-shelf nudge application and likely contributes to the positive result.
- **Action research caveat.** This is not a blinded RCT. There is no bias correction, no preregistration (as far as the available summary indicates), and the consultant-participant relationship introduces demand characteristics. Expect the true effect under colder delivery conditions to be smaller. Do not generalize the positive result to cold-digital nudge delivery.
- **Formalization as a nudgeable behavior.** Moving a firm from "no documented prevention plan" to "documented and updated plan with specified actions" is a medium-friction, episodic behavior — more nudgeable than ongoing eco-scheme enrollment (ILVO) but more friction than a one-click app download (Asada et al.).

### A-Tech Alignment
- **A-Coder (compliance/security nudges):** For high-stakes compliance nudges (security policy acknowledgment, code-review mandate adoption, SBOM formalization), embed the nudge in a guided workshop or pairing session rather than a cold notification. Use the OPPBTP diagnosis-mechanism-intervention logic: diagnose the team's current compliance gap, identify the behavioral mechanism (e.g., "they don't know what an SBOM is" → information mechanism → demonstration nudge; "they know but it's too much effort" → friction mechanism → default/automation nudge), then deliver the matched nudge within the relational session.
- **Be Practical (consulting methodology):** The diagnosis-mechanism-intervention logic is directly transferable to AI-adoption consulting. When advising a business on AI policy or open-source governance adoption, diagnose the gap, identify the mechanism, and match the intervention — rather than applying a one-size-fits-all nudge.
- **Builder's Club (community safety norms):** Community safety/preparedness norms are more likely to stick when delivered through a relational channel (community manager, moderated session) than through a pinned post. The OPPBTP result supports investing in relational delivery for behavioral asks in communities.
- **General:** The OPPBTP positive is the benchmark for what relational embedding can achieve. Use it to justify the cost of a workshop/consultancy channel when a cold-digital nudge would likely fail.

---

## Study 3: Disaster Preparedness App — Asada et al. Online RCT (HETEROGENEOUS)

**Authors:** Asada et al., University of Osaka
**Title:** Nudge-based video and flyer interventions for disaster-preparedness app download willingness
**Journal:** Journal of Behavioral Economics and Finance, Vol. 19, 2026

### Study Design
- Online randomized controlled trial
- Multi-arm design testing video-based and flyer-based nudge interventions
- Outcome: willingness to download a disaster-preparedness app (self-reported intent, not actual installation)
- Heterogeneous treatment effect analysis by gender and region (Osaka Prefecture residents vs others)

### Sample
- Online participants (exact N not specified in the available summary)
- Participants included both male and female respondents
- Participants from Osaka Prefecture (the disaster-preparedness context) and other regions

### Interventions
**Video conditions (three types):**
1. **Usability-focused video** — emphasizing how easy the app is to use (reducing perceived friction/effort as the behavioral mechanism)
2. **Close-up perspective video** — a perspective/framing variant (likely immersive or personal-perspective framing)
3. **Quiz-based video** — interactive/engagement-based framing (testing knowledge to raise salience and involvement)

**Flyer conditions (two variants):**
1. Flyer variant A
2. Flyer variant B
(Specific framing of the two flyer variants not detailed in the available summary; both are static/text-image formats.)

**Audio conditions:** The study controlled for whether the video was viewed with or without audio.

### Results
- **Flyers: NO significant treatment effects.** Neither flyer variant produced a statistically significant increase in willingness to download the app. This replicates the pattern that static/text-image nudges are ineffective for low-salience technology-adoption behaviors.
- **Videos: usability-focused video most effective when controlling for audio conditions.** Among the three video types, the usability-focused video produced the highest willingness to download, controlling for whether audio was on.
- **Audio effect:** Viewing with audio was associated with higher baseline willingness to download. Audio availability matters — silent video viewing attenuates the nudge effect.
- **Gender effect:** Being female was associated with higher baseline willingness to download the disaster-preparedness app.
- **Heterogeneous treatment effects (the key finding):**
  - **Usability-focused video** significantly increased download intent among **females** and among **Osaka Prefecture residents** specifically.
  - **Quiz-based video** significantly increased download intent among **females** and among **Osaka Prefecture residents** specifically.
  - The close-up perspective video did not show significant heterogeneous effects in these subgroups (or the available summary does not report them).
  - These subgroup effects were significant where the pooled (all-participant) average may have been weaker or non-significant — demonstrating that pooled averages can obscure real subgroup effects.

### Implications
- **Rich media (video) outperforms static media (flyers) for technology-adoption nudges.** The flyer null and the video positive replicate a broader pattern: static text/image nudges are weak for low-salience behaviors, while video — especially usability-focused video — can move technology-adoption intent.
- **Usability framing is the strongest video nudge for app adoption.** When the target behavior is adopting a technology artifact (app, tool, platform), framing the nudge around usability ("this is easy to use") outperforms close-up perspective and quiz-based framings. This aligns with the friction-reduction principle: the binding constraint in technology adoption is often perceived effort, and usability framing directly addresses it.
- **Audio availability is a design variable, not a background condition.** Designers should treat audio-on as the default and measure silent-viewing effects separately. A video nudge viewed without audio is a meaningfully weaker intervention.
- **Heterogeneous effects demand segmented design.** The significant effects for females and Osaka Prefecture residents — where pooled effects may have been weaker — demonstrate that a one-size-fits-all nudge averages real subgroup effects toward null. Pre-specify segments, randomize within segments, and analyze heterogeneous treatment effects. This is especially important for public-safety interventions where the relevant population is geographically and demographically segmented.
- **Intent ≠ behavior.** The outcome is willingness to download, not actual app installation. Intent effects should be discounted 30–50% when estimating real-world install lift. The `nudge-persistence-technology-adoption` skill raises the further question of whether downloaded apps translate into actual preparedness behavior — a second persistence gap beyond the intent-to-download gap.
- **Regional relevance matters.** Osaka Prefecture residents (for whom the disaster-preparedness context is locally salient) responded more strongly than non-residents. This suggests that nudge effectiveness scales with the local relevance/salience of the target behavior — a design principle for geographically targeted public-safety campaigns.

### A-Tech Alignment
- **A-Coder (developer tool adoption):** When nudging developers to adopt a new tool feature, prefer a short usability-focused video walkthrough over text tooltips or static banners. Ensure audio is on by default. Segment by role (frontend vs backend vs DevOps) and expect heterogeneous adoption effects — the Asada et al. gender and region heterogeneity is the analogue for role and team heterogeneity in developer-tool adoption.
- **Be Practical (learning content):** When producing content that includes behavioral nudges (e.g., "complete the next module," "try this exercise"), segment by audience persona and test which framing (usability-focused, quiz-based, close-up perspective) resonates per segment. Do not ship one framing to all learners.
- **Builder's Club (community preparedness/safety):** For community safety/preparedness campaigns, produce usability-focused video content, make audio available, and target by region/language for heterogeneous lift. Do not rely on pinned text "nudges" — the flyer null shows static text is ineffective for low-salience behaviors.
- **General:** The Asada et al. study is the benchmark for (a) rich-media-over-static nudge design, (b) usability-framing for technology adoption, (c) audio as a design variable, and (d) pre-specified heterogeneous treatment effect analysis for segmented populations.

---

## Cross-Study Synthesis

| Dimension | ILVO (Agriculture) | OPPBTP (Construction) | Asada et al. (Disaster Prep) |
|---|---|---|---|
| **Outcome** | NULL (all arms) | POSITIVE (formalization) | HETEROGENEOUS (video > flyer; gender + region) |
| **Behavioral friction** | High (ongoing eco-scheme enrollment) | Medium (one-time plan formalization) | Low (one-time app download intent) |
| **Channel** | Cold digital | Relational (consultancy workshop) | Cold digital (online video/flyer) |
| **Medium** | Digital BCI (text/messaging) | In-person training + nudge | Video (rich) vs flyer (static) |
| **Segmentation** | None tested | By firm diagnosis | Gender + region (pre-specified) |
| **Evidence type** | Preregistered RCT, N=14,285 | Action research (not RCT) | Online RCT (intent outcome) |
| **Bias-correction risk** | Low (preregistered, powered) | High (action research, no correction) | Medium (online RCT, intent not behavior) |
| **Consistency with bias-corrected baseline** | Consistent (near-zero for high-friction digital) | Above baseline — likely channel-driven | Consistent for flyers (null); video effects are modest and subgroup-specific |

**Meta-lesson:** Nudge effectiveness in novel domains is moderated by (1) behavioral friction, (2) channel relational proximity, (3) medium richness, and (4) population segmentation. The three studies collectively show that nudges can fail (ILVO), succeed (OPPBTP), or succeed only for specific subgroups (Asada et al.) — and the difference is largely explained by where the intervention sits on these four moderators. Use the Novel-Domain Nudge Feasibility Matrix and the five-step Decision Framework in SKILL.md to classify a proposed nudge before designing it.