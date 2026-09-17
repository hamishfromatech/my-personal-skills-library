# SLM Fine-Tuning Playbook

A practical guide to domain-specific fine-tuning of Small Language Models for A-Tech use cases.

---

## Hardware Requirements

| Model Size | Fine-Tuning Method | GPU Memory | Hardware Example | Cost/Hour |
|------------|-------------------|------------|------------------|-----------|
| 7B | LoRA (r=16) | 16GB | RTX 4090 (24GB) | $0.50–1.00 |
| 7B | QLoRA (4-bit) | 8GB | RTX 3080 (10GB) | $0.30–0.60 |
| 14B | LoRA (r=8) | 32GB | A100 (40GB) | $1.20–2.50 |
| 14B | QLoRA (4-bit) | 16GB | RTX 4090 (24GB) | $0.50–1.00 |
| 24B | LoRA (r=8) | 48GB | 2× A100 (40GB) | $2.40–5.00 |

---

## Data Preparation

### Minimum Viable Dataset Sizes

| Domain | Examples Needed | Quality Standard |
|--------|-----------------|------------------|
| Code completion | 5K–10K | Unit-tested, reviewed |
| Documentation generation | 2K–5K | Human-verified accuracy |
| Code review style | 3K–8K | Senior engineer reviewed |
| Pedagogical dialogue | 5K–10K | Learning outcome validated |
| Customer support | 10K–20K | Resolution verified |

### Data Format

```json
{
  "instruction": "Refactor this function to use async/await",
  "input": "function fetchData() { return fetch('/api').then(r => r.json()); }",
  "output": "async function fetchData() { const response = await fetch('/api'); return response.json(); }",
  "domain": "javascript",
  "quality_score": 4.5
}
```

---

## Training Configuration

### LoRA Hyperparameters

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| r (rank) | 8–32 | Higher for complex domains, lower for simple |
| alpha | 2× r | Standard scaling |
| dropout | 0.05 | Prevent overfitting on small datasets |
| learning rate | 1e-4 to 2e-4 | Conservative for SLMs |
| batch size | 4–8 | Limited by GPU memory |
| epochs | 3–5 | Early stopping prevents overfitting |
| warmup steps | 100 | Stabilize early training |

### QLoRA (4-bit) Additional Settings

```python
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.bfloat16
)
```

---

## Evaluation Protocol

1. **Automatic metrics:** Perplexity, BLEU, ROUGE on holdout set
2. **Functional correctness:** Code compiles, tests pass (for code models)
3. **Human evaluation:** 50 examples rated 1–5 by domain expert
4. **A/B test:** Fine-tuned vs. base model on real user tasks
5. **Regression test:** Ensure base capabilities not degraded

**Pass criteria:**
- Perplexity reduction > 15% on domain test set
- Human rating ≥ 4.0/5.0 on domain tasks
- Base model capabilities preserved (MMLU drop < 3%)

---

## Deployment

### Serving Options

| Runtime | Throughput | Latency | Best For |
|---------|-----------|---------|----------|
| vLLM | High | Low | Production serving, batched requests |
| TGI (Hugging Face) | High | Low | Hugging Face ecosystem integration |
| ONNX Runtime | Medium | Low | Edge, mobile, cross-platform |
| llama.cpp | Medium | Medium | CPU-only deployment, consumer hardware |
| MLC LLM | Low | Medium | Mobile and embedded devices |

### Quantization for Serving

| Precision | Size Reduction | Quality Impact | Use Case |
|-----------|---------------|----------------|----------|
| FP16 | 2× | Minimal | GPU serving default |
| INT8 | 4× | ~1–2% task degradation | High-throughput serving |
| INT4 (GPTQ/AWQ) | 8× | ~3–5% task degradation | Edge, memory-constrained |

---

## A-Tech Domain Recipes

### A-Coder: Internal Codebase Fine-Tuning
1. Extract 10K code snippets from internal repos (with license check)
2. Generate instruction-response pairs using existing code + docstrings
3. Fine-tune Qwen3-8B with LoRA r=16
4. Evaluate on held-out test files
5. Deploy via vLLM on internal GPU server

### Be Practical: Curriculum Fine-Tuning
1. Extract 5K pedagogical exchanges from course content
2. Add Socratic questioning patterns, common misconceptions
3. Fine-tune Phi-4 with LoRA r=8
4. Evaluate on learning outcome assessments
5. Deploy via ONNX Runtime for cross-platform course app

### Builder's Club: Open-Source Contribution Fine-Tuning
1. Collect 8K high-quality PR reviews, issue discussions
2. Extract constructive feedback patterns, contribution guidelines
3. Fine-tune Mistral Small 3 with LoRA r=16
4. Evaluate on review quality ratings
5. Deploy as community API with rate limiting
