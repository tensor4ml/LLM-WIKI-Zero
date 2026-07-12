# LLM Wiki Skills Usage Guide

이 문서는 LLM Wiki v2 Framework의 핵심 스킬인 `ingest`, `lint`, `query`의 사용법을 설명합니다.

---

# 1. 기본 개념

세 스킬의 역할은 다음과 같습니다.

| 스킬 | 목적 |
|---|---|
| `$ingest` | 새로운 원본 자료를 `20_Wiki/`에 통합 |
| `$query` | 기존 Wiki를 검색하여 질문에 답변 |
| `$lint` | Wiki 구조, 링크, 중복, 모순 등을 점검 |

권장 작업 흐름은 다음과 같습니다.

```text
10_Raw에 자료 추가
        ↓
$ingest
        ↓
20_Wiki 생성 및 갱신
        ↓
$query
        ↓
Wiki 기반 질의 및 분석
        ↓
$lint
        ↓
구조와 품질 점검
```

---

# 2. Ingest Skill

## 2.1 목적

`$ingest`는 `10_Raw/`의 원본 자료를 읽고 다음 작업을 수행합니다.

- Source 페이지 생성
- Concept 페이지 생성 또는 갱신
- Entity 페이지 생성 또는 갱신
- Topic 페이지 생성 또는 갱신
- Question 페이지 생성
- 필요한 경우 Synthesis 페이지 생성
- Obsidian 링크 추가
- `index.md` 갱신
- `log.md` 갱신
- `_workspace/ingest-state.json` 갱신

기본 원칙은 전체 Wiki를 다시 읽는 것이 아니라 새로 추가되거나 변경된 자료만 처리하는 것입니다.

---

## 2.2 Default Mode

특정 원본 파일을 처리합니다.

```text
$ingest 10_Raw/10.02_Inbox/example.md
```

PDF도 동일하게 사용할 수 있습니다.

```text
$ingest 10_Raw/10.02_Inbox/example.pdf
```

수행 내용:

- 지정된 파일만 읽음
- Source 페이지 생성
- 관련 Concept, Entity, Topic 페이지 생성 또는 갱신
- 관련 링크 추가
- 새 페이지만 `index.md`에 추가
- 실제 변경이 있으면 `log.md` 갱신

추천 상황:

- 논문 한 편 추가
- 웹 문서 한 개 추가
- 특정 파일만 Wiki에 반영

---

## 2.3 Inbox Mode

다음 폴더의 미처리 또는 변경 파일을 처리합니다.

```text
10_Raw/10.02_Inbox/
```

실행:

```text
$ingest inbox
```

또는:

```text
$ingest 10_Raw/10.02_Inbox/
```

특징:

- `_workspace/ingest-state.json`과 파일 해시를 비교
- 이미 처리했고 변경되지 않은 파일은 건너뜀
- 한 번에 최대 5개 처리
- 나머지는 Deferred로 보고

추천 상황:

- 새로 받은 자료를 Inbox에 모아두고 일괄 처리
- 하루 단위로 새로운 자료 정리

---

## 2.4 Clippings Mode

다음 폴더의 웹 클리핑 자료를 처리합니다.

```text
10_Raw/10.01_Clippings/
```

실행:

```text
$ingest clippings
```

또는:

```text
$ingest 10_Raw/10.01_Clippings/
```

특징:

- Inbox Mode와 같은 증분 처리 방식
- 메뉴, 광고, 쿠키 배너, 반복 헤더 등 보일러플/레이트 제외
- 웹 문서의 제목, 저자, 발행일, URL 등 메타데이터 우선 보존
- 원문을 길게 복사하지 않고 요약과 핵심 근거만 저장

추천 상황:

- Obsidian Web Clipper 자료 처리
- 기사, 블로그, 웹페이지 클리핑 정리

---

## 2.5 Changed Mode

Git에서 새로 추가되거나 수정된 원본만 처리합니다.

```text
$ingest changed
```

내부적으로 다음과 같은 변경 파일을 찾습니다.

```bash
git diff --name-only --diff-filter=ACMR -- 10_Raw
git diff --cached --name-only --diff-filter=ACMR -- 10_Raw
git ls-files --others --exclude-standard -- 10_Raw
```

추천 상황:

- Git 기반으로 Wiki를 관리
- 최근 수정된 자료만 다시 반영
- 정기적인 증분 ingest

---

## 2.6 Source-Only Mode

Source 페이지만 생성하거나 갱신합니다.

```text
$ingest source-only 10_Raw/10.02_Inbox/example.pdf
```

생성하지 않는 항목:

- Concept
- Entity
- Topic
- Question
- Synthesis

대신 생성 후보만 Completion Report에 표시합니다.

추천 상황:

- 많은 자료를 먼저 빠르게 적재
- 나중에 Deep Mode로 통합
- 토큰 사용량을 최소화하고 싶을 때

---

## 2.7 Deep Mode

원본 자료를 기존 Wiki와 더 깊게 연결합니다.

```text
$ingest deep 10_Raw/10.02_Inbox/example.pdf
```

수행 가능한 작업:

- 기존 Concept과 비교
- 기존 Topic과 통합
- 모순과 차이점 탐지
- Source Attribution 점검
- Synthesis 생성 또는 갱신
- 관련 Question 생성

주의:

- 가장 많은 토큰을 사용
- 전체 Wiki를 무제한 스캔하지 않음
- 명시적으로 요청한 경우에만 사용

추천 상황:

- 핵심 논문
- 중요한 보고서
- 기존 Wiki 구조를 바꿀 정도로 중요한 자료
- 여러 Source를 연결하는 분석

---

## 2.8 Ingest 사용 예시

### 논문 한 편 일반 처리

```text
$ingest 10_Raw/10.02_Inbox/paper.pdf
```

### 웹 클리핑 처리

```text
$ingest clippings
```

### 새 파일 전체 처리

```text
$ingest inbox
```

### Source 페이지만 생성

```text
$ingest source-only 10_Raw/10.02_Inbox/article.md
```

### 핵심 논문 심층 통합

```text
$ingest deep 10_Raw/10.02_Inbox/important-paper.pdf
```

---

# 3. Query Skill

## 3.1 목적

`$query`는 기존 `20_Wiki/`를 검색하여 답변합니다.

기본적으로 다음 순서로 관련 페이지를 찾습니다.

1. 파일명
2. `title`
3. `aliases`
4. `tags`
5. Wiki 링크
6. `rg` 키워드 검색
7. 좁혀진 후보에 대한 의미 분석

전체 `index.md`나 전체 Wiki를 처음부터 읽지 않습니다.

---

## 3.2 Answer Mode

기본 질의 모드입니다.

```text
$query RAG와 Persistent Knowledge Base의 차이는 무엇인가?
```

또는:

```text
$query Wiki 기준으로 Graphify의 역할을 설명해줘
```

특징:

- 관련 후보 최대 10개
- 전체 본문을 읽는 페이지는 기본 최대 5개
- 답변 후 자동 저장하지 않음
- `log.md`를 갱신하지 않음

추천 상황:

- 개념 설명
- 특정 주제에 대한 Wiki 기반 답변
- 단순 질문

---

## 3.3 Compare Mode

두 개 이상의 개념, 도구, Source를 비교합니다.

```text
$query compare RAG and LLM Wiki
```

또는:

```text
$query Obsidian과 NotebookLM을 Wiki 기준으로 비교해줘
```

특징:

- 비교 대상에 직접 관련된 페이지만 읽음
- 표 형식 우선
- 차이점, 공통점, 장단점, 적용 상황 정리
- 모순 또는 범위 차이 표시

추천 상황:

- 기술 비교
- 방법론 비교
- Source 간 주장 비교
- 도구 선택

---

## 3.4 Explore Mode

개념 간 연결과 관련 주제를 탐색합니다.

```text
$query explore Knowledge Compounding
```

또는:

```text
$query LLM Wiki와 연결된 개념과 질문을 찾아줘
```

출력 내용:

- 관련 Concept
- 관련 Entity
- 관련 Topic
- 연결 관계
- Open Questions
- Wiki의 지식 공백

추천 상황:

- 새로운 연구 주제 탐색
- Wiki 구조 파악
- 연관 개념 발견
- 후속 질문 생성

---

## 3.5 Source Verification Mode

Wiki의 주장을 Source 페이지 또는 Raw Source로 검증합니다.

```text
$query source "Graphify는 의미 기반 관계를 자동 추출한다"
```

또는:

```text
$query 이 주장에 대한 근거 Source를 찾아줘
```

특징:

- 먼저 `20_Wiki/20.01_Sources/` 확인
- Wiki 근거가 부족할 때만 Raw Source 확인
- 확인된 내용과 확인되지 않은 내용을 구분

추천 상황:

- 주장 검증
- 출처 확인
- 논문 근거 확인
- Wiki 내용의 신뢰도 점검

---

## 3.6 Deep Mode

여러 페이지를 종합한 심층 분석을 수행합니다.

```text
$query deep LLM Wiki와 RAG의 관계를 분석해줘
```

가능한 작업:

- 여러 Source 비교
- 모순 분석
- 근거 수준 비교
- 통합 프레임워크 작성
- 불확실성 분석

주의:

- 명시적으로 `deep`을 요청할 때만 실행
- 기본 최대 10개 관련 페이지
- 가장 많은 토큰을 사용

추천 상황:

- 연구 분석
- 전략적 비교
- 복수 Source 종합
- Synthesis 후보 생성

---

## 3.7 Save Mode

Query 결과를 Wiki에 저장합니다.

```text
$query RAG와 LLM Wiki의 차이를 설명해줘. 결과를 Wiki에 저장해줘.
```

직접 답변은 Question 페이지로 저장합니다.

```text
20_Wiki/20.05_Questions/
```

넓은 비교나 통합 분석은 Synthesis 페이지로 저장합니다.

```text
20_Wiki/20.06_Syntheses/
```

저장 시에만:

- `index.md` 갱신
- `log.md` 갱신

추천 상황:

- 반복적으로 참고할 답변
- 연구 결과
- 비교 분석
- 팀 공유가 필요한 지식

---

## 3.8 Query 사용 예시

### 일반 질문

```text
$query Wiki 기준으로 RAG를 설명해줘
```

### 비교

```text
$query compare Graphify and traditional keyword search
```

### 관계 탐색

```text
$query explore Persistent Knowledge Base
```

### 근거 확인

```text
$query source 이 주장에 연결된 Source를 보여줘
```

### 심층 분석

```text
$query deep LLM Wiki, RAG, Graphify의 역할 차이를 분석해줘
```

### 결과 저장

```text
$query 위 분석을 Synthesis 페이지로 저장해줘
```

---

# 4. Lint Skill

## 4.1 목적

`$lint`는 Wiki의 구조와 품질을 점검합니다.

주요 검사 항목:

- 필수 폴더와 파일
- Frontmatter
- Index
- Log
- Broken Links
- Orphan Pages
- Duplicate Pages
- Contradictions
- Missing Pages
- Source Attribution
- Page Structure

기본 모드는 저비용 정적 검사를 수행하며, 의미 분석은 Deep Mode에서만 수행합니다.

---

## 4.2 Fast Mode

기본 경량 검사입니다.

```text
$lint fast
```

또는 변경 파일이 없을 때:

```text
$lint
```

검사 항목:

- 필수 경로
- Frontmatter
- Index 일관성
- 최근 Log 형식
- Broken Links
- Orphan Pages
- Heading 구조
- 페이지 크기 후보

하지 않는 것:

- 의미 기반 중복 분석
- 모순 분석
- 오래된 주장 분석
- 주장 단위 Source 검증

추천 상황:

- 일상적인 Wiki 점검
- Ingest 이후 빠른 확인
- Git Commit 전 확인

---

## 4.3 Changed Mode

변경된 Wiki 파일만 점검합니다.

```text
$lint changed
```

기본 `$lint` 실행 시 Git 변경 파일이 있으면 자동으로 Changed Mode가 선택됩니다.

검사 항목:

- 변경된 파일의 Frontmatter
- 새로 생긴 Broken Link
- Index 영향
- Folder와 Type 정합성
- Heading 구조
- Source Section 구조
- 직접 연결된 페이지 최대 3개

추천 상황:

- 작업 후 즉시 검증
- Pull Request 전 점검
- 토큰을 최소화한 품질 검사

---

## 4.4 Links Mode

링크 관련 문제만 검사합니다.

```text
$lint links
```

검사 항목:

- Broken Links
- Ambiguous Links
- Orphan Pages
- Duplicate Aliases
- Index Links

지원 링크 형식:

```text
[[Page]]
[[Page|Alias]]
[[Page#Heading]]
[[Page#Heading|Alias]]
```

추천 상황:

- Obsidian 그래프가 비정상적으로 보일 때
- 문서 이동 또는 이름 변경 후
- Orphan Page 확인
- Graphify 실행 전

---

## 4.5 Structure Mode

폴더, 메타데이터, Index, Log, Heading 구조를 검사합니다.

```text
$lint structure
```

검사 항목:

- 필수 파일과 폴더
- YAML Frontmatter
- Filename 규칙
- Page Type과 Folder의 일치
- Index 누락 및 중복
- 최근 Log 형식
- Heading 구조
- Page Size 후보
- Source Section 존재 여부

추천 상황:

- Wiki 구조를 변경한 후
- 새로운 Folder 규칙을 적용한 후
- Template을 변경한 후

---

## 4.6 Scoped Mode

특정 파일 또는 폴더만 검사합니다.

```text
$lint 20_Wiki/20.02_Concepts/
```

또는:

```text
$lint 20_Wiki/20.04_Topics/supply-chain.md
```

다른 모드와 결합할 수 있습니다.

```text
$lint links 20_Wiki/20.02_Concepts/
```

```text
$lint deep 20_Wiki/20.04_Topics/supply-chain.md
```

특징:

- 지정 경로가 Hard Boundary
- 전체 Wiki로 범위를 확장하지 않음
- 필요한 관련 페이지 최대 3개만 추가 확인

추천 상황:

- 특정 폴더 리팩터링 후
- 한 문서만 점검
- 토큰을 최소화한 집중 검사

---

## 4.7 Deep Mode

의미 기반 품질 검사를 수행합니다.

```text
$lint deep
```

가능한 검사:

- Duplicate / Overlapping Pages
- Contradictions / Tensions
- Stale Claims
- Missing Pages
- Claim-Level Source Attribution
- Semantic Structure Review

주의:

- 가장 많은 토큰 사용
- 후보를 먼저 정적 검색으로 좁힘
- 최대 중복 후보 10쌍
- 최대 모순 후보 5그룹
- 결과가 크면 Partial로 종료

추천 상황:

- 주기적인 Wiki 품질 감사
- 중요한 연구 결과 정리 전
- 여러 Source가 충돌할 가능성이 있을 때
- 중복 Concept가 늘어났을 때

---

## 4.8 Lint 사용 예시

### 일상 점검

```text
$lint fast
```

### 현재 변경 파일만 점검

```text
$lint changed
```

### 링크만 점검

```text
$lint links
```

### Concept 폴더 구조 점검

```text
$lint structure 20_Wiki/20.02_Concepts/
```

### 특정 Topic 심층 점검

```text
$lint deep 20_Wiki/20.04_Topics/example-topic.md
```

---

# 5. 권장 운영 루틴

## 자료를 추가할 때

```text
$ingest source-only 10_Raw/10.02_Inbox/example.pdf
```

중요 자료라면:

```text
$ingest deep 10_Raw/10.02_Inbox/example.pdf
```

---

## 웹 클리핑을 정리할 때

```text
$ingest clippings
```

---

## Wiki에서 질문할 때

```text
$query Wiki 기준으로 질문 내용
```

---

## 답변을 보존할 때

```text
$query 질문 내용. 결과를 Wiki에 저장해줘.
```

---

## 작업 직후 점검

```text
$lint changed
```

---

## 주간 점검

```text
$lint fast
```

필요하면:

```text
$lint links
```

---

## 월간 심층 점검

```text
$lint deep
```

---

# 6. 토큰 사용량 기준

상대적인 토큰 사용량은 다음과 같습니다.

| 작업 | 토큰 사용량 |
|---|---|
| `$query` 일반 답변 | 낮음 |
| `$ingest source-only` | 낮음 |
| `$lint fast` | 낮음 |
| `$lint changed` | 낮음 |
| `$ingest default` | 중간 |
| `$query compare` | 중간 |
| `$lint links` | 중간 |
| `$ingest inbox` | 중간 |
| `$ingest clippings` | 중간 |
| `$query deep` | 높음 |
| `$ingest deep` | 높음 |
| `$lint deep` | 높음 |

토큰 절약을 위해 평소에는 다음을 우선 사용하십시오.

```text
$ingest source-only
$query
$lint changed
$lint fast
```

Deep Mode는 정말 필요한 경우에만 사용합니다.

---

# 7. 가장 추천하는 명령 조합

```text
# 웹 자료 수집
$ingest clippings

# Inbox 자료 정리
$ingest inbox

# 특정 논문 처리
$ingest 10_Raw/10.02_Inbox/paper.pdf

# Wiki 질문
$query Wiki 기준으로 이 개념을 설명해줘

# 비교
$query compare A and B

# 변경 파일 점검
$lint changed

# 링크 점검
$lint links

# 전체 심층 품질 감사
$lint deep
```
