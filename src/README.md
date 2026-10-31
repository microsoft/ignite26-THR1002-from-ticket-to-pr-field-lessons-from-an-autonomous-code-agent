# Source

Generic GitHub Copilot custom agents that reproduce the **Grill → Coding → Code Review** flow from THR1002.

They are not the AzBr Code Agent's production prompts. They are a starting point you can adapt to your team's process.

| Agent | Step | What it does |
|-------|------|--------------|
| [`grill`](agents/grill.agent.md) | Grill | Asks about open decisions until the ticket is ready to build |
| [`ticket-readiness`](agents/ticket-readiness.agent.md) | Coding | Checks the ticket against the codebase before any code |
| [`planner`](agents/planner.agent.md) | Coding | Drafts a plan and iterates until a person approves it |
| [`implementer`](agents/implementer.agent.md) | Coding | Runs one sub-agent per task and opens one pull request |
| [`code-reviewer`](agents/code-reviewer.agent.md) | Code Review | Reviews the PR against the ticket and the codebase |

## Use them in your repository

1. Copy the files from `agents/` into `.github/agents/` in your repository.
2. Open the repository in VS Code and open Copilot Chat in agent mode.
3. Select the agent from the agent picker and run the steps in order:
   1. **grill**: paste a raw ticket, then answer the questions.
   2. **ticket-readiness**: paste the final ticket.
   3. **planner**: ask for a plan, then reply `approve` when it's right.
   4. **implementer**: execute the approved plan.
   5. **code-reviewer**: point it at the pull request and the ticket.

Learn more about [custom agents](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/create-custom-agents).

## Adapt them

- Add your team's Definition of Ready to `grill` and `ticket-readiness`.
- Add your build and test commands to `implementer`.
- Add your review checklist (security, performance, conventions) to `code-reviewer`.
- Restrict tools per agent (for example, read-only for `code-reviewer`) using the `tools` property in the front matter.

## Automate them

Once the manual flow works, trigger the same agents from Azure DevOps [service hooks](https://learn.microsoft.com/azure/devops/service-hooks/overview): use the work item state and tags to choose the step, and post every action to a Microsoft Teams channel. See [architecture](../docs/architecture.md) and [field lessons](../docs/field-lessons.md).
