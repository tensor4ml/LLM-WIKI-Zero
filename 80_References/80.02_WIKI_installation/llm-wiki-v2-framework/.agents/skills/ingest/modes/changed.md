# Changed Ingest Mode

Read:

```text
../references/state.md
../../common/templates.md
../../common/indexing.md
../../common/logging.md
```

Detect sources with:

```bash
git diff --name-only --diff-filter=ACMR -- 10_Raw
git diff --cached --name-only --diff-filter=ACMR -- 10_Raw
git ls-files --others --exclude-standard -- 10_Raw
```

Process at most 5 changed sources.

Ignore generated Wiki files and state changes.

For renamed sources, update `source_path` and state instead of creating duplicate source pages.
