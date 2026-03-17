# TASK-028: E2E Tests

## Dependencies: TASK-026
## Wave: 7
## Files: `app/playwright.config.ts`, `app/e2e/app-load.spec.ts`, `app/e2e/demo-mode.spec.ts`, `app/e2e/timeline.spec.ts`, `app/e2e/panels.spec.ts`, `app/e2e/theme.spec.ts`, `app/e2e/keyboard.spec.ts`
## Skills: `playwright-patterns.md`

## Instructions
**playwright.config.ts**: testDir ./e2e, headless, baseURL localhost:5173, webServer command `npm run dev`.

Before writing tests: `npx playwright install chromium`.

**app-load.spec.ts**: App loads, empty state visible with 3 buttons, no console errors.

**demo-mode.spec.ts**: Click Demo → panels populate, topic browser shows topics, timeline bar appears, 3D canvas visible, time display shows 00:00.

**timeline.spec.ts**: Play advances time, pause stops, scrub seeks, step buttons work, speed selector works.

**panels.spec.ts**: Default has 4 panels, Add Panel adds new tab, close removes tab.

**theme.spec.ts**: Default dark, toggle switches to light, persists across refresh.

**keyboard.spec.ts**: Space=play/pause, Arrows=step, Home/End=seek.

Use `data-testid` selectors. Use `page.waitForTimeout` after async actions. Run: `npx playwright test`. ALL tests must pass.

## Acceptance Criteria
- [ ] 15+ E2E tests across 6 files
- [ ] All tests pass in headless Chromium
- [ ] Tests cover: load, demo, timeline, panels, theme, keyboard
