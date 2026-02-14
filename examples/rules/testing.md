---
paths:
  - "**/*.test.*"
  - "**/*_test.*"
  - "**/test/**"
  - "**/tests/**"
---

# Testing Rules

- Colocate tests with source (e.g., `foo.test.ts` next to `foo.ts`)
- Name test files to match their source files
- Run tests before pushing when you touch logic
- Test behavior, not implementation details
- Use descriptive test names that explain the expected outcome
