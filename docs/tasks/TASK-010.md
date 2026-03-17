# TASK-010: MCAP Worker + Loader [CORE small files, SHELL large files]

## Dependencies: TASK-002, TASK-005
## Wave: 3
## Files: `app/src/core/mcapWorker.ts`, `app/src/core/mcapLoader.ts`, `app/src/core/mcapIndex.ts`
## Read: `src/types.ts`, `src/stores/dataStore.ts`
## Skills: `mcap-browser-patterns.md`, `web-worker-patterns.md`
## npm packages: `@mcap/core`, `@mcap/browser`, `@mcap/support`

## Instructions

**mcapLoader.ts** (main thread): Creates worker via `new Worker(new URL('./mcapWorker.ts', import.meta.url), { type: 'module' })`. Sends file ArrayBuffer. Handles all McapWorkerResponse types. Updates dataStore progressively. On `done`/`rangeComplete`, updates timelineStore.

**mcapWorker.ts** (worker thread): Uses `@mcap/core` (`McapIndexedReader`) with a custom `ArrayBufferReadable` wrapper (since `BlobReadable` from `@mcap/browser` requires File API not available in workers). Handles 3 request types:
- `index`: Create `ArrayBufferReadable` from received buffer, initialize `McapIndexedReader`, build McapFileIndex from `reader.channelsById`, `reader.schemasById`, `reader.chunkIndexes`. Post `indexComplete`.
- `loadOverview`: [SHELL] Post a console message "Overview computation coming soon" and `rangeComplete`.
- `loadRange`: [SHELL] Post a console message "Range loading coming soon" and `rangeComplete`.

**For small files (<200MB)**: After indexing, iterate all messages via `reader.readMessages()`. For each message, get channel via `reader.channelsById.get(message.channelId)`, get schema. Extract scalars/images/overlays. Post them back.

**mcapIndex.ts**: Export `buildMcapIndex(reader: McapIndexedReader, sourceId: SourceId): McapFileIndex`. Reads `reader.channelsById` → TopicInfo, reads `reader.chunkIndexes` → McapChunkRef. Convert bigint times to seconds: `Number(bigintNs) / 1e9`.

**ArrayBufferReadable**: Implement in the worker file. Simple class with `size(): Promise<bigint>` and `read(offset: bigint, length: bigint): Promise<Uint8Array>` that reads from the ArrayBuffer.

**Message decoding**: Check `channel.messageEncoding`. For "json": `new TextDecoder().decode(message.data)` → `JSON.parse`. For unknown encodings: skip (log a warning). Use `extractScalars(obj)` to recursively pull out numeric fields.

**Transfer Float64Array buffers** via Transferable: `self.postMessage(msg, [timestamps.buffer, values.buffer])`.

## Acceptance Criteria
- [ ] Worker creates successfully (Vite bundles it)
- [ ] Small file full-load path works end-to-end
- [ ] Topic type detection categorizes correctly
- [ ] Progressive updates reach dataStore
- [ ] [SHELL] features log informative messages, not empty stubs

## Error Handling — CRITICAL

**Loading failures must be visible to the user:**
- If the file is not valid MCAP: post `{ type: 'error', message: 'Not a valid MCAP file: [filename]' }`
- If parsing throws: post `{ type: 'error', message: 'Failed to parse [filename]: [error.message]' }`
- The main thread (mcapLoader.ts) must call `showToast({ message, variant: 'error' })` when it receives an error response
- Never swallow worker errors. Always forward to the UI.

**Unsupported encoding notification:**
- After full load, count topics where messageEncoding is not 'json'
- If count > 0: show toast "N topics use unsupported encoding (e.g., CDR). Scalar data not available for these topics."
- In the Topic Browser, these topics should show a warning indicator
