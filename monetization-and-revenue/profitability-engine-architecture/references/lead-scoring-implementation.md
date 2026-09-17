# Lead Scoring Implementation

## Pipeline Architecture

```python
# lead_scorer.py
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import re

class OwnedLeadScorer:
    def __init__(self, model_path="local/llama-3.1-8b-scoring"):
        self.tokenizer = AutoTokenizer.from_pretrained(model_path)
        self.model = AutoModelForSequenceClassification.from_pretrained(model_path)
        self.criteria = self._load_criteria()
    
    def score(self, inquiry_text, source="email"):
        """Score a single lead inquiry."""
        normalized = inquiry_text.lower()
        
        scores = {}
        for criterion in self.criteria:
            matched_signals = [s for s in criterion["signals"] if s in normalized]
            scores[criterion["name"]] = {
                "weight": criterion["weight"],
                "matched": len(matched_signals),
                "raw_score": len(matched_signals) * criterion["weight"]
            }
        
        total = sum(s["raw_score"] for s in scores.values())
        normalized_score = min(100, total)  # Cap at 100
        
        return {
            "score": normalized_score,
            "explanation": scores,
            "source": source,
            "recommendation": "hot" if normalized_score >= 80 else 
                             "warm" if normalized_score >= 50 else "cold"
        }
    
    def batch_score(self, inquiries):
        """Score multiple inquiries for dashboard generation."""
        return [self.score(i["text"], i["source"]) for i in inquiries]
```

## Dashboard Template

```html
<!-- Simple owned dashboard served from local server -->
<!DOCTYPE html>
<html>
<head><title>Lead Scoring Dashboard</title></head>
<body>
  <h1>Today's Leads</h1>
  <table>
    <thead>
      <tr><th>Source</th><th>Score</th><th>Top Signals</th><th>Action</th></tr>
    </thead>
    <tbody>
      {{#each leads}}
      <tr class="{{recommendation}}">
        <td>{{source}}</td>
        <td>{{score}}</td>
        <td>{{explanation}}</td>
        <td><a href="/crm/lead/{{id}}">View</a></td>
      </tr>
      {{/each}}
    </tbody>
  </table>
</body>
</html>
```

## Model Training Notes

- Base model: Llama 3.1 8B or Qwen3-8B (sufficient for binary classification tasks)
- Fine-tuning data: 500–1,000 labeled inquiries (hot vs. cold) from your historical data
- Training method: LoRA, 4-bit quantization for efficiency
- Evaluation: F1 score on holdout set; target >0.75
- Deployment: Single GPU (RTX 3090 or better) serves scoring API
