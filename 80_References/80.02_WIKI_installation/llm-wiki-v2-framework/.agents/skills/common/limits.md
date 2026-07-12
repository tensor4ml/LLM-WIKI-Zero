# Common Processing Limits

These are defaults. A mode may define stricter limits.

- maximum source files per run: 5
- maximum changed files per run: 20
- maximum candidate pages per target: 10
- maximum fully read related pages per target: 5
- maximum semantic-analysis pages per run: 10
- maximum related pages loaded per target: 3
- maximum duplicate candidate pairs: 10
- maximum contradiction groups: 5
- maximum missing-page suggestions: 10
- maximum source-attribution reviews: 10
- maximum index sections read: 2
- maximum recent log lines read: 200

When a limit is exceeded:

1. finish deterministic checks,
2. process highest-priority candidates,
3. mark the result `Partial`,
4. list deferred files or checks,
5. do not silently increase scope.
