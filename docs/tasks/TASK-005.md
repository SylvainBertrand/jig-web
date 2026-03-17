# TASK-005: Data Store

## Dependencies: TASK-002, TASK-003
## Wave: 2
## Files: `app/src/stores/dataStore.ts`
## Read: `src/types.ts`, `src/constants.ts`
## Skills: `zustand-patterns.md`

## Instructions
The most complex store. Internal state uses Maps keyed by `"sourceId::topic::field"` for scalar data.

**Internal maps** (not exposed directly, accessed via getters):
- `_scalarData: Map<string, ScalarSeries>` — keyed by `"sourceId::topic::field"`
- `_imageData: Map<string, ImageMessage[]>` — keyed by `"sourceId::topic"`
- `_overlayData: Map<string, OverlayMessage[]>` — keyed by `"sourceId::topic"`
- `_debugOverlayData: Map<string, DebugOverlay3D[]>` — keyed by `"sourceId::topic"`
- `_transformCache: Map<string, ScalarSeries>` — keyed by transform ID
- `_fuseIndex: Fuse<SignalRef>` — rebuilt when topics change

**Getters** (all fully implemented):
- `getScalarSeries(sourceId, topic, field)` → lookup in `_scalarData`
- `getImageAt(sourceId, topic, t)` → binary search on sorted image array for timestamp <= t
- `getOverlayAt(sourceId, topic, t)` → binary search
- `getDebugOverlayAt(sourceId, topic, t)` → binary search
- `getTransformedSeries(transformId)` → lookup in `_transformCache`
- `getAllSignalRefs()` → flatten all topics' fields into SignalRef array
- `searchSignals(query)` → fuse.js fuzzy search
- `getTopicsForSource(sourceId)` → filter topics array
- `getImageTopics()` → filter topics where schemaName contains "Image"
- `getOverlayTopics()` → filter topics where schemaName contains "Detection" etc.
- `getOverviewSeries(sourceId, topic, field)` → lookup in overviewSeries map
- `isRangeLoaded(sourceId, timeRange)` → check loadedRanges

**Binary search helper**: Implement a `binarySearchFloor(timestamps: Float64Array, t: number): number` that returns the index of the largest timestamp <= t. Use this for all time-based lookups.

**State**: `sources: DataSource[]`, `topics: TopicInfo[]`, `transforms: TransformConfig[]`, `isLoading`, `loadProgress`, `error`, `fileIndices: Map`, `loadedRanges: Map`, `overviewSeries: Map`

**All actions must be fully implemented.** Import `Fuse` from `fuse.js`. When creating a new Map in `set()`, always create a new Map instance (Zustand needs new references for re-renders).

[SHELL] features: `requestRange` and `evictOutsideRange` — implement with a console.log("Range loading: coming soon") message and a no-op. Do NOT leave as empty bodies.

## Acceptance Criteria
- [ ] All getters return correct data types
- [ ] Binary search finds correct index for edge cases (before first, after last, exact match)
- [ ] fuse.js search works on signal display names
- [ ] Zero empty function bodies
- [ ] Compiles with zero errors
