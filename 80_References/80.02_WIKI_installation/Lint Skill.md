---
name: lint
description: LLM Wiki의 상태를 검사, 정리, 개선할 때 이 스킬을 사용합니다.
---

# Lint 스킬 (Lint Skill)

## 목적 (Purpose)

LLM Wiki의 상태를 검사, 정리, 개선할 때 이 스킬을 사용합니다.

목표는 깨진 링크, 고아 페이지, 중복 개념, 오래된 주장, 누락된 페이지, 약한 소스 귀속, 구조적 문제를 찾아 Wiki가 성장함에 따라 일관성을 유지하는 것입니다.

## 이 스킬을 사용하는 경우 (When to Use This Skill)

사용자가 다음과 같이 말할 때 이 스킬을 사용하세요:

- "Wiki를 린트해줘."
- "Wiki 상태 점검해줘."
- "깨진 링크를 찾아줘."
- "고아 페이지를 찾아줘."
- "모순을 확인해줘."
- "Wiki를 정리해줘."
- "Wiki 상태 점검해줘."
- "중복 페이지 찾아줘."
- "링크 깨진 것 확인해줘."

## 핵심 규칙 (Core Rules)

1. 원본 소스를 수정하지 않습니다.
2. 사용자의 명시적인 승인 없이 Wiki 페이지를 삭제하지 않습니다.
3. 먼저 계획을 제안하지 않고 대규모 리팩토링을 수행하지 않습니다.
4. 파괴적인 변경 전에 먼저 문제를 보고하는 것을 선호합니다.
5. 매 lint 과정 후 `20_Wiki/log.md`를 업데이트합니다.
6. lint 보고서를 생성하는 경우 `20_Wiki/20.06_Syntheses/` 하위에 저장합니다.
7. 새 유지보수 페이지가 생성되면 `20_Wiki/index.md`를 업데이트합니다.
8. 명확한 이유가 없는 한 기존 내용을 보존합니다.
9. 모순을 숨기는 대신 드러냅니다.
10. 우선순위 순서로 다음 조치를 권장합니다.

## Lint 워크플로우 (Lint Workflow)

### 1. Wiki 구조 검사 (Inspect Wiki Structure)

다음 경로들이 존재하는지 확인합니다:

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

누락된 파일 또는 디렉터리를 보고합니다.

### 2. index.md 확인 (Check index.md)

다음 파일을 검사합니다:

```text
20_Wiki/index.md
```

다음 항목을 찾습니다:

- 인덱스에 나열되어 있으나 디스크에 없는 페이지
- 디스크에 있으나 인덱스에 없는 페이지
- 중복 인덱스 항목
- 잘못된 섹션에 있는 페이지
- 불명확한 한 줄 요약
- 오래된 항목

### 3. log.md 확인 (Check log.md)

다음 파일을 검사합니다:

```text
20_Wiki/log.md
```

다음 항목을 찾습니다:

- 최근 작업의 누락
- 일관성 없는 로그 헤딩
- 추가 전용(append-only) 원칙을 벗어난 편집
- 작업 유형이 없는 항목
- 생성 또는 수정된 파일이 없는 항목
- 비고가 없는 항목

예상 로그 형식:

```markdown
## [YYYY-MM-DD] 작업 유형 | 제목

- 작업 (Operation):
- 생성 (Created):
- 수정 (Updated):
- 비고 (Notes):
```

### 4. 깨진 링크 확인 (Check Broken Links)

Wiki 파일에서 Obsidian 스타일 링크를 스캔합니다:

```markdown
[[페이지 이름]]
```

기존 페이지로 연결되지 않는 링크를 감지합니다.

보고합니다:

```markdown
## 깨진 링크 (Broken Links)

- `[[누락된 페이지]]`
  - 발견 위치: path/to/file.md
  - 권장 조치: 페이지 생성 / 링크 이름 변경 / 링크 제거
```

### 5. 고아 페이지 확인 (Check Orphan Pages)

인바운드 링크가 없는 페이지를 찾습니다.

보고합니다:

```markdown
## 고아 페이지 (Orphan Pages)

- path/to/page.md
  - 권장 인바운드 링크:
    - [[관련 페이지]]
```

`index.md`, `log.md`, 또는 독립형 유지보수 보고서는 워크플로우에서 명확히 분리되어 있지 않는 한 문제가 있는 고아로 분류하지 않습니다.

### 6. 중복 또는 겹치는 페이지 확인 (Check Duplicate or Overlapping Pages)

동일한 개념을 다루는 것으로 보이는 페이지를 찾습니다.

예시:

- `rag.md`와 `retrieval-augmented-generation.md`
- `llm-wiki.md`와 `persistent-llm-wiki.md`
- `obsidian.md`와 `obsidian-tool.md`

보고합니다:

```markdown
## 중복 / 겹치는 페이지 (Duplicate / Overlapping Pages)

- 후보 그룹:
  - path/to/page-a.md
  - path/to/page-b.md
- 문제:
- 권장사항:
  - 병합 / 이름 변경 / 더 명확한 구분으로 별도 유지
```

### 7. 오래되거나 모순된 주장 확인 (Check Stale or Contradictory Claims)

페이지 간에 충돌하는 주장을 찾습니다.

보고합니다:

```markdown
## 모순 / 긴장 관계 (Contradictions / Tensions)

- 주장 A:
  - 페이지:
  - 증거:
- 주장 B:
  - 페이지:
  - 증거:
- 권장사항:
```

증거가 명확히 하나의 주장을 지지하지 않는 한 조용히 하나의 주장을 선택하지 않습니다.

### 8. 누락된 페이지 확인 (Check Missing Pages)

자주 언급되지만 페이지가 없는 개념, 엔티티, 도구 또는 토픽을 찾습니다.

보고합니다:

```markdown
## 생성할 누락된 페이지 (Missing Pages to Create)

- [[권장 페이지]]
  - 언급된 위치:
    - path/to/file.md
  - 권장 유형: concept/entity/topic/synthesis
  - 이유:
```

### 9. 소스 귀속 확인 (Check Source Attribution)

소스 페이지에 링크하지 않고 주장을 하는 Wiki 페이지를 찾습니다.

보고합니다:

```markdown
## 약한 소스 귀속 (Weak Source Attribution)

- path/to/page.md
  - 지원되지 않거나 약하게 지원된 주장:
  - 권장 소스:
```

### 10. 페이지 크기 및 구조 확인 (Check Page Size and Structure)

너무 크거나, 너무 작거나, 구조가 좋지 않은 페이지를 식별합니다.

보고합니다:

```markdown
## 구조적 문제 (Structural Issues)

- path/to/page.md
  - 문제:
  - 권장사항:
```

예시:

- 대형 페이지를 개념 페이지로 분할
- 작은 중복 페이지 병합
- 누락된 frontmatter 추가
- 관련 링크 추가
- 소스 섹션 추가
- 불명확한 제목 이름 변경

## Lint 보고서 출력 (Lint Report Output)

다음 경로에 lint 보고서를 생성합니다:

```text
20_Wiki/20.06_Syntheses/
```

파일명:

```text
wiki-health-check-YYYY-MM-DD.md
```

다음 형식을 사용합니다:

```markdown
---
type: synthesis
title: Wiki 상태 점검 YYYY-MM-DD
created:
updated:
tags:
  - maintenance
  - lint
---

# Wiki 상태 점검 YYYY-MM-DD (Wiki Health Check YYYY-MM-DD)

## 요약 (Summary)

## 누락된 핵심 파일 또는 디렉터리 (Missing Core Files or Directories)

## 인덱스 문제 (Index Issues)

## 로그 문제 (Log Issues)

## 깨진 링크 (Broken Links)

## 고아 페이지 (Orphan Pages)

## 중복 / 겹치는 페이지 (Duplicate / Overlapping Pages)

## 모순 / 긴장 관계 (Contradictions / Tensions)

## 생성할 누락된 페이지 (Missing Pages to Create)

## 약한 소스 귀속 (Weak Source Attribution)

## 구조적 문제 (Structural Issues)

## 권장 다음 조치 (Recommended Next Actions)
```

## 심각도 수준 (Severity Levels)

각 문제를 다음과 같이 분류합니다:

```text
심각 (Critical)
높음 (High)
중간 (Medium)
낮음 (Low)
```

다음 의미를 사용합니다:

- 심각: Wiki 사용을 방해하거나 심각한 잘못된 정보를 초래하는 경우
- 높음: 깨진 링크, 누락된 인덱스, 주요 모순
- 중간: 고아 페이지, 약한 소스 귀속, 중복 페이지
- 낮음: 서식, 명명, 사소한 구성 문제

## 선택적 수정 모드 (Optional Fix Mode)

기본적으로 lint는 문제를 보고하기만 해야 합니다.

사용자가 명시적으로 수정을 요청하는 경우에만 수정을 수행합니다.

추가 확인 없이 허용되는 안전한 수정:

- `20_Wiki/index.md` 업데이트
- 현재 lint 과정에 대한 누락된 로그 항목 추가
- 명확히 식별 가능한 페이지에 누락된 frontmatter 추가
- lint 보고서 생성

확인이 필요한 수정:

- 페이지 삭제
- 페이지 병합
- 페이지 이름 변경
- 폴더 간 페이지 이동
- 주요 섹션 재작성
- 주장을 교체하여 모순 해결

## index.md 업데이트 (Update index.md)

lint 보고서가 생성된 경우 `20_Wiki/index.md`의 유지보수 또는 합성 섹션에 추가합니다.

예시:

```markdown
- [[Wiki 상태 점검 YYYY-MM-DD]] — 깨진 링크, 고아 페이지, 중복 페이지, 오래된 주장을 다루는 유지보수 보고서.
```

## log.md 업데이트 (Update log.md)

다음 항목을 추가합니다:

```markdown
## [YYYY-MM-DD] lint | Wiki 상태 점검

- 작업 (Operation): lint
- 생성 (Created):
  - [[Wiki 상태 점검 YYYY-MM-DD]]
- 수정 (Updated):
  - [[index]]
  - [[log]]
- 비고 (Notes):
  - 깨진 링크, 고아 페이지, 중복 페이지, 모순, 누락된 페이지, 소스 귀속을 확인했습니다.
```

## 완료 보고 (Completion Report)

린팅 후 다음과 같이 보고합니다:

```markdown
# Lint 보고서 (Lint Report)

## 상태 (Status)
완료 / 부분 완료 / 실패

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 심각 문제 (Critical Issues)
- ...

## 높은 우선순위 문제 (High Priority Issues)
- ...

## 권장 다음 조치 (Recommended Next Actions)
1. ...
2. ...
3. ...
```

## 오류 처리 (Failure Handling)

Wiki가 너무 큰 경우 단계적으로 lint를 수행합니다:

1. 구조 및 인덱스
2. 링크
3. 중복
4. 모순
5. 소스 귀속

파일을 읽을 수 없는 경우 보고합니다.

Wiki가 아직 존재하지 않는 경우 먼저 설치 워크플로우를 실행하도록 권장합니다.
