# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the Planner, Coder, and Designer; breaks the work into scoped phases, delegates tasks, and integrates the result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the project and produces an implementation plan, including steps, file assignments, dependencies, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's user experience and visual design, including accessibility, information hierarchy, responsive behavior, and Project Pulse styling. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements application code and logic within the assigned scope, follows repository patterns, and validates changes. | `.github/agents/coder.agent.md` |

I am using GitHub Copilot CLI in a Codespace to orchestrate the work.
