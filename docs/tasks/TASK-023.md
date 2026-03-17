# TASK-023: Chart — Pickers

## Dependencies: TASK-022, TASK-009
## Wave: 5
## Files: `app/src/panels/chart/SignalPicker.tsx`, `app/src/panels/chart/TransformPicker.tsx`, `app/src/panels/chart/ExpressionEditor.tsx`

## Instructions
**SignalPicker**: Popover triggered by "Add" button. Search input with fuse.js. List signals grouped by source (colored dot + source label). Click adds signal to chart. Already-added signals show checkmark.

**TransformPicker**: Popover triggered by "f(x)" button. Two sections: (1) Unary — select signal from chart, choose transform type (derivative, integral, etc.), set params if needed. (2) Binary — select two signals, choose difference or ratio. On confirm: call dataStore.addTransform with new TransformConfig, add transform's output to chart signals.

**ExpressionEditor**: Popover. Variable binding: dropdowns to assign a,b,c to signals. Expression text input. Output name text input. Preview button (optional). Apply button creates TransformConfig with type:'expression'.

## Acceptance Criteria
- [ ] SignalPicker fuzzy search works
- [ ] TransformPicker creates transforms that appear on chart
- [ ] ExpressionEditor evaluates mathjs expressions
