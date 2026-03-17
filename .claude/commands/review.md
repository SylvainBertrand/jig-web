---
description: "Run the code-reviewer subagent to check code quality, consistency, and spec compliance."
---

# /review — Code Quality Review

Delegate to the `code-reviewer` subagent. It will inspect the codebase for:
- TypeScript quality (no `any`, proper generics, correct typing)
- Consistency (naming conventions, import patterns, store usage)
- Spec compliance (interfaces match spec, all required features present)
- Dead code, hardcoded values, missing error handling

Report issues with file:line references and severity.
