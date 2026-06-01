# ai-memory

Shared AI memory workspace for keeping personal context in one repository so
tools like Claude, VS Code Copilot, Codex, Gemini, and Antigravity can all use
the same information.

## Folder layout

- `shared/about-me/` — profile, bio, and background
- `shared/preferences/` — communication, coding, and workflow preferences
- `shared/projects/` — active project context and notes
- `shared/reference/` — reusable docs, links, and checklists
- `tools/claude/` — Claude-specific memory or prompts
- `tools/vscode-copilot/` — VS Code Copilot-specific memory or prompts
- `tools/codex/` — Codex-specific memory or prompts
- `tools/gemini/` — Gemini-specific memory or prompts
- `tools/antigravity/` — Antigravity-specific memory or prompts

Each folder includes a `.gitkeep` file so the structure is tracked in Git and
ready to fill in.
