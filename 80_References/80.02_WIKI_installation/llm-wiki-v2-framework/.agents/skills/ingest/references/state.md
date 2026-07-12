# Ingest State

Use:

```text
_workspace/ingest-state.json
```

Suggested structure:

```json
{
  "10_Raw/10.02_Inbox/example.md": {
    "sha256": "FILE_HASH",
    "source_page": "20_Wiki/20.01_Sources/example.md",
    "processed_at": "ISO_TIMESTAMP",
    "status": "processed"
  }
}
```

Calculate hash with:

```bash
shasum -a 256 path/to/source
```

Skip unchanged processed sources unless reprocessing is requested.

Update state only after successful writes.
