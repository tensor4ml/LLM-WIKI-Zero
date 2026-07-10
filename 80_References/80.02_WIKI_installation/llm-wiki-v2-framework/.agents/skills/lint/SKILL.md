---
name: lint
description: Run scoped, token-efficient health checks on the LLM Wiki.
---

# Lint Router

## Mode Selection

- explicit file or directory -> `modes/scoped.md`
- `links` -> `modes/links.md`
- `structure` -> `modes/structure.md`
- `deep` or `full` -> `modes/deep.md`
- `changed` -> `modes/changed.md`
- no mode:
  - changed Wiki files exist -> `modes/changed.md`
  - otherwise -> `modes/fast.md`

## Required Common Documents

Always read:

```text
../common/safety.md
../common/search.md
../common/limits.md
../common/status.md
```

Read only when needed:

```text
../common/indexing.md
../common/logging.md
../common/language.md
```

Read exactly one mode document unless Scoped Mode is combined with one operational mode.
