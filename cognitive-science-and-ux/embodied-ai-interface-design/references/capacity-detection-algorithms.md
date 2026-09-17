# Capacity Detection Algorithms

## Behavioral Proxy Methods (Local-Only)

### Keystroke Dynamics Fatigue Model
- **Input**: Inter-key interval (IKI) variance, error rate, backspace frequency
- **Model**: Simple threshold + rolling window
- **Accuracy**: ~78% for detecting elevated fatigue in 15-minute windows
- **Privacy**: No keystroke content captured; only timing metadata

### Pause Pattern Analysis
- **Input**: Duration of pauses between actions, pause location in workflow
- **Insight**: Pauses >30s in mid-task suggest cognitive load spike; pauses >2min suggest possible interruption or capacity drop
- **Calibration**: Per-user baseline established over 3–5 sessions

### Command Retry Rate
- **Input**: Frequency of repeated commands, failed command patterns
- **Threshold**: >3 failed attempts in 5 minutes triggers tier reduction suggestion

## Voice-Based Detection (On-Device)

### Vocal Fatigue Markers
- **Features**: Fundamental frequency stability, speech rate, articulation precision
- **Model**: Lightweight CNN running on-device
- **Accuracy**: ~72% for detecting motor fatigue; ~65% for detecting cognitive fatigue
- **Note**: Requires 30+ seconds of continuous speech for reliable inference

### Environmental Audio Context
- **Feature**: Ambient sound classification (quiet workspace, conversation, medical environment)
- **Use**: Contextualize other signals — high cognitive load in quiet workspace is fatigue; same load during conversation is normal
