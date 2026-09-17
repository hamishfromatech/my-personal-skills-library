# RLM Architecture Patterns

Detailed implementation templates for Recursive Language Model systems.

## Pattern 1: Basic Recursive Processor

```python
class RecursiveLanguageModel:
    def __init__(self, base_model, max_context=32000, max_depth=3):
        self.base_model = base_model
        self.max_context = max_context
        self.max_depth = max_depth

    def process(self, input_text: str, depth: int = 0) -> str:
        # Base case: input fits in context
        if len(input_text) <= self.max_context or depth >= self.max_depth:
            return self.base_model.generate(input_text)

        # Recursive case: decompose and process
        snippets = self._decompose(input_text)
        results = []

        for i, snippet in enumerate(snippets):
            # Check if this snippet needs deeper analysis
            if self._needs_recursion(snippet, depth):
                result = self.process(snippet, depth + 1)
            else:
                result = self.base_model.generate(snippet)
            results.append({"index": i, "result": result})

        # Synthesize — model can navigate back to original
        synthesis_prompt = self._build_synthesis_prompt(
            original=input_text,
            results=results,
            task_context=self._extract_task_context(input_text)
        )
        return self.base_model.generate(synthesis_prompt)

    def _decompose(self, text: str) -> list[str]:
        """Decompose text into processable snippets.
        
        Strategy depends on content type:
        - Code: split by module/function boundaries
        - Documents: split by section/paragraph boundaries
        - Mixed: use semantic boundary detection
        """
        if self._is_code(text):
            return self._split_by_modules(text)
        elif self._is_structured_doc(text):
            return self._split_by_sections(text)
        else:
            return self._split_by_semantic_boundaries(text)

    def _needs_recursion(self, snippet: str, current_depth: int) -> bool:
        """Determine if a snippet needs recursive processing."""
        return (
            len(snippet) > self.max_context
            and current_depth < self.max_depth
            and self._has_internal_structure(snippet)
        )

    def _build_synthesis_prompt(self, original, results, task_context):
        """Build a synthesis prompt that lets the model navigate back to original."""
        return f"""
Task Context: {task_context}

Original Input Summary: {self._summarize(original, max_tokens=2000)}

Partial Results from Recursive Processing:
{self._format_results(results)}

Synthesize a complete answer that:
1. Addresses the original task
2. Cross-references findings across partial results
3. Resolves any contradictions between partial results
4. Preserves important details from the original input
"""
```

## Pattern 2: Codebase Analysis RLM

```python
class CodebaseAnalysisRLM(RecursiveLanguageModel):
    def analyze_codebase(self, repo_path: str, question: str) -> dict:
        """Analyze an entire codebase to answer a question."""
        
        # Phase 1: Map structure
        structure = self._map_repo_structure(repo_path)
        # structure = {
        #   "modules": ["auth/", "api/", "models/", "utils/"],
        #   "entry_points": ["main.py", "app.py"],
        #   "dependencies": {"auth/": ["models/", "utils/"]}
        # }

        # Phase 2: Recursive analysis per module
        module_analyses = {}
        for module in structure["modules"]:
            module_content = self._load_module(repo_path, module)
            if self._is_relevant(module_content, question):
                # Recursive call: analyze this module
                analysis = self.process(
                    f"Analyze this module for: {question}\n\n{module_content}"
                )
                module_analyses[module] = analysis

        # Phase 3: Cross-module synthesis
        # Navigate back to original structure for context
        synthesis = self._synthesize_cross_module(
            question=question,
            structure=structure,
            module_analyses=module_analyses
        )

        return {
            "answer": synthesis,
            "modules_analyzed": list(module_analyses.keys()),
            "dependency_map": structure["dependencies"]
        }

    def _is_relevant(self, content: str, question: str) -> bool:
        """Quick relevance check — avoids processing irrelevant modules."""
        # Use a fast SLM for classification
        return self.fast_slm.classify_relevance(content, question)
```

## Pattern 3: Long-Document Reasoning RLM

```python
class DocumentReasoningRLM(RecursiveLanguageModel):
    def analyze_document(self, document: str, task: str) -> dict:
        """Reason over a long document (e.g., 500-page contract)."""
        
        # Phase 1: Structural decomposition
        sections = self._decompose_document(document)
        # sections = [
        #   {"title": "Definitions", "content": "...", "page_range": "1-12"},
        #   {"title": "Obligations", "content": "...", "page_range": "13-45"},
        #   ...
        # ]

        # Phase 2: Build reference index (navigate back during synthesis)
        definitions = self._extract_definitions(sections[0])
        # This index is available to all subsequent recursive calls

        # Phase 3: Recursive analysis per section
        section_analyses = []
        for section in sections:
            if section["title"] == "Definitions":
                continue  # Already processed
            
            # Enrich with definitions for cross-referencing
            enriched = self._enrich_with_definitions(section, definitions)
            
            # Recursive call with focused context
            analysis = self.process(
                f"Task: {task}\n\nSection: {section['title']}\n"
                f"Definitions context: {definitions}\n\n{enriched}"
            )
            section_analyses.append(analysis)

        # Phase 4: Final synthesis with full navigation
        final = self._synthesize(
            task=task,
            section_analyses=section_analyses,
            definitions=definitions,
            original_structure=[s["title"] for s in sections]
        )

        return {
            "analysis": final,
            "sections_processed": len(section_analyses),
            "cross_references": self._extract_cross_refs(section_analyses)
        }
```

## Pattern 4: Privacy-First Local-Cloud Hybrid RLM

```python
class PrivacyFirstRLM:
    """RLM that keeps most processing local, only sending summaries to cloud."""
    
    def __init__(self, local_slm, cloud_frontier=None):
        self.local = local_slm  # RLM-Qwen3-8B running locally
        self.cloud = cloud_frontier  # Optional, only for final synthesis
        self.local_token_count = 0
        self.cloud_token_count = 0

    def process(self, input_text: str, task: str) -> str:
        # All recursive decomposition and per-snippet processing: LOCAL
        snippets = self._decompose(input_text)
        partial_results = []
        
        for snippet in snippets:
            result = self.local.process(snippet)  # Stays on device
            partial_results.append(result)
            self.local_token_count += len(snippet)

        # Build local synthesis
        local_synthesis = self.local.synthesize(partial_results)
        
        if self.cloud and self._needs_frontier_reasoning(task):
            # Only send the SUMMARY to cloud — not raw data
            # This keeps the vast majority of data local
            self.cloud_token_count += len(local_synthesis)
            return self.cloud.refine(local_synthesis, task)
        else:
            return local_synthesis

    def privacy_report(self) -> dict:
        total = self.local_token_count + self.cloud_token_count
        return {
            "local_processing_pct": self.local_token_count / total * 100,
            "cloud_processing_pct": self.cloud_token_count / total * 100,
            "data_exfiltrated": "Summaries only — raw data stayed local"
        }
```

## Cost Comparison: RLM vs. Frontier Model

| Approach | Model | Parameters | Cost per Task | Quality |
|----------|-------|-----------|---------------|---------|
| Frontier (context dump) | GPT-5 | ~1T | $$$$ | Degrades on long context |
| RLM with SLM | RLM-Qwen3-8B | 8B | $ | +28.3% over base, approaches GPT-5 on 3 tasks |
| RLM hybrid (local + cloud) | RLM-Qwen3-8B + GPT-5 | Mixed | $$ | Frontier synthesis quality, local privacy |

**Key insight:** RLM-Qwen3-8B is 50x smaller than GPT-5 yet approaches its quality on long-context tasks. This makes it viable for local-first deployment — the privacy and cost advantages compound.