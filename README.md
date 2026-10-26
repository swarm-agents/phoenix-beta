# Phoenix private beta: getting started with Claude Code

Phoenix researches a question with a team of AI agents and returns a report in which every claim quotes the source page it came from. In this beta you use Phoenix from Claude Code, as a connector with four tools.

You need Claude Code installed and signed in, and an invitation from the Phoenix team.

## 1. Add the connector

In a terminal, run:

```
claude mcp add --transport http phoenix https://phoenix-staging-gzuqcv2lrq-ey.a.run.app/mcp
```

## 2. Sign in

Start Claude Code, type `/mcp`, choose `phoenix` and then authenticate. Your browser opens the Phoenix sign-in page; sign in there and return to Claude Code. `/mcp` then shows `phoenix` as connected, with 5 tools.

## 3. Get your beta credits

Ask Claude:

> Use the phoenix whoami tool and show my account id.

Send that account id to the Phoenix team member who invited you. We add $20 of beta credits to your account. Until then, your account has a zero balance and runs are refused.

## 4. Research a question

Ask Claude in plain words. It calls the tools for you:

| Tool | What it does | Costs credits |
|---|---|---|
| `estimate_run` | Checks a budget against your balance and limits, and says whether a run would start | No |
| `start_run` | Starts a run on your question with the budget you give, and returns a run id | Holds the budget |
| `check_run` | Shows a run's progress and spend so far | No |
| `get_report` | Returns the finished report, short or full | No |

For example:

> Use phoenix to estimate a run with a $3 budget, then start it on this question: ...

A run takes about 15 to 25 minutes. Ask Claude to check on it every few minutes, then ask for the short report, and the full one if you want every claim and its source.

## Limits and money

- When a run starts, its budget is held from your balance. When it ends, you pay only what it spent, and the rest is returned to your balance.
- A single run can hold at most $10.
- Each connection can spend at most $50 per calendar month.
- In the beta, a run is charged at cost: what its model calls, searches and page fetches cost.

## Help

Questions or problems: contact the Phoenix team member who invited you.
