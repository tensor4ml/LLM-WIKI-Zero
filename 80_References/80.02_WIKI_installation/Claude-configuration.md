# Claude Configuration

## AI Identity

`{AIName}`: Claude

## Agent Instruction File

`{AgentInstructionFile}`: `CLAUDE.md`

Claude should use `CLAUDE.md` as the project-root instruction file.

## Agent Environment

`{AgentEnvironment}`:

- Create or update `CLAUDE.md` at the project root.
- Keep the common Wiki structure from `LLM Wiki Prompt.md`.
- Keep detailed operating rules under `30_Rules/`.
- Keep the project-local Claude environment under `.claude/`.
- Keep project-local skills under `.claude/skills/`.
- Do not create `AGENTS.md` for Claude unless the user explicitly requests compatibility with another agent.

## Agent Directory

`{AgentDirectory}`: `.claude`

## Skill Directory

`{SkillDirectory}`: `.claude/skills`

Required skill placeholders:

- `.claude/skills/ingest/SKILL.md`
- `.claude/skills/query/SKILL.md`
- `.claude/skills/lint/SKILL.md`

## Completion Behavior

When reporting completion, list `CLAUDE.md` under updated or created files instead of `AGENTS.md`.
