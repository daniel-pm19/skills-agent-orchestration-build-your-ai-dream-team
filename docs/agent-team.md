# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team orchestrated through GitHub Copilot CLI in a Codespace.

- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the dashboard work into phases, delegate to specialist agents, manage dependencies, keep file scopes clean, and verify that the final result fits together. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the repository and requirements, identify risks and dependencies, and produce an ordered implementation plan with file assignments and validation expectations. Definition: `.github/agents/planner.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the dashboard logic, fix issues, and add any runnable app support needed for the Project Pulse app, such as a VS Code launch configuration. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shape the UX, information hierarchy, accessibility, layout, and visual polish for the Project Pulse dashboard so it reads like a finished product. Definition: `.github/agents/designer.agent.md`.

This team is coordinated with GitHub Copilot CLI inside a Codespace, so the Orchestrator can delegate work to the specialist agents while the local workflow remains structured and traceable.