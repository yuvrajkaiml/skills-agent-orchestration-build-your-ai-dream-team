# Agent team

For Mona's Project Pulse dashboard, I will use a custom four-agent team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsible for researching the repo, reading relevant files, checking docs and dependencies, and producing a practical implementation plan with file ownership, dependencies, risks, validation expectations, and open questions. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsible for coordinating the work, breaking the plan into phases, delegating tasks to specialists without writing the implementation, and verifying that the overall result fits together. Definition: `.github/agents/orchestrator.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsible for dashboard UX/UI, accessibility, information hierarchy, interaction flow, visual polish, and responsive styling for Project Pulse. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the assigned code changes, keeping the app deterministic and testable, and validating the work inside the scoped files. Definition: `.github/agents/coder.agent.md`.

This setup uses GitHub Copilot CLI in a Codespace as the orchestration layer, with the specialist agents handling planning, design, and implementation in a coordinated workflow.
