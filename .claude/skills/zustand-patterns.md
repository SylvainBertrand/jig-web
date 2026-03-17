# Zustand Patterns

## Basic Store with TypeScript

```typescript
import { create } from 'zustand';

interface CounterState {
  count: number;
  increment: () => void;
  decrement: () => void;
}

export const useCounterStore = create<CounterState>((set, get) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));
```

## Persist Middleware (localStorage)

```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

export const useLayoutStore = create<LayoutState>()(
  persist(
    (set, get) => ({
      theme: 'dark' as const,
      savedLayouts: [],
      toggleTheme: () => set((s) => ({ theme: s.theme === 'dark' ? 'light' : 'dark' })),
      // ... other actions
    }),
    {
      name: 'jig-layout',  // localStorage key
      partialize: (state) => ({
        theme: state.theme,
        savedLayouts: state.savedLayouts,
      }), // only persist these fields
    }
  )
);
```

## Selectors (Prevent Unnecessary Re-renders)

```typescript
// BAD — subscribes to ALL state changes
const { currentTime, isPlaying } = useTimelineStore();

// GOOD — subscribes only to currentTime changes
const currentTime = useTimelineStore((s) => s.currentTime);
const isPlaying = useTimelineStore((s) => s.isPlaying);

// GOOD — multiple values with shallow equality
import { useShallow } from 'zustand/react/shallow';
const { currentTime, isPlaying } = useTimelineStore(
  useShallow((s) => ({ currentTime: s.currentTime, isPlaying: s.isPlaying }))
);
```

## Accessing Store Outside React (in plain functions)

```typescript
// Get current state snapshot
const currentTime = useTimelineStore.getState().currentTime;

// Call an action
useTimelineStore.getState().setCurrentTime(5.0);

// Subscribe to changes (returns unsubscribe function)
const unsub = useTimelineStore.subscribe(
  (state) => state.currentTime,
  (currentTime) => { /* react to change */ }
);
```

## Store with Internal Maps (DataStore Pattern)

```typescript
interface DataState {
  topics: TopicInfo[];
  // Private internal storage — not directly accessible
  _scalarData: Map<string, ScalarSeries>;
  _imageData: Map<string, ImageMessage[]>;

  // Public getters
  getScalarSeries: (sourceId: string, topic: string, field: string) => ScalarSeries | null;

  // Actions
  addScalarSeries: (key: string, timestamps: Float64Array, values: Float64Array) => void;
}

export const useDataStore = create<DataState>((set, get) => ({
  topics: [],
  _scalarData: new Map(),
  _imageData: new Map(),

  getScalarSeries: (sourceId, topic, field) => {
    const key = `${sourceId}::${topic}::${field}`;
    return get()._scalarData.get(key) ?? null;
  },

  addScalarSeries: (key, timestamps, values) => {
    set((state) => {
      const newMap = new Map(state._scalarData);
      newMap.set(key, { timestamps, values });
      return { _scalarData: newMap };
    });
  },
}));
```

## Important: Maps and Zustand

Zustand uses reference equality for change detection. When mutating a `Map`, you must create a new `Map` instance for React to re-render:

```typescript
// BAD — mutates in place, no re-render
state._scalarData.set(key, value);

// GOOD — new Map triggers re-render
const newMap = new Map(state._scalarData);
newMap.set(key, value);
return { _scalarData: newMap };
```

## Cross-Store Access

```typescript
// From dataStore, access timelineStore:
import { useTimelineStore } from './timelineStore';

// Inside an action:
loadDemoData: () => {
  // ... generate data
  set({ topics, _scalarData: dataMap });
  // Then update timeline store
  useTimelineStore.getState().setTimeRange([0, 30]);
  useTimelineStore.getState().setCurrentTime(0);
},
```

## Verified against: Zustand v4 official docs. Persist middleware API is well-known and stable.

## CRITICAL: Selector Rules (Reactivity)

**ALWAYS use selectors when reading store state in components:**

```typescript
// CORRECT — re-renders ONLY when isPlaying changes
const isPlaying = useTimelineStore(s => s.isPlaying);
const currentTime = useTimelineStore(s => s.currentTime);

// WRONG — re-renders on EVERY store update (any field)
const { isPlaying, currentTime } = useTimelineStore();

// WRONG — same problem, just different syntax
const store = useTimelineStore();
const isPlaying = store.isPlaying;
```

**One selector per value, on separate lines.** This ensures each value triggers re-renders independently.

**For derived values, use a selector that computes:**
```typescript
// CORRECT — memoized selector
const timeDisplay = useTimelineStore(s => formatTime(s.currentTime));

// WRONG — reads entire store, reformats every render
const { currentTime } = useTimelineStore();
const timeDisplay = formatTime(currentTime);
```

**When mutating Maps or Sets in store, always create new instances:**
```typescript
set(state => {
  const newMap = new Map(state._scalarData);
  newMap.set(key, value);
  return { _scalarData: newMap };
});
```
