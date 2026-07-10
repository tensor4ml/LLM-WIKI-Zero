# Graphify Update Mode

Update only changed Wiki files when supported.

Use Git to narrow scope:

```bash
git diff --name-only --diff-filter=ACMR -- 20_Wiki
git diff --cached --name-only --diff-filter=ACMR -- 20_Wiki
```

If Graphify does not support incremental updates, rebuild only the smallest practical directory.

Report semantic-backend and sub-agent limitations.
