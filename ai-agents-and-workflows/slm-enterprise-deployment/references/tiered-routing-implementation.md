# Tiered Routing Implementation

Architecture and code patterns for SLM/LLM tiered routing in production.

---

## Request Classification Model

A lightweight classifier routes requests to the appropriate model tier.

### Features

| Feature | Source | Weight |
|---------|--------|--------|
| Token count | Request payload | 0.15 |
| Complexity score | AST depth, nested structure | 0.25 |
| Domain indicator | Keywords ("architecture", "research", "theorem") | 0.20 |
| Sensitivity flag | PII/PHI/financial data patterns | 0.25 |
| User preference | Explicit setting or history | 0.15 |

### Classification Rules

```python
def classify_request(request, user_prefs):
    features = extract_features(request)
    score = weighted_sum(features)
    
    # Hard overrides
    if features.sensitivity == "high":
        return "slm_local"  # Never send sensitive data to cloud
    
    if user_prefs.always_deep_reasoning:
        return "llm_cloud"
    
    # Score-based routing
    if score < 0.4:
        return "slm_local"
    elif score < 0.7:
        return "slm_enterprise"  # Larger SLM on internal server
    else:
        return "llm_cloud"
```

---

## Router Architecture

```
┌─────────────────────────────────────────────┐
│              API Gateway                      │
│  (rate limiting, auth, request logging)       │
└──────────────┬───────────────────────────────┘
               │
┌──────────────▼───────────────────────────────┐
│           Request Classifier                  │
│  (token count, complexity, domain, sensitivity)│
└───────┬──────────────┬──────────────┬────────┘
        │              │              │
┌───────▼─────┐ ┌──────▼──────┐ ┌─────▼───────┐
│  Local SLM  │ │ Enterprise  │ │  Cloud LLM  │
│  (Gemma 4B) │ │  SLM        │ │  (GPT-4o)   │
│             │ │  (Phi-4)    │ │             │
│ • Privacy   │ │             │ │ • Research  │
│ • Offline   │ │ • Team      │ │ • Novel     │
│ • Low       │ │ • Shared    │ │ • Complex   │
│   latency   │ │   GPU       │ │   arch      │
└───────┬─────┘ └──────┬──────┘ └─────┬───────┘
        │              │              │
└───────┴──────────────┴──────────────┴────────┘
│         Unified Response Formatter              │
│  (consistent schema, source attribution,        │
│   confidence score, latency metadata)           │
└─────────────────────────────────────────────────┘
```

---

## Deployment Templates

### Docker Compose (Edge Server)

```yaml
version: '3.8'
services:
  slm-router:
    image: atech/slm-router:latest
    ports:
      - "8080:8080"
    environment:
      - DEFAULT_TIER=slm_local
      - LLM_API_KEY=${LLM_API_KEY}
      - LLM_ENDPOINT=${LLM_ENDPOINT}
    volumes:
      - ./models:/models:ro
      - ./config:/config:ro
    depends_on:
      - vllm
  
  vllm:
    image: vllm/vllm-openai:latest
    runtime: nvidia
    volumes:
      - ./models:/models:ro
    environment:
      - MODEL_NAME=Qwen/Qwen3-8B
      - TENSOR_PARALLEL_SIZE=1
      - GPU_MEMORY_UTILIZATION=0.85
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: 1
              capabilities: [gpu]
```

### Kubernetes (Enterprise)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: slm-tier
spec:
  replicas: 3
  selector:
    matchLabels:
      app: slm-tier
  template:
    metadata:
      labels:
        app: slm-tier
    spec:
      containers:
      - name: vllm
        image: vllm/vllm-openai:latest
        resources:
          limits:
            nvidia.com/gpu: 1
            memory: "40Gi"
        env:
        - name: MODEL_NAME
          value: "microsoft/Phi-4"
        - name: TENSOR_PARALLEL_SIZE
          value: "1"
      - name: router
        image: atech/slm-router:latest
        ports:
        - containerPort: 8080
        env:
        - name: DEFAULT_TIER
          value: "slm_enterprise"
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: slm-tier-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: slm-tier
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

---

## Response Formatter Specification

All tiers return the same response schema:

```json
{
  "content": "...",
  "metadata": {
    "tier": "slm_local",
    "model": "gemma-3-4b",
    "latency_ms": 89,
    "confidence": 0.91,
    "token_count": {
      "input": 245,
      "output": 128
    },
    "privacy_score": "maximum",
    "carbon_g": 0.003
  },
  "attribution": {
    "source": "local_inference",
    "data_egress": false,
    "audit_hash": "sha256:abc123..."
  }
}
```

This ensures consistent UX regardless of which model served the request.
