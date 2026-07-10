# LLM Wiki Common Framework

This directory contains shared rules for all LLM Wiki skills.

Skills must load only the common documents they explicitly need.

Do not load the entire `common/` directory.

## Shared Documents

- `search.md` — candidate discovery and reading order
- `language.md` — response and artifact language
- `limits.md` — token, file, and semantic-analysis limits
- `logging.md` — append-only log behavior
- `indexing.md` — index update behavior
- `linking.md` — Obsidian linking rules
- `naming.md` — filenames and titles
- `page-types.md` — page-type purposes
- `templates.md` — reusable Wiki page templates
- `status.md` — completion states
- `safety.md` — immutable-source and destructive-change rules

## Loading Rule

Each skill router must:

1. resolve its mode,
2. load only required common documents,
3. load one mode document,
4. avoid loading unrelated skill or common documents.
