---
name: ingest
description: Use this skill when the user asks to ingest, process, import, summarize, or integrate a new source into the LLM Wiki
---

# Ingest Skill

## Purpose

Use this skill when the user asks to ingest, process, import, summarize, or integrate a new source into the LLM Wiki.

The goal of ingestion is not only to summarize a source.  
The goal is to integrate the source into the persistent Markdown Wiki by creating and updating source pages, concept pages, entity pages, topic pages, synthesis pages, index entries, and log entries.

## When to Use This Skill

Use this skill when the user says things like:

- "Ingest this source."
    
- "Process the new document."
    
- "Add this article to the wiki."
    
- "Import the files in 10_Raw/10.02_Inbox."
    
- "Read this paper and update the wiki."
    
- "이 문서를 Wiki에 반영해줘."
    
- "새 자료를 LLM Wiki에 넣어줘."
    

## Core Rules

1. Never modify files under `10_Raw/`.
    
2. Treat raw sources as immutable source-of-truth documents.
    
3. Create or update Markdown files only under `20_Wiki/`, unless explicitly instructed otherwise.
    
4. Every created or updated Wiki page must use YAML frontmatter.
    
5. Use Obsidian-style links: `[[Page Name]]`.
    
6. Do not invent claims that are not supported by the source.
    
7. Mark uncertain claims explicitly.
    
8. If a new source contradicts existing Wiki content, record the contradiction instead of silently overwriting the old claim.
    
9. Update `20_Wiki/index.md` after every ingest.
    
10. Append an entry to `20_Wiki/log.md` after every ingest.
    

## Expected Directory Structure

Assume this structure:

```text
llm-wiki/
├─ .agents/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ 10_Raw/
│  ├─ 10.01_Clippings/
│  ├─ 10.02_Inbox/
│  └─ 10.03_Assets/
├─ 20_Wiki/
│  ├─ index.md
│  ├─ log.md
│  ├─ overview.md
│  ├─ 20.01_Sources/
│  ├─ 20.02_Concepts/
│  ├─ 20.03_Entities/
│  ├─ 20.04_Topics/
│  ├─ 20.05_Questions/
│  ├─ 20.06_Syntheses/
│  └─ 20.07_Templates/
└─ 30_Rules/
```

If the expected structure is missing, create the missing directories and files before continuing.

## Ingest Workflow

Follow this sequence.

### 1. Locate Source

Check the source location provided by the user.

If no specific file is named, inspect:

```text
10_Raw/10.02_Inbox/
10_Raw/10.01_Clippings/
```

Identify unprocessed sources.

### 2. Read Source

Read the source carefully.

Extract:

- title
    
- author or origin if available
    
- date if available
    
- source type
    
- main argument
    
- key claims
    
- important facts
    
- definitions
    
- examples
    
- entities
    
- tools
    
- concepts
    
- tensions or contradictions
    
- open questions
    
- possible follow-up topics
    

### 3. Create Source Page

Create a source summary page in:

```text
20_Wiki/20.01_Sources/
```

Use lowercase kebab-case filename.

Example:

```text
20_Wiki/20.01_Sources/2026-07-07-llm-wiki-pattern.md
```

Use this format:

```markdown
---
type: source
title:
created:
updated:
source_path:
status: processed
tags:
---

# Source Title

## Summary

## Key Claims

## Important Details

## Evidence

## Related Concepts

## Related Entities

## Related Topics

## Contradictions / Tensions

## Open Questions

## Source Notes
```

### 4. Create or Update Concept Pages

For each important concept, create or update a page in:

```text
20_Wiki/20.02_Concepts/
```

Concept examples:

- RAG
    
- Persistent Knowledge Base
    
- Wiki Ingestion
    
- Wiki Linting
    
- Knowledge Compounding
    
- Cross-Reference Maintenance
    

Use this format:

```markdown
---
type: concept
title:
created:
updated:
aliases:
tags:
---

# Concept Name

## Definition

## Why It Matters

## Key Points

## Related Concepts

## Related Sources

## Open Questions
```

### 5. Create or Update Entity Pages

For each person, organization, product, tool, system, location, or named object, create or update a page in:

```text
20_Wiki/20.03_Entities/
```

Entity examples:

- Obsidian
    
- NotebookLM
    
- ChatGPT
    
- qmd
    
- Marp
    
- Dataview
    
- Obsidian Web Clipper
    

Use this format:

```markdown
---
type: entity
title:
created:
updated:
category:
aliases:
tags:
---

# Entity Name

## Description

## Role / Relevance

## Key Details

## Related Concepts

## Related Sources

## Open Questions
```

### 6. Create or Update Topic Pages

For broader subject areas, create or update pages in:

```text
20_Wiki/20.04_Topics/
```

Topic examples:

- Personal Knowledge Management
    
- LLM-Assisted Research
    
- Markdown Wiki Workflow
    
- Internal Team Knowledge Bases
    

Use this format:

```markdown
---
type: topic
title:
created:
updated:
tags:
---

# Topic Name

## Overview

## Key Concepts

## Important Sources

## Related Entities

## Current Synthesis

## Open Questions
```

### 7. Create or Update Synthesis Pages

If ingestion creates a useful cross-source comparison, argument, or integrated insight, create or update a page in:

```text
20_Wiki/20.06_Syntheses/
```

Use this format:

```markdown
---
type: synthesis
title:
created:
updated:
tags:
---

# Synthesis Title

## Core Argument

## Supporting Evidence

## Comparison / Analysis

## Implications

## Related Sources

## Related Concepts

## Open Questions
```

### 8. Update Links

Add Obsidian-style links across pages.

Examples:

```markdown
- [[RAG]]
- [[LLM Wiki]]
- [[Persistent Knowledge Base]]
- [[Obsidian]]
```

Every source page should link to related concept, entity, and topic pages.

Every concept, entity, and topic page should link back to relevant source pages.

### 9. Update index.md

Update:

```text
20_Wiki/index.md
```

Maintain these sections:

```markdown
# LLM Wiki Index

## Overview

## Sources

## Topics

## Concepts

## Entities

## Questions

## Syntheses

## Maintenance
```

Each entry should use this format:

```markdown
- [[Page Name]] — one-line summary.
```

### 10. Update log.md

Append a new entry to:

```text
20_Wiki/log.md
```

Use this format:

```markdown
## [YYYY-MM-DD] ingest | Source Title

- Operation: ingest
- Source:
  - 10_Raw/10.01_Clippings/example.md
- Created:
  - [[Page Name]]
- Updated:
  - [[Page Name]]
- Notes:
  - ...
```

Do not delete or rewrite old log entries.

## Completion Report

After ingestion, report:

```markdown
# Ingest Report

## Status
Complete / Partial / Failed

## Source Processed
- ...

## Created Files
- ...

## Updated Files
- ...

## Key Takeaways
- ...

## Contradictions / Tensions
- ...

## Open Questions
- ...

## Recommended Next Steps
1. ...
2. ...
3. ...
```

## Failure Handling

If a source cannot be read, report the issue clearly.

If the source is too large, process it in sections.

If the source contains images, first process the text, then inspect referenced images when possible.

If the existing Wiki structure is inconsistent, preserve existing content and report the inconsistency before making major changes.
