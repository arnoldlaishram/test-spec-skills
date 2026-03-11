# Test Spec Generator

AI skills for generating mobile app test specifications as local markdown files under a `.test/` folder.

## Skills

| Skill | What it does |
|---|---|
| `generate-test-spec` | Generate test specs from your codebase and save as `.test/*.md` files |

## Commands

| Command | Triggers |
|---|---|
| `/generate-test-spec` | Invoke `generate-test-spec` skill |

## Manual Install

```bash
npx -y ai-agent-skills install final-run/test-spec-skills --agent claude
npx -y ai-agent-skills install final-run/test-spec-skills --agent cursor
npx -y ai-agent-skills install final-run/test-spec-skills --agent codex
```