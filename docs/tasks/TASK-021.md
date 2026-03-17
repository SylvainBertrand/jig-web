# TASK-021: 3D Viewer — Robot + Overlays

## Dependencies: TASK-020, TASK-005
## Wave: 5
## Files: `app/src/panels/viewer3d/RobotModel.tsx`, `app/src/panels/viewer3d/DebugOverlays.tsx`, `app/src/panels/viewer3d/EnvironmentObjects.tsx`
## Skills: `urdf-loading.md`, `threejs-r3f-patterns.md`

## Instructions
**RobotModel**: Load URDF via urdf-loader. Support multiple instances (one per RobotAppearance in config). Each driven by its sourceId's joint data. On useFrame: read currentTime → binary search joint position series → setJointValue. Appearance: override material color if set, apply opacity, toggle visible. Camera tracking: when trackLink set, report link world position to parent for OrbitControls target update.

**DebugOverlays**: Read from dataStore.getDebugOverlayAt(). Render VectorOverlay as ArrowHelper, PointOverlay as small sphere + Html label from drei, SphereOverlay as transparent mesh.

**EnvironmentObjects**: Render primitives from config: box (boxGeometry), cylinder, sphere, plane. Each with MeshStandardMaterial and specified color.

## Acceptance Criteria
- [ ] Demo URDF loads and renders
- [ ] Joint positions animate with timeline
- [ ] Debug overlays visible (CoM, CoP, force vector)
- [ ] Environment primitives render
