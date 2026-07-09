---
name: ingest
description: 사용자가 새로운 소스를 LLM Wiki에 수집, 처리, 가져오기, 요약, 또는 통합 요청 시 이 스킬을 사용합니다.
---

# Ingest 스킬 (Ingest Skill)

## 목적 (Purpose)

사용자가 새로운 소스를 LLM Wiki에 수집, 처리, 가져오기, 요약, 또는 통합 요청 시 이 스킬을 사용합니다.

수집의 목표는 단순히 소스를 요약하는 것이 아닙니다.  
목표는 소스 페이지, 개념 페이지, 엔티티 페이지, 토픽 페이지, 합성 페이지, 인덱스 항목, 로그 항목을 생성 및 업데이트하여 소스를 지속적인 Markdown Wiki에 통합하는 것입니다.

## 이 스킬을 사용하는 경우 (When to Use This Skill)

사용자가 다음과 같이 말할 때 이 스킬을 사용하세요:

- "이 소스를 수집해줘."
- "새 문서를 처리해줘."
- "이 기사를 Wiki에 추가해줘."
- "10_Raw/10.02_Inbox의 파일을 가져와줘."
- "이 논문을 읽고 Wiki를 업데이트해줘."
- "이 문서를 Wiki에 반영해줘."
- "새 자료를 LLM Wiki에 넣어줘."

## 핵심 규칙 (Core Rules)

1. `10_Raw/` 하위의 파일을 절대 수정하지 않습니다.
2. 원본 소스는 불변의 진실의 원천 문서로 취급합니다.
3. 명시적으로 지시받지 않는 한 `20_Wiki/` 하위에만 Markdown 파일을 생성하거나 업데이트합니다.
4. 생성 또는 업데이트된 모든 Wiki 페이지는 YAML frontmatter를 사용해야 합니다.
5. Obsidian 스타일 링크를 사용합니다: `[[페이지 이름]]`.
6. 소스에 근거하지 않은 주장을 만들어내지 않습니다.
7. 불확실한 주장은 명시적으로 표시합니다.
8. 새 소스가 기존 Wiki 내용과 충돌하는 경우, 이전 주장을 조용히 덮어쓰는 대신 충돌을 기록합니다.
9. 매 수집 후 `20_Wiki/index.md`를 업데이트합니다.
10. 매 수집 후 `20_Wiki/log.md`에 항목을 추가합니다.

## 예상 디렉터리 구조 (Expected Directory Structure)

다음 구조를 가정합니다:

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

예상 구조가 없는 경우, 계속하기 전에 누락된 디렉터리와 파일을 생성합니다.

## Ingest 워크플로우 (Ingest Workflow)

다음 순서를 따르세요.

### 1. 소스 위치 확인 (Locate Source)

사용자가 제공한 소스 위치를 확인합니다.

특정 파일이 지정되지 않은 경우 다음을 검사합니다:

```text
10_Raw/10.02_Inbox/
10_Raw/10.01_Clippings/
```

미처리 소스를 식별합니다.

### 2. 소스 읽기 (Read Source)

소스를 꼼꼼히 읽습니다.

다음 항목을 추출합니다:

- 제목
- 가능한 경우 저자 또는 출처
- 가능한 경우 날짜
- 소스 유형
- 주요 논거
- 핵심 주장
- 중요한 사실
- 정의
- 예시
- 엔티티
- 도구
- 개념
- 긴장 관계 또는 모순
- 미해결 질문
- 가능한 후속 토픽

### 3. 소스 페이지 생성 (Create Source Page)

다음 경로에 소스 요약 페이지를 생성합니다:

```text
20_Wiki/20.01_Sources/
```

소문자 케밥 케이스 파일명을 사용합니다.

예시:

```text
20_Wiki/20.01_Sources/2026-07-07-llm-wiki-pattern.md
```

다음 형식을 사용합니다:

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

# 소스 제목

## 요약 (Summary)

## 핵심 주장 (Key Claims)

## 중요한 세부 정보 (Important Details)

## 증거 (Evidence)

## 관련 개념 (Related Concepts)

## 관련 엔티티 (Related Entities)

## 관련 토픽 (Related Topics)

## 모순 / 긴장 관계 (Contradictions / Tensions)

## 미해결 질문 (Open Questions)

## 소스 비고 (Source Notes)
```

### 4. 개념 페이지 생성 또는 업데이트 (Create or Update Concept Pages)

각 중요한 개념에 대해 다음 경로에 페이지를 생성하거나 업데이트합니다:

```text
20_Wiki/20.02_Concepts/
```

개념 예시:

- RAG
- 지속적 지식 베이스 (Persistent Knowledge Base)
- Wiki 수집 (Wiki Ingestion)
- Wiki 린팅 (Wiki Linting)
- 지식 복합화 (Knowledge Compounding)
- 교차 참조 유지보수 (Cross-Reference Maintenance)

다음 형식을 사용합니다:

```markdown
---
type: concept
title:
created:
updated:
aliases:
tags:
---

# 개념 이름

## 정의 (Definition)

## 중요성 (Why It Matters)

## 핵심 포인트 (Key Points)

## 관련 개념 (Related Concepts)

## 관련 소스 (Related Sources)

## 미해결 질문 (Open Questions)
```

### 5. 엔티티 페이지 생성 또는 업데이트 (Create or Update Entity Pages)

각 인물, 조직, 제품, 도구, 시스템, 위치 또는 명명된 객체에 대해 다음 경로에 페이지를 생성하거나 업데이트합니다:

```text
20_Wiki/20.03_Entities/
```

엔티티 예시:

- Obsidian
- NotebookLM
- ChatGPT
- qmd
- Marp
- Dataview
- Obsidian Web Clipper

다음 형식을 사용합니다:

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

# 엔티티 이름

## 설명 (Description)

## 역할 / 관련성 (Role / Relevance)

## 핵심 세부 정보 (Key Details)

## 관련 개념 (Related Concepts)

## 관련 소스 (Related Sources)

## 미해결 질문 (Open Questions)
```

### 6. 토픽 페이지 생성 또는 업데이트 (Create or Update Topic Pages)

더 넓은 주제 영역에 대해 다음 경로에 페이지를 생성하거나 업데이트합니다:

```text
20_Wiki/20.04_Topics/
```

토픽 예시:

- 개인 지식 관리 (Personal Knowledge Management)
- LLM 보조 연구 (LLM-Assisted Research)
- Markdown Wiki 워크플로우 (Markdown Wiki Workflow)
- 내부 팀 지식 베이스 (Internal Team Knowledge Bases)

다음 형식을 사용합니다:

```markdown
---
type: topic
title:
created:
updated:
tags:
---

# 토픽 이름

## 개요 (Overview)

## 핵심 개념 (Key Concepts)

## 중요한 소스 (Important Sources)

## 관련 엔티티 (Related Entities)

## 현재 합성 (Current Synthesis)

## 미해결 질문 (Open Questions)
```

### 7. 합성 페이지 생성 또는 업데이트 (Create or Update Synthesis Pages)

수집 과정에서 유용한 교차 소스 비교, 논거 또는 통합된 인사이트가 생성되는 경우 다음 경로에 페이지를 생성하거나 업데이트합니다:

```text
20_Wiki/20.06_Syntheses/
```

다음 형식을 사용합니다:

```markdown
---
type: synthesis
title:
created:
updated:
tags:
---

# 합성 제목

## 핵심 논거 (Core Argument)

## 지원 증거 (Supporting Evidence)

## 비교 / 분석 (Comparison / Analysis)

## 시사점 (Implications)

## 관련 소스 (Related Sources)

## 관련 개념 (Related Concepts)

## 미해결 질문 (Open Questions)
```

### 8. 링크 업데이트 (Update Links)

페이지 전체에 Obsidian 스타일 링크를 추가합니다.

예시:

```markdown
- [[RAG]]
- [[LLM Wiki]]
- [[Persistent Knowledge Base]]
- [[Obsidian]]
```

모든 소스 페이지는 관련 개념, 엔티티, 토픽 페이지에 링크해야 합니다.

모든 개념, 엔티티, 토픽 페이지는 관련 소스 페이지로 역링크해야 합니다.

### 9. index.md 업데이트 (Update index.md)

다음 파일을 업데이트합니다:

```text
20_Wiki/index.md
```

다음 섹션들을 유지합니다:

```markdown
# LLM Wiki 인덱스

## 개요

## 소스

## 토픽

## 개념

## 엔티티

## 질문

## 합성

## 유지보수
```

각 항목은 다음 형식을 사용합니다:

```markdown
- [[페이지 이름]] — 한 줄 요약.
```

### 10. log.md 업데이트 (Update log.md)

다음 파일에 새 항목을 추가합니다:

```text
20_Wiki/log.md
```

다음 형식을 사용합니다:

```markdown
## [YYYY-MM-DD] ingest | 소스 제목

- 작업 (Operation): ingest
- 소스 (Source):
  - 10_Raw/10.01_Clippings/example.md
- 생성 (Created):
  - [[페이지 이름]]
- 수정 (Updated):
  - [[페이지 이름]]
- 비고 (Notes):
  - ...
```

이전 로그 항목을 삭제하거나 재작성하지 않습니다.

## 완료 보고 (Completion Report)

수집 후 다음과 같이 보고합니다:

```markdown
# Ingest 보고서 (Ingest Report)

## 상태 (Status)
완료 / 부분 완료 / 실패

## 처리된 소스 (Source Processed)
- ...

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 핵심 인사이트 (Key Takeaways)
- ...

## 모순 / 긴장 관계 (Contradictions / Tensions)
- ...

## 미해결 질문 (Open Questions)
- ...

## 권장 다음 단계 (Recommended Next Steps)
1. ...
2. ...
3. ...
```

## 오류 처리 (Failure Handling)

소스를 읽을 수 없는 경우 문제를 명확히 보고합니다.

소스가 너무 큰 경우 섹션별로 처리합니다.

소스에 이미지가 포함된 경우 먼저 텍스트를 처리한 후 가능한 경우 참조된 이미지를 검사합니다.

기존 Wiki 구조가 일관성이 없는 경우 기존 내용을 보존하고 주요 변경 전에 불일치를 보고합니다.
