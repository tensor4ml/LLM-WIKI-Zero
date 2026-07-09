# LLM Wiki Rules Modularization Prompt

당신은 Markdown 기반 **LLM Wiki**를 구축하고 유지보수하는 에이전트입니다.

이번 작업의 목표는 세 가지 AI 환경(Claude, Codex, Antigravity) 모두를 프로젝트에서 동시에 사용할 수 있도록 환경을 통합 구축하는 것입니다. 각 AI의 진입점 파일(`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`)과 프로젝트-local 에이전트 디렉터리 및 skill 디렉터리를 모두 생성 및 설정해야 합니다.

각 AI에 맞게 다음 설정 파일들을 모두 참조하여 환경을 구축하세요.
- Claude: `80_References/80.02_WIKI_installation/Claude-configuration.md` (루트 지시 파일: `CLAUDE.md`, 에이전트 디렉터리: `.claude`)
- Codex: `80_References/80.02_WIKI_installation/Codex-configuration.md` (루트 지시 파일: `AGENTS.md`, 에이전트 디렉터리: `.codex`)
- Antigravity: `80_References/80.02_WIKI_installation/Antigravity-configuration.md` (루트 지시 파일: `GEMINI.md`, 에이전트 디렉터리: `.agent`)

이번 작업의 목표는 각 AI의 루트 지시 파일(`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`)에 모든 규칙을 길게 넣지 않고, 각 지시 파일은 기본 규칙과 라우팅 역할만 담당하게 만드는 것입니다.
세부 운영 규칙은 별도의 Markdown 파일로 분리하여 `30_Rules/` 디렉터리에 저장하며, 이는 세 AI 모두가 공통으로 참조합니다.

즉, 다음 구조를 만드세요.

```text
llm-wiki/
├─ .claude/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ .codex/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ .agent/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ CLAUDE.md
├─ AGENTS.md
├─ GEMINI.md
├─ README.md
│
├─ 80_References/
│  └─ 80.02_WIKI_installation/
│     ├─ Claude-configuration.md
│     ├─ Codex-configuration.md
│     ├─ Antigravity-configuration.md
│     ├─ Ingest Skill.md
│     ├─ Query Skill.md
│     └─ Lint Skill.md
│
├─ 10_Raw/
│  ├─ 10.01_Clippings/
│  ├─ 10.02_Inbox/
│  └─ 10.03_Assets/
│
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
│
├─ 30_Rules/
│  ├─ Core-Principles.md
│  ├─ Directory-Structure.md
│  ├─ Page-Types.md
│  ├─ Frontmatter.md
│  ├─ Naming-and-Linking.md
│  ├─ Index-and-Log.md
│  ├─ Source-Attribution.md
│  ├─ Contradictions-and-Uncertainty.md
│  ├─ Refactor-Policy.md
│  └─ Operation-Reporting.md
```

---

## 1. 작업 목표

다음 작업을 수행하세요.

1. 현재 프로젝트 구조를 확인합니다.
2. 세 가지 AI(Claude, Codex, Antigravity) 환경을 모두 구축하기 위해 설정 파일(`Claude-configuration.md`, `Codex-configuration.md`, `Antigravity-configuration.md`)을 각각 읽고 분석합니다.
3. 세 가지 AI별로 지시 파일(`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`), 에이전트 디렉터리(`.claude`, `.codex`, `.agent`), skill 디렉터리(`.claude/skills`, `.codex/skills`, `.agent/skills`)를 결정합니다.
4. 누락된 모든 디렉터리를 생성합니다.
5. `CLAUDE.md`, `AGENTS.md`, `GEMINI.md` 각각을 얇은 기본 규칙 파일로 작성하되, 각 AI의 이름과 에이전트/스킬 경로를 적절히 반영하여 작성합니다. (예: `GEMINI.md`에는 `{SkillDirectory}` 대신 `.agent/skills`가 들어감)
6. 세부 Rule들을 `30_Rules/` 아래의 별도 공통 Markdown 파일로 작성합니다.
7. 세 AI의 skill 디렉터리 각각에 `ingest/SKILL.md`, `query/SKILL.md`, `lint/SKILL.md`가 없으면 `80_References/80.02_WIKI_installation/`의 참조 Skill 파일(각각 `Ingest Skill.md`, `Query Skill.md`, `Lint Skill.md`)의 내용을 사용해 생성합니다.
8. `20_Wiki/index.md`, `20_Wiki/log.md`, `20_Wiki/overview.md`가 없으면 초기 파일을 생성합니다.
9. 원본 자료가 있는 `10_Raw/` 파일은 절대 수정하지 않습니다.
10. 마지막에 생성·수정한 파일 목록을 보고합니다.

---

## 2. 핵심 설계 원칙

각 AI의 루트 지시 파일(`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`)은 다음만 담당해야 합니다.

- 에이전트 역할 정의
- 절대 지켜야 하는 최소 규칙
- `30_Rules/` 파일 로딩 순서
- 각 AI에 맞는 skill 디렉터리 라우팅
- 작업 완료 보고 형식

반대로, 아래 내용은 루트 지시 파일에 길게 넣지 말고 `30_Rules/` 파일로 분리하세요.

- 디렉터리 구조 상세 설명
- 페이지 유형별 상세 규칙
- frontmatter 규칙
- naming/linking 규칙
- index/log 규칙
- source attribution 규칙
- contradiction/uncertainty 처리 규칙
- refactor 정책
- operation report 형식

---

## 3. 루트 지시 파일 (`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`) 작성 규칙

각 AI 설정에 맞게 지시 파일(`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`)을 작성하세요.
각 파일에서 `{SkillDirectory}`는 해당 에이전트의 skill 디렉터리 경로(`.claude/skills`, `.codex/skills`, `.agent/skills`)로 치환하여 작성해야 합니다.

구체적으로 다음과 같이 매핑 및 치환하여 세 파일을 각각 작성하세요.

1. **CLAUDE.md**:
   - `{AgentInstructionFile}`: CLAUDE.md
   - `{SkillDirectory}`: `.claude/skills`
2. **AGENTS.md**:
   - `{AgentInstructionFile}`: AGENTS.md
   - `{SkillDirectory}`: `.codex/skills`
3. **GEMINI.md**:
   - `{AgentInstructionFile}`: GEMINI.md
   - `{SkillDirectory}`: `.agent/skills`

각 파일에 적용할 기본 지시 파일의 템플릿(아래 템플릿의 `{SkillDirectory}` 및 `{AgentInstructionFile}` 부분을 각 AI에 맞게 치환할 것)은 다음과 같습니다.


````markdown
# LLM Wiki Agent

## Role

You are the maintainer of an LLM-managed Markdown Wiki.

The user curates sources, asks questions, reviews outputs, and guides priorities.
You maintain the Wiki structure, update Markdown pages, preserve links, track changes, and keep the knowledge base coherent over time.

This Wiki is a persistent, compounding knowledge layer built from raw sources and user questions.

---

## Non-Negotiable Rules

1. Never modify files under `10_Raw/` unless the user explicitly instructs you to do so.
2. Treat raw sources as immutable source-of-truth documents.
3. Maintain generated knowledge under `20_Wiki/`.
4. Use Obsidian-style links: `[[Page Name]]`.
5. Every meaningful Wiki page must include YAML frontmatter.
6. Update `20_Wiki/index.md` after meaningful Wiki changes.
7. Append to `20_Wiki/log.md` after meaningful operations.
8. Do not invent unsupported claims.
9. Surface contradictions and uncertainty explicitly.
10. Before destructive operations, propose a plan and wait for explicit user approval.

---

## Rule Files

Detailed rules are stored under `30_Rules/`.

When working on the Wiki, read the relevant rule files before making changes.

Load rule files in this order when applicable:

1. `30_Rules/Core-Principles.md`
2. `30_Rules/Directory-Structure.md`
3. `30_Rules/Page-Types.md`
4. `30_Rules/Frontmatter.md`
5. `30_Rules/Naming-and-Linking.md`
6. `30_Rules/Index-and-Log.md`
7. `30_Rules/Source-Attribution.md`
8. `30_Rules/Contradictions-and-Uncertainty.md`
9. `30_Rules/Refactor-Policy.md`
10. `30_Rules/Operation-Reporting.md`

If a rule file conflicts with this agent instruction file, this agent instruction file takes priority.

---

## Skill Routing

Use the appropriate skill depending on the user request.

### Ingest

Use `{SkillDirectory}/ingest/SKILL.md` when the user asks to:

- ingest a source
- process a new document
- import files
- add an article, paper, transcript, note, or report to the Wiki
- process files in `10_Raw/10.02_Inbox/`

### Query

Use `{SkillDirectory}/query/SKILL.md` when the user asks to:

- answer from the Wiki
- summarize what the Wiki says
- compare existing concepts
- synthesize existing Wiki knowledge
- save a reusable answer back into the Wiki

### Lint

Use `{SkillDirectory}/lint/SKILL.md` when the user asks to:

- lint the Wiki
- check Wiki health
- find broken links
- find orphan pages
- detect duplicate pages
- detect contradictions
- check stale or weakly sourced claims

---

## Priority Order

When instructions conflict, follow this priority order:

1. User’s explicit instruction
2. Safety and data preservation
3. This agent instruction file
4. Relevant rule files
5. Relevant skill file
6. Existing Wiki conventions
7. General best practices

---

## Completion Report

After every meaningful Wiki operation, report:

```markdown
# LLM Wiki Operation Report

## Status
Complete / Partial / Failed

## Skill Used
Ingest / Query / Lint / None / Multiple

## Rule Files Used
- ...

## Created Files
- ...

## Updated Files
- ...

## Raw Sources Modified
No

## Key Decisions
- ...

## Issues / Uncertainties
- ...

## Recommend## 5. 30_Rules/Directory-Structure.md 작성

````markdown
# Directory Structure Rules

## Required Structure

Use this structure:

```text
llm-wiki/
├─ .claude/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ .codex/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ .agent/
│  └─ skills/
│     ├─ ingest/
│     │  └─ SKILL.md
│     ├─ query/
│     │  └─ SKILL.md
│     └─ lint/
│        └─ SKILL.md
├─ CLAUDE.md
├─ AGENTS.md
├─ GEMINI.md
├─ README.md
├─ 80_References/
│  └─ 80.02_WIKI_installation/
│     ├─ Claude-configuration.md
│     ├─ Codex-configuration.md
│     ├─ Antigravity-configuration.md
│     ├─ Ingest Skill.md
│     ├─ Query Skill.md
│     └─ Lint Skill.md
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

## Raw Layer

Path:

```text
10_Raw/
```

The raw layer stores original source materials.

Rules:

- Do not modify files under `10_Raw/`.
- Do not normalize, rename, delete, or move raw files unless the user explicitly asks.
- If a raw source is messy, preserve it and create clean Wiki pages from it.
- If source metadata is unclear, mark it as unknown.

## Wiki Layer

Path:

```text
20_Wiki/
```

The Wiki layer stores LLM-generated Markdown pages.

Rules:
- Create and update generated knowledge under `20_Wiki/`.
- Keep pages readable in Obsidian.
- Use YAML frontmatter.
- Use lowercase kebab-case filenames.
- Use Obsidian-style links.

## Rules Layer

Path:

```text
30_Rules/
```

The rules layer stores modular maintenance rules.

Rules:

- Keep CLAUDE.md, AGENTS.md, and GEMINI.md concise.
- Put detailed standards in rule files.
- Update rule files when the Wiki process evolves.

## Skills Layer

Path:

```text
.claude/skills/
.codex/skills/
.agent/skills/
```

The skills layer stores project-specific task workflows under the project-local agent directory for each AI environment.

Rules:

- Use skills for operational procedures.
- Use rules for global standards.
- If a skill conflicts with a rule, follow the rule unless the user explicitly overrides it.

````

---�     ├─ Codex-configuration.md
│     ├─ Antigravity-configuration.md
│     ├─ Ingest Skill.md
│     ├─ Query Skill.md
│     └─ Lint Skill.md
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
````

## Raw Layer

Path:

```text
10_Raw/
```

The raw layer stores original source materials.

Rules:

- Do not modify files under `10_Raw/`.
- Do not normalize, rename, delete, or move raw files unless the user explicitly asks.
- If a raw source is messy, preserve it and create clean Wiki pages from it.
- If source metadata is unclear, mark it as unknown.

## Wiki Layer

Path:

```text
20_Wiki/
```

The Wiki layer stores LLM-generated Markdown pages.

Rules:
- Create and update generated knowledge under `20_Wiki/`.
- Keep pages readable in Obsidian.
- Use YAML frontmatter.
- Use lowercase kebab-case filenames.
- Use Obsidian-style links.

## Rules Layer

Path:

```text
30_Rules/
```

The rules layer stores modular maintenance rules.

Rules:

- Keep `{AgentInstructionFile}` concise.
- Put detailed standards in rule files.
- Update rule files when the Wiki process evolves.

## Skills Layer

Path:

```text
{SkillDirectory}/
```

The skills layer stores project-specific task workflows under the project-local agent directory.

Rules:

- Use skills for operational procedures.
- Use rules for global standards.
- If a skill conflicts with a rule, follow the rule unless the user explicitly overrides it.

````

---

## 6. 30_Rules/Page-Types.md 작성

```markdown
# Page Type Rules

## Source Pages

Path:

```text
20_Wiki/20.01_Sources/
````

Purpose:

A source page summarizes and indexes one raw source.

Use for:

- articles
- papers
- reports
- books
- transcripts
- clipped web pages
- uploaded documents

Source pages should include:

- summary
- key claims
- important details
- evidence
- related concepts
- related entities
- related topics
- contradictions or tensions
- open questions

## Concept Pages

Path:

```text
20_Wiki/20.02_Concepts/
```

Purpose:

A concept page explains one reusable idea.

Examples:

- RAG
- Persistent Knowledge Base
- Knowledge Compounding
- Wiki Linting

Concept pages should include:

- definition
- why it matters
- key points
- related concepts
- related sources
- open questions

## Entity Pages

Path:

```text
20_Wiki/20.03_Entities/
```

Purpose:

An entity page describes a named object.

Examples:

- person
- organization
- company
- product
- software tool
- place
- project

Entity pages should include:

- description
- role or relevance
- key details
- related concepts
- related sources
- open questions

## Topic Pages

Path:

```text
20_Wiki/20.04_Topics/
```

Purpose:

A topic page acts as a hub for a broader subject area.

Topic pages should include:

- overview
- key concepts
- important sources
- related entities
- current synthesis
- open questions

## Question Pages

Path:

```text
20_Wiki/20.05_Questions/
```

Purpose:

A question page saves a reusable user question and answer.

Save a question only when the answer has future value.

Do not save:

- trivial questions
- one-off checks
- typo fixes
- short commands

## Synthesis Pages

Path:

```text
20_Wiki/20.06_Syntheses/
```

Purpose:

A synthesis page combines multiple pages, sources, or ideas into a higher-level analysis.

Use for:

- comparisons
- frameworks
- arguments
- decision memos
- literature-style syntheses
- strategic summaries
- maintenance reports

````

---

## 7. 30_Rules/Frontmatter.md 작성

```markdown
# Frontmatter Rules

## Requirement

Every meaningful Wiki page should include YAML frontmatter.

## Required Fields

```yaml
---
type:
title:
created:
updated:
tags:
---
````

## Optional Fields

```yaml
aliases:
source_path:
status:
related:
confidence:
owner:
version:
---
```

## Allowed Type Values

Use these values when applicable:

```text
source
concept
entity
topic
question
synthesis
maintenance
overview
index
log
template
```

## Date Format

Use `YYYY-MM-DD`.

## Simplicity Rule

Do not overcomplicate frontmatter in the early stage.

Add metadata only when it improves:

- navigation
- search
- filtering
- maintenance
- source tracking

````

---

## 8. 30_Rules/Naming-and-Linking.md 작성

```markdown
# Naming and Linking Rules

## Filename Rules

Use lowercase kebab-case filenames.

Good:

```text
llm-wiki.md
rag.md
persistent-knowledge-base.md
wiki-ingestion-workflow.md
obsidian-web-clipper.md
llm-wiki-vs-rag.md
````

Avoid:

```text
LLM Wiki.md
RAG 정리.md
new note.md
문서1.md
My Great Summary.md
```

## Page Title Rules

Markdown page titles may use normal capitalization.

Example:

```markdown
# LLM Wiki
```

Filename:

```text
llm-wiki.md
```

## Link Rules

Use Obsidian-style links.

Examples:

```markdown
[[LLM Wiki]]
[[RAG]]
[[Persistent Knowledge Base]]
[[Obsidian]]
```

## Linking Expectations

- Source pages should link to related concepts, entities, and topics.
- Concept pages should link to related concepts and supporting sources.
- Entity pages should link to related concepts, topics, and sources.
- Topic pages should function as hubs.
- Synthesis pages should link to all major pages they draw from.
- Question pages should link to the pages used in the answer.

## Link Quality

Avoid excessive linking of every repeated word.

Prefer meaningful links that improve navigation.

````

---

## 9. 30_Rules/Index-and-Log.md 작성

```markdown
# Index and Log Rules

## index.md

Path:

```text
20_Wiki/index.md
````

Purpose:

`index.md` is the content-oriented map of the Wiki.

It should help both humans and LLMs quickly find relevant pages.

Maintain this structure:

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

Each entry should follow this format:

```markdown
- [[Page Name]] — one-line summary.
```

Rules:

- Update `index.md` whenever new Wiki pages are created.
- Update `index.md` when page names change.
- Remove or fix entries that point to missing pages.
- Keep summaries short and useful.
- Do not turn `index.md` into a full article.

## log.md

Path:

```text
20_Wiki/log.md
```

Purpose:

`log.md` is the chronological record of Wiki operations.

It should be append-only.

Use this format:

```markdown
## [YYYY-MM-DD] operation | Title

- Operation:
- Created:
  - [[Page Name]]
- Updated:
  - [[Page Name]]
- Source:
  - 10_Raw/10.01_Clippings/example.md
- Notes:
  - ...
```

Common operation types:

```text
setup
ingest
query
lint
refactor
maintenance
```

Rules:

- Append a log entry after every meaningful operation.
- Do not delete old log entries.
- Do not rewrite history unless correcting a clear formatting error.
- Keep log entries concise but traceable.
- Include files created and updated when possible.

````

---

## 10. 30_Rules/Source-Attribution.md 작성

```markdown
# Source Attribution Rules

## Purpose

Claims in Wiki pages should be traceable to sources.

The Wiki should make it clear where important claims came from.

## Rules

1. Link important claims to source pages where possible.
2. Do not fabricate citations.
3. Do not fabricate relationships between sources and claims.
4. If a claim lacks support, mark it as weakly attributed.
5. If source metadata is missing, mark it as unknown.
6. Prefer source pages over raw source paths when linking within the Wiki.

## Related Sources Section

Use a section like this when appropriate:

```markdown
## Related Sources

- [[2026-07-07-llm-wiki-pattern]]
````

## Weak Attribution

If a page makes important claims without a source link, mark it during lint.

Use labels such as:

```text
Source Needed
Weakly Attributed
Needs Verification
```

````

---

## 11. 30_Rules/Contradictions-and-Uncertainty.md 작성

```markdown
# Contradictions and Uncertainty Rules

## Contradiction Handling

When new information conflicts with existing content:

1. Do not silently overwrite the older claim.
2. Identify both claims.
3. Link each claim to its supporting source or page.
4. Mark the conflict under a dedicated section.

Use section names such as:

```markdown
## Contradictions / Tensions
````

or:

```markdown
## Source Tensions
```

Example wording:

```markdown
- Source A suggests X, while Source B suggests Y.
- This may reflect different time periods, definitions, assumptions, or source quality.
- Needs verification.
```

If the newer source clearly supersedes the older one, state that explicitly and preserve the historical context.

## Uncertainty Handling

Do not hide uncertainty.

Use explicit markers:

```text
Uncertain
Needs Verification
Open Question
Partially Supported
Source Needed
Weakly Attributed
```

## Open Questions

When an issue cannot be resolved, add an open question.

Example:

```markdown
## Open Questions

- Does this claim apply generally, or only within the source’s specific context?
- Is there newer evidence that supersedes this?
```

````

---

## 12. 30_Rules/Refactor-Policy.md 작성

```markdown
# Refactor Policy

## What Counts as Refactor

Refactor means:

- renaming files
- moving pages
- merging pages
- splitting pages
- deleting pages
- changing page taxonomy
- rewriting major sections
- changing the structure of `index.md`

## Required Refactor Plan

Before any major refactor, produce a plan.

Use this format:

```markdown
# Refactor Plan

## Reason

## Proposed Changes

## Files to Create

## Files to Rename

## Files to Move

## Files to Merge

## Files to Delete

## Links to Update

## Risks

## Confirmation Needed
````

## Destructive Changes

Do not perform destructive refactors without user approval.

Destructive changes include:

- deleting pages
- merging pages
- renaming pages
- moving large sets of files
- replacing major sections
- resolving contradictions by removing one side

## Safe Changes

Safe actions that do not require separate approval:

- creating missing directories
- creating missing starter files
- adding a new page during ingest
- updating index after page creation
- appending to log
- creating lint reports
- adding missing frontmatter to newly created pages

````

---

## 13. 30_Rules/Operation-Reporting.md 작성

```markdown
# Operation Reporting Rules

## Purpose

Every meaningful Wiki operation should end with a concise report.

The report should make it clear:

- what was done
- which skill was used
- which rule files were used
- what files were created
- what files were updated
- whether raw sources were modified
- what remains uncertain
- what to do next

## Standard Report Format

Use this format:

```markdown
# LLM Wiki Operation Report

## Status
Complete / Partial / Failed

## Skill Used
Ingest / Query / Lint / None / Multiple

## Rule Files Used
- ...

## Created Files
- ...

## Updated Files
- ...

## Raw Sources Modified
No

## Key Decisions
- ...

## Issues / Uncertainties
- ...

## Recommended Next Steps
1. ...
2. ...
3. ...
````

## Reporting Rules

- If no files were changed, say so clearly.
- If a task partially failed, explain what succeeded and what failed.
- If a source could not be read, identify the source and the reason.
- If assumptions were made, list them.
- Keep the report concise.

````

---

## 14. Skill 파일 생성

Skill 원본 파일은 `80_References/80.02_WIKI_installation/` 폴더에 있습니다.

세 가지 에이전트의 skill 디렉터리(`.claude/skills`, `.codex/skills`, `.agent/skills`) 각각에 아래 파일들이 없다면 placeholder를 만들지 말고, 해당 원본 파일의 내용을 사용해서 생성하세요.

### 각 디렉터리별 생성 대상:
1. **Claude 용**:
   - `.claude/skills/ingest/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Ingest Skill.md`)
   - `.claude/skills/query/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Query Skill.md`)
   - `.claude/skills/lint/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Lint Skill.md`)
2. **Codex 용**:
   - `.codex/skills/ingest/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Ingest Skill.md`)
   - `.codex/skills/query/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Query Skill.md`)
   - `.codex/skills/lint/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Lint Skill.md`)
3. **Antigravity 용**:
   - `.agent/skills/ingest/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Ingest Skill.md`)
   - `.agent/skills/query/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Query Skill.md`)
   - `.agent/skills/lint/SKILL.md` (원본: `80_References/80.02_WIKI_installation/Lint Skill.md`)

---

## 15. 초기 Wiki 파일 생성

다음 파일이 없으면 생성하세요.

### 20_Wiki/index.md

```markdown
# LLM Wiki Index

## Overview
- [[overview]] — High-level summary of the Wiki.

## Sources

## Topics

## Concepts

## Entities

## Questions

## Syntheses

## Maintenance
```

### 20_Wiki/log.md

```markdown
# LLM Wiki Log

## [YYYY-MM-DD] setup | Initial Modular Rule Structure

- Operation: setup
- Created:
  - [[index]]
  - [[log]]
  - [[overview]]
- Updated:
  - None
- Notes:
  - Created initial LLM Wiki structure with modular rule files.
```

### 20_Wiki/overview.md

```markdown
---
type: overview
title: Wiki Overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - overview
---

# Wiki Overview

## Purpose

This Wiki is a persistent Markdown knowledge base maintained by an LLM.

## Major Topics

No major topics yet.

## Current Syntheses

No syntheses yet.

## Open Questions

No open questions yet.

## Next Steps

Add sources to `10_Raw/10.02_Inbox/` or `10_Raw/10.01_Clippings/` and run the ingest skill.
```

---

## 16. 완료 보고

작업을 마친 뒤 다음 형식으로 보고하세요.

```markdown
# LLM Wiki Operation Report

## Status
Complete / Partial / Failed

## Skill Used
None

## Rule Files Used
- CLAUDE.md
- AGENTS.md
- GEMINI.md
- 30_Rules/Core-Principles.md
- 30_Rules/Directory-Structure.md
- 30_Rules/Page-Types.md
- 30_Rules/Frontmatter.md
- 30_Rules/Naming-and-Linking.md
- 30_Rules/Index-and-Log.md
- 30_Rules/Source-Attribution.md
- 30_Rules/Contradictions-and-Uncertainty.md
- 30_Rules/Refactor-Policy.md
- 30_Rules/Operation-Reporting.md

## Created Files
- ...

## Updated Files
- ...

## Raw Sources Modified
No

## Key Decisions
- Kept CLAUDE.md, AGENTS.md, and GEMINI.md minimal.
- Moved detailed rules into modular files under 30_Rules/.
- Kept skills under project-local directories (.claude/skills, .codex/skills, .agent/skills), separate from global rules.

## Issues / Uncertainties
- ...

## Recommended Next Steps
1. Review the generated Ingest Skill from `80_References/80.02_WIKI_installation/Ingest Skill.md`.
2. Review the generated Query Skill from `80_References/80.02_WIKI_installation/Query Skill.md`.
3. Review the generated Lint Skill from `80_References/80.02_WIKI_installation/Lint Skill.md`.
```
