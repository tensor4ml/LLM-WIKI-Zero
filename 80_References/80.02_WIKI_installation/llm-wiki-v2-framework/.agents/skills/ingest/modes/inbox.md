# Inbox Ingest Mode

Inspect only:

```text
10_Raw/10.02_Inbox/
```

Read:

```text
../references/state.md
../../common/templates.md
../../common/indexing.md
../../common/logging.md
```

Process at most 5 unprocessed or changed sources.

Prioritize:

1. explicitly named files,
2. untracked files,
3. changed files,
4. oldest unprocessed files.

Do not inspect other raw directories unless requested.
