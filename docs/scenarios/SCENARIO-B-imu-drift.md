# Scenario B: IMU Drift Analysis

## User Goal
I want to check if the IMU is drifting by plotting angular velocity and looking at the integral over time, while cross-referencing with the robot's orientation in 3D.

## Required Layout
- Chart: IMU angular_velocity.x/y/z + integral transform of angular_velocity.z
- 3D Viewer: robot orientation

## Required Interactions
1. Open MCAP (or Demo)
2. Add IMU angular velocity signals to chart
3. Apply integral transform to angular_velocity.z
4. Scrub timeline and observe if integrated value diverges
5. Compare with 3D viewer orientation

## What This Tests
- Transform engine works correctly (integral)
- Multiple signals + transform output on same chart
- Cross-panel sync between chart and 3D viewer
