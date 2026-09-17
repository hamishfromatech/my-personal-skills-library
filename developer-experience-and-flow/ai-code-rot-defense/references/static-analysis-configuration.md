# Static Analysis Configuration Templates

Ready-to-use configurations for AI code rot defense tools.

---

## jscpd (Duplication Detection)

```json
{
  "threshold": 5,
  "reporters": ["html", "console"],
  "ignore": ["**/tests/**", "**/node_modules/**", "**/*.min.js"],
  "format": ["javascript", "typescript", "python", "java", "go"],
  "output": "./reports/duplication",
  "blame": true,
  "aiMode": {
    "compareAgainstExisting": true,
    "utilityCatalogPath": "./UTILITIES.md"
  }
}
```

**CI gate:** Fail build if duplication increases >3% from baseline.

---

## SonarQube / SonarCloud

```properties
# sonar-project.properties
sonar.projectKey=atech-project
sonar.sources=src/
sonar.tests=tests/
sonar.exclusions=**/node_modules/**,**/dist/**,**/*.test.js

# AI-specific quality gates
sonar.coverage.exclusions=**/generated/**
sonar.technicalDebt.ratingThreshold=B
sonar.duplications.excludedPaths=**/vendor/**

# Custom rules for AI-generated code
sonar.aiGenerated.labelPath=.ai-generated-labels/
sonar.aiGenerated.reviewRequired=true
```

**Quality gate:**
- Coverage ≥ 70%
- Duplicated lines ≤ 3%
- Maintainability rating ≥ B
- Security rating ≥ A
- AI-generated code must pass all rules (no exemptions)

---

## Semgrep (Security + Custom Rules)

```yaml
# .semgrep/ai-code-rot-rules.yml
rules:
  - id: ai-reimplements-utility
    patterns:
      - pattern: function $FUNC(...) { ... }
      - pattern-not-inside: import { $FUNC } from "utils/"
    message: "Possible utility re-implementation. Check UTILITIES.md"
    severity: WARNING
    languages: [js, ts]

  - id: ai-missing-error-handling
    patterns:
      - pattern: |
          async function $FUNC(...) {
            const $RES = await fetch(...);
            return $RES.json();
          }
    message: "AI-generated fetch without error handling"
    severity: ERROR
    languages: [js, ts]

  - id: ai-hardcoded-value
    patterns:
      - pattern: const $VAR = "..."
      - metavariable-regex:
          metavariable: $VAR
          regex: '(API_KEY|SECRET|PASSWORD|TOKEN)'
    message: "Hardcoded sensitive value detected"
    severity: ERROR
    languages: [js, ts, python]

  - id: ai-console-log
    patterns:
      - pattern: console.log(...)
    message: "console.log found — use structured logger instead"
    severity: WARNING
    languages: [js, ts]
```

---

## dependency-cruiser (Architecture Validation)

```javascript
// .dependency-cruiser.js
module.exports = {
  forbidden: [
    {
      name: 'no-circular',
      severity: 'error',
      from: {},
      to: { circular: true }
    },
    {
      name: 'no-unmatched-dependency',
      severity: 'warn',
      from: {},
      to: { pathNot: '^src/' }
    },
    {
      name: 'ai-domain-separation',
      comment: 'AI-generated code must respect domain boundaries',
      severity: 'error',
      from: { path: '^src/ai-generated/' },
      to: { path: '^src/core/' }
    }
  ],
  options: {
    doNotFollow: { path: 'node_modules' },
    tsConfig: { fileName: './tsconfig.json' },
    reporterOptions: { dot: { collapsePattern: 'node_modules/[^/]+' } }
  }
};
```

---

## GitHub Actions Pipeline

```yaml
name: AI Code Quality Gates

on: [pull_request]

jobs:
  ai-quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Detect AI-generated files
        run: |
          # Label PR if >50% lines from AI
          git diff --stat origin/main | grep -E "\.ai\.(js|ts|py)" > ai-files.txt || true
          # Store label for downstream jobs
          echo "ai_generated=true" >> $GITHUB_ENV

      - name: Run duplication detection
        run: |
          npx jscpd --config .jscpd.json
          # Compare against baseline from main branch
          npx jscpd --config .jscpd.json --baseline reports/main-baseline.json

      - name: Run static analysis
        run: |
          npx semgrep --config .semgrep/ --config "p/owasp-top-ten" src/
          npx dependency-cruiser src/

      - name: Run tests
        run: |
          npm test -- --coverage --coverageThreshold='{"global":{"branches":70}}'

      - name: Architecture review
        run: |
          npx dependency-cruiser src/ --validate

      - name: Post PR comment
        if: failure()
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '⚠️ AI Code Rot Defense gates failed. See logs for duplication, security, or architecture violations.'
            })
```
