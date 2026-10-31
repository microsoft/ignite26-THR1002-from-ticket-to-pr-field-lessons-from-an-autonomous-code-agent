# Field lessons

What running a coding agent for every team taught us, and what I recommend if you build your own.

## In production today

The AzBr Code Agent is an internal solution built by [AzureBrasil.cloud](https://www.azurebrasil.cloud/) on Azure. It isn't open source, but the pattern is yours to reuse.

| Metric | Today |
|--------|-------|
| Pull requests reviewed by the agent | **100%** |
| Tickets coded end to end by the agent | **~20%** |
| Teams using it | **All teams**, one bot instance per team |
| Observability | Teams can ask about tickets, implementations, logs, and infrastructure metrics in Microsoft Teams |

**In short:** the agent reviews every PR and codes about one in five tickets.

## What we learned, and what I recommend

### 1. Vague tickets become vague PRs

The quality of the PR is capped by the quality of the ticket. An agent that starts coding from a one-line ticket will make decisions nobody asked for.

**Recommendation:** grill every ticket before any code. Make the agent list open decisions and ask a person until none are left. Then check the ticket against the codebase once more before planning.

### 2. Full autonomy erodes team trust

Teams accept an agent faster when they can see and approve what it will do before it does it.

**Recommendation:** put people on the decisions, not the keystrokes. Keep three checkpoints: resolved decisions (Grill), an approved plan (Coding), and a merge after review (Code Review).

### 3. One giant prompt doesn't scale

A single prompt that grills, plans, codes, and reviews is hard to debug and hard to improve.

**Recommendation:** split the work. Use one agent per step with a narrow job, and one sub-agent per task in the approved plan. Each piece can be tuned on its own.

### 4. Teams ask: "what is the bot doing?"

As soon as the agent works in the background, people want to know what it changed, why, and whether production is fine.

**Recommendation:** make every action visible in Teams, and make the answers queryable. Give a Foundry agent read access through MCP (ticket and implementation data, logs) and A2A (infrastructure metrics).

### 5. Review is the widest door

Reviewing a PR doesn't change any code, so the risk is low and every team can adopt it on day one. That's why review covers 100% of PRs today, while coding covers about 20% of tickets.

**Recommendation:** start with review, measure it, then grow into coding.

## The big lesson

**Normalize the process first, then automate it.** The agent works because every team now follows the same Grill → Coding → Review flow. Automating a different process for every team would have meant building a different agent for every team.

## Before you go to production

- [ ] One bot identity per team, with least-privilege access to repositories and boards
- [ ] Secrets out of prompts and config files. Prefer Managed Identity and Workload Identity Federation.
- [ ] MCP servers protected with Microsoft Entra ID. See [architecture](architecture.md#securing-the-tool-calls).
- [ ] Every agent action logged and posted where the team already works
- [ ] A people checkpoint before code is written and before it's merged
- [ ] Metrics from day one: PRs reviewed, tickets coded, plans rejected, review comments accepted
