# Common Search Policy

Use deterministic discovery before semantic reading.

## Search Order

1. explicit user path
2. exact filename
3. frontmatter `title`
4. frontmatter `aliases`
5. tags
6. existing wikilinks
7. filename keyword
8. heading search
9. local full-text search
10. semantic comparison of narrowed candidates only

## Suggested Commands

```bash
rg -n --glob '*.md' '^title:|^aliases:|^tags:' 20_Wiki
rg -n --glob '*.md' 'KEYWORD' 20_Wiki
rg -n --glob '*.md' '\[\[Page Name(\||#|\]\])' 20_Wiki
```

## Reading Order

For a candidate page, inspect:

1. path
2. filename
3. frontmatter
4. title
5. headings
6. wikilinks
7. first paragraph
8. relevant sections
9. full body only when necessary

Do not read `20_Wiki/index.md` first unless the selected mode explicitly requires index validation or the user asks to navigate through the index.

Do not semantically compare every page.
