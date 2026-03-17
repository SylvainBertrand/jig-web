# Task Index — Jig Web 1-Shot v3

All tasks are atomic. Each runs in a fresh subagent context.
Tasks within the same wave are independent and can run in parallel.

## Wave 0 — Design System (before any code)

| ID | Title | Outputs |
|---|---|---|
| TASK-000 | Design system generation | docs/DESIGN-SYSTEM.md |

## Wave 1 — Foundation (no dependencies)

| ID | Title | Files Created |
|---|---|---|
| TASK-001 | Project config | package.json, tsconfig.json, vite.config.ts, etc. |
| TASK-002 | Core types | src/types.ts |
| TASK-003 | Constants & theme | src/constants.ts, src/styles/globals.css |

## Wave 2 — Stores (depends: Wave 1)

| ID | Title | Files Created |
|---|---|---|
| TASK-004 | Timeline store | src/stores/timelineStore.ts |
| TASK-005 | Data store | src/stores/dataStore.ts |
| TASK-006 | Session, Layout, Annotation stores | 3 store files |
| TASK-007 | Demo URDF | public/demo/robot.urdf |

## Wave 3 — Core Logic (depends: Wave 2)

| ID | Title | Files Created |
|---|---|---|
| TASK-008 | Demo data generator | src/core/demoData.ts |
| TASK-009 | Transforms engine | src/core/transforms.ts |
| TASK-010 | MCAP worker + loader | src/core/mcapWorker.ts, mcapLoader.ts, mcapIndex.ts |
| TASK-011 | Rosbridge client | src/core/rosbridgeClient.ts |
| TASK-012 | Utilities | src/core/downsample.ts, chunkCache.ts, mcapExporter.ts |
| TASK-013 | Custom hooks | src/hooks/*.ts |

## Wave 4 — Shell Components (depends: Wave 2 + 3)

| ID | Title | Files Created |
|---|---|---|
| TASK-014 | App entry + shell | src/main.tsx, App.tsx, AppShell.tsx, EmptyState.tsx |
| TASK-015 | Toolbar | src/components/Toolbar.tsx |
| TASK-016 | Timeline bar | src/components/TimelineBar.tsx |
| TASK-017 | Panel container + registry | PanelContainer.tsx, registry.ts |
| TASK-018 | Dialogs | ConnectionDialog.tsx, ExportDialog.tsx |

## Wave 5 — Panels (depends: Wave 4)

| ID | Title | Files Created |
|---|---|---|
| TASK-019 | Topic Browser | TopicBrowserPanel.tsx, SignalTree.tsx |
| TASK-020 | 3D Viewer — scene | Viewer3DPanel.tsx, SceneSetup.tsx, FrameAxes.tsx |
| TASK-021 | 3D Viewer — robot + overlays | RobotModel.tsx, DebugOverlays.tsx, EnvironmentObjects.tsx |
| TASK-022 | Chart — core | ChartPanel.tsx, UPlotChart.tsx |
| TASK-023 | Chart — pickers | SignalPicker.tsx, TransformPicker.tsx, ExpressionEditor.tsx |
| TASK-024 | Image panel | ImagePanel.tsx, ImageOverlays.tsx |

## Wave 6 — Integration + Polish (depends: Wave 5)

| ID | Title | What |
|---|---|---|
| TASK-025 | Design polish | Apply design system from TASK-000 |
| TASK-026 | Integration wiring | Wire all components, keyboard shortcuts, layout persistence |

## Wave 6.5 — Smoke Test + Design Audit

| ID | Title | What |
|---|---|---|
| TASK-030 | Smoke test + design audit | Verify app runs, demo loads, design system applied |

## Wave 7 — Testing (depends: Wave 6.5)

| ID | Title | Files Created |
|---|---|---|
| TASK-027 | Unit tests | vitest.config.ts, src/__tests__/*.test.ts |
| TASK-028 | E2E tests | playwright.config.ts, e2e/*.spec.ts |
| TASK-029 | README + docs | README.md |

## Wave 8 — Playwright MCP Black-Box QA

| ID | Title | What |
|---|---|---|
| TASK-031 | Black-box QA | Behavioral specs + scenarios + spec completeness audit |

## Wave 9 — Fix Loop

| ID | Title | What |
|---|---|---|
| TASK-032 | Fix QA issues | Fix bugs from Wave 8, max 3 iterations |

---

**Total: 33 tasks, 10 waves, ~50 files**
