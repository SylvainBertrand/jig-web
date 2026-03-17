# TASK-030: Smoke Test + Design Audit

## Dependencies: All Wave 5 + Wave 6 tasks
## Wave: 6.5

## Files to Create/Edit
- Edits to components based on design audit findings

## Instructions

### Part 1: Smoke Test

Run these commands. ALL must pass before proceeding:

```bash
cd app
npm run dev &
sleep 5
# Verify dev server responds
curl -s http://localhost:5173 | grep -q 'Jig' && echo "SMOKE: Dev server OK" || echo "SMOKE: FAILED — dev server not responding"
kill %1

npm run build && echo "SMOKE: Build OK" || echo "SMOKE: Build FAILED"
```

If the smoke test fails, fix the issue before continuing.

### Part 2: Verify Demo Mode Works

```bash
cd app && npm run dev &
sleep 5
npx playwright install chromium 2>/dev/null
```

Write and run a quick Playwright script (NOT a permanent test file) that:
1. Opens http://localhost:5173
2. Clicks the "Demo" button
3. Waits 2 seconds
4. Takes a screenshot
5. Verifies: Topic Browser has content, timeline bar is visible, 3D canvas exists, no console errors

If demo mode is broken, fix it now. This is a BLOCKING gate.

### Part 3: Design Audit

Read `docs/DESIGN-SYSTEM.md`. Walk through EVERY component file in `src/components/` and `src/panels/` and check:
1. All colors use CSS variables (no hardcoded hex)
2. Spacing uses 4px/8px grid (no random pixel values)
3. Button heights are 28px (compact)
4. Font sizes match the design system
5. Popovers/dropdowns are used for frequent actions (not modals)
6. FlexLayout chrome (tabs, borders) matches the theme

Fix any violations found.

```bash
kill %1  # stop dev server
```

## Post-Task
```bash
git add -A && git commit -m "TASK-030: Smoke test passed + design audit fixes"
```

## Acceptance Criteria
- [ ] Dev server starts and responds
- [ ] `npm run build` passes
- [ ] Demo mode loads and all panels populate
- [ ] No hardcoded colors in component files
- [ ] Spacing follows 4px/8px grid
