# BHV-003: Demo Mode

## Scenario 1: Load demo data
GIVEN the empty state is showing
WHEN I click "Demo"
THEN the empty state disappears
AND the default panel layout appears with 4 panels
AND the Topic Browser shows topics: /joint_states, /imu/data, /camera/image, /detections, /debug/overlays
AND the Chart panel shows at least one signal trace
AND the 3D Viewer shows a colored robot arm on a grid
AND the Image panel shows a camera frame
AND the Timeline bar shows time range 0:00 to 0:30
AND 3 annotation markers appear on the timeline scrub bar

## Scenario 2: Scrub in demo mode
GIVEN demo data is loaded
WHEN I click the timeline scrub bar at the middle
THEN the time display updates to approximately 0:15
AND the 3D robot arm pose changes
AND the chart cursor moves to the new time
AND the camera image updates
