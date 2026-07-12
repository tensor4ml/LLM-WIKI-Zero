# Common Logging Policy

Use:

```text
20_Wiki/log.md
```

## Append Conditions

Append only when:

- a Wiki page was created,
- a Wiki page was updated,
- a report was created,
- Critical or High issues were found and recording is required,
- or the user explicitly requests logging.

Do not log read-only routine queries.

## Reading Limit

Read only the latest 200 lines unless a full log audit is explicitly requested.

## Append-Only Rule

Do not rewrite, reorder, or delete old entries.

## Generic Format

```markdown
## [YYYY-MM-DD] operation | Title

- Operation:
- Source:
- Question:
- Pages Used:
- Created:
- Updated:
- Notes:
```

Include only fields relevant to the operation and only files actually changed.
