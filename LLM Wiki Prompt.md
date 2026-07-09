# LLM Wiki 규칙 모듈화 프롬프트

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
# LLM Wiki 에이전트

## 역할 (Role)

당신은 LLM이 관리하는 Markdown Wiki의 유지보수 담당자입니다.

사용자는 소스를 큐레이션하고, 질문을 하고, 출력 결과를 검토하며, 우선순위를 안내합니다.
당신은 Wiki 구조를 유지하고, Markdown 페이지를 업데이트하고, 링크를 보존하고, 변경사항을 추적하며, 지식 베이스를 시간이 지남에 따라 일관성 있게 유지합니다.

이 Wiki는 원본 소스와 사용자 질문으로부터 구축된 지속적이고 복합적인 지식 레이어입니다.

---

## 절대 지켜야 하는 규칙 (Non-Negotiable Rules)

1. 사용자가 명시적으로 지시하지 않는 한 `10_Raw/` 하위의 파일을 절대 수정하지 않습니다.
2. 원본 소스는 불변의 진실의 원천 문서로 취급합니다.
3. 생성된 지식은 `20_Wiki/` 하위에서 유지합니다.
4. Obsidian 스타일 링크를 사용합니다: `[[페이지 이름]]`.
5. 모든 의미 있는 Wiki 페이지에는 YAML frontmatter가 포함되어야 합니다.
6. 의미 있는 Wiki 변경 후에는 `20_Wiki/index.md`를 업데이트합니다.
7. 의미 있는 작업 후에는 `20_Wiki/log.md`에 내용을 추가합니다.
8. 근거 없는 주장을 만들어내지 않습니다.
9. 모순과 불확실성을 명시적으로 드러냅니다.
10. 파괴적인 작업 전에는 계획을 제안하고 사용자의 명시적인 승인을 기다립니다.

---

## 규칙 파일 (Rule Files)

세부 규칙은 `30_Rules/` 하위에 저장되어 있습니다.

Wiki 작업 시, 변경사항을 적용하기 전에 관련 규칙 파일을 읽으세요.

해당되는 경우 다음 순서로 규칙 파일을 로드하세요.

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

규칙 파일이 이 에이전트 지시 파일과 충돌하는 경우, 이 에이전트 지시 파일이 우선합니다.

---

## 스킬 라우팅 (Skill Routing)

사용자 요청에 따라 적절한 스킬을 사용하세요.

### Ingest (수집)

사용자가 다음을 요청할 때 `{SkillDirectory}/ingest/SKILL.md`를 사용하세요.

- 소스 수집
- 새 문서 처리
- 파일 가져오기
- 기사, 논문, 스크립트, 메모, 보고서를 Wiki에 추가
- `10_Raw/10.02_Inbox/`의 파일 처리

### Query (조회)

사용자가 다음을 요청할 때 `{SkillDirectory}/query/SKILL.md`를 사용하세요.

- Wiki에서 답변 찾기
- Wiki 내용 요약
- 기존 개념 비교
- 기존 Wiki 지식 합성
- 재사용 가능한 답변을 Wiki에 저장

### Lint (검사)

사용자가 다음을 요청할 때 `{SkillDirectory}/lint/SKILL.md`를 사용하세요.

- Wiki 검사
- Wiki 상태 확인
- 깨진 링크 찾기
- 고아 페이지 찾기
- 중복 페이지 감지
- 모순 감지
- 오래되거나 근거가 약한 주장 확인

---

## 우선순위 (Priority Order)

지시사항이 충돌할 경우 다음 우선순위를 따르세요.

1. 사용자의 명시적인 지시
2. 안전 및 데이터 보존
3. 이 에이전트 지시 파일
4. 관련 규칙 파일
5. 관련 스킬 파일
6. 기존 Wiki 관례
7. 일반적인 모범 사례

---

## 완료 보고 (Completion Report)

모든 의미 있는 Wiki 작업 후 다음 형식으로 보고하세요.

```markdown
# LLM Wiki 작업 보고서

## 상태 (Status)
완료 / 부분 완료 / 실패

## 사용한 스킬 (Skill Used)
Ingest / Query / Lint / 없음 / 복수

## 사용한 규칙 파일 (Rule Files Used)
- ...

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 원본 소스 수정 여부 (Raw Sources Modified)
없음

## 주요 결정 사항 (Key Decisions)
- ...

## 문제 / 불확실성 (Issues / Uncertainties)
- ...

## 권장 다음 단계 (Recommended Next Steps)
1. ...
2. ...
3. ...
```
````

---

## 4. 30_Rules/Core-Principles.md 작성

````markdown
# 핵심 원칙 (Core Principles)

이 문서는 LLM Wiki의 핵심 철학적·운영적 원칙을 정의합니다.

## 1. 지속적 복합 지식 레이어 (Persistent Compounding Knowledge Layer)
LLM Wiki는 단순한 대화 기록이나 임시 작업 공간이 아닙니다. 시간이 지남에 따라 원본 소스와 사용자 상호작용으로부터 지식을 수집, 정리, 합성하도록 설계된 지속적이고 복합적인 지식 레이어입니다.
- **지속적(Persistent)**: 세션이 종료되어도 정보가 유지되고 정제됩니다.
- **복합적(Compounding)**: 새로운 소스와 Q&A는 처음부터 다시 설명하는 것이 아니라 기존 개념 위에 구축됩니다.
- **연결된(Linked)**: 모든 페이지는 Obsidian 스타일 링크로 상호 연결되어 탐색 가능한 지식 그래프를 형성합니다.

## 2. 사용자와 에이전트의 역할 (User and Agent Roles)
- **사용자**: 소스를 큐레이션하고, 질문을 하고, 출력 결과를 검토하며, 우선순위를 안내합니다.
- **에이전트(LLM)**: Wiki 구조를 유지하고, Markdown 페이지를 업데이트하고, 링크를 보존하고, 변경사항을 추적하며, 지식 베이스를 일관성 있고 깔끔하며 최신 상태로 유지합니다.

## 3. 절대 지켜야 하는 규칙 (Non-Negotiable Rules)
1. 사용자가 명시적으로 지시하지 않는 한 `10_Raw/` 하위의 파일을 절대 수정하지 않습니다.
2. 원본 소스는 불변의 진실의 원천 문서로 취급합니다.
3. 생성된 지식은 `20_Wiki/` 하위에서 유지합니다.
4. Obsidian 스타일 링크를 사용합니다: `[[페이지 이름]]`.
5. 모든 의미 있는 Wiki 페이지에는 YAML frontmatter가 포함되어야 합니다.
6. 의미 있는 Wiki 변경 후에는 `20_Wiki/index.md`를 업데이트합니다.
7. 의미 있는 작업 후에는 `20_Wiki/log.md`에 내용을 추가합니다.
8. 근거 없는 주장을 만들어내지 않습니다.
9. 모순과 불확실성을 명시적으로 드러냅니다.
10. 파괴적인 작업 전에는 계획을 제안하고 사용자의 명시적인 승인을 기다립니다.
````

---

## 5. 30_Rules/Directory-Structure.md 작성

````markdown
# 디렉터리 구조 규칙 (Directory Structure Rules)

## 필수 구조 (Required Structure)

다음 구조를 사용하세요.

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

## 원본 레이어 (Raw Layer)

경로:

```text
10_Raw/
```

원본 레이어는 원본 소스 자료를 저장합니다.

규칙:

- `10_Raw/` 하위의 파일을 수정하지 않습니다.
- 사용자가 명시적으로 요청하지 않는 한 원본 파일을 정규화, 이름 변경, 삭제 또는 이동하지 않습니다.
- 원본 소스가 지저분한 경우, 그대로 보존하고 깔끔한 Wiki 페이지를 새로 생성합니다.
- 소스 메타데이터가 불분명한 경우, unknown으로 표시합니다.

## Wiki 레이어 (Wiki Layer)

경로:

```text
20_Wiki/
```

Wiki 레이어는 LLM이 생성한 Markdown 페이지를 저장합니다.

규칙:
- `20_Wiki/` 하위에 생성된 지식을 만들고 업데이트합니다.
- Obsidian에서 읽기 쉽게 페이지를 유지합니다.
- YAML frontmatter를 사용합니다.
- 소문자 케밥 케이스 파일명을 사용합니다.
- Obsidian 스타일 링크를 사용합니다.

## 규칙 레이어 (Rules Layer)

경로:

```text
30_Rules/
```

규칙 레이어는 모듈식 유지보수 규칙을 저장합니다.

규칙:

- `{AgentInstructionFile}`을 간결하게 유지합니다.
- 세부 표준은 규칙 파일에 넣습니다.
- Wiki 프로세스가 발전할 때 규칙 파일을 업데이트합니다.

## 스킬 레이어 (Skills Layer)

경로:

```text
{SkillDirectory}/
```

스킬 레이어는 프로젝트-local 에이전트 디렉터리 하위에 프로젝트 특화 작업 워크플로우를 저장합니다.

규칙:

- 운영 절차에는 스킬을 사용합니다.
- 전역 표준에는 규칙을 사용합니다.
- 스킬이 규칙과 충돌하는 경우, 사용자가 명시적으로 재정의하지 않는 한 규칙을 따릅니다.
````

---

## 6. 30_Rules/Page-Types.md 작성

````markdown
# 페이지 유형 규칙 (Page Type Rules)

## 소스 페이지 (Source Pages)

경로:

```text
20_Wiki/20.01_Sources/
```

목적:

소스 페이지는 하나의 원본 소스를 요약하고 인덱싱합니다.

다음에 사용하세요:

- 기사
- 논문
- 보고서
- 도서
- 스크립트/대본
- 클리핑된 웹 페이지
- 업로드된 문서

소스 페이지에 포함해야 할 내용:

- 요약
- 핵심 주장
- 중요한 세부 정보
- 증거
- 관련 개념
- 관련 엔티티
- 관련 토픽
- 모순 또는 긴장 관계
- 미해결 질문

## 개념 페이지 (Concept Pages)

경로:

```text
20_Wiki/20.02_Concepts/
```

목적:

개념 페이지는 재사용 가능한 하나의 아이디어를 설명합니다.

예시:

- RAG
- 지속적 지식 베이스 (Persistent Knowledge Base)
- 지식 복합화 (Knowledge Compounding)
- Wiki 린팅 (Wiki Linting)

개념 페이지에 포함해야 할 내용:

- 정의
- 중요성
- 핵심 포인트
- 관련 개념
- 관련 소스
- 미해결 질문

## 엔티티 페이지 (Entity Pages)

경로:

```text
20_Wiki/20.03_Entities/
```

목적:

엔티티 페이지는 명명된 객체를 설명합니다.

예시:

- 인물
- 조직
- 회사
- 제품
- 소프트웨어 도구
- 장소
- 프로젝트

엔티티 페이지에 포함해야 할 내용:

- 설명
- 역할 또는 관련성
- 핵심 세부 정보
- 관련 개념
- 관련 소스
- 미해결 질문

## 토픽 페이지 (Topic Pages)

경로:

```text
20_Wiki/20.04_Topics/
```

목적:

토픽 페이지는 더 넓은 주제 영역의 허브 역할을 합니다.

토픽 페이지에 포함해야 할 내용:

- 개요
- 핵심 개념
- 중요한 소스
- 관련 엔티티
- 현재 합성 내용
- 미해결 질문

## 질문 페이지 (Question Pages)

경로:

```text
20_Wiki/20.05_Questions/
```

목적:

질문 페이지는 재사용 가능한 사용자 질문과 답변을 저장합니다.

미래 가치가 있는 답변이 있을 때만 질문을 저장하세요.

저장하지 않을 것:

- 사소한 질문
- 일회성 확인
- 오타 수정
- 짧은 명령

## 합성 페이지 (Synthesis Pages)

경로:

```text
20_Wiki/20.06_Syntheses/
```

목적:

합성 페이지는 여러 페이지, 소스 또는 아이디어를 상위 수준의 분석으로 결합합니다.

다음에 사용하세요:

- 비교
- 프레임워크
- 논증
- 의사결정 메모
- 문헌 스타일 합성
- 전략적 요약
- 유지보수 보고서
````

---

## 7. 30_Rules/Frontmatter.md 작성

````markdown
# Frontmatter 규칙 (Frontmatter Rules)

## 요구 사항 (Requirement)

모든 의미 있는 Wiki 페이지에는 YAML frontmatter가 포함되어야 합니다.

## 필수 필드 (Required Fields)

```yaml
---
type:
title:
created:
updated:
tags:
---
```

## 선택적 필드 (Optional Fields)

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

## 허용되는 type 값 (Allowed Type Values)

해당되는 경우 다음 값들을 사용하세요:

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

## 날짜 형식 (Date Format)

`YYYY-MM-DD` 형식을 사용하세요.

## 단순성 원칙 (Simplicity Rule)

초기 단계에서 frontmatter를 지나치게 복잡하게 만들지 마세요.

다음을 향상시킬 때만 메타데이터를 추가하세요:

- 탐색
- 검색
- 필터링
- 유지보수
- 소스 추적
````

---

## 8. 30_Rules/Naming-and-Linking.md 작성

````markdown
# 명명 및 링크 규칙 (Naming and Linking Rules)

## 파일명 규칙 (Filename Rules)

소문자 케밥 케이스 파일명을 사용하세요.

좋은 예:

```text
llm-wiki.md
rag.md
persistent-knowledge-base.md
wiki-ingestion-workflow.md
obsidian-web-clipper.md
llm-wiki-vs-rag.md
```

피해야 할 예:

```text
LLM Wiki.md
RAG 정리.md
new note.md
문서1.md
My Great Summary.md
```

## 페이지 제목 규칙 (Page Title Rules)

Markdown 페이지 제목은 일반적인 대소문자 규칙을 사용할 수 있습니다.

예시:

```markdown
# LLM Wiki
```

파일명:

```text
llm-wiki.md
```

## 링크 규칙 (Link Rules)

Obsidian 스타일 링크를 사용하세요.

예시:

```markdown
[[LLM Wiki]]
[[RAG]]
[[Persistent Knowledge Base]]
[[Obsidian]]
```

## 링크 기대치 (Linking Expectations)

- 소스 페이지는 관련 개념, 엔티티 및 토픽에 링크해야 합니다.
- 개념 페이지는 관련 개념과 지원 소스에 링크해야 합니다.
- 엔티티 페이지는 관련 개념, 토픽 및 소스에 링크해야 합니다.
- 토픽 페이지는 허브 역할을 해야 합니다.
- 합성 페이지는 참조하는 모든 주요 페이지에 링크해야 합니다.
- 질문 페이지는 답변에 사용된 페이지에 링크해야 합니다.

## 링크 품질 (Link Quality)

반복되는 모든 단어에 과도하게 링크하는 것을 피하세요.

탐색을 향상시키는 의미 있는 링크를 선호하세요.
````

---

## 9. 30_Rules/Index-and-Log.md 작성

````markdown
# 인덱스 및 로그 규칙 (Index and Log Rules)

## index.md

경로:

```text
20_Wiki/index.md
```

목적:

`index.md`는 Wiki의 내용 중심 지도입니다.

인간과 LLM 모두가 관련 페이지를 빠르게 찾을 수 있도록 도와야 합니다.

다음 구조를 유지하세요:

```markdown
# LLM Wiki 인덱스

## 개요 (Overview)

## 소스 (Sources)

## 토픽 (Topics)

## 개념 (Concepts)

## 엔티티 (Entities)

## 질문 (Questions)

## 합성 (Syntheses)

## 유지보수 (Maintenance)
```

각 항목은 다음 형식을 따라야 합니다:

```markdown
- [[페이지 이름]] — 한 줄 요약.
```

규칙:

- 새 Wiki 페이지가 생성될 때마다 `index.md`를 업데이트합니다.
- 페이지 이름이 변경될 때 `index.md`를 업데이트합니다.
- 누락된 페이지를 가리키는 항목을 제거하거나 수정합니다.
- 요약은 짧고 유용하게 유지합니다.
- `index.md`를 전체 기사로 만들지 않습니다.

## log.md

경로:

```text
20_Wiki/log.md
```

목적:

`log.md`는 Wiki 작업의 시간순 기록입니다.

추가만 가능한(append-only) 방식으로 유지해야 합니다.

다음 형식을 사용하세요:

```markdown
## [YYYY-MM-DD] 작업 유형 | 제목

- 작업 (Operation):
- 생성 (Created):
  - [[페이지 이름]]
- 수정 (Updated):
  - [[페이지 이름]]
- 소스 (Source):
  - 10_Raw/10.01_Clippings/example.md
- 비고 (Notes):
  - ...
```

일반적인 작업 유형:

```text
setup
ingest
query
lint
refactor
maintenance
```

규칙:

- 모든 의미 있는 작업 후 로그 항목을 추가합니다.
- 오래된 로그 항목을 삭제하지 않습니다.
- 명확한 서식 오류를 수정하는 경우가 아니라면 기록을 재작성하지 않습니다.
- 로그 항목을 간결하지만 추적 가능하게 유지합니다.
- 가능하면 생성된 파일과 수정된 파일을 포함합니다.
````

---

## 10. 30_Rules/Source-Attribution.md 작성

````markdown
# 소스 귀속 규칙 (Source Attribution Rules)

## 목적 (Purpose)

Wiki 페이지의 주장은 소스로 추적 가능해야 합니다.

Wiki는 중요한 주장이 어디서 왔는지 명확히 해야 합니다.

## 규칙 (Rules)

1. 가능하면 중요한 주장을 소스 페이지에 링크합니다.
2. 인용을 조작하지 않습니다.
3. 소스와 주장 사이의 관계를 조작하지 않습니다.
4. 주장에 근거가 없으면 약하게 귀속된 것으로 표시합니다.
5. 소스 메타데이터가 누락된 경우 unknown으로 표시합니다.
6. Wiki 내에서 링크할 때 원본 소스 경로보다 소스 페이지를 선호합니다.

## 관련 소스 섹션 (Related Sources Section)

적절한 경우 다음과 같은 섹션을 사용하세요:

```markdown
## 관련 소스 (Related Sources)

- [[2026-07-07-llm-wiki-pattern]]
```

## 약한 귀속 (Weak Attribution)

페이지가 소스 링크 없이 중요한 주장을 할 경우, lint 시 표시하세요.

다음과 같은 레이블을 사용하세요:

```text
소스 필요 (Source Needed)
약하게 귀속됨 (Weakly Attributed)
검증 필요 (Needs Verification)
```
````

---

## 11. 30_Rules/Contradictions-and-Uncertainty.md 작성

````markdown
# 모순 및 불확실성 규칙 (Contradictions and Uncertainty Rules)

## 모순 처리 (Contradiction Handling)

새로운 정보가 기존 내용과 충돌할 때:

1. 이전 주장을 조용히 덮어쓰지 않습니다.
2. 두 주장 모두를 식별합니다.
3. 각 주장을 지원 소스 또는 페이지에 링크합니다.
4. 전용 섹션 하에 충돌을 표시합니다.

다음과 같은 섹션 이름을 사용하세요:

```markdown
## 모순 / 긴장 관계 (Contradictions / Tensions)
```

또는:

```markdown
## 소스 긴장 관계 (Source Tensions)
```

예시 표현:

```markdown
- 소스 A는 X를 제안하고, 소스 B는 Y를 제안합니다.
- 이는 서로 다른 시기, 정의, 가정 또는 소스 품질을 반영할 수 있습니다.
- 검증이 필요합니다.
```

더 새로운 소스가 이전 소스를 명확히 대체하는 경우, 이를 명시적으로 언급하고 역사적 맥락을 보존합니다.

## 불확실성 처리 (Uncertainty Handling)

불확실성을 숨기지 마세요.

명시적인 표시자를 사용하세요:

```text
불확실 (Uncertain)
검증 필요 (Needs Verification)
미해결 질문 (Open Question)
부분적으로 지원됨 (Partially Supported)
소스 필요 (Source Needed)
약하게 귀속됨 (Weakly Attributed)
```

## 미해결 질문 (Open Questions)

문제를 해결할 수 없을 때 미해결 질문을 추가하세요.

예시:

```markdown
## 미해결 질문 (Open Questions)

- 이 주장이 일반적으로 적용되는가, 아니면 소스의 특정 맥락 내에서만 적용되는가?
- 이를 대체하는 더 새로운 증거가 있는가?
```
````

---

## 12. 30_Rules/Refactor-Policy.md 작성

````markdown
# 리팩토링 정책 (Refactor Policy)

## 리팩토링의 정의 (What Counts as Refactor)

리팩토링은 다음을 의미합니다:

- 파일 이름 변경
- 페이지 이동
- 페이지 병합
- 페이지 분할
- 페이지 삭제
- 페이지 분류 체계 변경
- 주요 섹션 재작성
- `index.md` 구조 변경

## 필수 리팩토링 계획 (Required Refactor Plan)

주요 리팩토링 전에 계획을 작성하세요.

다음 형식을 사용하세요:

```markdown
# 리팩토링 계획 (Refactor Plan)

## 이유 (Reason)

## 제안된 변경사항 (Proposed Changes)

## 생성할 파일 (Files to Create)

## 이름 변경할 파일 (Files to Rename)

## 이동할 파일 (Files to Move)

## 병합할 파일 (Files to Merge)

## 삭제할 파일 (Files to Delete)

## 업데이트할 링크 (Links to Update)

## 위험 요소 (Risks)

## 승인 필요 여부 (Confirmation Needed)
```

## 파괴적인 변경 (Destructive Changes)

사용자 승인 없이 파괴적인 리팩토링을 수행하지 마세요.

파괴적인 변경에는 다음이 포함됩니다:

- 페이지 삭제
- 페이지 병합
- 페이지 이름 변경
- 대용량 파일 세트 이동
- 주요 섹션 교체
- 한쪽을 제거하여 모순 해결

## 안전한 변경 (Safe Changes)

별도의 승인이 필요 없는 안전한 작업:

- 누락된 디렉터리 생성
- 누락된 초기 파일 생성
- 수집 중 새 페이지 추가
- 페이지 생성 후 인덱스 업데이트
- 로그에 내용 추가
- lint 보고서 생성
- 새로 생성된 페이지에 누락된 frontmatter 추가
````

---

## 13. 30_Rules/Operation-Reporting.md 작성

````markdown
# 작업 보고 규칙 (Operation Reporting Rules)

## 목적 (Purpose)

모든 의미 있는 Wiki 작업은 간결한 보고서로 마무리해야 합니다.

보고서는 다음을 명확히 해야 합니다:

- 무엇을 했는가
- 어떤 스킬을 사용했는가
- 어떤 규칙 파일을 사용했는가
- 어떤 파일을 생성했는가
- 어떤 파일을 수정했는가
- 원본 소스를 수정했는가
- 무엇이 불확실한가
- 다음에 무엇을 해야 하는가

## 표준 보고서 형식 (Standard Report Format)

다음 형식을 사용하세요:

```markdown
# LLM Wiki 작업 보고서 (LLM Wiki Operation Report)

## 상태 (Status)
완료 / 부분 완료 / 실패

## 사용한 스킬 (Skill Used)
Ingest / Query / Lint / 없음 / 복수

## 사용한 규칙 파일 (Rule Files Used)
- ...

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 원본 소스 수정 여부 (Raw Sources Modified)
없음

## 주요 결정 사항 (Key Decisions)
- ...

## 문제 / 불확실성 (Issues / Uncertainties)
- ...

## 권장 다음 단계 (Recommended Next Steps)
1. ...
2. ...
3. ...
```

## 보고 규칙 (Reporting Rules)

- 변경된 파일이 없으면 명확히 말하세요.
- 작업이 부분적으로 실패한 경우 성공한 것과 실패한 것을 설명하세요.
- 소스를 읽을 수 없었다면 소스와 이유를 식별하세요.
- 가정을 했다면 목록에 나열하세요.
- 보고서를 간결하게 유지하세요.
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
# LLM Wiki 인덱스 (LLM Wiki Index)

## 개요 (Overview)
- [[overview]] — Wiki의 상위 수준 요약.

## 소스 (Sources)

## 토픽 (Topics)

## 개념 (Concepts)

## 엔티티 (Entities)

## 질문 (Questions)

## 합성 (Syntheses)

## 유지보수 (Maintenance)
```

### 20_Wiki/log.md

```markdown
# LLM Wiki 로그 (LLM Wiki Log)

## [YYYY-MM-DD] setup | 초기 모듈식 규칙 구조 구축

- 작업 (Operation): setup
- 생성 (Created):
  - [[index]]
  - [[log]]
  - [[overview]]
- 수정 (Updated):
  - 없음
- 비고 (Notes):
  - 모듈식 규칙 파일을 포함한 초기 LLM Wiki 구조를 생성했습니다.
```

### 20_Wiki/overview.md

```markdown
---
type: overview
title: Wiki 개요
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - overview
---

# Wiki 개요 (Wiki Overview)

## 목적 (Purpose)

이 Wiki는 LLM이 유지보수하는 지속적인 Markdown 지식 베이스입니다.

## 주요 토픽 (Major Topics)

아직 주요 토픽이 없습니다.

## 현재 합성 (Current Syntheses)

아직 합성이 없습니다.

## 미해결 질문 (Open Questions)

아직 미해결 질문이 없습니다.

## 다음 단계 (Next Steps)

소스를 `10_Raw/10.02_Inbox/` 또는 `10_Raw/10.01_Clippings/`에 추가하고 ingest 스킬을 실행하세요.
```

---

## 16. 완료 보고

작업을 마친 뒤 다음 형식으로 보고하세요.

```markdown
# LLM Wiki 작업 보고서 (LLM Wiki Operation Report)

## 상태 (Status)
완료 / 부분 완료 / 실패

## 사용한 스킬 (Skill Used)
없음

## 사용한 규칙 파일 (Rule Files Used)
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

## 생성한 파일 (Created Files)
- ...

## 수정한 파일 (Updated Files)
- ...

## 원본 소스 수정 여부 (Raw Sources Modified)
없음

## 주요 결정 사항 (Key Decisions)
- CLAUDE.md, AGENTS.md, GEMINI.md를 최소화된 형태로 유지했습니다.
- 세부 규칙을 30_Rules/ 하위의 모듈식 파일로 이동했습니다.
- 스킬은 전역 규칙과 분리하여 프로젝트-local 디렉터리(.claude/skills, .codex/skills, .agent/skills) 하위에 유지했습니다.

## 문제 / 불확실성 (Issues / Uncertainties)
- ...

## 권장 다음 단계 (Recommended Next Steps)
1. `80_References/80.02_WIKI_installation/Ingest Skill.md`에서 생성된 Ingest 스킬을 검토하세요.
2. `80_References/80.02_WIKI_installation/Query Skill.md`에서 생성된 Query 스킬을 검토하세요.
3. `80_References/80.02_WIKI_installation/Lint Skill.md`에서 생성된 Lint 스킬을 검토하세요.
```
