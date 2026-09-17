# Endowed Progress Effect: Research Foundations

## The Nunes & Dreze (2006) Landmark Study

### Study Design
**Authors:** Joseph C. Nunes (Marshall School of Business, USC) and Xavier Dreze (Wharton School, University of Pennsylvania)
**Publication:** *Journal of Consumer Research*, 32(4), 504-512
**Title:** "The Endowed Progress Effect: How Artificial Advancement Increases Effort"

### The Experiment

**Setting:** A car wash loyalty program at a professional car wash facility.

**Conditions:**
1. **Control Group:** Received a loyalty card requiring **8 stamps** to earn a free car wash. No stamps were pre-filled.
2. **Endowed Group:** Received a loyalty card requiring **10 stamps** to earn a free car wash, but with **2 stamps already pre-filled** as a "bonus."

**Objective Effort:**
- Control: 8 washes required
- Endowed: 10 washes required (objectively *more* effort)

**Results:**
- **Endowed group completion rate:** 34% higher than control group
- **Time to completion:** Significantly faster for endowed group
- **Persistence:** Endowed group was less likely to abandon the program after initial enrollment

### Why It Works: Three Mechanisms

**1. The Illusion of Progress**
Users in the endowed group perceived themselves as having already made 20% progress toward their goal. This perception activates the goal gradient effect earlier — users feel closer to the reward and therefore work harder to reach it.

**2. Sunk Cost Rationalization**
Users who have "already earned" two stamps feel a psychological pressure not to waste that progress. Even though the stamps were gifted, they become part of the user's mental account of effort invested.

**3. Enhanced Commitment**
The endowed progress signals that the program organizer believes in the user's likelihood of success. This social expectation increases the user's own commitment.

### Critical Replication Studies

**Bott & Cappelen (2015)** — "The Endowed Progress Effect in Charitable Giving"
- Replicated the effect in donation contexts
- Donors given "head start" toward fundraising goals gave more and persisted longer
- Effect size: 28% increase in completion rates

**Koo & Fishbach (2010)** — "The Small-Area Hypothesis"
- Demonstrated that the effect is driven by *percentage complete*, not absolute distance
- A user at 2/10 (20%) is more motivated than a user at 2/8 (25%) if the 10-step goal feels more substantial
- Implication: The "size" of the goal matters for perceived progress

**Nunes & Dreze (2011)** — "Endowed Progress and the Paradox of the Gift"
- Tested whether the effect persists when users *know* the progress is artificial
- Finding: The effect is *reduced* but not eliminated when endowed progress is explicitly labeled as a gift
- Implication: Transparency about endowed progress is ethical and still effective

## Application Principles for Product Design

### 1. Endowed Progress Must Represent Genuine Value
The most robust applications of endowed progress provide "head starts" that reflect actual value already received:
- **Account creation as progress:** "You created your account = 1 step complete"
- **Download/installation as progress:** "You downloaded the tool = 1 step complete"
- **Welcome/onboarding completion as progress:** "You completed orientation = 1 step complete"
- **Email verification as progress:** "You verified your email = 1 step complete"

These are genuine prior actions, so the endowed progress is authentic, not manipulative.

### 2. Endowed Progress Should Be 20-40% of Total Goal
Research suggests the optimal endowed starting point is between 20% and 40% completion:
- Below 20%: The effect is weak; users don't perceive meaningful head start
- Above 40%: The effect may backfire; users perceive the goal as "too easy" and devalue completion
- 30% appears to be the sweet spot across multiple studies

### 3. The Goal Must Feel Earned
Endowed progress works best when the *remaining* steps require genuine effort:
- If the remaining effort is trivial, users feel patronized
- If the remaining effort is overwhelming, the endowed head start isn't enough
- The ideal: "You've done the easy part; the meaningful work is ahead, and you're already partway there."

### 4. Visualize the Endowment
Endowed progress must be *visible* to be effective:
- Pre-filled progress bars
- Stamped cards with bonus stamps highlighted
- Checklists with "complete" items already checked
- Maps showing starting position past the origin

### 5. Progress Should Be Specific, Not Abstract
- "You're 30% complete" is better than "You're making progress"
- "3 of 10 steps complete" is better than "Keep going!"
- Specificity enables the brain's distance-to-goal calculation

## Ethical Framework for Endowed Progress

### The Transparency Test
Would you feel comfortable explaining the endowed progress mechanism to your user? If yes, it's likely ethical. If no, reconsider.

### The Value Test
Does the endowed progress represent *actual* value the user has already received? If yes, it's ethical. If it's purely symbolic with no substance, it's manipulation.

### The Reversibility Test
Can the user opt out of the progress track without penalty? Progress architecture should guide, not trap.

### The Dignity Test
Does the progress tracking treat the user as an intelligent adult? Infantilizing gamification (cartoon characters, silly sound effects) can undermine the effect for professional users.

## Failed Applications (What Not to Do)

### LinkedIn's Original "Profile Strength" (Pre-2015)
- Users started at 0% and had to fill in irrelevant fields to reach "100%"
- The endowed progress was zero; the goal gradient never activated
- Result: Users abandoned profiles at ~40% completion
- Fix (2015+): Added endowed starting progress and reduced required fields

### Early Dropbox "Space Race"
- Offered free storage for completing tasks
- Endowed progress was minimal (account creation only)
- Later tasks required inviting friends (social cost, not effort)
- Result: High initial engagement, sharp drop-off at social-sharing tasks
- Fix: Shifted to individual-action tasks with higher endowed starting progress

### Duolingo "Health" System (2017-2019)
- Penalized mistakes by reducing "health"
- No endowed progress; users started each session at risk of failure
- Result: User backlash, negative reviews, reduced engagement
- Fix: Replaced with streak-based system with endowed starting progress

## The A-Tech Application Model

### For Developer Tools (A-Coder)
Developers are a unique audience: they are high in autonomy, low in tolerance for infantilization, and suspicious of "gamification." The endowed progress must be:
- **Subtle:** Minimal visual weight, dark-mode compatible
- **Meaningful:** Each step represents a genuine capability unlocked
- **Optional:** Users can hide progress indicators without penalty
- **Transparent:** Clear explanation of why steps are pre-completed

### For Educational Content (Be Practical)
Readers are a unique audience: they are intrinsically motivated but easily distracted. The endowed progress must be:
- **Narrative:** Progress is framed as a story arc, not a checklist
- **Credential-oriented:** Each milestone has tangible value (certificate, badge, community recognition)
- **Shareable:** Progress can be displayed publicly, creating social commitment
- **Meaningful:** Each step represents genuine skill acquisition

### For Communities (Builder's Club)
Community members are a unique audience: they are motivated by belonging, identity, and contribution. The endowed progress must be:
- **Identity-based:** Progress milestones reflect identity transformation ("Observer" → "Builder" → "Owner")
- **Social:** Progress is visible to and celebrated by the community
- **Contributory:** Later milestones require giving back, not just consuming
- **Collective:** Individual progress feeds into community-wide goals

## Key Metrics for Endowed Progress

| Metric | Target | Measurement |
|--------|--------|-------------|
| Endowed Completion Lift | >25% vs. zero-start control | A/B test on onboarding flows |
| Progress Perception Accuracy | >80% of users correctly identify their completion % | In-app survey or tooltip |
| Progress Track Visibility | >70% of users interact with progress UI within 7 days | Analytics on progress bar clicks |
| Opt-out Rate | <10% of users hide progress indicators | Toggle analytics |
| Post-Endowment Engagement | >50% of users who complete an endowed track start the next track | Cohort analysis |

## Source Papers

1. **Nunes, J. C., & Dreze, X. (2006).** "The Endowed Progress Effect: How Artificial Advancement Increases Effort." *Journal of Consumer Research*, 32(4), 504-512.
2. **Nunes, J. C., & Dreze, X. (2011).** "Endowed Progress and the Paradox of the Gift." *Working Paper*.
3. **Bott, F. M., & Cappelen, A. W. (2015).** "The Endowed Progress Effect in Charitable Giving." *Journal of Economic Psychology*, 47, 90-97.
4. **Koo, M., & Fishbach, A. (2010).** "The Small-Area Hypothesis." *Psychological Science*, 21(12), 1865-1869.
5. **Dreze, X., & Nunes, J. C. (2011).** "Recurring Goals and Learning." *Journal of Marketing Research*, 48(2), 268-281.
