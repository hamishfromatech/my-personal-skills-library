# Barron, Damgaard & Gravert — "Nudge Me!" Evidence Base

**Citation:** Barron, Kai; Damgaard, Mette Trier; Gravert, Christina. "Nudge Me! A field experiment on reminders for medication adherence." *Journal of Economic Behavior & Organization*, vol. 241 (2026). DOI: 10.1016/j.jebo.2025.107368. RePEc handle: eee:jeborg:v:241:y:2026:i:c:s0167268125004858.

## Study Design

- **Setting:** National mobile-health platform (South Africa's MomConnect-style national service), delivered at population scale.
- **Population:** Over **4,000 pregnant women**, adherence target: iron supplementation during pregnancy.
- **Randomized reminder arms:**
  1. **Purely attentional** — a nudge-to-action only (salience + prompt, no added content).
  2. **Informational** — the reminder adds health information beyond the prompt itself.
  3. **Morally persuasive** — framing that appeals to obligation/identity (e.g., responsibility to the baby).
- **Dual outcome families** (the design's distinguishing feature):
  1. **Behavioral:** adherence to iron supplementation.
  2. **Monetary/revealed-preference:** willingness to pay (WTP) for future reminders, measured as a real choice, not a survey rating.

## Headline Results

| Finding | Result | Why it matters |
|---|---|---|
| Free-reminder take-up | **Over 80% chose additional reminders** when offered free, despite high self-reported adherence | Demand for the *mechanism* exceeds what self-report predicts; the stated-preference gap is the product surface |
| WTP for reminders | **Many participants willing to pay** for future reminders | First clean revealed-preference valuation of a nudge in a low-income, public-platform setting |
| Exposure → demand loop | **Reminders increase BOTH adherence AND WTP** for more reminders | Reminding compounds rather than wears off (inverts the typical decay finding); the feature sells itself |
| Information penalty | **Informational reminders reduce BOTH adherence and WTP** vs simple reminders | Well-intended content is a negative-sum edit; the reminder's job is friction removal, not education |
| Moral persuasion | (No consistent advantage over attentional reminders) | Frame choice is second-order; payload simplicity is first-order |

## Mechanism: Why Information Hurts a Reminder

1. **Processing cost.** A reminder works by being cognitively cheap. Informational content reintroduces reading, evaluation, and decision burden at the exact moment the nudge is trying to lower decision cost.
2. **Attention dilution.** Extra text competes with the action cue for the same glance.
3. **Reactance/judgment channel.** Content invites scrutiny ("do I believe this?"), converting an automatic response into a deliberative one — the library's `sustained-attention-purchase-paradox` mechanism applied to nudges: deliberation invites objection.
4. **Valuation dilution.** WTP falls because the recipient is paying (in effort) for the same cue. Priced friction shows up as stated-and-revealed demand loss.

## Design Rules (extracted)

1. **Keep nudges cognitively cheap.** Default payload = cue + link + action verb.
2. **Never bundle education into a nudge.** Send content as a separate, opt-in message; if the platform can't split, drop the content.
3. **Sell the cleaner cue.** Premium tiers should monetize *less* (quiet hours, frequency caps, no-education mode) rather than *more*.
4. **Track revealed demand, not just compliance.** Opt-in-to-more-reminders is a cheap proxy for the WTP margin; a drop after a content change is the penalty signal.
5. **Expect compounding, plan capacity.** Because reminders increase their own demand, early exposure raises future opt-in volume — provision notification budget accordingly instead of treating reminder opt-ins as a fixed pool.

## A/B Template (adaptation of the design)

| Arm | Payload | Success metric pair |
|---|---|---|
| Control | Current notification | adherence rate + follow-on opt-in rate |
| Attentional | cue + link only | same pair |
| Informational | cue + link + 1–2 sentence explainer | same pair (expect penalty) |
| Premium | user-configurable quiet hours / no-info mode | conversion + retention |

**Guardrail:** pre-register both metrics; a content "improvement" that raises short-term clicks while lowering opt-in-to-more-reminders is the penalty in action.

## Limitations (state honestly)

- Single country, single supplement (iron), single population (pregnant women). Generalize the **mechanism**, not the medical framing.
- Adherence is partly self-reported at baseline; the behavioral gap between self-report and demand is itself one of the findings.
- WTP elicited on a national platform with likely price-sensitivity; absolute WTP magnitudes may not transfer to high-income markets.
- The paper's reminder types are public-health texts; AI-assistant nudges add a personalization dimension the study doesn't test (an open question, not a license).

## Adjacent Skills (library cross-reference)

- `engagement-gated-nudge-effectiveness-2026` — engagement as the gate on nudge effects; the information penalty is a content-side gate.
- `nudge-effectiveness-reality-check` — small-effects calibration; pair WTP evidence with effect-size honesty.
- `boosting-empowering-behavior-change` — reminders as the simplest boost; this adds the monetization angle.
- `digital-addiction-economics-2026` — the other revealed-WTP-for-commitment result in the library; same revealed-preference family.
- `intent-assistant-attention-steering` — gentle-notification pattern (30s max repeat, dismissible, polite) — a concrete instantiation of "keep the nudge dumb."
- `ai-companion-attachment-economics` — consent-based monetization posture; reminder WTP is a consent-compatible revenue line.

## A-Tech Applications

- **A-Coder / dev tools:** changelog and CI notifications should be cue+link; move rationale into docs. "Reminder fatigue" in dev tools is often the information penalty in disguise.
- **Be Practical / courses:** lesson-reminder emails should prompt action ("finish module 3 → link"), not re-teach content.
- **Builder's Club / community products:** a paid "focus tier" (fewer, dumber nudges) is evidence-backed; richer notifications are not a value-add by default.
- **Content angles:** "People pay to be reminded — and pay more when you say less."