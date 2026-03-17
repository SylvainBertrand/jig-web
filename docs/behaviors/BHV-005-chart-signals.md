# BHV-005: Chart Signal Management

## Scenario 1: Add signal via picker
GIVEN chart panel is visible
WHEN I click "Add" in the signal bar
THEN a signal picker popover appears with search input
WHEN I type "position" and click "position[0]"
THEN a new trace appears on the chart
AND a signal chip appears in the signal bar

## Scenario 2: Remove signal
WHEN I click x on a signal chip
THEN the trace and chip disappear

## Scenario 3: Drag-drop from Topic Browser
WHEN I drag a field from Topic Browser onto the Chart
THEN the chart shows a new trace for that field

## Scenario 4: Multiple signals distinct colors
GIVEN three signals on the chart
THEN each trace has a visually distinct color
