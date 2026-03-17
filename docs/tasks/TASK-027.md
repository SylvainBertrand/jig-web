# TASK-027: Unit Tests

## Dependencies: All Wave 6 tasks
## Wave: 7
## Files: `app/vitest.config.ts`, `app/src/__tests__/timelineStore.test.ts`, `app/src/__tests__/annotationStore.test.ts`, `app/src/__tests__/dataStore.test.ts`, `app/src/__tests__/demoData.test.ts`, `app/src/__tests__/transforms.test.ts`, `app/src/__tests__/downsample.test.ts`
## Skills: `vitest-patterns.md`

## Instructions
Write comprehensive unit tests. Create fresh store instances per test (call `create()` from zustand directly, don't use the global singleton).

**timelineStore.test.ts**: setCurrentTime clamps, play/pause toggle, step forward/backward, seekToStart/End, speed changes.

**annotationStore.test.ts**: addAnnotation generates unique IDs, remove works, getAnnotationsInRange filters correctly.

**dataStore.test.ts**: addSource/removeSource, addScalarSeries/getScalarSeries round-trip, getImageAt binary search (test: before first, after last, exact match, between), searchSignals fuzzy match.

**demoData.test.ts**: joint positions have 3000 samples, velocities are derivatives of positions (check a few values), IMU z-accel mean ≈ 9.81, image data has 300 frames at 320×240.

**transforms.test.ts**: derivative of t² ≈ 2t, integral of constant = linear, abs works, scale/offset work, difference A-B correct, ratio handles div-by-zero.

**downsample.test.ts**: correct number of buckets, min/max values correct for known input.

Run: `npx vitest run`. ALL tests must pass.

## Acceptance Criteria
- [ ] 100+ tests across 6 files
- [ ] All tests pass
- [ ] Covers stores, transforms, demo data, utils
