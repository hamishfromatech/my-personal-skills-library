# Privacy-First Behavioral Signal Methodology

## Technical Implementation

### Signal Collection Without PII

**On-Device Processing:**
All behavioral signals are processed locally using edge ML models. Only aggregated, pseudonymous metrics leave the device. Individual behavioral traces never reach the server.

**Signal Categories and Privacy Safeguards:**

| Signal | Collection Method | Privacy Safeguard | Predictive Power |
|--------|-------------------|-------------------|------------------|
| Fixation duration | Browser IntersectionObserver | Aggregated per-page, no user ID | High (engagement) |
| Scroll velocity | Scroll event listeners | Pseudonymous session token | Medium (interest) |
| Typing cadence | Keystroke timing (local) | Never leaves device | High (comprehension) |
| Error rate | Form validation events | Aggregated by field, not user | Medium (friction) |
| Tab-switching | Page Visibility API | Session-level only | Medium (attention) |
| Re-read patterns | Scroll direction tracking | Pseudonymous | Medium (dissonance) |

### Aggregation Architecture
```
User Device → Local Signal Processing → Anonymized Feature Vector →
→ Edge Aggregation → Differential Privacy Noise → Central Analytics
```

**Differential Privacy Parameters:**
- Epsilon: 1.0 (strong privacy guarantee)
- Delta: 10^-6
- Minimum cohort size: 100 users before data is surfaced

### Cultural Adaptation
Emotional triggers vary cross-culturally. Implementation requires:
- Market-specific baseline calibration
- Local community review of automated classifications
- Regular retraining on local behavioral patterns
- Avoid universalization of Western emotional valence labels

### Regulatory Compliance Mapping

| Regulation | Requirement | Implementation |
|------------|-------------|----------------|
| GDPR Article 9 | No special category data | No biometric collection |
| GDPR Article 22 | No solely automated decisions | Human review of targeting rules |
| CCPA | Right to delete | Behavioral profile deletion API |
| DSA | Transparency for algorithmic curation | Disclosure of signal types used |
| FTC | No dark patterns | Explicit consent for personalization |
