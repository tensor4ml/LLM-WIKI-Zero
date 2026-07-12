# Clippings Ingest Mode

## Purpose

Process only unprocessed or changed source files under:

```text
10_Raw/10.01_Clippings/
```

This mode is equivalent to Inbox Mode, but its source scope is fixed to the Clippings directory.

## Required Documents

Read:

```text
../references/state.md
../../common/templates.md
../../common/indexing.md
../../common/logging.md
```

The shared router already loads the required common safety, search, limits, linking, naming, and status policies.

## Source Selection

Use the ingest state file and file hashes.

Do not assume every file in `10_Raw/10.01_Clippings/` is unprocessed.

Process at most 5 sources per run.

Prioritize sources in this order:

1. explicitly named files inside the Clippings directory,
2. new untracked files,
3. modified files,
4. oldest unprocessed files.

## Allowed Scope

The hard source boundary is:

```text
10_Raw/10.01_Clippings/
```

Do not inspect or process files from:

```text
10_Raw/10.02_Inbox/
10_Raw/10.03_Assets/
```

unless the user explicitly requests an additional scope.

Related Wiki pages may still be searched under `20_Wiki/` according to the common search limits.

## Workflow

For each selected clipping:

1. validate the path and readability,
2. calculate its SHA-256 hash,
3. compare it with `_workspace/ingest-state.json`,
4. skip unchanged sources already marked as processed,
5. read the clipping progressively,
6. extract the main argument, central claims, evidence, concepts, entities, topics, tensions, and open questions,
7. narrow related Wiki candidates,
8. create or update the source page,
9. create or update only high-value related pages,
10. add meaningful bidirectional links,
11. update affected index sections only for newly created pages,
12. append a log entry only after successful Wiki changes,
13. update ingest state only after successful writes.

## Clipping-Specific Guidance

Clippings may contain:

- web article metadata,
- publication names,
- canonical URLs,
- author bylines,
- clipped highlights,
- quoted passages,
- tags from Obsidian Web Clipper,
- duplicated navigation or boilerplate text.

Prefer the article's substantive content.

Ignore or minimize:

- cookie banners,
- navigation menus,
- recommendation widgets,
- advertisements,
- repeated headers and footers,
- unrelated comments,
- duplicated clipped text.

Preserve useful source metadata in the source page when available.

Do not reproduce long copyrighted passages. Summarize and record concise evidence instead.

## Source Page Behavior

The source page is required for every successfully processed clipping.

Use the common Source template.

When useful, retain clipping metadata in frontmatter or `Source Notes`, including:

- original title,
- author,
- publication,
- publication date,
- clipping date,
- canonical URL,
- source path.

Do not invent unavailable metadata.

## Batch Handling

When more than 5 unprocessed or changed clippings exist:

1. process the highest-priority 5,
2. do not silently continue into another batch,
3. mark the result `Partial`,
4. list the remaining clipping paths under `Deferred`.

## Unchanged Files

When the current hash matches the stored processed hash:

- skip the file,
- do not update the source page,
- do not update `index.md`,
- do not append a log entry,
- report the file under `Skipped`.

## Output

Include:

- Clippings files found,
- sources processed,
- sources skipped,
- sources deferred,
- Wiki files created,
- Wiki files updated,
- contradictions or tensions,
- incomplete metadata or extraction limitations.

Use the common completion status definitions.
