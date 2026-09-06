# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a custom team for
building Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, breaks work into phases, assigns explicit file scopes, manages dependencies, and verifies that the integrated result works together. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies risks and edge cases, and produces an ordered implementation plan with file assignments and validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the application logic, fixes bugs, keeps behavior deterministic and testable, and prepares the assigned runnable-app support such as the Project Pulse launch configuration. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's UI/UX direction, including information hierarchy, accessibility, responsive behavior, visual clarity, project cards, status badges, and priority treatment. | `.github/agents/designer.agent.md` |

The Orchestrator will have the Planner research and plan the work first, then
delegate implementation and design tasks to the Coder and Designer while
keeping their file scopes clear and coordinating integration.
