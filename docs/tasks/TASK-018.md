# TASK-018: Dialogs

## Dependencies: TASK-006, TASK-012
## Wave: 4
## Files: `app/src/components/ConnectionDialog.tsx`, `app/src/components/ExportDialog.tsx`

## Instructions
Both are modal dialogs: centered, backdrop blur, close on Escape, rounded-xl, shadow-2xl.

**ConnectionDialog**: URL input (default ws://localhost:9090), Connect/Cancel buttons. After connect: show topic list with checkboxes (fetched via rosbridgeClient.getTopicList), Subscribe Selected button, Disconnect button. Green/red dot for connection status. [SHELL] — the UI must be complete and functional, but will show "Connection failed — is rosbridge running?" when no robot is available.

**ExportDialog**: Source selector dropdown, time range inputs (start/end), "Use current view range" button, topic filter (All or specific), format radio (CSV/JSON), Export/Cancel. Export calls mcapExporter.exportData().

## Acceptance Criteria
- [ ] Both dialogs open/close correctly
- [ ] ConnectionDialog shows URL input and status
- [ ] ExportDialog triggers download on Export click
