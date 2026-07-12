# LLM Wiki Skills

Core skills are under `.agents/skills/`.

Use:

- `$ingest` to integrate new raw sources
- `$query` to answer from existing Wiki content
- `$lint` to inspect Wiki health
- `$graphify` to build or query structural/semantic relationships

Each skill uses a router.

The router must resolve the mode before loading common or mode documents.

Do not load every file in a skill directory.

Shared rules are under:

```text
.agents/skills/common/
```

Load only explicitly required shared documents.

Never modify `10_Raw/`.
