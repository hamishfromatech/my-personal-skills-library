# Nurture Sequence Templates

## Email Sequences

### Sequence: Warm Lead (Low Awareness)
```
Day 0: Welcome + Value Resource
Subject: "The one thing most [industry] businesses get wrong"
Body: Educational content. No pitch. Soft CTA to reply with their biggest challenge.

Day 3: Case Study
Subject: "How [Similar Company] solved [Problem] in [Timeframe]"
Body: Detailed case study with specific metrics. CTA: "See if this applies to you."

Day 7: Objection Handler
Subject: "The real reason [solution] feels risky"
Body: Address common fear/objection. Build trust through transparency.

Day 14: Direct Offer
Subject: "Ready to talk?"
Body: Soft consultation offer. Low-commitment CTA.

Day 21: Breakup or Downgrade
Subject: "Should I close your file?"
Body: Polite check-in. Offer newsletter subscription as alternative.
```

### Sequence: Hot Lead (High Intent)
```
Day 0: Immediate Response
Subject: "Re: [Their Original Subject] — let's schedule"
Body: Acknowledge specific points from their inquiry. Propose specific times.

Day 1: Reminder + Social Proof
Subject: "Quick reminder + how we helped [Client]"
Body: Short. Reiterate proposed times. One relevant testimonial.

Day 3: Final Direct Ask
Subject: "Last call on this"
Body: Assume they are busy, not uninterested. Offer async alternative (loom video, written proposal).
```

## SMS Sequences

```
SMS rules: Under 160 characters, single CTA, no links in first message.

Day 0: "Hi [Name], got your inquiry about [topic]. Quick question: are you looking for [outcome A] or [outcome B]? — [Your name]"

Day 2 (if no reply): "No pressure if timing's off. Want me to check back next month? Reply STOP to opt out."
```

## LinkedIn DM Sequences

```
Connection request: "Hi [Name], saw your post on [topic]. I've been working on something similar at [Company]. Would love to connect."

Day 3 (after connect): "Thanks for connecting. Your point about [specific detail] resonated — we found [insight] in our work. Thought you might find it relevant."

Day 7: "Quick question: are you currently [struggling with / exploring] [problem you solve]? No pitch — genuinely curious about your experience."

Day 14: "If you're open to it, I'd love to share a 5-minute loom on how we solved [similar problem] for [Company]. Only if it's relevant."
```

## Personalization Variables

| Variable | Source | Example |
|---|---|---|
| {{first_name}} | Form / CRM | "Hi Sarah" |
| {{company}} | Email domain / LinkedIn | "At Acme Corp" |
| {{industry}} | Enrichment API or inference | "In manufacturing" |
| {{content_topic}} | Page they visited / content they downloaded | "Your interest in automation" |
| {{last_engagement}} | Interaction timestamp | "Since you downloaded the guide last week" |
| {{specific_challenge}} | Form field or inferred | "Reducing customer churn" |
