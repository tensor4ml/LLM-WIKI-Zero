# Common Indexing Policy

Use:

```text
20_Wiki/index.md
```

Update the index only when a new page is created or the user explicitly requests index maintenance.

Do not update the index for:

- read-only queries,
- updated existing pages,
- skipped ingestion,
- routine lint with no report.

Read only the affected section.

Expected sections:

```markdown
## Sources
## Topics
## Concepts
## Entities
## Questions
## Syntheses
## Maintenance
```

Entry format:

```markdown
- [[Page Name]] — one-line summary.
```

Do not reorder unrelated sections.
