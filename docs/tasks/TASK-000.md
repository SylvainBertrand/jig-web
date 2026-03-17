# TASK-000: Design System Generation

## Dependencies: None
## Wave: 0

## Files to Create
- `docs/DESIGN-SYSTEM.md`

## Skills: `.claude/skills/frontend-design.md`

## Instructions

Before writing ANY component code, generate a design system for this application. This is a robotics data visualization workbench — technical, data-dense, used by engineers. NOT a marketing site.

**Read the UX Priority Map in docs/SPEC.md first.** High-frequency features need 1-click access.

Generate `docs/DESIGN-SYSTEM.md` with:

1. **Design direction**: Technical/engineering tool. Information-dense. Compact controls. Dark-first (robotics tools are used in labs/offices where dark themes reduce eye strain).

2. **Layout principles**:
   - Panels are the primary workspace — maximize their area
   - Toolbar: thin (48px), icon-primary, text labels only at wide viewports
   - Timeline: thin (56px), always visible when data loaded
   - Topic Browser: collapsible sidebar (20% width default)
   - No modals for frequent actions (signal picker, settings = popovers/slide-ins)
   - Modals only for infrequent actions (connect, export)

3. **Interaction patterns**:
   - Drag-drop is the PRIMARY way to add signals (not menus)
   - Double-click is the SECONDARY way (creates new panel)
   - Right-click context menus for advanced actions
   - Keyboard shortcuts for playback (Space, arrows, Home/End, A)

4. **Color system**: Based on the CSS variables in constants.ts. Dark theme primary. Chart trace colors must be perceptually distinct (not just hue-shifted).

5. **Typography**: Inter for UI, JetBrains Mono for data values and timestamps. 13px base. Compact line-height (1.3) for data-dense panels.

6. **Spacing**: 4px base grid. 8px standard gap. 4px compact gap (inside panels). 16px section gap.

7. **Component patterns**: Specify for each component type:
   - Buttons: 28px height (compact), rounded-md, icon + text or icon-only
   - Inputs: 28px height, monospace for numeric values
   - Dropdowns: 28px, compact
   - Popovers: max-w-sm, slide from anchor point
   - Signal chips: inline-flex, colored dot + text + ×

**Generate 2 layout proposals** for the default panel arrangement:
- Option A: Classic layout (Topic Browser left sidebar, main area split horizontally)
- Option B: [your proposal — optimize for the UX priority map]

Pick the one that minimizes clicks for high-frequency features. Explain the tradeoff.

## Post-Task
Commit: `git add -A && git commit -m "TASK-000: Design system"`
