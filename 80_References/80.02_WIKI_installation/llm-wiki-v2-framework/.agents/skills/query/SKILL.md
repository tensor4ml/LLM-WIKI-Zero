---
name: query
description: Answer questions from the existing LLM Wiki with minimal context loading.
---

# Query Router

## Mode Selection

- general question -> `modes/answer.md`
- comparison -> `modes/compare.md`
- relationship exploration -> `modes/explore.md`
- source verification -> `modes/source.md`
- `deep` -> `modes/deep.md`
- explicit save request -> answer mode plus `modes/save.md`

## Required Common Documents

Always read:

```text
../common/search.md
../common/limits.md
../common/status.md
```

Read only when needed:

```text
../common/language.md
../common/templates.md
../common/indexing.md
../common/logging.md
```

Read one answer mode. Load `save.md` only after an explicit save request.

Do not read `index.md` first by default.
Do not log routine read-only queries.
