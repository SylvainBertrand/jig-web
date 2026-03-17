# TASK-006: Session, Layout, Annotation Stores

## Dependencies: TASK-002, TASK-003
## Wave: 2
## Files: `app/src/stores/sessionStore.ts`, `app/src/stores/layoutStore.ts`, `app/src/stores/annotationStore.ts`
## Read: `src/types.ts`, `src/constants.ts`
## Skills: `zustand-patterns.md`

## Instructions

**sessionStore**: Manages active data sources. State: `activeSources: SourceId[]`, `remoteConnection: RosbridgeConfig | null`, `isRemoteConnected: boolean`. Actions: `openMcapFile(file)` — assigns color from SOURCE_PALETTE based on source count, creates SourceId like `"file:${file.name}"`, calls `dataStore.loadFromMcap(file, sourceId)`. `openDemo()` — adds "demo" source, calls `dataStore.loadDemoData()`. `closeSource(id)` — removes from activeSources, calls `dataStore.removeSource(id)`. `connectRemote(url)`, `disconnectRemote()`, `subscribeRemoteTopic(topic)`, `unsubscribeRemoteTopic(topic)`, `publishRemote(topic, type, msg)`.

**layoutStore**: Uses `persist` middleware. State: `theme: 'dark'|'light'`, `savedLayouts: SavedLayout[]`, `panelConfigs: Record<string, Record<string, unknown>>`. Actions: `toggleTheme`, `setTheme`, `saveLayout(name, json)`, `loadLayout(name)`, `deleteLayout(name)`, `setPanelConfig`, `getPanelConfig`. Persist `theme` and `savedLayouts` to localStorage key `"jig-layout"`.

**annotationStore**: Uses `persist` middleware. State: `annotations: Annotation[]`. Actions: `addAnnotation(omit id)` — generate UUID via `crypto.randomUUID()`. `removeAnnotation(id)`, `updateAnnotation(id, patch)`, `clearAnnotations()`, `getAnnotationsInRange(t0, t1)` — filter by timestamp. Persist to localStorage key `"jig-annotations"`.

## Acceptance Criteria
- [ ] All three stores compile
- [ ] layoutStore and annotationStore persist to localStorage
- [ ] sessionStore.openDemo calls dataStore.loadDemoData
- [ ] annotationStore generates unique IDs
