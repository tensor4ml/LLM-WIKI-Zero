---
name: query
description: 기존 LLM Wiki에서 질문에 답할 때 이 스킬을 사용합니다.
---
# Query 스킬 (Query Skill)

## 목적 (Purpose)

사용자가 LLM Wiki에 대한 질문을 할 때 이 스킬을 사용합니다.

목표는 원시 메모리만으로 답하는 것이 아니라 축적된 Wiki에서 답하는 것입니다.  
Wiki는 사용자와 원본 소스 사이의 컴파일된 지식 레이어로 취급됩니다.

## 이 스킬을 사용하는 경우 (When to Use This Skill)

사용자가 다음과 같이 말할 때 이 스킬을 사용하세요:

- "Wiki에서 물어봐..."
- "Wiki를 기반으로..."
- "Wiki가 ...에 대해 뭐라고 하나요?"
- "Wiki에서 이 개념들을 비교해줘."
- "...사이의 연결고리를 찾아줘."
- "Wiki 기준으로 답변해줘."
- "이 주제에 대해 Wiki에서 찾아서 답해줘."

## 핵심 규칙 (Core Rules)

1. 먼저 `20_Wiki/index.md`를 읽습니다.
2. 인덱스를 사용하여 관련 페이지를 식별합니다.
3. 답변 전에 관련 Wiki 페이지를 읽습니다.
4. Wiki 페이지가 충분하지 않거나 소스 검증이 필요한 경우에만 원본 소스를 사용합니다.
5. Wiki에 관련 내용이 있는 경우 일반 모델 메모리만으로 답하지 않습니다.
6. 사용한 Wiki 페이지를 인용하거나 언급합니다.
7. 답변이 재사용 가능한 지식을 생성하는 경우, 적절하면 질문 페이지 또는 합성 페이지로 저장합니다.
8. `20_Wiki/log.md`에 쿼리 항목을 추가합니다.
9. 원본 소스를 수정하지 않습니다.
10. 모순을 조용히 해결하지 않습니다. 드러냅니다.

## Query 워크플로우 (Query Workflow)

### 1. 인덱스 읽기 (Read Index)

다음 파일을 읽는 것으로 시작합니다:

```text
20_Wiki/index.md
```

다음 경로 하에서 후보 페이지를 식별합니다:

```text
20_Wiki/20.01_Sources/
20_Wiki/20.02_Concepts/
20_Wiki/20.03_Entities/
20_Wiki/20.04_Topics/
20_Wiki/20.05_Questions/
20_Wiki/20.06_Syntheses/
20_Wiki/20.07_Templates/
```

### 2. 관련 페이지 선택 (Select Relevant Pages)

사용자의 질문에 답할 가능성이 높은 페이지를 선택합니다.

다음 순서로 우선순위를 정합니다:

1. 합성 페이지 (Synthesis pages)
2. 토픽 페이지 (Topic pages)
3. 개념 페이지 (Concept pages)
4. 엔티티 페이지 (Entity pages)
5. 소스 페이지 (Source pages)
6. 필요한 경우에만 원본 소스 (Raw sources)

### 3. 관련 페이지 읽기 (Read Relevant Pages)

선택한 페이지를 읽습니다.

다음 항목을 추출합니다:

- 직접적인 답변
- 지원 주장
- 소스 참조
- 관련 개념
- 모순
- 불확실성
- 누락된 정보

### 4. 사용자에게 답변하기 (Answer the User)

명확하고 직접적으로 답변합니다.

유용한 경우 다음과 같이 구조화합니다:

```markdown
## 답변 (Answer)

## 지원 Wiki 페이지 (Supporting Wiki Pages)

## 핵심 증거 (Key Evidence)

## 모순 / 불확실성 (Contradictions / Uncertainty)

## 권장 다음 단계 (Recommended Next Step)
```

사용한 Wiki 페이지를 언급합니다.

예시:

```markdown
사용한 Wiki 페이지:
- [[LLM Wiki]]
- [[RAG]]
- [[Persistent Knowledge Base]]
- [[LLM Wiki vs RAG]]
```

### 5. 답변 저장 여부 결정 (Decide Whether to Save the Answer)

재사용 가능한 경우 답변을 저장합니다.

출력이 주로 사용자 질문에 대한 직접적인 답변인 경우 질문 페이지로 저장합니다.

경로:

```text
20_Wiki/20.05_Questions/
```

출력이 더 넓은 분석, 비교, 프레임워크 또는 통합된 인사이트인 경우 합성 페이지로 저장합니다.

경로:

```text
20_Wiki/20.06_Syntheses/
```

사소한 일회성 답변은 저장하지 않습니다.

### 6. 질문 페이지 형식 (Question Page Format)

`20_Wiki/20.05_Questions/`에 저장할 경우 다음을 사용합니다:

```markdown
---
type: question
title:
created:
updated:
status: answered
tags:
---

# 질문 제목

## 질문 (Question)

## 답변 요약 (Answer Summary)

## 상세 답변 (Detailed Answer)

## 지원 페이지 (Supporting Pages)

## 관련 페이지 (Related Pages)

## 후속 조치 (Resulting Actions)
```

### 7. 합성 페이지 형식 (Synthesis Page Format)

`20_Wiki/20.06_Syntheses/`에 저장할 경우 다음을 사용합니다:

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

### 8. 새 페이지가 생성된 경우 index.md 업데이트 (Update index.md If New Page Is Created)

새 질문 또는 합성 페이지가 생성된 경우 다음을 업데이트합니다:

```text
20_Wiki/index.md
```

올바른 섹션 하에 새 페이지를 추가합니다.

다음 형식을 사용합니다:

```markdown
- [[페이지 이름]] — 한 줄 요약.
```

### 9. log.md 업데이트 (Update log.md)

다음 파일에 쿼리 항목을 추가합니다:

```text
20_Wiki/log.md
```

다음 형식을 사용합니다:

```markdown
## [YYYY-MM-DD] query | 쿼리 제목

- 작업 (Operation): query
- 질문 (Question):
  - 사용자 질문 내용
- 사용한 페이지 (Pages Used):
  - [[페이지 이름]]
- 생성 (Created):
  - [[페이지 이름]]
- 수정 (Updated):
  - [[페이지 이름]]
- 비고 (Notes):
  - ...
```

## 완료 보고 (Completion Report)

답변 후 간략하게 보고합니다:

```markdown
# Query 보고서 (Query Report)

## 답변 (Answer)
...

## 사용한 Wiki 페이지 (Wiki Pages Used)
- ...

## Wiki에 저장 여부 (Saved Back to Wiki)
예 / 아니오

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 불확실성 (Uncertainty)
- ...

## 권장 다음 단계 (Recommended Next Steps)
1. ...
2. ...
```

## 답변 기준 (Answering Standards)

직접적인 답변을 우선합니다.

개념, 엔티티, 소스, 주장 또는 워크플로우를 비교할 때는 표를 사용합니다.

운영 권장사항에는 글머리 기호를 사용합니다.

누락된 정보는 명시적으로 표시합니다.

Wiki에 충분한 정보가 없는 경우, 그렇다고 말하고 다음에 추가해야 할 소스를 권장합니다.

## 오류 처리 (Failure Handling)

`20_Wiki/index.md`가 없는 경우 답변 전에 `20_Wiki/` 디렉터리를 검사하고 최소한의 인덱스를 재구성합니다.

관련 페이지가 없는 경우 사용 가능한 자료로 답변하고 누락된 페이지 생성을 권장합니다.

모순이 존재하는 경우 양측을 모두 제시하고 각 주장을 지원하는 소스 또는 페이지를 식별합니다.
