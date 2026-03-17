# TASK-009: Transforms Engine

## Dependencies: TASK-002
## Wave: 3
## Files: `app/src/core/transforms.ts`
## Read: `src/types.ts`

## Instructions
Export: `computeTransform(config: TransformConfig, getSignal: (ref: SignalRef) => ScalarSeries | null): ScalarSeries | null`

**Built-in transforms** (all fully implemented, no stubs):
- `derivative`: `(v[i+1]-v[i])/(t[i+1]-t[i])`, pad last value with previous derivative
- `integral`: cumulative trapezoidal rule: `sum += 0.5*(v[i]+v[i-1])*(t[i]-t[i-1])`
- `moving_average`: rolling mean with `params.windowSize` (default 10)
- `fft_magnitude`: Implement DFT (or use mathjs `fft` if available). Output timestamps = frequency bins (Hz), values = magnitude. N/2 output points.
- `abs`: element-wise `Math.abs`
- `scale`: multiply by `params.factor`
- `offset`: add `params.value`
- `difference`: `A[i] - B[i]` (two signals, aligned by interpolation if needed)
- `ratio`: `A[i] / B[i]` (handle div-by-zero → NaN)

**Expression transforms**: Use `mathjs.evaluate()`. Map letters a,b,c... to `inputSignals[0], [1], [2]...`. Evaluate element-wise across timestamp arrays.

**Two-signal alignment**: For difference/ratio/expressions with multiple signals from different sources, interpolate the shorter onto the longer's timestamps using linear interpolation.

## Acceptance Criteria
- [ ] All 9 built-in transforms produce correct results
- [ ] Expression evaluation works with mathjs
- [ ] No empty function bodies
