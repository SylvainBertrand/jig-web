# Scenario A: Joint Tracking Error Investigation

## User Goal
I want to compare the joint position tracking error for the left and right legs while watching the 3D robot state and checking the logger camera for visual anomalies.

## Required Layout
- Chart 1: left leg joint positions + commands (overlaid for tracking error)
- Chart 2: right leg joint positions + commands
- 3D Viewer: robot pose synced to timeline
- Image: camera feed synced to timeline

## Required Interactions
1. Open MCAP file
2. In Topic Browser, find joint_states topic, expand fields
3. Drag position[0], position[1], position[2] to Chart 1
4. Drag position[3], position[4], position[5] to Chart 2
5. Scrub timeline to areas of interest
6. Verify all 4 panels stay synced at the same time
7. Add annotations at interesting moments
8. Export the annotated time range as CSV

## What This Tests
- Panel system supports 4+ panels visible simultaneously
- Drag-drop from Topic Browser to specific chart instances works
- Cross-panel timeline synchronization is accurate
- Annotations appear on all charts
- Export respects time range selection
