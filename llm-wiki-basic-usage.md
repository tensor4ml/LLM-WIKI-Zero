---
type: synthesis
title: LLM Wiki 기본 사용법
created: 2026-07-08
updated: 2026-07-08
tags:
  - llm-wiki
  - graphify
  - workflow
---
2
# LLM Wiki 기본 사용법

## Core Argument

이 프로젝트는 사용자가 source를 모으고, LLM이 그 source를 읽어 Markdown Wiki로 컴파일하며, 질문과 답변이 다시 Wiki에 축적되는 방식으로 운영한다. 사용자가 언급한 Andrej Karpathy식 LLM Wiki 방법론을 이 프로젝트 맥락에서 해석하면 핵심은 단순한 채팅 기록이 아니라, LLM이 계속 읽고 갱신할 수 있는 지속적 지식층을 만드는 것이다.

현재 프로젝트 규칙상 raw source는 `10_Raw/`에 보존하고, LLM이 정리한 지식은 `20_Wiki/`에 만들며, 운영 규칙은 `30_Rules/`와 `AGENTS.md`가 관리한다. graphify는 이 Markdown Wiki를 별도의 knowledge graph로 만들어서 관계 탐색, 질문, 경로 추적에 사용한다.

## Project Layers

1. `10_Raw/`

   원본 source를 보관하는 영역이다. 사용자가 명시하지 않는 한 수정하지 않는다. 새 article, clipping, transcript, note, report는 보통 `10_Raw/10.02_Inbox/` 또는 `10_Raw/10.01_Clippings/`에 둔다.

2. `20_Wiki/`

   LLM이 생성한 지식층이다. source 요약, concept, entity, topic, question, synthesis, maintenance report가 이곳에 저장된다. Obsidian-style link인 `[[Page Name]]`을 사용한다.

3. `30_Rules/`

   Wiki 품질 규칙이다. frontmatter, naming, index/log, attribution, contradiction handling 같은 유지보수 기준을 정의한다.

4. `.agents/skills/`

   반복 작업 절차를 정의한다. 현재 핵심 skill은 ingest, query, lint, graphify다.

5. `graphify-out/`

   graphify가 만든 knowledge graph 출력물이다. `graph.json`, `GRAPH_REPORT.md`, `graph.html`, query memory 등이 들어간다.

## Basic Workflow

1. Source를 추가한다.

   새 자료는 raw layer에 둔다. 예:

   ```text
   10_Raw/10.02_Inbox/
   10_Raw/10.01_Clippings/
   ```

2. Ingest를 요청한다.

   예:

   ```text
   10_Raw/10.02_Inbox/의 새 문서를 ingest 해줘.
   이 article을 Wiki에 추가해줘.
   ```

   결과물은 보통 `20_Wiki/20.01_Sources/`에 source page로 만들어지고, 관련 concept/entity/topic page가 필요하면 추가된다.

3. Wiki에 질문한다.

   예:

   ```text
   Wiki 기준으로 M4 맥미니의 장점은?
   이 주제에 대해 Wiki에서 찾아서 답해줘.
   ```

   재사용 가치가 있는 답변은 `20_Wiki/20.05_Questions/`에 question page로 저장한다.

4. 여러 페이지를 종합한다.

   특정 주제의 비교, 운영 방식, decision memo, 방법론 정리는 `20_Wiki/20.06_Syntheses/`에 synthesis page로 저장한다.

5. Wiki 상태를 점검한다.

   예:

   ```text
   Wiki health check 해줘.
   broken links와 orphan pages를 찾아줘.
   약하게 출처가 달린 claim을 찾아줘.
   ```

6. graphify로 관계를 본다.

   Wiki가 커지면 graphify로 전체 연결 구조를 만들어 질문, path, explanation을 수행한다.

## Skill Usage

### Ingest

사용 시점:

- 새 source를 Wiki에 넣을 때
- `10_Raw/10.02_Inbox/`의 문서를 처리할 때
- article, paper, transcript, report를 요약하고 연결할 때

대표 요청:

```text
10_Raw/10.02_Inbox/의 새 파일을 ingest 해줘.
이 clipping을 source page로 처리해줘.
```

### Query

사용 시점:

- 기존 Wiki 기준으로 답할 때
- concept/entity/topic을 비교할 때
- 재사용 가능한 질문 답변을 저장할 때

대표 요청:

```text
Wiki 기준으로 답변해줘.
이 주제에 대해 Wiki에서 찾아서 정리해줘.
```

### Lint

사용 시점:

- broken link, orphan page, duplicate page를 찾을 때
- source attribution이 약한 claim을 점검할 때
- contradiction이나 stale claim을 찾을 때

대표 요청:

```text
Wiki health check 해줘.
broken links를 찾아줘.
중복 페이지나 약한 출처를 점검해줘.
```

### Graphify

사용 시점:

- Wiki 전체를 knowledge graph로 만들 때
- 문서 간 관계, god node, surprising connection을 찾을 때
- graph 기반으로 질문하거나 두 concept 사이 path를 볼 때

대표 명령:

```bash
$graphify 20_Wiki
```

`20_Wiki/`를 graph로 빌드한다. 결과는 root의 `graphify-out/`에 생성된다.

```bash
$graphify query "M4 맥을 구입후 처음에 설치해야 할것들은?"
```

기존 graph에서 질문한다.

```bash
graphify update 20_Wiki
```

graphify 저장소를 갱신한다. 문서 변경 후 graph가 오래되었을 때 사용한다.

```bash
graphify codex install
```

Codex hook을 설치해 graphify check가 자동으로 동작하게 한다.

```bash
graphify path "Mac Mini M4" "Windows to Mac Transition"
graphify explain "Mac Mini M4"
```

두 node 사이 경로를 보거나 특정 node 주변 설명을 확인한다.

## Graphify Outputs

주요 출력물:

- `graphify-out/graph.html` - 브라우저에서 여는 interactive graph
- `graphify-out/GRAPH_REPORT.md` - god nodes, surprising connections, suggested questions가 있는 report
- `graphify-out/graph.json` - raw graph data
- `graphify-out/memory/` - query result feedback

사용 원칙:

- codebase나 project content 질문은 먼저 `graphify query "<question>"`를 시도한다.
- graph가 오래되었으면 `graphify update 20_Wiki` 또는 필요 시 `$graphify 20_Wiki`로 다시 만든다.
- graphify 결과가 부족하면 `20_Wiki/index.md`, 관련 Wiki page, rule file을 직접 읽어 보완한다.

## Daily Recipes

### 새 자료를 넣고 정리하기

1. 원본을 `10_Raw/10.02_Inbox/`에 넣는다.
2. `ingest`를 요청한다.
3. 생성된 source/concept/entity/topic page를 확인한다.
4. `20_Wiki/index.md`와 `20_Wiki/log.md`가 갱신되었는지 확인한다.
5. 필요하면 `$graphify 20_Wiki` 또는 `graphify update 20_Wiki`를 실행한다.

### Wiki에서 답변 만들기

1. `query`를 요청한다.
2. 답변이 재사용 가능하면 question page로 저장한다.
3. supporting pages와 uncertainty를 명시한다.
4. index/log를 갱신한다.

### 구조 점검하기

1. `lint`를 요청한다.
2. broken link, orphan page, duplicate page, contradiction을 확인한다.
3. maintenance report를 `20_Wiki/20.06_Syntheses/`에 저장한다.

### Graph로 탐색하기

1. `$graphify 20_Wiki`로 graph를 만든다.
2. `graphify-out/GRAPH_REPORT.md`에서 god nodes와 suggested questions를 본다.
3. `$graphify query "질문"`으로 graph 기반 답변을 얻는다.
4. `graphify path "A" "B"`로 관계 경로를 확인한다.
5. `graphify explain "Node"`로 특정 node 주변 맥락을 본다.

## Guardrails

- `10_Raw/`는 원본 source이므로 직접 수정하지 않는다.
- 중요한 claim은 source page나 supporting page에 연결한다.
- 불확실한 claim은 `Needs Verification`, `Uncertain`, `Source Needed`처럼 표시한다.
- contradiction은 덮어쓰지 말고 별도 섹션에 남긴다.
- 의미 있는 Wiki 변경 후에는 `20_Wiki/index.md`와 `20_Wiki/log.md`를 갱신한다.
- page filename은 lowercase kebab-case를 사용한다.
- page 간 연결은 `[[Page Name]]` 형식을 사용한다.

## Supporting Evidence

- [[overview]] defines the Wiki as a persistent Markdown knowledge base.
- [[Wiki Health Check 2026-07-08]] confirms the required `20_Wiki/` structure is present and navigable.
- [[2026-07-07 Wiki Lint Report]] records that index entries resolve and no broken Obsidian links were found at the time of that check.
- `AGENTS.md` defines the role, priority order, raw-source immutability, index/log requirements, and graphify usage rules.
- `80_References/graphify-installation.md` records the local graphify setup commands and common graphify usage commands.

## Open Questions

- The user's reference to Andrej Karpathy's LLM Wiki methodology is treated here as the project premise, but no dedicated source page for that methodology currently exists in `20_Wiki/`.
- `graphify update 20_Wiki` behavior for Markdown semantic changes may need practical verification after the graph grows.
- `20_Wiki/log.md` still lacks YAML frontmatter according to existing maintenance reports.
