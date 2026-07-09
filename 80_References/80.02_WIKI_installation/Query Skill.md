---
name: query
description: Use this skill when answering questions from the existing LLM wiki.
---
# Query Skill

## Purpose

Use this skill when the user asks a question against the LLM Wiki.

The goal is to answer from the accumulated Wiki, not from raw memory alone.  
The Wiki is treated as the compiled knowledge layer between the user and the raw sources.

## When to Use This Skill

Use this skill when the user says things like:

- "Ask the wiki..."
    
- "Based on the wiki..."
    
- "What does the wiki say about..."
    
- "Compare these concepts from the wiki."
    
- "Find connections between..."
    
- "Wiki 기준으로 답변해줘."
    
- "이 주제에 대해 Wiki에서 찾아서 답해줘."
    

## Core Rules

1. Read `20_Wiki/index.md` first.
    
2. Use the index to identify relevant pages.
    
3. Read relevant Wiki pages before answering.
    
4. Use raw sources only when Wiki pages are insufficient or when source verification is needed.
    
5. Do not answer purely from general model memory if the Wiki contains relevant content.
    
6. Cite or mention the Wiki pages used.
    
7. If the answer produces reusable knowledge, save it as a question page or synthesis page when appropriate.
    
8. Append a query entry to `20_Wiki/log.md`.
    
9. Do not modify raw sources.
    
10. Do not silently resolve contradictions. Surface them.
    

## Query Workflow

### 1. Read Index

Start by reading:

```text
20_Wiki/index.md
```

Identify candidate pages under:

```text
20_Wiki/20.01_Sources/
20_Wiki/20.02_Concepts/
20_Wiki/20.03_Entities/
20_Wiki/20.04_Topics/
20_Wiki/20.05_Questions/
20_Wiki/20.06_Syntheses/
20_Wiki/20.07_Templates/
```

### 2. Select Relevant Pages

Choose pages likely to answer the user’s question.

Prioritize:

1. Synthesis pages
    
2. Topic pages
    
3. Concept pages
    
4. Entity pages
    
5. Source pages
    
6. Raw sources, only if needed
    

### 3. Read Relevant Pages

Read the selected pages.

Extract:

- direct answer
    
- supporting claims
    
- source references
    
- related concepts
    
- contradictions
    
- uncertainty
    
- missing information
    

### 4. Answer the User

Answer clearly and directly.

When useful, structure the answer as:

```markdown
## Answer

## Supporting Wiki Pages

## Key Evidence

## Contradictions / Uncertainty

## Recommended Next Step
```

Mention the Wiki pages used.

Example:

```markdown
Used Wiki pages:
- [[LLM Wiki]]
- [[RAG]]
- [[Persistent Knowledge Base]]
- [[LLM Wiki vs RAG]]
```

### 5. Decide Whether to Save the Answer

Save the answer if it is reusable.

Save as a Question Page when the output is mainly a direct answer to the user’s question.

Path:

```text
20_Wiki/20.05_Questions/
```

Save as a Synthesis Page when the output is a broader analysis, comparison, framework, or integrated insight.

Path:

```text
20_Wiki/20.06_Syntheses/
```

Do not save trivial one-off answers.

### 6. Question Page Format

If saving to `20_Wiki/20.05_Questions/`, use:

```markdown
---
type: question
title:
created:
updated:
status: answered
tags:
---

# Question Title

## Question

## Answer Summary

## Detailed Answer

## Supporting Pages

## Related Pages

## Resulting Actions
```

### 7. Synthesis Page Format

If saving to `20_Wiki/20.06_Syntheses/`, use:

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

### 8. Update index.md If New Page Is Created

If a new question or synthesis page is created, update:

```text
20_Wiki/index.md
```

Add the new page under the correct section.

Use this format:

```markdown
- [[Page Name]] — one-line summary.
```

### 9. Update log.md

Append a query entry to:

```text
20_Wiki/log.md
```

Use this format:

```markdown
## [YYYY-MM-DD] query | Query Title

- Operation: query
- Question:
  - User question here
- Pages Used:
  - [[Page Name]]
- Created:
  - [[Page Name]]
- Updated:
  - [[Page Name]]
- Notes:
  - ...
```

## Completion Report

After answering, report briefly:

```markdown
# Query Report

## Answer
...

## Wiki Pages Used
- ...

## Saved Back to Wiki
Yes / No

## Created Files
- ...

## Updated Files
- ...

## Uncertainty
- ...

## Recommended Next Steps
1. ...
2. ...
```

## Answering Standards

Prefer direct answers.

Use tables when comparing concepts, entities, sources, claims, or workflows.

Use bullet points for operational recommendations.

Mark missing information explicitly.

If the Wiki does not contain enough information, say so and recommend what source should be added next.

## Failure Handling

If `20_Wiki/index.md` is missing, inspect the `20_Wiki/` directory and rebuild a minimal index before answering.

If relevant pages are missing, answer from available material and recommend creating the missing pages.

If contradictions exist, present both sides and identify which source or page supports each claim.
