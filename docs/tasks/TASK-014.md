# TASK-014: App Entry + Shell

## Dependencies: TASK-003, TASK-006
## Wave: 4
## Files: `app/src/main.tsx`, `app/src/App.tsx`, `app/src/components/AppShell.tsx`, `app/src/components/EmptyState.tsx`

## Instructions
**main.tsx**: `createRoot(document.getElementById('root')!).render(<App />)`. Import globals.css.

**App.tsx**: Read `layoutStore.theme`. Apply `data-theme` attribute on root div. Render `<AppShell />`. Root div: `w-screen h-screen bg-bg text-text-primary overflow-hidden`.

**AppShell.tsx**: Flex column, full viewport. Toolbar (top, h-12) → progress bar (h-1, when loading) → PanelContainer (flex-1) or EmptyState → TimelineBar (bottom, h-14, only when data loaded). Check `dataStore.sources.length > 0` to decide.

**EmptyState.tsx**: Centered content: Jig logo/name, description text "Open an MCAP file, connect to a robot, or load demo data", three buttons (Open MCAP, Connect, Demo). Each button has an icon from lucide-react (FolderOpen, Wifi, Play). `data-testid="empty-state"`, `data-testid="btn-open-mcap"`, `data-testid="btn-connect"`, `data-testid="btn-demo"`.

## Acceptance Criteria
- [ ] App renders EmptyState when no data
- [ ] Theme class applied to root
- [ ] All data-testid attributes present

## Error UX Requirements

**AppShell must include a toast notification system.** Create a simple toast component:
- `src/components/Toast.tsx`: floating notification in bottom-right corner
- Auto-dismiss after 5 seconds, or click to dismiss
- Three variants: success (green), warning (amber), error (red)
- Export a `useToast()` hook or store action that any component can call: `showToast({ message, variant })`
- All error handlers throughout the app call this instead of console.error alone

**AppShell must include an error boundary** wrapping each panel:
- If a panel crashes, show "Something went wrong" + Retry button inside that panel
- Other panels continue working (error is contained)

Add `data-testid="toast-container"` to the toast container.
