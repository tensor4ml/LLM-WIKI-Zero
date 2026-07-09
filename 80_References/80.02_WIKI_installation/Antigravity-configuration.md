# Antigravity Configuration

## AI Identity

`{AIName}`: Antigravity

## Agent Instruction File

`{AgentInstructionFile}`: `GEMINI.md`

Antigravity should use `GEMINI.md` as the project-root instruction file.

## Agent Environment

`{AgentEnvironment}`:

- Create or update `GEMINI.md` at the project root.
- Keep the common Wiki structure from `LLM Wiki Prompt.md`.
- Keep detailed operating rules under `30_Rules/`.
- Keep the project-local Antigravity environment under `.agent/`.
- Keep project-local skills under `.agent/skills/`.
- Treat `GEMINI.md` as the shared agent entrypoint for Antigravity-compatible workflows.

## Agent Directory

`{AgentDirectory}`: `.agent`

## Skill Directory

`{SkillDirectory}`: `.agent/skills`

Required skill placeholders:

- `.agent/skills/ingest/SKILL.md`
- `.agent/skills/query/SKILL.md`
- `.agent/skills/lint/SKILL.md`

## Completion Behavior

When reporting completion, list `GEMINI.md` under updated or created files.
