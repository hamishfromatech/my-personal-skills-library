# CodeRabbit PR Analysis: AI vs. Human Code Generation Report

## Study Design

CodeRabbit analyzed **470 open-source GitHub pull requests** — 320 AI-co-authored and 150 human-only — using a structured issue taxonomy to compare quality systematically. The analysis covered readability, security, logic/correctness, error handling, performance, concurrency, dependency correctness, and formatting.

## Key Findings

### Overall Issue Density
- **AI-generated PRs:** 10.83 issues per PR
- **Human-only PRs:** 6.45 issues per PR
- **Difference:** 1.7× more issues in AI-generated code

### Readability Issues
- **3× higher** readability issues in AI contributions — the single largest difference across the dataset
- AI optimizes for working code, not human comprehension
- Symptoms: long functions, inconsistent naming, minimal comments, nested complexity

### Security Vulnerabilities
- **2.74× more security issues** per PR in AI-generated code
- Most common pattern: improper password handling
- Extended catalogue: input validation failures, authentication bypasses, SQL injection risks, hardcoded credentials
- AI training data includes insecure examples; models lack security-first thinking

### Logic and Correctness
- **75% more common** in AI PRs
- Business logic errors, misconfigurations, edge case handling failures

### Error Handling
- **Nearly 2× more often** omitted in AI code
- Missing null checks, early returns, guardrails, comprehensive exception logic

### Performance Regressions
- Small in number but **heavily skewed toward AI**
- Excessive I/O operations approximately **8× more common**
- Concurrency and dependency correctness saw roughly **2× increases**

### Formatting and Naming
- **2.66× more formatting problems** despite automated formatters
- **Nearly 2× more naming inconsistencies** — unclear naming and generic identifiers

### High-Issue Outliers
- Much more common in AI PRs, creating heavy review workloads
- Unlike human code where error rates correlate with developer experience, AI code quality is unpredictable
- Every line requires verification regardless of how plausible it appears

## Citation
CodeRabbit. "State of AI vs Human Code Generation Report." December 2025.
