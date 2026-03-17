# Web Worker Patterns (Vite)

## Creating a Worker in Vite

Vite handles worker bundling natively. No config needed.

```typescript
// Main thread — create worker with module type
const worker = new Worker(
  new URL('./mcapWorker.ts', import.meta.url),
  { type: 'module' }
);

// Worker file can use ES imports normally
// Vite bundles it as a separate entry point
```

## Typed Message Protocol

```typescript
// types.ts — shared between main thread and worker
export type WorkerRequest = 
  | { type: 'parse'; fileBuffer: ArrayBuffer }
  | { type: 'loadRange'; timeRange: [number, number] };

export type WorkerResponse =
  | { type: 'progress'; progress: number }
  | { type: 'done'; result: SomeType }
  | { type: 'error'; message: string };
```

```typescript
// mcapWorker.ts
self.onmessage = async (e: MessageEvent<WorkerRequest>) => {
  const msg = e.data;
  switch (msg.type) {
    case 'parse':
      try {
        // ... parsing logic
        const resp: WorkerResponse = { type: 'done', result: data };
        self.postMessage(resp);
      } catch (err) {
        const resp: WorkerResponse = { type: 'error', message: String(err) };
        self.postMessage(resp);
      }
      break;
  }
};
```

```typescript
// Main thread — typed handler
worker.onmessage = (e: MessageEvent<WorkerResponse>) => {
  switch (e.data.type) {
    case 'progress': /* update UI */ break;
    case 'done': /* process result */ break;
    case 'error': /* handle error */ break;
  }
};
```

## Transferable Objects (Zero-Copy)

```typescript
// Transfer ArrayBuffer ownership — no copy, O(1)
const timestamps = new Float64Array(3000);
const values = new Float64Array(3000);
// ... fill arrays

self.postMessage(
  { type: 'scalarData', timestamps, values },
  [timestamps.buffer, values.buffer]  // transfer list
);
// WARNING: timestamps and values are now detached (length = 0) in the worker
```

## File.slice for Large Files

```typescript
// Instead of reading entire file into ArrayBuffer:
// Main thread keeps the File reference, worker requests byte ranges

// Main thread
worker.onmessage = (e) => {
  if (e.data.type === 'readBytes') {
    const { offset, length, requestId } = e.data;
    const slice = file.slice(offset, offset + length);
    slice.arrayBuffer().then(buffer => {
      worker.postMessage({ type: 'bytesResult', requestId, buffer }, [buffer]);
    });
  }
};
```

## Error Handling

```typescript
// Main thread — catch worker errors
worker.onerror = (e) => {
  console.error('Worker error:', e.message);
  dataStore.getState().setError(`Worker crashed: ${e.message}`);
};

// Worker — wrap all async work in try/catch
self.onmessage = async (e) => {
  try {
    // ... work
  } catch (err) {
    self.postMessage({ type: 'error', message: err instanceof Error ? err.message : String(err) });
  }
};
```

## Termination

```typescript
// Clean up when done
worker.terminate(); // immediate, no cleanup callback in worker
```

## Verified — Standard well-documented APIs. Low hallucination risk.
