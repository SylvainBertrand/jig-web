# Playwright E2E Testing Patterns

## Setup

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  timeout: 30000,
  retries: 1,
  use: {
    baseURL: 'http://localhost:5173',
    headless: true,
    screenshot: 'only-on-failure',
  },
  webServer: {
    command: 'npm run dev',
    port: 5173,
    reuseExistingServer: true,
    timeout: 15000,
  },
});
```

```bash
# Install browser
npx playwright install chromium
```

## Basic Test Structure

```typescript
import { test, expect } from '@playwright/test';

test.describe('App Load', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
    await page.waitForLoadState('networkidle');
  });

  test('shows empty state with three buttons', async ({ page }) => {
    await expect(page.getByTestId('empty-state')).toBeVisible();
    await expect(page.getByTestId('btn-open-mcap')).toBeVisible();
    await expect(page.getByTestId('btn-connect')).toBeVisible();
    await expect(page.getByTestId('btn-demo')).toBeVisible();
  });

  test('no console errors on load', async ({ page }) => {
    const errors: string[] = [];
    page.on('console', (msg) => {
      if (msg.type() === 'error') errors.push(msg.text());
    });
    await page.goto('/');
    await page.waitForTimeout(2000);
    expect(errors).toHaveLength(0);
  });
});
```

## Testing Demo Mode

```typescript
test.describe('Demo Mode', () => {
  test('demo loads and populates panels', async ({ page }) => {
    await page.goto('/');
    await page.getByTestId('btn-demo').click();

    // Wait for panels to render
    await page.waitForTimeout(1000);

    // Empty state should be gone
    await expect(page.getByTestId('empty-state')).not.toBeVisible();

    // Panel container should have content
    await expect(page.getByTestId('panel-container')).toBeVisible();

    // Timeline bar should appear
    await expect(page.getByTestId('btn-play-pause')).toBeVisible();
  });

  test('3D viewer has a canvas', async ({ page }) => {
    await page.goto('/');
    await page.getByTestId('btn-demo').click();
    await page.waitForTimeout(1500);

    // R3F renders to a canvas element
    const canvas = page.locator('canvas');
    await expect(canvas.first()).toBeVisible();
  });
});
```

## Testing Timeline Controls

```typescript
test.describe('Timeline', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
    await page.getByTestId('btn-demo').click();
    await page.waitForTimeout(1000);
  });

  test('play advances time', async ({ page }) => {
    const timeBefore = await page.getByTestId('time-display').textContent();
    await page.getByTestId('btn-play-pause').click();
    await page.waitForTimeout(500);
    await page.getByTestId('btn-play-pause').click(); // pause
    const timeAfter = await page.getByTestId('time-display').textContent();
    expect(timeAfter).not.toBe(timeBefore);
  });

  test('keyboard space toggles play/pause', async ({ page }) => {
    await page.keyboard.press('Space');
    await page.waitForTimeout(300);
    await page.keyboard.press('Space');
    // Verify time changed (similar to above)
  });

  test('keyboard arrows step timeline', async ({ page }) => {
    const timeBefore = await page.getByTestId('time-display').textContent();
    await page.keyboard.press('ArrowRight');
    const timeAfter = await page.getByTestId('time-display').textContent();
    expect(timeAfter).not.toBe(timeBefore);
  });
});
```

## Testing Theme Toggle

```typescript
test('theme toggle switches to light', async ({ page }) => {
  await page.goto('/');

  // Default should be dark (no 'light' class)
  const root = page.locator('body > div').first();
  await expect(root).not.toHaveClass(/light/);

  // Click theme toggle
  await page.getByTestId('btn-theme-toggle').click();

  // Should now have 'light' class
  await expect(root).toHaveClass(/light/);
});
```

## Testing Panel System

```typescript
test.describe('Panels', () => {
  test('add panel creates new tab', async ({ page }) => {
    await page.goto('/');
    await page.getByTestId('btn-demo').click();
    await page.waitForTimeout(1000);

    // Count tabs before
    const tabsBefore = await page.locator('.flexlayout__tab_button').count();

    // Open add panel dropdown and click Chart
    await page.getByTestId('btn-add-panel').click();
    await page.getByText('Chart').click();

    const tabsAfter = await page.locator('.flexlayout__tab_button').count();
    expect(tabsAfter).toBeGreaterThan(tabsBefore);
  });
});
```

## Testing Topic Browser Search

```typescript
test('topic search filters results', async ({ page }) => {
  await page.goto('/');
  await page.getByTestId('btn-demo').click();
  await page.waitForTimeout(1000);

  const searchInput = page.getByTestId('topic-search');
  await searchInput.fill('position');
  await page.waitForTimeout(200); // debounce

  // Should show filtered results containing "position"
  const visibleItems = page.locator('[data-testid*="signal-"]');
  const count = await visibleItems.count();
  expect(count).toBeGreaterThan(0);
});
```

## Key Patterns

- **Always `waitForTimeout` or `waitForSelector` after actions** that trigger async state updates.
- **Use `data-testid`** attributes for reliable selectors (the integrator agent adds these).
- **`page.goto('/')` + `waitForLoadState('networkidle')`** ensures the app is fully loaded.
- **FlexLayout tabs** use `.flexlayout__tab_button` class — use this for tab counting/clicking.
- **Canvas elements** (Three.js, uPlot) can't be inspected for content — just verify they exist and have dimensions.
- **Don't test library internals** — test user-visible behavior.

## Verified — Standard well-documented APIs. Low hallucination risk.
