# Antigravity 설정 (Antigravity Configuration)

## AI 정체성 (AI Identity)

`{AIName}`: Antigravity

## 에이전트 지시 파일 (Agent Instruction File)

`{AgentInstructionFile}`: `GEMINI.md`

Antigravity는 프로젝트 루트 지시 파일로 `GEMINI.md`를 사용해야 합니다.

## 에이전트 환경 (Agent Environment)

`{AgentEnvironment}`:

- 프로젝트 루트에 `GEMINI.md`를 생성하거나 업데이트합니다.
- `LLM Wiki Prompt.md`의 공통 Wiki 구조를 유지합니다.
- 세부 운영 규칙은 `30_Rules/` 하위에 유지합니다.
- 프로젝트-local Antigravity 환경은 `.agent/` 하위에 유지합니다.
- 프로젝트-local 스킬은 `.agent/skills/` 하위에 유지합니다.
- `GEMINI.md`를 Antigravity 호환 워크플로우의 공유 에이전트 진입점으로 취급합니다.

## 에이전트 디렉터리 (Agent Directory)

`{AgentDirectory}`: `.agent`

## 스킬 디렉터리 (Skill Directory)

`{SkillDirectory}`: `.agent/skills`

필수 스킬 파일:

- `.agent/skills/ingest/SKILL.md`
- `.agent/skills/query/SKILL.md`
- `.agent/skills/lint/SKILL.md`

## 완료 보고 방식 (Completion Behavior)

완료를 보고할 때 생성 또는 수정된 파일 목록에 `GEMINI.md`를 포함합니다.
