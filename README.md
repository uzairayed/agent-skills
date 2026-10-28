# agent-skills

Skills I use across my coding agents and personal assistants.

To install one, paste this repo link or a skill folder link into your agent (Cursor, Grok Bot, Claude Code, Codex) and ask it to add the skill, or run `npx skills add uzairayed/agent-skills`.

## Skills

| Skill | For | What it does |
| :--- | :--- | :--- |
| [`feature-map-project`](./skills/feature-map-project/SKILL.md) | Codebases (Cursor, Claude Code, Codex) | Keeps a `<100` line `FEATURE_MAP.md` in your repo that maps your product words and UI labels ("the checkout drawer", "Buy Now button") to exact file paths, so the agent opens the right file immediately instead of running 10 `rg` searches every session. |
| [`feature-map-assistant`](./skills/feature-map-assistant/SKILL.md) | Personal assistants (Grok Bot, multi-device agents) | Keeps a `FEATURE_MAP.md` in your assistant's workspace that maps your shorthand for devices ("my PC", "my Mac"), local repo paths, Notion databases, Drive folders, and subagents to their exact IDs and paths so it stops re-asking for them. |

## License

MIT
