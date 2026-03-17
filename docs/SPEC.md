# Jig Web — Specification Reference

**Note**: Detailed implementation instructions are in `docs/tasks/TASK-NNN.md`. Behavioral acceptance criteria are in `docs/behaviors/`. Usage scenarios are in `docs/scenarios/`.

## Overview

Jig is a browser-based robotics data visualization workbench. It loads MCAP log files, connects to live robots via rosbridge WebSocket, and provides synchronized 3D visualization, time-series charting, and camera image display. Client-side only, no backend.

## UX Priority Map

High-frequency features must be reachable in fewer interactions.

| Feature | Frequency | Max Actions | Method |
|---|---|---|---|
| Play/Pause | Every session, many times | 1 | Keyboard (Space) or 1 click |
| Scrub timeline | Every session, many times | 1 | Click/drag on scrub bar |
| Add signal to chart | Every session, many times | 1 | Drag-drop from Topic Browser |
| Step forward/backward | Every session, many times | 1 | Keyboard (arrows) |
| Open MCAP file | Once per session | 2 | Click Open + file picker |
| Create new chart | Multiple per session | 1 | Double-click signal in Topic Browser |
| Remove signal from chart | Multiple per session | 1 | Click × on signal chip |
| Change playback speed | Occasional | 2 | Click dropdown + select |
| Add annotation | Occasional | 2 | Click flag + type label |
| Toggle theme | Rare | 1 | Click icon |
| Export data | Rare | 3+ | Dialog is appropriate |
| Connect rosbridge | Rare | 3+ | Dialog is appropriate |
| Add environment object | Rare | 3+ | Settings panel is appropriate |

## Usage Scenarios

See `docs/scenarios/` for full descriptions. CC should generate 2-3 additional scenarios during Wave 0.

- **Scenario A**: Joint tracking error investigation — compare joints across legs with 3D + camera
- **Scenario B**: IMU drift analysis — plot angular velocity + integral transform
- **Scenario C**: Quick log triage — scan topics, double-click to create charts, annotate
- **Scenario D**: Multi-log comparison — overlay same signals from two files

## Feature Tiers

| Feature | Tier |
|---|---|
| Panel system (split, tab, drag, resize, save/load) | CORE |
| 3D Viewer (URDF, joints, frames, overlays, env objects, camera tracking) | CORE |
| Chart (uPlot, signals, transforms, expressions, cursor sync) | CORE |
| Image (display, bbox overlays, keypoints) | CORE |
| Topic Browser (tree, search, drag-drop, grouped plotting) | CORE |
| Timeline (play/pause/scrub/step, annotations, multi-source bars) | CORE |
| Demo mode (synthetic data, works out of the box) | CORE |
| Dark/light theme | CORE |
| Keyboard shortcuts (Space, arrows, Home/End, A) | CORE |
| MCAP loading (small files, Web Worker, progress, error handling) | CORE |
| Multi-log comparison | CORE |
| Export (CSV/JSON, time-range crop) | CORE |
| Unit tests (vitest, 100+) | CORE |
| E2E tests (playwright, 15+) | CORE |
| README with setup instructions | CORE |
| Error UX (user-visible messages for all failures) | CORE |
| Remote session (rosbridge WebSocket) | SHELL |
| MCAP large file support (indexed chunked loading) | SHELL |
| MCAP overview series (downsampled band) | SHELL |
| On-demand range loading + memory eviction | SHELL |
| IndexedDB chunk cache | SHELL |

## Error UX Requirements

Every error must be visible to the user. Specific requirements:

- **MCAP load failure**: Toast/banner with filename and error message
- **MCAP unsupported encoding**: Topic Browser shows warning icon per topic, notification with count
- **Rosbridge connection failure**: Dialog shows "Connection failed — is rosbridge running?"
- **Export failure**: Toast with error message
- **Runtime error in panel**: Panel shows inline error boundary with "Something went wrong" + retry button
- **File picker cancelled**: No error (silent OK), empty state remains

## Acceptance Criteria

See `docs/behaviors/BHV-*.md` for behavioral acceptance criteria.
See `docs/scenarios/SCENARIO-*.md` for usage scenario acceptance criteria.

### Summary Checklist
- [ ] All 15 behavioral specs pass (verified via Playwright MCP in Wave 8)
- [ ] All 4 usage scenarios are completable
- [ ] Every [CORE] feature has a visible, reachable UI element
- [ ] Zero TypeScript errors
- [ ] Zero empty function stubs
- [ ] Unit tests: 100+ passing
- [ ] E2E tests: 15+ passing
- [ ] README.md exists with working quick start
- [ ] Every error path shows a user-visible message
- [ ] Dev server starts and demo mode works end-to-end
