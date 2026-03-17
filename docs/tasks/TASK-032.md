# TASK-032: Fix QA Issues (Iteration 1)

## Dependencies: TASK-031
## Wave: 9

## Instructions

Read the three QA reports in `docs/reports/`:
- `qa-behavioral.md`
- `qa-scenarios.md`
- `qa-spec-completeness.md`

For each FAIL and MISSING item, prioritize by:
1. **Blocking**: Prevents a usage scenario from working → fix first
2. **High**: Behavioral spec fails → fix second
3. **Medium**: Spec feature missing from UI → fix third

For each fix:
1. Identify the root cause (wrong selector? missing event handler? store not updating?)
2. Fix the code
3. Run `npx tsc --noEmit` after each fix
4. Verify the fix manually or with a quick Playwright check

After all fixes:
```bash
cd app && npm run build
cd app && npx vitest run
git add -A && git commit -m "TASK-032: QA fix iteration 1"
```

If there are still blocking issues, repeat the QA (re-run TASK-031's process) and fix again. Maximum 3 iterations total.

## Post-Task
Update `docs/STATE.md` with final status.
Write `docs/reports/final-summary.md` with:
- Task completion (N/32)
- Build/test results
- Behavioral spec pass rate
- Scenario pass rate
- Spec completeness score
- Top remaining issues
