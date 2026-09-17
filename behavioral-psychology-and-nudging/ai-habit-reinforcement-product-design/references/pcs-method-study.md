# Predicting Context Sensitivity (PCS) Method Study

## Study Design
Analyzed over 12 million gym visits and 40 million instances of hospital handwashing to uncover how habits take shape using machine learning.

## Key Results
- **Mean AUC: 0.806** — Strong predictive ability for adherence to specific behaviors
- **Habit formation varies dramatically by behavior type:**
  - Gym habits: 68–78 days median to reach 95% automaticity
  - Hospital handwashing: ~9 shifts (≈220 handwashing opportunities)
- **Context sensitivity:** The PCS method predicts how sensitive a behavior is to environmental context — highly context-sensitive habits are harder to transfer across environments

## Implications for Product Design
1. **Match behavior complexity to expected timeline:** Don't expect complex coding habits to form in weeks
2. **Environment design matters:** Habits tied to specific contexts (IDE open, morning coffee) form faster than time-based habits
3. **Transfer risk:** When users change environments (new job, new device), habits may break. Design for re-anchoring.

## Sources
- Predicting Context Sensitivity study (gym visits + handwashing dataset)
- Cited in Personos AI Research Insights (2025)
