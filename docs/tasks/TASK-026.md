# TASK-026: Integration Wiring

## Dependencies: All Wave 5 tasks
## Wave: 6
## Files: Edits across multiple files (cross-cutting)

## Instructions
Wire everything together:
1. **Replace placeholder panel components** in registry.ts with actual imports of Viewer3DPanel, ChartPanel, ImagePanel, TopicBrowserPanel
2. **Fix all import paths** — ensure every component imports from the correct location
3. **Verify demo mode end-to-end**: sessionStore.openDemo → demoData generates → dataStore populates → panels render data
4. **Verify PanelContainer** creates all 4 panel types correctly with PanelProps
5. **Add keyboard shortcuts** in AppShell: Space=play/pause, Arrows=step, Home/End=seek, A=annotate
6. **Add missing data-testid** attributes if any were missed
7. **Run `npm run build`** — must pass with zero errors
8. **Auto-persist FlexLayout**: Ensure the FlexLayout model JSON is saved to localStorage on every change and restored on mount

## Acceptance Criteria
- [ ] `npm run build` passes
- [ ] `npm run dev` starts without errors
- [ ] Demo mode works end-to-end (click Demo → all panels populate)
- [ ] Keyboard shortcuts work
- [ ] Layout persists across refresh
