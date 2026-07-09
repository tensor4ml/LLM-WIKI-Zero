# Claude 설정 (Claude Configuration)

## AI 정체성 (AI Identity)

`{AIName}`: Claude

## 에이전트 지시 파일 (Agent Instruction File)

`{AgentInstructionFile}`: `CLAUDE.md`

Claude는 프로젝트 루트 지시 파일로 `CLAUDE.md`를 사용해야 합니다.

## 에이전트 환경 (Agent Environment)

`{AgentEnvironment}`:

- 프로젝트 루트에 `CLAUDE.md`를 생성하거나 업데이트합니다.
- `LLM Wiki Prompt.md`의 공통 Wiki 구조를 유지합니다.
- 세부 운영 규칙은 `30_Rules/` 하위에 유지합니다.
- 프로젝트-local Claude 환경은 `.claude/` 하위에 유지합니다.
- 프로젝트-local 스킬은 `.claude/skills/` 하위에 유지합니다.
- 사용자가 다른 에이전트와의 호환성을 명시적으로 요청하지 않는 한 Claude를 위해 `AGENTS.md`를 생성하지 않습니다.

## 에이전트 디렉터리 (Agent Directory)

`{AgentDirectory}`: `.claude`

## 스킬 디렉터리 (Skill Directory)

`{SkillDirectory}`: `.claude/skills`

필수 스킬 파일:

- `.claude/skills/ingest/SKILL.md`
- `.claude/skills/query/SKILL.md`
- `.claude/skills/lint/SKILL.md`

## 완료 보고 방식 (Completion Behavior)

완료를 보고할 때 생성 또는 수정된 파일 목록에 `AGENTS.md` 대신 `CLAUDE.md`를 포함합니다.
