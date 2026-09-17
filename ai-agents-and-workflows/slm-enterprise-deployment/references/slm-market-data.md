# SLM Market Data and Benchmarks

---

## Market Size Projections

| Year | Market Size | Source |
|------|-------------|--------|
| 2025 | $0.93B | MarketsandMarkets |
| 2026 | $1.2B (est.) | Industry consensus |
| 2032 | $5.45B | MarketsandMarkets |
| CAGR | 28.7% | MarketsandMarkets |

**Gartner prediction:** By 2027, task-specific models will have 3× the usage volume of general-purpose LLMs.

---

## Performance Benchmarks

### Microsoft Phi-4 (14B)
- MMLU: 88.0%
- Coding (HumanEval): 72.5%
- Math (GSM8K): 91.2%
- Energy vs. GPT-3.5: 92% less
- License: MIT

### NVIDIA Efficiency Data (June 2025)
- 7B SLM latency: 10–30× faster than 70–175B LLM
- Energy: 10–30× less kWh per 1K requests
- Cost: 10–30× less $ per 1M tokens
- Note: Exact multiplier depends on quantization, batch size, and hardware

### MIT RLM-Qwen3-8B
- Long-context improvement: +28.3% over base Qwen3-8B
- Approaches GPT-5 quality on three tasks despite 50× smaller size
- Recursive self-calling enables 2 orders of magnitude beyond context windows
- Status: Research prototype, not yet production-ready

---

## Vendor Positioning

| Vendor | SLM Family | Strength | Enterprise Fit |
|--------|-----------|----------|--------------|
| Microsoft | Phi-4 | General capability, Microsoft ecosystem | High (Azure integration) |
| Alibaba | Qwen3 | Multilingual, open weights | High (Apache 2.0 license) |
| Mistral | Mistral Small 3 | Code quality, European compliance | High (GDPR-native) |
| Google | Gemma 3 | Edge optimization, mobile | High (Android ecosystem) |
| Meta | Llama 3.1 | Research, permissive license | Moderate (license restrictions) |
| NVIDIA | NeMo (various) | Hardware optimization | High (DGX integration) |

---

## License Implications for A-Tech

- **Apache 2.0 (Qwen, Mistral, Gemma):** Commercial use, modification, distribution permitted. Ideal for open-core model.
- **MIT (Phi):** Commercial use permitted. Microsoft ecosystem integration valuable.
- **Llama 3.1 License:** Commercial use permitted above 700M users requires special license. Not a near-term concern for A-Tech.

---

## Competitive Threats to LLM APIs

If SLM quality continues to improve at current rates:
- By 2028, 70%+ of enterprise coding tasks may be addressable by SLMs
- API-dependent business models face margin compression
- Local-first becomes default expectation for privacy-sensitive sectors
- Frontier LLM providers pivot to "reasoning specialist" positioning

A-Tech's early SLM positioning creates a 2–3 year competitive moat before incumbents adapt.
