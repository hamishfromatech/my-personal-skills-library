# FND Design Insights: From Limitation to Design Principle

## Lived Experience Design Notes

### The Seizure Prodrome Window
- **Duration**: 30 seconds to 5 minutes of warning signs before motor seizure
- **Design implication**: Systems must react fast with dramatic simplification — not gradual degradation
- **Interface response**: Single large button, voice-only, or automatic pause with safe-state enforcement
- **Anti-pattern**: Gradual menu simplification that takes too long; user may be incapacitated before reaching simplified state

### Post-Seizure Cognitive State
- **Duration**: 15 minutes to 2 hours of reduced comprehension, memory gaps, emotional lability
- **Design implication**: Interfaces should not assume user remembers context from pre-seizure session
- **Interface response**: Auto-save state with timestamped recovery markers; offer "resume from checkpoint" rather than assuming continuity
- **Anti-pattern**: Dumping user back into complex workflow they started 30 minutes ago with no recap

### Hand Contracture and Motor Alternatives
- **Observation**: On days with right-hand contracture, voice becomes primary input; eye-gaze secondary
- **Design implication**: Interfaces must support graceful degradation across input modalities without requiring reconfiguration
- **Interface response**: Automatic modality promotion based on detected input patterns (no voice input detected for 30s? Offer keyboard. No keystrokes for 60s? Promote voice.)
- **Anti-pattern**: Modal settings that require fine motor control to change

### The Three-Minute Walking Limit
- **Observation**: Fatigue compounds across short physical efforts
- **Design implication**: Remote presence tools must be robust enough for primary professional participation
- **Interface response**: High-quality async communication tools that reduce need for synchronous physical presence
- **Anti-pattern**: Assuming "video call" is sufficient accessibility; may still require grooming, positioning, camera management

### Community Feedback on Digital Twin Delegation
- **Positive**: "Knowing my messages get a thoughtful response even when I'm down means I don't drop out of communities"
- **Concern**: "I want to know when I'm talking to a person's twin versus the person"
- **Boundary**: "My twin should never apologize for me. It can say 'I'm handling this' but not 'I'm sorry I couldn't be here'"
