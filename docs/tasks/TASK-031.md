# TASK-031: Playwright MCP Black-Box QA

## Dependencies: All previous tasks
## Wave: 8

## Files to Create
- `docs/reports/qa-behavioral.md`
- `docs/reports/qa-scenarios.md`
- `docs/reports/qa-spec-completeness.md`

## Tools Required
Playwright MCP server. Use ONLY browser tools:
- browser_navigate, browser_click, browser_type, browser_take_screenshot, browser_snapshot
- Do NOT read source code. Do NOT use Bash to inspect files. Black-box only.

## Instructions

Start the dev server: `cd app && npm run dev &`
Wait for it to be ready.

### Part 1: Behavioral Spec Verification

Read each file in `docs/behaviors/BHV-*.md`. For each scenario:
1. Navigate to http://localhost:5173
2. Perform the GIVEN/WHEN/THEN steps literally
3. Take a screenshot after each THEN assertion
4. Record PASS or FAIL with evidence

Write results to `docs/reports/qa-behavioral.md`:
```markdown
# Behavioral QA Report

## BHV-001: Empty State
- Scenario 1: [PASS/FAIL] — [evidence]

## BHV-002: Open MCAP
- Scenario 1: [PASS/FAIL] — [evidence]
...
```

### Part 2: Usage Scenario Verification

Read each file in `docs/scenarios/SCENARIO-*.md`. For each:
1. Attempt the full workflow described
2. Note where the workflow breaks or requires unexpected workarounds
3. Take screenshots at key steps

Write results to `docs/reports/qa-scenarios.md`.

### Part 3: Spec Completeness Audit

Read `docs/SPEC.md`. For EVERY `[CORE]` feature listed:
1. Find the UI element that enables this feature
2. If the feature has no visible/reachable UI element, mark as MISSING
3. If the feature has a UI element but it doesn't work, mark as BROKEN

Write results to `docs/reports/qa-spec-completeness.md`.

### Part 4: Summary

At the end, write a summary: total PASS/FAIL/MISSING counts, and a prioritized list of the top 10 blocking issues.

```bash
kill %1  # stop dev server
git add -A && git commit -m "TASK-031: Black-box QA reports"
```
