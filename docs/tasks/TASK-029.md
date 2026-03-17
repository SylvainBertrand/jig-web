# TASK-029: README + Documentation

## Dependencies: All previous tasks
## Wave: 7
## Files: `app/README.md`

## Instructions
Write a complete README.md:

```markdown
# Jig — Robotics Visualization Workbench

Browser-based tool for inspecting robot log files, live robot data, and 3D models with synchronized time-series charts.

## Quick Start

\`\`\`bash
cd app
npm install
npm run dev
\`\`\`

Open http://localhost:5173. Click "Demo" to load sample data.

## Features

- **Panel System**: Drag, split, tab, and resize panels (3D Viewer, Chart, Image, Topic Browser)
- **MCAP Log Loading**: Open .mcap files via File API, parsed in Web Worker
- **3D Viewer**: URDF robot rendering, joint animation, coordinate frames, debug overlays
- **Charts**: uPlot time-series with signal picker, transforms, expressions
- **Image Panel**: Camera feed display with bounding box and keypoint overlays
- **Timeline**: Play/pause/scrub/step with annotation markers
- **Multi-Log**: Compare signals from multiple log files on the same chart
- **Rosbridge**: Connect to live ROS 2 robots via WebSocket
- **Export**: Crop and download data as CSV or JSON
- **Theming**: Dark and light themes

## Build

\`\`\`bash
npm run build    # Production build
npm run test     # Unit tests (vitest)
npm run e2e      # E2E tests (playwright)
\`\`\`

## Tech Stack

React 18, TypeScript, Vite, Tailwind CSS, Three.js, uPlot, FlexLayout-react, @mcap/browser, Zustand, mathjs.

## Keyboard Shortcuts

| Key | Action |
|---|---|
| Space | Play/Pause |
| ←/→ | Step backward/forward |
| Home/End | Seek to start/end |
| A | Add annotation |
```

## Acceptance Criteria
- [ ] README.md exists with setup instructions
- [ ] Running the Quick Start steps works
