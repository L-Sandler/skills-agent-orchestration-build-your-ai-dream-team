# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard, with GitHub Copilot CLI in a Codespace coordinating the work:

- **Orchestrator** — **Claude Opus 4.7 (copilot)**. Breaks the work into phases, delegates scoped tasks to specialists, coordinates dependencies, and verifies the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- **Planner** — **Claude Opus 4.7 (copilot)**. Researches the repository, dependencies, documentation, and edge cases, then creates an implementation plan without writing code. Definition: `.github/agents/planner.agent.md`.
- **Designer** — **Gemini 3.1 Pro (copilot)**. Shapes the dashboard's user experience, accessibility, information hierarchy, responsive behavior, and visual styling. Definition: `.github/agents/designer.agent.md`.
- **Coder** — **GPT-5.5 (copilot)**. Implements assigned application logic and runnable-app support with clear, testable code, then validates the changes. Definition: `.github/agents/coder.agent.md`.

The Orchestrator will use the Planner's plan to assign non-overlapping work to the Designer and Coder, and I will direct and oversee the team through GitHub Copilot CLI in a Codespace.
