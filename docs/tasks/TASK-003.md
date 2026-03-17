# TASK-003: Constants & Theme CSS

## Dependencies: TASK-001
## Wave: 1

## Files to Create
- `app/src/constants.ts`
- `app/src/styles/globals.css`

## Skills: `.claude/skills/tailwind-theming.md`

## Instructions

**constants.ts**: Export these objects exactly:
- `DARK_THEME`: `{ bg: '#0f1117', surface: '#1a1d27', surfaceHover: '#232636', border: '#2a2d3a', borderLight: '#363a4a', textPrimary: '#e2e4e9', textSecondary: '#8b8fa3', textMuted: '#555970', accent: '#3b82f6', accentHover: '#2563eb', success: '#22c55e', warning: '#f59e0b', error: '#ef4444' }`
- `LIGHT_THEME`: `{ bg: '#f8f9fb', surface: '#ffffff', surfaceHover: '#f1f3f5', border: '#e2e4e9', borderLight: '#d1d5db', textPrimary: '#1a1d27', textSecondary: '#6b7280', textMuted: '#9ca3af', accent: '#2563eb', accentHover: '#1d4ed8', success: '#16a34a', warning: '#d97706', error: '#dc2626' }`
- `CHART_COLORS`: `['#3b82f6','#22c55e','#f59e0b','#ef4444','#a855f7','#ec4899','#14b8a6','#f97316']`
- `SOURCE_PALETTE`: `['#3b82f6','#22c55e','#f59e0b','#ef4444','#a855f7','#ec4899']`
- `ANNOTATION_PALETTE`: `['#ef4444','#f59e0b','#22c55e','#3b82f6','#a855f7','#ec4899']`
- `DEFAULT_PLAYBACK_SPEEDS`: `[0.1, 0.25, 0.5, 1, 2, 5, 10]`
- `SMALL_FILE_THRESHOLD`: `200 * 1024 * 1024` (200MB)
- `OVERVIEW_BUCKET_COUNT`: `2000`
- `LIVE_BUFFER_SECONDS`: `300`

**globals.css**: Tailwind directives + CSS custom properties for dark (`:root`) and light (`.light` or `[data-theme="light"]`) themes. All colors as `var(--name)`. Import `flexlayout-react/style/dark.css`. Include FlexLayout CSS overrides per skill file (tab styling, splitter, borders matching theme).

## Acceptance Criteria
- [ ] All constants exported with correct values
- [ ] CSS variables work for both dark and light themes
- [ ] FlexLayout base CSS imported and overridden
