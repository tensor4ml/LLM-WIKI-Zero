# Default Ingest Mode

Process one explicit source file or directory.

Read:

```text
../references/state.md
../../common/templates.md
../../common/indexing.md
../../common/logging.md
```

Workflow:

1. validate path,
2. check hash and state,
3. read progressively,
4. extract central claims and evidence,
5. narrow related Wiki candidates,
6. create or update the source page,
7. create only high-value related pages,
8. add meaningful links,
9. index only new pages,
10. append log only after changes,
11. update state.

Do not create a synthesis by default.
