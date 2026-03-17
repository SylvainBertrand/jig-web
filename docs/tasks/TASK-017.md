# TASK-017: Panel Container + Registry

## Dependencies: TASK-002
## Wave: 4
## Files: `app/src/components/PanelContainer.tsx`, `app/src/panels/registry.ts`
## Skills: `flexlayout-patterns.md`

## Instructions
**registry.ts**: Export `PANEL_REGISTRY: PanelDefinition[]` with 4 entries: viewer_3d ("3D Viewer", Box icon), chart ("Chart", LineChart icon), image ("Image", Image icon), topic_browser ("Topic Browser", List icon). Each has appropriate defaultConfig. Components will be lazy-imported — for now use placeholder components that render `<div>Panel: {type}</div>`. These get replaced in Wave 5.

**PanelContainer.tsx**: Wrap FlexLayout `<Layout>`. Create default Model JSON: row with Topic Browser (weight 20) on left, right column (weight 80) with 3D Viewer (weight 60 top) and tabset (Chart + Image tabs, weight 40 bottom). Factory function maps `node.getComponent()` to registry components, passing PanelProps. Auto-persist layout JSON to localStorage on model change. Restore from localStorage on mount. Export a function `addPanel(type: string)` that other components can call. `data-testid="panel-container"`.

## Acceptance Criteria
- [ ] Default layout renders 4 panels in correct arrangement
- [ ] FlexLayout handles split/tab/resize/close
- [ ] Layout persists to localStorage
- [ ] addPanel function works
