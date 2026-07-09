---
name: lint
description: Use this skill to inspect, clean, and improve the health of the LLM Wiki.
---

# Lint Skill

## Purpose

Use this skill to inspect, clean, and improve the health of the LLM Wiki.

The goal is to keep the Wiki coherent as it grows by finding broken links, orphan pages, duplicate concepts, stale claims, missing pages, weak source attribution, and structural problems.

## When to Use This Skill

Use this skill when the user says things like:

- "Lint the wiki."
    
- "Health-check the wiki."
    
- "Find broken links."
    
- "Find orphan pages."
    
- "Check for contradictions."
    
- "Clean up the wiki."
    
- "Wiki 상태 점검해줘."
    
- "중복 페이지 찾아줘."
    
- "링크 깨진 것 확인해줘."
    

## Core Rules

1. Do not modify raw sources.
    
2. Do not delete Wiki pages without explicit user approval.
    
3. Do not perform large refactors without first proposing a plan.
    
4. Prefer reporting issues before making destructive changes.
    
5. Update `20_Wiki/log.md` after every lint pass.
    
6. If creating a lint report, save it under `20_Wiki/20.06_Syntheses/`.
    
7. Update `20_Wiki/index.md` if new maintenance pages are created.
    
8. Preserve existing content unless there is a clear reason to change it.
    
9. Surface contradictions instead of hiding them.
    
10. Recommend next actions in priority order.
    

## Lint Workflow

### 1. Inspect Wiki Structure

Check that these paths exist:

```text
20_Wiki/index.md
20_Wiki/log.md
20_Wiki/overview.md
20_Wiki/20.01_Sources/
20_Wiki/20.02_Concepts/
20_Wiki/20.03_Entities/
20_Wiki/20.04_Topics/
20_Wiki/20.05_Questions/
20_Wiki/20.06_Syntheses/
20_Wiki/20.07_Templates/
```

Report missing files or directories.

### 2. Check index.md

Inspect:

```text
20_Wiki/index.md
```

Look for:

- pages listed in index but missing from disk
    
- pages on disk missing from index
    
- duplicate index entries
    
- pages under the wrong section
    
- unclear one-line summaries
    
- stale entries
    

### 3. Check log.md

Inspect:

```text
20_Wiki/log.md
```

Look for:

- missing recent operations
    
- inconsistent log headings
    
- non-append-only edits
    
- entries without operation type
    
- entries without created or updated files
    
- entries without notes
    

Expected log format:

```markdown
## [YYYY-MM-DD] operation | Title

- Operation:
- Created:
- Updated:
- Notes:
```

### 4. Check Broken Links

Scan Wiki files for Obsidian-style links:

```markdown
[[Page Name]]
```

Detect links that do not resolve to existing pages.

Report:

```markdown
## Broken Links

- `[[Missing Page]]`
  - Found in: path/to/file.md
  - Suggested action: create page / rename link / remove link
```

### 5. Check Orphan Pages

Find pages with no inbound links.

Report:

```markdown
## Orphan Pages

- path/to/page.md
  - Suggested inbound links:
    - [[Related Page]]
```

Do not classify `index.md`, `log.md`, or standalone maintenance reports as problematic orphans unless they are clearly disconnected from the workflow.

### 6. Check Duplicate or Overlapping Pages

Look for pages that appear to cover the same concept.

Examples:

- `rag.md` and `retrieval-augmented-generation.md`
    
- `llm-wiki.md` and `persistent-llm-wiki.md`
    
- `obsidian.md` and `obsidian-tool.md`
    

Report:

```markdown
## Duplicate / Overlapping Pages

- Candidate group:
  - path/to/page-a.md
  - path/to/page-b.md
- Issue:
- Recommendation:
  - merge / rename / keep separate with clearer distinction
```

### 7. Check Stale or Contradictory Claims

Look for claims that conflict across pages.

Report:

```markdown
## Contradictions / Tensions

- Claim A:
  - Page:
  - Evidence:
- Claim B:
  - Page:
  - Evidence:
- Recommendation:
```

Do not silently choose one claim unless the evidence clearly supports it.

### 8. Check Missing Pages

Find frequently mentioned concepts, entities, tools, or topics that lack pages.

Report:

```markdown
## Missing Pages to Create

- [[Suggested Page]]
  - Mentioned in:
    - path/to/file.md
  - Suggested type: concept/entity/topic/synthesis
  - Reason:
```

### 9. Check Source Attribution

Look for Wiki pages that make claims without linking to source pages.

Report:

```markdown
## Weak Source Attribution

- path/to/page.md
  - Unsupported or weakly supported claim:
  - Suggested source:
```

### 10. Check Page Size and Structure

Identify pages that are too large, too small, or poorly structured.

Report:

```markdown
## Structural Issues

- path/to/page.md
  - Issue:
  - Recommendation:
```

Examples:

- split large page into concept pages
    
- merge tiny duplicate pages
    
- add missing frontmatter
    
- add related links
    
- add source section
    
- rename unclear title
    

## Lint Report Output

Create a lint report under:

```text
20_Wiki/20.06_Syntheses/
```

Filename:

```text
wiki-health-check-YYYY-MM-DD.md
```

Use this format:

```markdown
---
type: synthesis
title: Wiki Health Check YYYY-MM-DD
created:
updated:
tags:
  - maintenance
  - lint
---

# Wiki Health Check YYYY-MM-DD

## Summary

## Missing Core Files or Directories

## Index Issues

## Log Issues

## Broken Links

## Orphan Pages

## Duplicate / Overlapping Pages

## Contradictions / Tensions

## Missing Pages to Create

## Weak Source Attribution

## Structural Issues

## Recommended Next Actions
```

## Severity Levels

Classify each issue as:

```text
Critical
High
Medium
Low
```

Use this meaning:

- Critical: prevents Wiki usage or causes serious misinformation
    
- High: broken links, missing index, major contradictions
    
- Medium: orphan pages, weak source attribution, duplicate pages
    
- Low: formatting, naming, minor organization issues
    

## Optional Fix Mode

By default, lint should report issues only.

Only perform fixes if the user explicitly asks for fixes.

Safe fixes allowed without additional confirmation:

- update `20_Wiki/index.md`
    
- add missing log entry for current lint pass
    
- add missing frontmatter to clearly identifiable pages
    
- create the lint report
    

Fixes requiring confirmation:

- delete pages
    
- merge pages
    
- rename pages
    
- move pages across folders
    
- rewrite major sections
    
- resolve contradictions by replacing claims
    

## Update index.md

If a lint report is created, add it under the Maintenance or Syntheses section of `20_Wiki/index.md`.

Example:

```markdown
- [[Wiki Health Check YYYY-MM-DD]] — Maintenance report covering broken links, orphan pages, duplicate pages, and stale claims.
```

## Update log.md

Append this entry:

```markdown
## [YYYY-MM-DD] lint | Wiki Health Check

- Operation: lint
- Created:
  - [[Wiki Health Check YYYY-MM-DD]]
- Updated:
  - [[index]]
  - [[log]]
- Notes:
  - Checked broken links, orphan pages, duplicate pages, contradictions, missing pages, and source attribution.
```

## Completion Report

After linting, report:

```markdown
# Lint Report

## Status
Complete / Partial / Failed

## Created Files
- ...

## Updated Files
- ...

## Critical Issues
- ...

## High Priority Issues
- ...

## Recommended Next Actions
1. ...
2. ...
3. ...
```

## Failure Handling

If the Wiki is too large, lint in phases:

1. structure and index
    
2. links
    
3. duplicates
    
4. contradictions
    
5. source attribution
    

If files cannot be read, report them.

If the Wiki does not exist yet, recommend running the setup workflow first.
