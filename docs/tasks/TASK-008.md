# TASK-008: Demo Data Generator

## Dependencies: TASK-005, TASK-006
## Wave: 3
## Files: `app/src/core/demoData.ts`
## Read: `src/types.ts`, `src/stores/dataStore.ts`, `src/stores/annotationStore.ts`

## Instructions
Export a function `generateDemoData()` that populates dataStore and annotationStore with synthetic data. 30 seconds at 100Hz = 3000 samples. Timestamps: `[0.00, 0.01, 0.02, ..., 29.99]`.

**Topic `/joint_states`** (schema `sensor_msgs/JointState`):
- `position[0..5]`: sinusoids. Frequencies: [0.5,0.3,0.7,0.4,0.6,0.2] Hz. Amplitudes: [1.0,0.8,0.6,0.9,0.5,0.7] rad. Phases: [0,π/4,π/2,π/3,π/6,π/5]. Formula: `A * sin(2π * f * t + φ)`.
- `velocity[0..5]`: analytical derivatives: `A * 2π * f * cos(2π * f * t + φ)`.
- `effort[0..5]`: noisy sinusoids at 1.5× position freq + gaussian noise σ=0.1. Use Box-Muller for gaussian: `Math.sqrt(-2*Math.log(Math.random())) * Math.cos(2*Math.PI*Math.random()) * sigma`.

**Topic `/imu/data`** (schema `sensor_msgs/Imu`):
- `angular_velocity.x/y/z`: sinusoids at 0.1, 0.2, 0.15 Hz + noise σ=0.05
- `linear_acceleration.x/y`: noise only σ=0.1
- `linear_acceleration.z`: 9.81 + noise σ=0.1

**Topic `/camera/image`** (schema `sensor_msgs/Image`): 10Hz, 300 frames, 320×240, rgb8. Use OffscreenCanvas to generate: gradient blue→green background, white circle (r = 30+20*sin(2π*0.5*t)), timestamp text. Extract pixel data as Uint8Array.

**Topic `/detections`** (schema `vision_msgs/Detection2DArray`): 10Hz. 2 bboxes: one tracking the circle, one fixed corner. 3 keypoints forming a rotating triangle.

**Topic `/debug/overlays`** (schema `visualization_msgs/MarkerArray`): 10Hz. Force vector at approximate end-effector position (compute from joint angles), CoM point (cyan), CoP point on ground (magenta).

**Annotations**: 3 entries: t=5s "Trajectory start" (red), t=15s "Peak oscillation" (amber), t=25s "Wind-down phase" (green). Add via `annotationStore.getState().addAnnotation()`.

Call `dataStore.getState().addSource(...)`, `.addTopic(...)`, `.addScalarSeries(...)`, `.addImageTopic(...)`, `.addOverlayTopic(...)`, `.addDebugOverlayTopic(...)` to populate.

## Acceptance Criteria
- [ ] `/joint_states` has 18 fields × 3000 samples each
- [ ] `/imu/data` has 6 fields × 3000 samples
- [ ] `/camera/image` has 300 frames at 320×240
- [ ] `/detections` has 300 overlay frames
- [ ] `/debug/overlays` has 300 frames with CoM, CoP, force vector
- [ ] 3 annotations at t=5, 15, 25
