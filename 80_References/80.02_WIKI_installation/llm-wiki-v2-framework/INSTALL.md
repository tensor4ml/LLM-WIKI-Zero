# LLM Wiki v2 Framework Installation

## Structure

Copy the `.agents/skills/` directory into the root of the LLM Wiki project.

```text
LLM-Wiki/
├── .agents/
│   └── skills/
│       ├── common/
│       ├── ingest/
│       ├── lint/
│       ├── query/
│       └── graphify/
├── 10_Raw/
├── 20_Wiki/
├── 30_Rules/
└── AGENTS.md
```

## Install

From the extracted package directory:

```bash
cp -R .agents/skills /path/to/LLM-Wiki/.agents/
```

Or copy individual skill directories.

## AGENTS.md

Merge the contents of `AGENTS-v2-snippet.md` into the project `AGENTS.md`.

Do not blindly overwrite project-specific instructions.

## State Directory

For incremental ingestion:

```bash
mkdir -p _workspace
```

`_workspace/ingest-state.json` is created after the first successful ingest.

## Recommended Checks

```bash
find .agents/skills -maxdepth 3 -type f | sort
git status
```

Then test:

```text
$ingest source-only 10_Raw/10.02_Inbox/example.md
$query What does the Wiki say about example?
$lint fast
$graphify 20_Wiki
```
