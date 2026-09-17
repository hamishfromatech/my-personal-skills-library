# Uncertainty Design Templates

## Be Practical — Mystery Playbook Drop

### Email Announcement
**Subject:** A mystery playbook just appeared in your library
**Preview text:** We're not telling you which one.

**Body:**
```
Hi [First Name],

You own the 12-Playbook Edition.

We just unlocked a surprise addition to your library.

We're not going to tell you which playbook it is.

You'll have to log in and see for yourself.

Hint: It has something to do with the exact problem you mentioned in your onboarding survey.

→ [Open My Library]

This playbook will be visible for 14 days, then it locks again for the next cohort.

Happy hunting.

— The A-Tech Team
```

### Library UI Treatment
```
When user opens library:
- A "Mystery Playbook" card appears with a question mark cover
- Hover state reveals a cryptic tagline, not the title
- Click opens the playbook normally; the "reveal" is simply seeing the real content
- After opening, the mystery wrapper disappears and the playbook is permanently unlocked

Visual: Dark card with animated question mark. Subtle glow pulse.
Audio: A soft "unwrapping" sound when clicked.
```

## A-Coder — Mystery Feature Hunt

### Release Notes Architecture
```
Standard section: Known features (listed normally)

Mystery section:
"3 hidden capabilities were added in this release.
They are discoverable through normal use.
No Easter eggs. No hidden menus.
Just features that appear when you do certain things.

First person to document each one in the forum gets their username in the next changelog.

Hints:
- One helps you write faster.
- One helps you see clearer.
- One helps you stay private."

This creates a community-wide treasure hunt that drives deeper product exploration than any tutorial.
```

### Variable Streak Rewards System
```
Daily active use = streak point.

At streak milestones (3, 7, 14, 30, 60, 90 days), a reward unlocks.

Reward pool (randomly selected at each milestone):
- Premium theme pack (value: $0, perceived value: high)
- 30-day trial of advanced local model pack
- Custom "Streak Master" badge
- Priority support queue access for 7 days
- Entry into monthly "Builder's Choice" vote
- Nothing but a progress point toward the 90-day "Legendary" reveal

The "nothing" outcome is critical: it maintains the integrity of the uncertainty. Users must know that "nothing this time" is a possible outcome, but that the 90-day resolution is guaranteed to be substantial.
```

## Open Source AI Builder's Club — Variable Contribution Recognition

### Contribution Reward Matrix
```
When a member makes a qualifying contribution (PR merged, high-quality forum post, documentation improvement, etc.), they receive:

Tier 1: Immediate recognition (always)
- Public thank-you in the weekly newsletter
- Contribution point on their profile

Tier 2: Variable surprise (delivered within 7 days)
Possible outcomes:
- Personal video message from founder (1 in 20 contributions)
- Featured member spotlight (1 in 10)
- Free hardware grant application fast-track (1 in 15)
- Early access to upcoming tool beta (1 in 8)
- Custom illustrated badge (1 in 5)
- "Mystery Box" — a physical mailed item (1 in 25)

Members are told:
"Every contribution is recognized. Some contributions unlock surprises. The surprise is chosen based on your contribution type, your history, and a little randomness. You'll know within a week."

This framing maintains the transparency contract: the reward is not guaranteed for every contribution, but when it comes, it is tied to real effort.
```

### Mystery Buildathon Clue System
```
7 days before Buildathon:
"The next Buildathon theme is hidden in this riddle:

'I see without cloud,
I think without rent,
I own what I build,
And I build what I meant.'

First 10 people to guess correctly get early access to the starter kit."

24 hours before Buildathon:
Riddle answer revealed: "Local-First AI"
Theme officially announced with full brief.

The 7-day uncertainty phase drives pre-registration and community puzzle-solving. The 24-hour certainty phase allows actual preparation.
```

## Tier Progression — Surprise Unlocks

### Progression Architecture
```
Instead of:
Bronze (50 pts) → Silver (150 pts) → Gold (300 pts) → Platinum (500 pts)

Use:
Bronze (50 pts) → [Surprise A at 75 pts] → Silver (150 pts) → [Surprise B at 200 pts] → Gold (300 pts) → [Surprise C at 400 pts] → Platinum (500 pts)

The "Surprise" milestones are unlabeled on the progress map. Members see a "?" marker. When they reach it, they discover:

Surprise A (at 75 pts):
Possible: Personal onboarding call, custom avatar, early access to new channel

Surprise B (at 200 pts):
Possible: 1:1 mentorship session, hardware discount code, guest blog opportunity

Surprise C (at 400 pts):
Possible: Lifetime discount on all products, invitation to advisory council, co-branded project opportunity

The key: the tier thresholds are known. The surprise thresholds are visible but unspecified. The combination of known goals and unknown midpoints creates a dual-track motivation system.
```

## Ethical Safeguards — The Uncertainty Contract

### Required Disclosure (must appear wherever uncertainty is used)
```
"The Reward Range
- Minimum: [specific, genuinely valuable outcome]
- Maximum: [aspirational but achievable outcome]
- Resolution: You will know by [specific timeline]
- Effort link: Higher [specific behavior] increases your chance of the maximum
- No zero: Every participant receives at least the minimum
- Opt-out: You may choose the fixed equivalent at any time"
```

### Opt-Out Mechanism
```
At every uncertainty touchpoint, provide a "I prefer certainty" path.

Example:
"You can choose:
→ Mystery reward (unknown, potentially higher value)
→ Fixed reward ($50 credit, guaranteed)

Your choice is remembered for future interactions."

This preserves autonomy and prevents the uncertainty from feeling coercive.
```

## Measurement Templates

### A/B Test: Fixed vs. Variable Rewards
```
Group A: "Complete this playbook exercise and receive a $20 discount code."
Group B: "Complete this playbook exercise and receive a reward between $10 and $50. The exact amount depends on your submission quality."

Measure:
- Exercise completion rate
- Submission quality score
- Post-reward satisfaction (1-10)
- 30-day retention

Expected result: Group B shows 15-40% higher completion with equal or higher satisfaction.
```

### Uncertainty Fatigue Tracker
```
If you deploy multiple uncertainty mechanics simultaneously, track:
- Participation rate per mechanic (declining = fatigue)
- User complaints about "being kept in the dark"
- Opt-out rate for fixed alternatives
- Support tickets asking "what will I get?"

Rule: If opt-out rate exceeds 15%, the uncertainty range is too wide or the disclosure is insufficient.
```
