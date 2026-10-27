# Phoenix private beta: getting started with Claude Code

Phoenix researches a question with a team of AI agents and returns a report in which every claim quotes the source page it came from. In this beta you use Phoenix from Claude Code, as a connector with six tools.

You need Claude Code installed and signed in, and an invitation from the Phoenix team.

## 1. Add the connector

In a terminal, run:

```
claude mcp add --transport http phoenix https://phoenix-staging-gzuqcv2lrq-ey.a.run.app/mcp
```

## 2. Sign in

Start Claude Code, type `/mcp`, choose `phoenix` and then authenticate. Your browser opens the Phoenix sign-in page; sign in there and return to Claude Code. `/mcp` then shows `phoenix` as connected, with 6 tools.

## 3. Get your beta credits

Ask Claude:

> Use the phoenix whoami tool and show my account id.

Send that account id to the Phoenix team member who invited you. We add $20 of beta credits to your account. Until then, your account has a zero balance and runs are refused.

Buying credits yourself with `buy_credits` is not switched on yet; until it is, the team adds your credits.

## 4. Research a question

Ask Claude in plain words. It calls the tools for you:

| Tool | What it does | Costs credits |
|---|---|---|
| `whoami` | Shows your account id, monthly cap and per-run limit | No |
| `estimate_run` | Checks a budget against your balance and limits, and says whether a run would start | No |
| `start_run` | Starts a run on your question with the budget you give, and returns a run id | Holds the budget |
| `check_run` | Shows a run's progress and spend so far | No |
| `get_report` | Returns the finished report, short or full | No |
| `buy_credits` | Returns a Stripe payment link for $10, $20 or $50 of credits. Not switched on yet: during the beta, the team adds your credits | Only if you pay |

For example:

> Use phoenix to estimate a run with a $3 budget, then start it on this question: ...

A run takes about 15 to 25 minutes. Ask Claude to check on it every few minutes, then ask for the short report, and the full one if you want every claim and its source.

## Example questions

Phoenix works best on questions with facts that can be checked against sources: numbers, dates, names, comparisons across several sources.

- Which obligations of the EU AI Act apply to providers of general-purpose AI models, from which dates, and what are the maximum fines?
- What capital requirements does MiCA set for crypto-asset service providers, by class of service, and which national regulators have published guidance on them?
- Which countries have rules on the use of AI in hiring, what do they require of employers, and since when?
- What subsidies are available in 2026 for installing a heat pump in Germany, Austria and the Czech Republic, how much do they pay, and who is eligible?
- How do the published uptime commitments and service credits of the largest cloud object-storage services compare?

A budget of $3 to $5 suits most questions; a broader question can use up to the $10 per-run limit.

## What you'll see

`check_run` and `get_report` show the run's state, one of:

| State | Meaning |
|---|---|
| `starting` | The run is waiting to begin |
| `running` | Agents are researching your question |
| `finishing` | Research has stopped; the report is being written and the spend settled |
| `finished` | The run is over and its spend is settled |
| `cancelled` | The run did not start |

They also show the spend. While the run is going ($Y is the spend so far, $B your budget):

> Spent so far: $Y of $B. Unspent budget returns to your balance when the run finishes.

When it has finished ($X is the unspent part):

> The run spent $Y of your $B budget. $X was unspent and has been returned to your balance.

Once a run has stopped, its stop reason says why, in one of these words:

| Stop reason | Meaning |
|---|---|
| finished: question answered | The agents checked enough claims to answer the question |
| finished: budget used | The budget ran out |
| finished: time limit reached | The run reached its time limit |
| finished: maximum research rounds reached | Phoenix researches in rounds, up to 12 per run, and this run used all of them |
| finished: no further research worth its cost | No research left was worth what it would cost |
| finished: all planned research done | Every planned piece of research was done |
| finished: interrupted before it was done | The run was interrupted by a problem on our side |
| cancelled: the run did not start, so nothing was charged | The run could not be started |

A run that stops on its budget also says:

> Part of the budget is held back to cover work still in progress, so a run may finish before the whole budget is spent.

The report starts with your question, the budget, the spend, the run time and how many sources it quotes.

## Limits and money

- When a run starts, its budget is held from your balance. When it ends, you pay only what it spent, and the rest is returned to your balance.
- Per-run limit: a single run can hold at most $10.
- Each connection can spend at most $50 per calendar month.
- In the beta, a run is charged at cost: what its model calls, searches and page fetches cost.

## Beta terms

- This is a beta. There is no uptime promise, and runs may fail.
- A run that fails to start costs nothing. A run that stops early is charged only what it spent.
- We keep your questions, reports, sources and spend for the duration of the beta. Only the Phoenix team can read them, and we delete them on request.
- The $20 beta credit is free, has no cash value and expires when the beta ends.
- Your question and the pages it leads to are processed by these providers: OpenRouter (which routes the model calls), Anthropic, Amazon Bedrock, Google, DeepInfra, CoreWeave and AtlasCloud (which run the models), Exa (search) and Firecrawl (page fetching). Default routing is not limited to EU regions.
- Your account, questions and reports are stored in Frankfurt (Neon, Google Cloud); sign-in is handled by WorkOS.

## Help

Questions or problems: contact the Phoenix team member who invited you.
