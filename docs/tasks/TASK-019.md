# TASK-019: Topic Browser Panel

## Dependencies: TASK-005, TASK-017
## Wave: 5
## Files: `app/src/panels/topicBrowser/TopicBrowserPanel.tsx`, `app/src/panels/topicBrowser/SignalTree.tsx`

## Instructions
**TopicBrowserPanel**: Search input at top (fuzzy, debounce 150ms via setTimeout). Toggle for "Group by source". Renders SignalTree. `data-testid="topic-search"`.

**SignalTree**: Hierarchical tree. When grouped by source: Source nodes (colored dot + label) → Topic nodes (name + schemaName secondary + messageCount badge) → Field leaf nodes (path). When ungrouped: Topic → Field.

**Drag-and-drop**: Field leafs: draggable, set `dataTransfer` with `application/x-jig-signal` containing `JSON.stringify(signalRef)`. Topic nodes: draggable, set `application/x-jig-topic-group` with `JSON.stringify({ sourceId, topic, fields: SignalRef[] })`.

**Double-click**: Field → create new chart panel with that signal (via PanelContainer.addPanel). Topic → create chart with ALL fields.

## Acceptance Criteria
- [ ] Tree renders all topics and fields from dataStore
- [ ] Fuzzy search filters correctly
- [ ] Drag sets correct dataTransfer types
- [ ] Double-click creates new chart panel
