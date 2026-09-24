# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team orchestrated through GitHub Copilot CLI in a Codespace.

- Orchestrator — Model: Claude Opus 4.7 (copilot) — Responsibilities: coordinates the full workflow, breaks work into phases, assigns clear file scopes to specialists, runs tasks in the right order, and verifies that the integrated result holds together. Definition: `.github/agents/orchestrator.agent.md`
- Planner — Model: Claude Opus 4.7 (copilot) — Responsibilities: researches the codebase and docs, identifies constraints and edge cases, and produces an ordered implementation plan with file ownership, dependencies, and validation expectations. Definition: `.github/agents/planner.agent.md`
- Coder — Model: GPT-5.5 (copilot) — Responsibilities: implements code, fixes bugs, and writes logic inside the assigned file scope, including runnable app support when needed for the dashboard preview. Definition: `.github/agents/coder.agent.md`
- Designer — Model: Gemini 3.1 Pro (copilot) — Responsibilities: shapes the UI/UX, accessibility, information hierarchy, responsiveness, and the polished visual language for the Project Pulse dashboard. Definition: `.github/agents/designer.agent.md`

This team is managed from the GitHub Copilot CLI in a Codespace, with the Orchestrator delegating work to the Planner, Coder, and Designer while keeping the build aligned with the project goals.
