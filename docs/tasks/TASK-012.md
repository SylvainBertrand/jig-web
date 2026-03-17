# TASK-012: Utility Modules

## Dependencies: TASK-002
## Wave: 3
## Files: `app/src/core/downsample.ts`, `app/src/core/chunkCache.ts`, `app/src/core/mcapExporter.ts`

## Instructions
**downsample.ts**: Export `downsampleMinMax(timestamps: Float64Array, values: Float64Array, bucketCount: number): { timestamps: Float64Array, minValues: Float64Array, maxValues: Float64Array }`. Divide time range into bucketCount equal bins. For each bin, compute min and max of values. Output timestamps are bin centers.

**chunkCache.ts**: Export `getChunkFromCache(sourceId, offset): Promise<ArrayBuffer|null>`, `setChunkInCache(sourceId, offset, data): Promise<void>`, `clearCacheForSource(sourceId): Promise<void>`. Use `idb-keyval` (get, set, del, keys). Key format: `"chunk:${sourceId}:${offset}"`.

**mcapExporter.ts**: Export `exportData(sourceId, timeRange, topics, format)`. Read scalar data from dataStore, filter by time range and topics. For CSV: generate one blob with header row + data rows. For JSON: structured object. Trigger download via `URL.createObjectURL` + temporary `<a>` element with `download` attribute.

## Acceptance Criteria
- [ ] downsample produces correct min/max buckets
- [ ] chunkCache uses idb-keyval correctly
- [ ] mcapExporter triggers browser download
