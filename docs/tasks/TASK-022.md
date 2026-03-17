# TASK-022: Chart — Core

## Dependencies: TASK-005, TASK-017
## Wave: 5
## Files: `app/src/panels/chart/ChartPanel.tsx`, `app/src/panels/chart/UPlotChart.tsx`
## Skills: `uplot-patterns.md`

## Instructions
**ChartPanel**: Signal bar at top (chips + "Add" button + "f(x)" button). Chart area below. Accept drops of `application/x-jig-signal` and `application/x-jig-topic-group`. Signal chips show colored dot + displayName (with source prefix if multi-source) + × remove.

**UPlotChart**: IMPERATIVE uPlot wrapper. Create instance in useEffect, destroy on cleanup. NEVER recreate on data change — use setData(). ResizeObserver on container → setSize(). Data: aligned Float64Array arrays [timestamps, values1, values2, ...]. Cursor: draw vertical line at currentTime via uPlot draw hook. Click handler: convert pixel to time, update timelineStore. Annotation markers: colored vertical lines at annotation timestamps. Zoom: uPlot built-in select, sync to timelineStore.viewRange.

**Critical**: uPlot is NOT React-friendly. Do not use uplot-react (it's not in deps). Create the instance imperatively.

**Colors**: Cycle through CHART_COLORS for each signal.

## Acceptance Criteria
- [ ] uPlot instance created and renders
- [ ] ResizeObserver resizes correctly
- [ ] Cursor line at currentTime
- [ ] Click seeks timeline
- [ ] Multiple signals with distinct colors
- [ ] Drop zone accepts signals
