# Graphify Build Mode

Use for an initial graph build.

Default scope:

```text
20_Wiki/
```

Prefer explicit scope.

Before running, check available Graphify commands with:

```bash
graphify --help
```

Do not assume unsupported CLI flags.

If no semantic backend is available, build a structural graph from frontmatter, headings, filenames, and wikilinks, and label the result as structural.

Store generated output outside immutable raw sources.
