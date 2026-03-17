# TASK-004: Timeline Store

## Dependencies: TASK-002
## Wave: 2
## Files: `app/src/stores/timelineStore.ts`
## Read: `src/types.ts`
## Skills: `zustand-patterns.md`

## Instructions
Implement the Zustand store for global playback state. Interface:
- State: `currentTime` (number), `timeRange` ([number,number], default [0,0]), `viewRange` ([number,number]), `isPlaying` (boolean), `playbackSpeed` (number, default 1)
- Actions: `setCurrentTime(t)` (clamp to timeRange), `setTimeRange`, `setViewRange`, `play`, `pause`, `togglePlayback`, `setPlaybackSpeed`, `stepForward(dt=0.01)`, `stepBackward(dt=0.01)`, `seekToStart`, `seekToEnd`
- `setCurrentTime` must clamp: `Math.max(timeRange[0], Math.min(timeRange[1], t))`

## Acceptance Criteria
- [ ] All actions implemented (no empty bodies)
- [ ] setCurrentTime clamps to timeRange
- [ ] Compiles with zero errors
