# Changed Lint Mode

Read:

```text
../references/severity.md
```

Detect:

```bash
git diff --name-only --diff-filter=ACMR -- 20_Wiki
git diff --cached --name-only --diff-filter=ACMR -- 20_Wiki
```

Inspect changed files and at most 3 directly related pages per file.

Check frontmatter, links, index impact, headings, placement, and source structure.

Do not perform full-Wiki semantic comparison.
