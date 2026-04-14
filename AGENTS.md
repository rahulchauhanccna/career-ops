# Career-Ops for AI Agents

Read `CLAUDE.md` for all project instructions, routing, and behavioral rules. They apply equally to all AI agents (Claude Code, Codex, Cline).

## Supported Agents

| Agent | Config File | Documentation |
|-------|-------------|---------------|
| Claude Code | `.claude/skills/career-ops/SKILL.md` | `CLAUDE.md` |
| Codex | `AGENTS.md` | `docs/CODEX.md` |
| Cline | `.clinerules` | `docs/CLINE.md` |

## Key Points

- Reuse the existing modes, scripts, templates, and tracker flow — do not create parallel logic.
- Store user-specific customization in `config/profile.yml`, `modes/_profile.md`, or `article-digest.md` — never in `modes/_shared.md`.
- Never submit an application on the user's behalf.

## Agent-Specific Setup

- **Codex**: See `docs/CODEX.md`
- **Cline**: See `docs/CLINE.md`