# Codex Configuration

## AI Identity

`{AIName}`: Codex

## Agent Instruction File

`{AgentInstructionFile}`: `AGENTS.md`

Codex should use `AGENTS.md` as the project-root instruction file.

## Agent Environment

`{AgentEnvironment}`:

- Create or update `AGENTS.md` at the project root.
- Keep the common Wiki structure from `LLM Wiki Prompt.md`.
- Keep detailed operating rules under `30_Rules/`.
- Keep the project-local Codex environment under `.codex/`.
- Keep project-local skills under `.codex/skills/`.
- Treat `.codex/skills/` as the local workflow directory referenced by `AGENTS.md`.

## Agent Directory

`{AgentDirectory}`: `.codex`

## Skill Directory

`{SkillDirectory}`: `.codex/skills`

Required skill placeholders:

- `.codex/skills/ingest/SKILL.md`
- `.codex/skills/query/SKILL.md`
- `.codex/skills/lint/SKILL.md`

## Completion Behavior

When reporting completion, list `AGENTS.md` under updated or created files.
