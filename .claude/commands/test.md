---
description: "Run the full test suite: TypeScript compilation, vitest unit tests, and Playwright E2E tests. Reports pass/fail status."
---

# /test — Run All Tests

1. `cd app && npx tsc --noEmit` — TypeScript compilation check
2. `cd app && npx vitest run` — Unit tests
3. `cd app && npx playwright test` — E2E browser tests

Report results for each step. If any step fails, show the first 30 lines of errors.
