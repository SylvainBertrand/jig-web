# TASK-002: Core TypeScript Interfaces

## Dependencies: TASK-001
## Wave: 1

## Files to Create
- `app/src/types.ts`

## Instructions

Create `app/src/types.ts` with ALL interfaces below. This is the single source of truth for the entire application. Every other file imports from here. **Do not omit or rename any interface.**

```typescript
// DATA SOURCES
export type SourceId = string; // e.g. "demo", "file:experiment.mcap", "remote:localhost:9090"

export interface DataSource {
  id: SourceId;
  type: 'demo' | 'mcap' | 'remote';
  label: string;
  color: string;
  timeRange: [number, number];
}

// WORKER MESSAGES
export type McapWorkerRequest =
  | { type: 'index'; fileBuffer: ArrayBuffer; sourceId: SourceId }
  | { type: 'loadRange'; sourceId: SourceId; timeRange: [number, number]; topics?: string[] }
  | { type: 'loadOverview'; sourceId: SourceId; bucketCount: number }

export type McapWorkerResponse =
  | { type: 'progress'; progress: number }
  | { type: 'indexComplete'; index: McapFileIndex }
  | { type: 'topic'; topicInfo: TopicInfo }
  | { type: 'scalarData'; key: string; timestamps: Float64Array; values: Float64Array }
  | { type: 'overviewData'; key: string; timestamps: Float64Array; minValues: Float64Array; maxValues: Float64Array }
  | { type: 'imageData'; topic: string; images: ImageMessage[] }
  | { type: 'overlayData'; topic: string; overlays: OverlayMessage[] }
  | { type: 'rangeComplete'; timeRange: [number, number] }
  | { type: 'error'; message: string }

// LARGE FILE INDEX
export interface McapFileIndex {
  sourceId: SourceId;
  fileSize: number;
  timeRange: [number, number];
  topics: TopicInfo[];
  chunks: McapChunkRef[];
}

export interface McapChunkRef {
  offset: number;
  length: number;
  timeRange: [number, number];
  topics: string[];
}

export interface LoadedRange {
  timeRange: [number, number];
  topics: Set<string>;
}

// DATA MODEL
export interface TopicInfo {
  sourceId: SourceId;
  name: string;
  schemaName: string;
  messageCount: number;
  timeRange: [number, number];
  fields: FieldInfo[];
}

export interface FieldInfo {
  path: string;
  displayName: string;
}

export interface ScalarSeries {
  timestamps: Float64Array;
  values: Float64Array;
}

export interface SignalRef {
  sourceId: SourceId;
  topic: string;
  field: string;
}

export interface TimestampedMessage {
  timestamp: number;
  data: Record<string, unknown>;
}

export interface ImageMessage {
  timestamp: number;
  width: number;
  height: number;
  encoding: string;
  data: Uint8Array;
}

// IMAGE OVERLAYS
export interface OverlayMessage {
  timestamp: number;
  items: OverlayItem[];
}

export type OverlayItem = BoundingBoxOverlay | KeypointOverlay | SegmentationMaskOverlay;

export interface BoundingBoxOverlay {
  type: 'bbox';
  x: number; y: number;
  width: number; height: number;
  label?: string;
  color?: string;
  confidence?: number;
}

export interface KeypointOverlay {
  type: 'keypoint';
  points: { x: number; y: number; label?: string }[];
  connections?: [number, number][];
  color?: string;
}

export interface SegmentationMaskOverlay {
  type: 'mask';
  width: number; height: number;
  data: Uint8Array;
  classColors: Record<number, string>;
  opacity?: number;
}

// 3D DEBUG OVERLAYS
export interface DebugOverlay3D {
  timestamp: number;
  items: Overlay3DItem[];
}

export type Overlay3DItem = VectorOverlay | PointOverlay | SphereOverlay;

export interface VectorOverlay {
  type: 'vector';
  origin: [number, number, number];
  direction: [number, number, number];
  color?: string;
  label?: string;
}

export interface PointOverlay {
  type: 'point';
  position: [number, number, number];
  color?: string;
  size?: number;
  label?: string;
}

export interface SphereOverlay {
  type: 'sphere';
  center: [number, number, number];
  radius: number;
  color?: string;
  opacity?: number;
}

// TRANSFORMS
export type BuiltinTransformType = 'derivative' | 'integral' | 'moving_average' | 'fft_magnitude' | 'abs' | 'scale' | 'offset' | 'difference' | 'ratio';

export interface TransformConfig {
  id: string;
  type: 'builtin' | 'expression';
  builtinType?: BuiltinTransformType;
  inputSignals: SignalRef[];
  params?: Record<string, number>;
  expression?: string;
  outputName: string;
}

// ANNOTATIONS
export interface Annotation {
  id: string;
  timestamp: number;
  endTimestamp?: number;
  label: string;
  color: string;
}

// ROBOT APPEARANCE
export interface RobotAppearance {
  sourceId: SourceId;
  urdfPath: string;
  color?: string;
  opacity: number;
  visible: boolean;
}

// ENVIRONMENT OBJECTS
export type EnvironmentObject =
  | { type: 'box'; position: [number,number,number]; size: [number,number,number]; color: string }
  | { type: 'cylinder'; position: [number,number,number]; radius: number; height: number; color: string }
  | { type: 'sphere'; position: [number,number,number]; radius: number; color: string }
  | { type: 'plane'; position: [number,number,number]; size: [number,number]; color: string };

// PANEL SYSTEM
export interface PanelDefinition {
  type: string;
  displayName: string;
  icon: string;
  component: React.ComponentType<PanelProps>;
  defaultConfig: Record<string, unknown>;
}

export interface PanelProps {
  panelId: string;
  config: Record<string, unknown>;
  onConfigChange: (patch: Record<string, unknown>) => void;
}

export type PlaybackSpeed = 0.1 | 0.25 | 0.5 | 1 | 2 | 5 | 10;

export interface SavedLayout {
  name: string;
  flexLayoutJson: object;
  panelConfigs: Record<string, Record<string, unknown>>;
}

// ROSBRIDGE
export interface RosbridgeConfig {
  url: string;
  subscribedTopics: string[];
}

export interface RosTopicInfo {
  name: string;
  type: string;
}
```

Copy these interfaces EXACTLY. Do not simplify, rename, or omit anything. Add `import React from 'react';` at the top for the `React.ComponentType` reference.

## Post-Task
```bash
cd app && npx tsc --noEmit
```
Update `docs/STATE.md`: mark TASK-002 as done.
```bash
git add -A && git commit -m "TASK-002: Core TypeScript interfaces"
```

## Acceptance Criteria
- [ ] `src/types.ts` contains all interfaces listed above
- [ ] `npx tsc --noEmit` passes (types file compiles)
- [ ] Every interface and type alias is exported
