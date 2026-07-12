---
name: ingest
description: Incrementally ingest new or changed sources into the LLM Wiki.
---

# Ingest Router

Resolve the mode before reading sources or Wiki pages.

## Mode Selection

- explicit source path -> `modes/default.md`
- `inbox` -> `modes/inbox.md`
- `clippings` or `10_Raw/10.01_Clippings/` -> `modes/clippings.md`
- `changed` -> `modes/changed.md`
- `source-only` -> `modes/source-only.md`
- `deep` or `full` -> `modes/deep.md`
- no path or mode -> `modes/inbox.md`

## Required Common Documents

Always read:

```text
../common/safety.md
../common/search.md
../common/limits.md
../common/linking.md
../common/naming.md
../common/status.md
```

Read only when needed:

```text
../common/templates.md
../common/indexing.md
../common/logging.md
../common/language.md
```

Read exactly one mode document.

Do not scan all of `10_Raw/` or `20_Wiki/`.
