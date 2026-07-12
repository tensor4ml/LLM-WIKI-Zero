---
name: graphify
description: Build, update, and query the Wiki graph using scoped Graphify operations.
---

# Graphify Router

This skill adapts Graphify to the shared LLM Wiki framework.

## Mode Selection

- build or initial graph -> `modes/build.md`
- update or changed files -> `modes/update.md`
- graph query -> `modes/query.md`
- graph health or validation -> `modes/validate.md`

## Required Common Documents

Always read:

```text
../common/safety.md
../common/search.md
../common/limits.md
../common/status.md
```

Read exactly one mode document.

Do not modify `10_Raw/`.
Do not use Graphify output as a substitute for source evidence.
