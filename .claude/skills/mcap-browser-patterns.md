# @mcap/browser + @mcap/core Patterns

## Installation
```bash
npm install @mcap/browser @mcap/core @mcap/support
```

**Important**: The MCAP SDK is split across packages:
- `@mcap/browser` — provides `BlobReadable` (wraps File/Blob for browser reading)
- `@mcap/core` — provides `McapIndexedReader`, `McapStreamReader`, types (Schema, Channel, Message, etc.)
- `@mcap/support` — provides `loadDecompressHandlers` for compressed chunks

## Reading an MCAP File in the Browser

```typescript
import { BlobReadable } from '@mcap/browser';
import { McapIndexedReader } from '@mcap/core';
import { loadDecompressHandlers } from '@mcap/support';

async function parseMcap(file: File) {
  const decompressHandlers = await loadDecompressHandlers();
  const readable = new BlobReadable(file);
  const reader = await McapIndexedReader.Initialize({ readable, decompressHandlers });

  // Access metadata
  console.log('Channels:', reader.channelsById);   // Map<number, Channel>
  console.log('Schemas:', reader.schemasById);      // Map<number, Schema>
  console.log('Stats:', reader.statistics);          // { messageCount, channelCount, ... }
  console.log('ChunkIndexes:', reader.chunkIndexes); // ChunkIndex[] for large file support

  // Iterate all messages
  for await (const message of reader.readMessages()) {
    // message: { channelId, logTime (bigint ns), publishTime, data (Uint8Array) }
    const channel = reader.channelsById.get(message.channelId);
    const schema = channel ? reader.schemasById.get(channel.schemaId) : undefined;
    console.log(channel?.topic, Number(message.logTime) / 1e9);
  }
}
```

## Key Types (from @mcap/core)

```typescript
interface Channel {
  id: number;
  topic: string;
  schemaId: number;
  messageEncoding: string;  // "json", "cdr", "protobuf", etc.
  metadata: Map<string, string>;
}

interface Schema {
  id: number;
  name: string;             // e.g. "sensor_msgs/msg/JointState"
  encoding: string;         // "jsonschema", "ros2msg", "protobuf", etc.
  data: Uint8Array;
}

interface Message {
  channelId: number;
  logTime: bigint;          // nanoseconds since epoch
  publishTime: bigint;
  data: Uint8Array;         // raw message bytes
}

interface ChunkIndex {
  startTime: bigint;
  endTime: bigint;
  chunkStartOffset: bigint;
  chunkLength: bigint;
  messageIndexOffsets: Map<number, bigint>;
}
```

- `logTime` is `bigint` in nanoseconds. Convert to seconds: `Number(message.logTime) / 1e9`
- `data` is raw bytes. Decoding depends on `channel.messageEncoding`.

## Message Decoding

MCAP messages are raw bytes. You need the schema to decode them. For ROS 2 CDR encoding:

```typescript
// Simple approach: if messages are JSON-encoded (some MCAP files use JSON)
if (schema.encoding === 'jsonschema') {
  const text = new TextDecoder().decode(message.data);
  const parsed = JSON.parse(text);
}

// For ROS 2 CDR: you'd need a CDR decoder. For the 1-shot, focus on:
// 1. Demo data (generated in-memory, no MCAP parsing needed)
// 2. JSON-encoded MCAP files (simplest to parse)
// The worker should attempt JSON decode first, fall back to raw field extraction
```

## Extracting Scalar Fields from Decoded Messages

```typescript
function extractScalars(obj: any, prefix: string = ''): { path: string; value: number }[] {
  const results: { path: string; value: number }[] = [];
  
  for (const [key, val] of Object.entries(obj)) {
    const path = prefix ? `${prefix}.${key}` : key;
    
    if (typeof val === 'number') {
      results.push({ path, value: val });
    } else if (Array.isArray(val)) {
      val.forEach((item, i) => {
        if (typeof item === 'number') {
          results.push({ path: `${key}[${i}]`, value: item });
        }
      });
    } else if (typeof val === 'object' && val !== null) {
      results.push(...extractScalars(val, path));
    }
  }
  
  return results;
}
```

## ChunkIndex for Large File Support

```typescript
// McapIndexedReader exposes chunk indexes directly
const reader = await McapIndexedReader.Initialize({ readable, decompressHandlers });

// Access chunk indexes
const chunkIndexes = reader.chunkIndexes; // ChunkIndex[]

// Each chunk has:
// - startTime/endTime: bigint nanoseconds
// - chunkStartOffset/chunkLength: bigint byte positions
// - messageIndexOffsets: Map<channelId, bigint>

// For range loading: filter chunks by time overlap
function getChunksInRange(chunks: typeof chunkIndexes, startNs: bigint, endNs: bigint) {
  return chunks.filter(c => c.endTime >= startNs && c.startTime <= endNs);
}

// Read messages from specific time range only:
for await (const message of reader.readMessages({
  startTime: BigInt(Math.floor(startSeconds * 1e9)),
  endTime: BigInt(Math.floor(endSeconds * 1e9)),
})) {
  // Only messages in the requested range
}
```

## Web Worker Usage

```typescript
// mcapWorker.ts — runs in a Web Worker
// IMPORTANT: In a Web Worker, you can't use BlobReadable (no File API).
// Instead, receive the ArrayBuffer and use McapIndexedReader with a custom readable.

import { McapIndexedReader } from '@mcap/core';

// Simple ArrayBuffer readable for workers
class ArrayBufferReadable {
  private buffer: Uint8Array;
  constructor(arrayBuffer: ArrayBuffer) {
    this.buffer = new Uint8Array(arrayBuffer);
  }
  async size() { return BigInt(this.buffer.byteLength); }
  async read(offset: bigint, length: bigint) {
    return new Uint8Array(this.buffer.buffer, Number(offset), Number(length));
  }
}

self.onmessage = async (e: MessageEvent) => {
  const { type, fileBuffer, sourceId } = e.data;

  if (type === 'index') {
    const readable = new ArrayBufferReadable(fileBuffer);
    const reader = await McapIndexedReader.Initialize({ readable });

    // Build topic info from channels + schemas
    const topics = [];
    for (const [id, channel] of reader.channelsById) {
      const schema = reader.schemasById.get(channel.schemaId);
      topics.push({
        sourceId,
        name: channel.topic,
        schemaName: schema?.name ?? 'unknown',
        messageCount: 0, // will be updated during message iteration
        // ...
      });
    }

    // Iterate messages and extract data
    for await (const message of reader.readMessages()) {
      const channel = reader.channelsById.get(message.channelId);
      // Decode based on channel.messageEncoding
      // For JSON: new TextDecoder().decode(message.data) → JSON.parse
      // Extract scalars, images, overlays
    }

    self.postMessage({ type: 'indexComplete', index: { topics, chunks: reader.chunkIndexes } });
  }
};
```

## Transferable Objects

```typescript
// Transfer Float64Array buffers for zero-copy
const timestamps = new Float64Array([...]);
const values = new Float64Array([...]);

self.postMessage(
  { type: 'scalarData', key, timestamps, values },
  [timestamps.buffer, values.buffer]  // transfer list
);
// After this, timestamps and values are no longer accessible in the worker
```

## Verified against: https://www.npmjs.com/package/@mcap/browser + https://www.npmjs.com/package/@mcap/core on 2026-03-16
