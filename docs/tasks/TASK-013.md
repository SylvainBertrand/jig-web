# TASK-013: Custom Hooks

## Dependencies: TASK-004, TASK-005
## Wave: 3
## Files: `app/src/hooks/useRenderLoop.ts`, `app/src/hooks/useSignalData.ts`, `app/src/hooks/useCurrentMessage.ts`

## Instructions
**useRenderLoop(callback: (dt: number) => void)**: Runs callback on every requestAnimationFrame. Returns start/stop functions. Tracks deltaTime between frames in ms.

**useSignalData(sourceId, topic, field)**: Returns `ScalarSeries | null` by calling `dataStore.getScalarSeries()`. Subscribe to dataStore changes efficiently (use a selector that checks `_scalarData` map).

**useCurrentMessage(sourceId, topic, getter: 'image'|'overlay'|'debugOverlay')**: Returns the message at current time. Reads `timelineStore.currentTime`, calls the appropriate dataStore getter.

## Acceptance Criteria
- [ ] useRenderLoop provides accurate deltaTime
- [ ] useSignalData returns null when no data
- [ ] useCurrentMessage performs binary search correctly
