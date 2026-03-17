---
description: "Fix issues reported by /test or /review. Takes a list of issues and dispatches fixes to the appropriate subagent based on file ownership."
argument-hint: "<paste issue list from /test or /review output>"
---

# /fix — Dispatch Issue Fixes

1. Parse the provided issue list
2. Group issues by file ownership (see CLAUDE.md agent ownership table)
3. For each group, delegate to the responsible subagent with the specific issues to fix
4. After fixes, re-run `/test` to verify
