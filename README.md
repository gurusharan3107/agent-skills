# agent-skills

A collection of reusable **agent skills** — portable, target-agnostic instructions an AI coding agent (e.g. Claude Code) loads on demand to work a particular kind of task.

Each skill lives in its own folder as a `SKILL.md` with YAML frontmatter (`name`, `description`) that tells the agent when to invoke it and how to apply it.

## Skills

| Skill | What it does |
|---|---|
| [`elon-algorithm`](elon-algorithm/SKILL.md) | Apply Musk's engineering algorithm to any skill, system, platform, codebase, or process — get it working, then question requirements → delete → simplify & optimize → accelerate → automate, strictly in that order. Includes the delete-until-you-add-10%-back rule and a measured before/after report. |
| [`resolution-copilot`](resolution-copilot/SKILL.md) | Resume the Resolution Copilot / agentic incident POC: ServiceNow live intake, evidence-gated command center, verified KB, staging repro. Points at branch `cursor/resolution-copilot-foundation-a168` and required `SERVICENOW_*` secrets. |

## Using a skill

Copy the skill's folder into your agent's skills directory (for Claude Code: `~/.claude/skills/`), or point your harness at this repo. The agent picks it up by its `SKILL.md` frontmatter and invokes it when a task matches the `description`.

## Adding a skill

Create `‹skill-name›/SKILL.md`:

```markdown
---
name: ‹skill-name›
description: "One-paragraph trigger + summary: what it's for and when to use it."
---

# ‹Skill title›

The instructions the agent follows...
```

Keep skills concise, actionable, and judgment-first — index + method, not exhaustive prose.

## License

MIT — see [LICENSE](LICENSE).
