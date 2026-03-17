# Jig Web — 1-Shot Build

## What You're Building
A browser-based robotics data visualization workbench. React + TypeScript + Tailwind. Client-side only.

## How to Build It
Run `/build` to execute the full pipeline. It reads `docs/TASKS-INDEX.md` and spawns a fresh subagent for each task.

## Execution Rules
- **EXECUTE, DO NOT PLAN.** Start working immediately. No plan mode.
- **Each task file in `docs/tasks/` is self-contained.** Subagents read ONLY their task file.
- **Update `docs/STATE.md`** after completing each task.
- **Git commit after each task.** Message format: `TASK-NNN: [title]`
- **Run `cd app && npx tsc --noEmit`** after every file write.

## Code Rules
- All types come from `src/types.ts`. Never redefine types locally.
- All colors come from `src/constants.ts` or CSS variables. No inline hex.
- No `any` types. Use proper TypeScript generics and narrowing.
- No empty function bodies. No `() => {}` stubs. No `// TODO`.
- `[SHELL]` features show visible "Coming soon" UI, never empty stubs.

## Error Handling — CRITICAL
- **Every `catch` block must produce a user-visible message** (toast, banner, or inline error). `console.error` alone is NEVER sufficient.
- **Every async operation must have error handling.** No unhandled promise rejections.
- **File loading failures must show the error to the user** with the filename and reason.
- **Never fail silently.** If something goes wrong, the user must know.

## Zustand — CRITICAL
- **Always use selectors**: `useStore(s => s.field)`. NEVER `useStore()` without a selector.
- **Never destructure from full state**: `const { x, y } = useStore()` causes re-render on ANY state change.
- **One selector per value**: `const x = useStore(s => s.x)` on separate lines, not combined.
- **New Map/Set instances** in `set()` — Zustand needs new references for re-renders.

## Anti-Patterns — NEVER DO THESE
- Never use `setTimeout`/`requestAnimationFrame` as a fix for rendering issues. Find the root cause.
- Never use `as any` or `// @ts-ignore` to suppress type errors.
- Never write a click handler that calls a store action without the component re-rendering to show the change.
- Never catch an error and only `console.log` it.
- Never leave a drag-drop handler that sets `dataTransfer` without a corresponding drop handler that reads it.

## Feature Tiers
- `[CORE]` — Must be fully implemented, tested, working.
- `[SHELL]` — UI present, architecture in place. Shows "Coming soon" for complex backend logic.
- `[FUTURE]` — Not in scope. Do not implement.

## Skills
Before implementing with a specific library, read the matching skill in `.claude/skills/`.
**If a skill's API doesn't match the library's TypeScript types** (.d.ts in node_modules after npm install), **trust the types over the skill**. Report the discrepancy.

## Compaction Survival
When compacting, always preserve:
- Current task ID and status from `docs/STATE.md`
- List of files modified in this session
- Any TypeScript error messages
- The wave number you're currently executing
