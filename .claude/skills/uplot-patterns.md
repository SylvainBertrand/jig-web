# uPlot Patterns

## Installation
```bash
npm install uplot
```

## Core Concept

uPlot is imperative — you create an instance, attach it to a DOM element, and update it via methods. Do NOT use declarative wrappers for fine control.

## Creating an Instance in React

```typescript
import uPlot from 'uplot';
import 'uplot/dist/uPlot.min.css';

function UPlotChart({ signals, currentTime, onTimeClick }: Props) {
  const containerRef = useRef<HTMLDivElement>(null);
  const plotRef = useRef<uPlot | null>(null);

  // Create/destroy on mount/unmount
  useEffect(() => {
    if (!containerRef.current) return;

    const opts: uPlot.Options = {
      width: containerRef.current.clientWidth,
      height: containerRef.current.clientHeight,
      cursor: {
        lock: false,
        sync: { key: 'jig-sync' },
      },
      scales: {
        x: { time: false },  // we use seconds, not unix timestamps
      },
      axes: [
        { stroke: '#8b8fa3', grid: { stroke: '#2a2d3a' } },  // x-axis
        { stroke: '#8b8fa3', grid: { stroke: '#2a2d3a' } },  // y-axis
      ],
      series: [
        {},  // x-axis series (always first, empty config)
        // ... signal series added dynamically
      ],
    };

    const data: uPlot.AlignedData = [new Float64Array(0)]; // empty initially
    plotRef.current = new uPlot(opts, data, containerRef.current);

    return () => {
      plotRef.current?.destroy();
      plotRef.current = null;
    };
  }, []);

  return <div ref={containerRef} style={{ width: '100%', height: '100%' }} />;
}
```

## Updating Data (WITHOUT recreating the instance)

```typescript
// Build aligned data array: [timestamps, values1, values2, ...]
const alignedData: uPlot.AlignedData = [
  timestamps,  // Float64Array or number[]
  ...signals.map(s => s.values),  // each is Float64Array or number[]
];

plotRef.current.setData(alignedData);
```

## Adding/Removing Series

```typescript
// Add a series
plotRef.current.addSeries({
  stroke: '#3b82f6',
  width: 1.5,
  label: 'position[0]',
}, seriesIndex);

// Remove a series (by index, 1-based — index 0 is always x-axis)
plotRef.current.delSeries(seriesIndex);
```

## Resize Handling (Critical for FlexLayout)

```typescript
useEffect(() => {
  if (!containerRef.current || !plotRef.current) return;

  const observer = new ResizeObserver((entries) => {
    for (const entry of entries) {
      const { width, height } = entry.contentRect;
      if (width > 0 && height > 0) {
        plotRef.current?.setSize({ width, height });
      }
    }
  });

  observer.observe(containerRef.current);
  return () => observer.disconnect();
}, []);
```

## Drawing a Cursor Line at currentTime

```typescript
// Use the setCursor API to position cursor
const xIdx = timestamps.findIndex(t => t >= currentTime);
if (plotRef.current && xIdx >= 0) {
  plotRef.current.setCursor({ left: plotRef.current.valToPos(currentTime, 'x'), top: 0 });
}

// Or draw a custom vertical line via hooks:
const opts: uPlot.Options = {
  hooks: {
    draw: [
      (u: uPlot) => {
        const ctx = u.ctx;
        const x = u.valToPos(currentTime, 'x', true);
        if (x < u.bbox.left || x > u.bbox.left + u.bbox.width) return;
        ctx.save();
        ctx.strokeStyle = '#ef4444';
        ctx.lineWidth = 1;
        ctx.setLineDash([4, 4]);
        ctx.beginPath();
        ctx.moveTo(x, u.bbox.top);
        ctx.lineTo(x, u.bbox.top + u.bbox.height);
        ctx.stroke();
        ctx.restore();
      },
    ],
  },
};
```

## Click to Seek

```typescript
const opts: uPlot.Options = {
  hooks: {
    setCursor: [
      (u: uPlot) => {
        // This fires on mouse move — use click instead
      },
    ],
  },
};

// Better: add click handler on container
containerRef.current.addEventListener('click', (e) => {
  if (!plotRef.current) return;
  const rect = containerRef.current!.getBoundingClientRect();
  const left = e.clientX - rect.left;
  const time = plotRef.current.posToVal(left, 'x');
  onTimeClick(time);
});
```

## Zoom (Select Region)

```typescript
const opts: uPlot.Options = {
  select: { show: true },  // enable region select
  hooks: {
    setSelect: [
      (u: uPlot) => {
        const min = u.posToVal(u.select.left, 'x');
        const max = u.posToVal(u.select.left + u.select.width, 'x');
        // Update viewRange in timeline store
        setViewRange([min, max]);
        u.setSelect({ left: 0, top: 0, width: 0, height: 0 }, false); // clear selection
      },
    ],
  },
};
```

## Series Colors

```typescript
const CHART_COLORS = ['#3b82f6', '#22c55e', '#f59e0b', '#ef4444', '#a855f7', '#ec4899', '#14b8a6', '#f97316'];

signals.forEach((signal, i) => {
  plotRef.current.addSeries({
    stroke: CHART_COLORS[i % CHART_COLORS.length],
    width: 1.5,
    label: signal.displayName,
  }, i + 1); // +1 because index 0 is x-axis
});
```

## Performance Tips

- Never recreate the uPlot instance on data change — use `setData()`.
- Use `Float64Array` for data (not regular arrays) — uPlot handles typed arrays efficiently.
- For 100k+ points, uPlot handles it natively — no downsampling needed for rendering.
- Debounce resize observer callbacks (uPlot's `setSize` is cheap but not free).

## UNVERIFIED — Written from training knowledge. CC should verify uPlot constructor options and API against uplot .d.ts types after npm install.
