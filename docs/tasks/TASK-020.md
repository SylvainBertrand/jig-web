# TASK-020: 3D Viewer — Scene

## Dependencies: TASK-017
## Wave: 5
## Files: `app/src/panels/viewer3d/Viewer3DPanel.tsx`, `app/src/panels/viewer3d/SceneSetup.tsx`, `app/src/panels/viewer3d/FrameAxes.tsx`
## Skills: `threejs-r3f-patterns.md`

## Instructions
**Viewer3DPanel**: Canvas from @react-three/fiber. Contains SceneSetup + RobotModel(s) + DebugOverlays + EnvironmentObjects + FrameAxes. Settings popover (gear icon): toggle grid/frames/overlays, background color presets, camera track dropdown, robot list, env object controls.

**SceneSetup**: Ambient light (0.4), directional light (0.8 at [5,10,5]). Ground grid on XY plane (Z-up: rotate gridHelper -π/2 around X). 1m squares, 20m extent. OrbitControls with damping, Z-up. Camera initial position [3,3,3], target [0,0,0].

**FrameAxes**: When showFrames=true, render RGB XYZ triads (0.15m) at each robot link origin. Use ArrowHelper or line geometry.

Camera `up` must be `[0,0,1]` everywhere (Z-up robotics convention).

## Acceptance Criteria
- [ ] Canvas renders with lights and grid
- [ ] Grid is on XY plane (Z-up)
- [ ] OrbitControls work (orbit, pan, zoom)
- [ ] Settings popover toggles grid/frames
