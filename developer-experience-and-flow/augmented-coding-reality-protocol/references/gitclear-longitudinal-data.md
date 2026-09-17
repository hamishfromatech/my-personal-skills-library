# GitClear Longitudinal Analysis: Code Quality Changes in the AI Era

## Study Design

GitClear analyzed **211 million lines of code changes** from 2020–2024, sourced from repositories owned by major tech companies and enterprises. The longitudinal design tracked how code quality metrics changed as AI adoption increased across the sample period.

## Key Findings

### Refactoring Collapse
- Refactoring dropped from **25% of changed lines in 2021** to **under 10% by 2024**
- Developers accept AI output without the iterative improvement they would apply to human-written code
- The decline accelerated after 2023, coinciding with widespread AI coding tool adoption

### Code Duplication Explosion
- Code duplication increased from **8.3% of changed lines in 2021** to **12.3% by 2024**
- This represents approximately **4× growth** in duplication events
- AI lacks whole-codebase context and regenerates similar logic instead of reusing existing functions

### Code Reuse Reversal
- For the first time historically, **copy-paste code exceeded moved code**
- This reverses two decades of DRY (Don't Repeat Yourself) best practices
- Developers no longer cross-reference before accepting AI output because that would eliminate perceived time savings

### Code Churn Acceleration
- Code churn (premature revisions where code is rewritten shortly after merging) **nearly doubled**
- AI generates code that passes tests but requires revision after integration testing, architectural review, or production deployment

### Maintainer Commentary
Rod Cope, CTO at Perforce Software: "AI is better the more context it has, but there is a limit on how much information can be supplied."

ThoughtWorks researchers observed that complacency sets in with prolonged use of coding assistants. Duplicate code and code churn rose even more than predicted in GitClear's 2024 research.

## Economic Translation
- Duplicated code means bugs require fixes in multiple locations
- Reduced refactoring means complexity compounds until rewrites become necessary
- Code churn means development time gets consumed by rework instead of new features
- These metrics translate directly to maintenance cost increases of 20–40% over 2–3 years

## Citation
GitClear. "Code Quality Changes in the AI Era." Longitudinal analysis of 211 million lines of code changes, 2020–2024.
