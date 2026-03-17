---
description: "Execute the full Jig build pipeline. Spawns fresh subagents for each atomic task across 10 waves."
---

**EXECUTE IMMEDIATELY. DO NOT PLAN. DO NOT ASK FOR CONFIRMATION.**

# /build — Wave-Based 1-Shot Build

## Startup

1. Read `docs/STATE.md` to check progress. Resume from the next incomplete wave.
2. Read `docs/TASKS-INDEX.md` for the full task graph.
3. Read `docs/SPEC.md` for feature tiers and UX priority map.
4. Ensure `docs/reports/` directory exists.

## Execution Loop

For each wave (0 through 9):

### Step 1 — Identify tasks
Read `docs/STATE.md`. Find all unchecked tasks in the current wave.

### Step 2 — Execute tasks
For EACH task, spawn a subagent:
- Subagent reads ONLY `docs/tasks/TASK-NNN.md` + relevant `.claude/skills/`
- Subagent creates/edits ONLY the files listed in its task
- When done, runs `cd app && npx tsc --noEmit`

### Step 3 — Verify wave
After ALL tasks in the wave complete:
```bash
cd app && npx tsc --noEmit 2>&1 | tail -20
```
Waves must compile clean before proceeding.

### Step 4 — Update STATE.md + Git commit
Mark completed tasks as `[x]`. Commit:
```bash
git add -A && git commit -m "Wave N complete: TASK-NNN, TASK-NNN, ..."
```

### Step 5 — Next wave

## Wave-Specific Gates

**Wave 0 (Design)**: No compilation check needed. Just verify `docs/DESIGN-SYSTEM.md` exists.

**Wave 1**: Run `cd app && npm install` after completion.

**Wave 6 (Integration)**: App must compile AND demo mode must work.

**Wave 6.5 (Smoke Test)**: BLOCKING gate. Dev server must start. Demo must load. Design audit must pass. If this fails, fix before continuing.

**Wave 7 (Testing)**:
```bash
cd app && npx vitest run
cd app && npx playwright test
```

**Wave 8 (QA)**: Uses Playwright MCP for black-box testing. Spawn the QA subagent with restricted tools:
```
--allowedTools "mcp__playwright__browser_navigate, mcp__playwright__browser_click, mcp__playwright__browser_type, mcp__playwright__browser_take_screenshot, mcp__playwright__browser_snapshot, mcp__playwright__browser_resize, Write, Read"
```
The Read tool is allowed ONLY for reading `docs/behaviors/`, `docs/scenarios/`, and `docs/SPEC.md` — NOT source code in `app/src/`. The QA agent reads behavioral specs, then verifies them in the browser. Start the dev server first: `cd app && npm run dev &`, wait for ready, then hand off to the QA agent.

**Wave 9 (Fix Loop)**: Fix issues from Wave 8 reports. Re-run QA. Max 3 iterations.

## Completion

After Wave 9 (or when all QA passes):
1. Write `docs/reports/final-summary.md`
2. `git add -A && git commit -m "Final: build complete"`
3. Output: build status, test results, behavioral spec pass rate, scenario pass rate, top remaining issues

## CRITICAL RULES

- **Every subagent gets a FRESH context.** No context accumulation.
- **Task files are self-contained.** Subagents read their task + skills. Nothing else.
- **No empty function bodies.** Stubs audit hook will flag them.
- **Every error path must show a user-visible message.** No silent failures.
- **Wave 6.5 is a BLOCKING gate.** The app must actually run.
- **Wave 8 QA is BLACK-BOX.** The agent cannot read source code.
