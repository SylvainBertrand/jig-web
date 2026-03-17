# Vitest Patterns

## Setup

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'jsdom',
    globals: true,           // describe, it, expect globally available
    include: ['src/__tests__/**/*.test.ts'],
    setupFiles: [],          // add setup files if needed
  },
});
```

```json
// package.json scripts
"test": "vitest run",
"test:watch": "vitest"
```

## Testing Zustand Stores

Create a fresh store for each test to avoid state leakage:

```typescript
import { create } from 'zustand';
import { describe, it, expect, beforeEach } from 'vitest';

// Re-create the store for each test
function createTestStore() {
  return create<TimelineState>((set, get) => ({
    currentTime: 0,
    timeRange: [0, 30] as [number, number],
    isPlaying: false,
    // ... copy full store implementation
  }));
}

describe('TimelineStore', () => {
  let store: ReturnType<typeof createTestStore>;

  beforeEach(() => {
    store = createTestStore();
  });

  it('setCurrentTime clamps to timeRange', () => {
    store.getState().setCurrentTime(50);
    expect(store.getState().currentTime).toBe(30); // clamped to max
  });

  it('stepForward advances by dt', () => {
    store.getState().setCurrentTime(5);
    store.getState().stepForward(0.01);
    expect(store.getState().currentTime).toBeCloseTo(5.01);
  });
});
```

## Testing Pure Functions

```typescript
import { computeTransform } from '../core/transforms';
import { describe, it, expect } from 'vitest';

describe('transforms', () => {
  const makeSeries = (values: number[], dt: number = 0.01) => ({
    timestamps: new Float64Array(values.map((_, i) => i * dt)),
    values: new Float64Array(values),
  });

  it('derivative computes finite differences', () => {
    // f(t) = t^2, f'(t) = 2t
    const n = 100;
    const dt = 0.01;
    const values = Array.from({ length: n }, (_, i) => (i * dt) ** 2);
    const series = makeSeries(values, dt);

    const result = computeTransform(
      { id: 'test', type: 'builtin', builtinType: 'derivative', inputSignals: [], outputName: 'test', params: {} },
      () => series
    );

    expect(result).not.toBeNull();
    // At t=0.5 (index 50), derivative should be ~1.0
    expect(result!.values[50]).toBeCloseTo(1.0, 1);
  });

  it('abs returns absolute values', () => {
    const series = makeSeries([-1, -2, 3, -4, 5]);
    const result = computeTransform(
      { id: 'test', type: 'builtin', builtinType: 'abs', inputSignals: [], outputName: 'test' },
      () => series
    );
    expect(Array.from(result!.values)).toEqual([1, 2, 3, 4, 5]);
  });
});
```

## Float64Array Comparison

```typescript
// Helper for comparing typed arrays with tolerance
function expectArrayClose(actual: Float64Array, expected: number[], tolerance = 1e-6) {
  expect(actual.length).toBe(expected.length);
  for (let i = 0; i < actual.length; i++) {
    expect(actual[i]).toBeCloseTo(expected[i], -Math.log10(tolerance));
  }
}

it('scale multiplies by factor', () => {
  const series = makeSeries([1, 2, 3]);
  const result = computeTransform(/* scale config with factor: 2 */, () => series);
  expectArrayClose(result!.values, [2, 4, 6]);
});
```

## Mocking Web Workers

```typescript
import { vi, describe, it, expect } from 'vitest';

// vitest runs in jsdom which doesn't have real Workers
// Mock the Worker constructor:
class MockWorker {
  onmessage: ((e: MessageEvent) => void) | null = null;
  postMessage(data: any) {
    // Simulate worker response
    setTimeout(() => {
      this.onmessage?.(new MessageEvent('message', {
        data: { type: 'done', timeRange: [0, 30] }
      }));
    }, 0);
  }
  terminate() {}
}

vi.stubGlobal('Worker', MockWorker);
```

## Testing Demo Data

```typescript
import { generateDemoData } from '../core/demoData';

describe('demoData', () => {
  it('generates correct number of joint position samples', () => {
    const data = generateDemoData();
    const series = data.scalarData.get('demo::/joint_states::position[0]');
    expect(series).toBeDefined();
    expect(series!.timestamps.length).toBe(3000); // 30s at 100Hz
    expect(series!.values.length).toBe(3000);
  });

  it('timestamps span 0 to ~30 seconds', () => {
    const data = generateDemoData();
    const series = data.scalarData.get('demo::/joint_states::position[0]');
    expect(series!.timestamps[0]).toBeCloseTo(0);
    expect(series!.timestamps[2999]).toBeCloseTo(29.99, 1);
  });

  it('IMU z-acceleration averages near 9.81', () => {
    const data = generateDemoData();
    const series = data.scalarData.get('demo::/imu/data::linear_acceleration.z');
    const mean = series!.values.reduce((a, b) => a + b, 0) / series!.values.length;
    expect(mean).toBeCloseTo(9.81, 0); // within 1 m/s²
  });
});
```

## Key Tips

- `toBeCloseTo(value, numDigits)` — numDigits is decimal places of precision
- Use `beforeEach` to reset shared state
- Avoid testing implementation details — test behavior
- Group related tests in `describe` blocks
- Keep each `it` focused on one assertion

## Verified — Standard well-documented APIs. Low hallucination risk.
