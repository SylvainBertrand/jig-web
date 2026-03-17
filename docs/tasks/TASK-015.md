# TASK-015: Toolbar

## Dependencies: TASK-006, TASK-017
## Wave: 4
## Files: `app/src/components/Toolbar.tsx`
## Read: `src/panels/registry.ts` (for panel type list)

## Instructions
Height 48px, dark surface background. Left to right: Logo "Jig" (bold, accent, monospace) | separator | Open MCAP button (FolderOpen icon, `<input type="file" accept=".mcap" multiple>` hidden) | Connect button (Wifi icon, opens ConnectionDialog) | Demo button (Play icon) | separator | Add Panel dropdown (Plus icon, lists all panel types from registry) | flex spacer | Sources indicator (colored dots for each active source, hover shows label, click shows close menu) | Export button (Download icon, opens ExportDialog, visible only when data loaded) | Layout dropdown (Layout icon — save name input, load/delete list) | Theme toggle (Sun/Moon icon). `data-testid="btn-add-panel"`, `data-testid="btn-theme-toggle"`.

## Acceptance Criteria
- [ ] All buttons render with correct icons
- [ ] File input accepts .mcap with multiple selection
- [ ] Theme toggle calls layoutStore.toggleTheme
- [ ] Add Panel dropdown lists all registered panel types
