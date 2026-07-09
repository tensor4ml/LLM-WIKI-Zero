# Codex 설정 (Codex Configuration)

## AI 정체성 (AI Identity)

`{AIName}`: Codex

## 에이전트 지시 파일 (Agent Instruction File)

`{AgentInstructionFile}`: `AGENTS.md`

Codex는 프로젝트 루트 지시 파일로 `AGENTS.md`를 사용해야 합니다.

## 에이전트 환경 (Agent Environment)

`{AgentEnvironment}`:

- 프로젝트 루트에 `AGENTS.md`를 생성하거나 업데이트합니다.
- `LLM Wiki Prompt.md`의 공통 Wiki 구조를 유지합니다.
- 세부 운영 규칙은 `30_Rules/` 하위에 유지합니다.
- 프로젝트-local Codex 환경은 `.codex/` 하위에 유지합니다.
- 프로젝트-local 스킬은 `.codex/skills/` 하위에 유지합니다.
- `.codex/skills/`를 `AGENTS.md`가 참조하는 로컬 워크플로우 디렉터리로 취급합니다.

## 에이전트 디렉터리 (Agent Directory)

`{AgentDirectory}`: `.codex`

## 스킬 디렉터리 (Skill Directory)

`{SkillDirectory}`: `.codex/skills`

필수 스킬 파일:

- `.codex/skills/ingest/SKILL.md`
- `.codex/skills/query/SKILL.md`
- `.codex/skills/lint/SKILL.md`

## 완료 보고 방식 (Completion Behavior)

완료를 보고할 때 생성 또는 수정된 파일 목록에 `AGENTS.md`를 포함합니다.
