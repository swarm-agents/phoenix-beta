# Phoenix private beta: getting started

Phoenix researches a question with a team of AI agents and returns a report in which every claim quotes the source page it came from. In this beta you use Phoenix from Claude Code, the Claude desktop app or Grok, as a connector with eleven tools. The steps below are for Claude Code; for the desktop app, see [Using Phoenix from the Claude desktop app](#using-phoenix-from-the-claude-desktop-app), and for Grok, [Using Phoenix from Grok](#using-phoenix-from-grok). Phoenix can also chat with one model at a time, from the connector or in your browser: see [Chat in threads](#chat-in-threads) and [Using Phoenix in your browser](#using-phoenix-in-your-browser).

You need Claude Code installed and signed in (or the Claude desktop app, or a Grok account), and an invitation from the Phoenix team.

## 1. Add the connector

In a terminal, run:

```
claude mcp add --transport http phoenix https://phoenix-staging-gzuqcv2lrq-ey.a.run.app/mcp
```

## 2. Sign in

Start Claude Code, type `/mcp`, choose `phoenix` and then authenticate. Your browser opens the Phoenix sign-in page; sign in there and return to Claude Code. `/mcp` then shows `phoenix` as connected, with 15 tools.

## 3. Get your beta credits

Ask Claude:

> Use the phoenix whoami tool and show my account id.

Send that account id to the Phoenix team member who invited you. We add $20 of beta credits to your account. Until then, your account has a zero balance and runs are refused. Ask for `whoami` again at any time to see your balance.

You can also buy credits yourself with `buy_credits`. Buying credits takes a real card payment through Stripe, and the credits are added as soon as Stripe confirms it.

## 4. Research a question

Ask Claude in plain words. It calls the tools for you:

| Tool | What it does | Costs credits |
|---|---|---|
| `whoami` | Shows your account id, your balance, this connection's monthly limit and how much of it is left, and your per-run limit | No |
| `estimate_run` | Checks a budget against your balance and limits, and says whether a run would start | No |
| `start_run` | Starts a run on your question with the budget you give, and returns a run id | Holds the budget |
| `check_run` | Shows a run's progress and spend so far | No |
| `get_report` | Returns the finished report, short or full | No |
| `buy_credits` | Returns a Stripe payment link for $10, $20 or $50 of credits; they're added once Stripe confirms the payment. It's a real card payment | Only if you pay |

The other six tools: `check_models` (see [Templates](#templates)), `plan_route` (see [Plan where a prompt goes](#plan-where-a-prompt-goes)), and `send_message`, `list_threads`, `get_thread` and `rename_thread`, for chat (see [Chat in threads](#chat-in-threads)). Three more look after the apps connected to your account: `list_connections`, and `revoke_connection` and `allow_connection` for its owner. A revoked app is refused, even if it connects again, until you allow it.

For example:

> Use phoenix to estimate a run with a $3 budget, then start it on this question: ...

A run takes about 15 to 25 minutes. Ask Claude to check on it every few minutes, then ask for the short report, and the full one if you want every claim and its source.

## Using Phoenix from the Claude desktop app

Phoenix also works as a custom connector in the Claude desktop app. The same steps work on claude.ai in a browser.

1. Open the connector settings and choose **Add custom connector**.
2. Enter a name, such as `Phoenix`, and the MCP server URL: `https://phoenix-staging-gzuqcv2lrq-ey.a.run.app/mcp`. Choose **Continue**.
3. The Phoenix sign-in page opens. If you already use Phoenix from Claude Code or Grok, sign in with the same login: your credits belong to that login, and a different one starts a new account with a zero balance.
4. In a new chat, ask Claude:

   > Use the Phoenix whoami tool and show my account id and balance.

   If your balance is zero, send the account id to the Phoenix team member who invited you, as in "3. Get your beta credits" above.

Each app you connect from is its own connection, on the same account and balance. Your monthly cap ($50 to start) counts everything you start, from every app; each connection also stops at its own $50 a month, so one leaked app can't use up your month. On a Claude Team or Enterprise plan, an owner may need to allow custom connectors first.

## Using Phoenix from Grok

Phoenix also works as a custom connector in Grok.

1. Go to grok.com/connectors, choose **New Connector**, then **Custom**.
2. Enter the connector address: `https://phoenix-staging-gzuqcv2lrq-ey.a.run.app/mcp`
3. Grok opens the Phoenix sign-in page. If you already use Phoenix from Claude Code, sign in with the same login: your credits belong to that login, and a different one starts a new account with a zero balance. New to Phoenix? Sign in with any of the options and keep using that one.
4. Ask Grok:

   > Use the phoenix whoami tool and show my account id and balance.

   If your balance is zero, send the account id to the Phoenix team member who invited you, as in "3. Get your beta credits" above.

On Grok Business or Enterprise, a team admin must add the connector before members can use it.

## Example questions

Phoenix works best on questions with facts that can be checked against sources: numbers, dates, names, comparisons across several sources.

- Which obligations of the EU AI Act apply to providers of general-purpose AI models, from which dates, and what are the maximum fines?
- What capital requirements does MiCA set for crypto-asset service providers, by class of service, and which national regulators have published guidance on them?
- Which countries have rules on the use of AI in hiring, what do they require of employers, and since when?
- What subsidies are available in 2026 for installing a heat pump in Germany, Austria and the Czech Republic, how much do they pay, and who is eligible?
- How do the published uptime commitments and service credits of the largest cloud object-storage services compare?

A budget of $3 to $5 suits most questions; a broader question can use up to the $10 per-run limit. The smallest budget a run accepts is $2.25: below that, a run cannot afford to check what it finds.

Each run follows a fixed plan: one call splits your question into topics and questions, research agents search the web and quote what they find, an independent model checks each finding against its quote, findings that disagree are settled, and the report is written. Phoenix sizes each step from your budget and the time limit, and returns what it does not spend.

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
| finished: maximum research rounds reached | Phoenix researched in rounds, up to 12 per run, and this run used all of them (only when Phoenix plans in rounds; today's runs follow a fixed plan) |
| finished: no further research worth its cost | No research left was worth what it would cost |
| finished: all planned research done | Every planned piece of research was done |
| finished: interrupted before it was done | The run was interrupted by a problem on our side |
| finished: the research planner failed twice; the report covers the work done | The step that plans the research failed twice; Phoenix wrote the report from what was already found |
| cancelled: the run did not start, so nothing was charged | The run could not be started |

A run that stops on its budget also says:

> Part of the budget is held back to cover work still in progress, so a run may finish before the whole budget is spent.

The report starts with your question, the budget, the spend, the run time and how many sources it quotes. If the budget left was too small to check some findings at their worst-case price, it also says how many checks were skipped; those findings are unchecked.

### How this research was done

Every run keeps a step-by-step record of how Phoenix researched your question, built from the run's own records: what it planned, which agents it sent, what each agent found, what the independent check confirmed, which disagreements it found and how they were settled, and why it stopped. Each step has the minute it happened, counted from the start of the run.

- While a run is going, `check_run` shows the latest few steps.
- The short report sums up the process in a few lines.
- The full report ends with a section called "How this research was done" that lists the steps. A very long run shows its first and last steps and says how many are not shown.

Each step is one of these kinds:

| Kind | What it records |
|---|---|
| `planned` | How the question was split into workstreams |
| `decided` | Only when Phoenix plans in rounds, which today's runs do not. Each research round: what was known, how many important findings were verified, how many important questions were still open, the budget and time left, and how many proposed agents were sent and why the others were not, or why the research stopped |
| `sent` | An agent sent to work, with its search and its estimated cost |
| `returned` | An agent finished: how many pages it read, how many findings it made with quotes, and how many quotes were dropped because they were not found word for word on the page |
| `checked` | An independent model checked an agent's findings against their quotes |
| `disagreement` | Two findings disagreed, and how the disagreement was settled |
| `stopped` | The research stopped, and why |
| `assembled` | The report was written: how many findings it has and how many sources it quotes |

## Templates

Ask your assistant which AI models meet a set of rules, and it calls Phoenix's `check_models` tool. Phoenix keeps a catalogue of models and the hosts that run them, and checks them against one of five templates or rules you give:

- EU processing (`eu_processing`): Your requests are processed only inside the EU.
- Confidential (`confidential`): The host neither trains on your data nor keeps it.
- EU regulated (`eu_regulated`): EU processing, no training on your data, and a developer not headquartered in China.
- Open weights (`open_weights`): A model whose weights are published and whose licence allows commercial use.
- Budget ceiling (`budget_ceiling`): A low-cost model: at most $0.50 in and $1.50 out per million tokens.

Every result shows the rules in plain words and, for each model on each host, each rule's outcome with the quote it rests on and the date that value was last checked:

- **pass**: the catalogue holds a statement for this case, and its value meets the rule.
- **fail**: the catalogue holds a statement for this case, and its value breaks the rule.
- **unknown**: the catalogue holds no usable statement for this case; an unknown counts as a fail (the check fails closed), so a model is in only when every rule passes.

## Plan where a prompt goes

Before you send a chat message, you can see where it would go and what it would cost. Ask your assistant, for example:

> Use phoenix plan_route for this prompt, with replies of up to 2,000 tokens: ...

Phoenix answers with three choices, each with the model, the host that would run it and the most the call could cost:

- **Balanced**: the lowest price among the models Phoenix lists as top quality for this kind of task.
- **Cheapest**: the lowest price that passes your rules.
- **Private**: the cheapest host that neither trains on your data nor keeps it.

Each price is the most the call can cost: every token of your prompt plus the longest reply you allow, rounded up to a whole cent. For each model left out, Phoenix says which rule left it out. It also lists every other model and host that passes, with the most each could cost, cheapest first, so you can choose one yourself. You can add a template or your own rules, as with `check_models`.

`plan_route` is free. It calls no model and sends your prompt nowhere, and Phoenix neither stores nor logs the prompt. Given a thread, it prices your message as it would be sent in that thread, with the thread's recent messages.

### Earlier messages in the price

In a thread, the plan says how many of the thread's earlier messages your message would carry (`carried`): 0 in a new thread, and in a long thread only the newest that fit. Each route also says how much of its price is for those earlier messages (`carried_cents`, rounded up to a whole cent). That share is part of the route's price, never added to it.

### Personal data

Phoenix checks your prompt for personal data and reports the kinds it found and how many of each, never the words themselves. It looks for seven kinds: IBANs, card numbers, email addresses and phone numbers, found by fixed rules, and people's names, postal addresses and organisations, found by a small model that may miss some. If that model does not answer in time, the plan says names, postal addresses and organisations were not checked. In a thread, the check also counts what the earlier messages your message carries held, replies included. Phoenix does not change your message, and it can miss things.

What happens when personal data is found depends on your privacy policy, one of three:

- **Just tell me** (`tell`): the plan shows what it found, and your route stays as you chose it.
- **Suggest Private** (`suggest`, the default): the plan shows what it found, recommends Private and lists it first. You can still send on another route.
- **Always use Private** (`always`): when personal data is found, or names, postal addresses and organisations could not be checked, only routes that pass the Private rules can take the message. The plan marks every other route as not selectable and says why, and `send_message` refuses them. If no private route can take the message, it is not sent.

Your policy applies to your messages in every thread, including threads other people own. You set it in the web app (see **Your policy** under [Using Phoenix in your browser](#using-phoenix-in-your-browser)); an assistant connected to your account follows it and cannot change it.

### Alerts

Each plan also has `alert`: at most one thing worth telling you before you send, or nothing. It is judged on the route you have chosen (the first route the plan lists when you haven't chosen one). Today an alert is one of:

- **Personal data in earlier messages**: in a thread, earlier messages that your message would carry held personal data of a kind your policy counts, and the route you chose does not pass the Private rules. The alert says that route will receive those earlier messages too, and offers to stay on Private or send anyway.
- **Private costs no more**: no personal data was found, and Private's "at most" is no higher than the route you chose. The alert shows both prices.
- **An offer from your own choices**: under Suggest Private, if you followed the Private recommendation at least 9 of your last 10 times, Phoenix offers to always use Private; if you marked a kind as not personal data at least 8 times in 30 days, it offers to stop checking for that kind. It is only an offer: your policy changes only if you confirm it, in the web app. Ignoring it changes nothing.

An alert names kinds and counts only, never what was found. Each alert is given once: the first two once per thread, an offer once. Until then, every plan repeats it. When your assistant has shown you an alert and you send, it passes the alert's id with the message (`alerts_shown`), so you are not told again.

## Chat in threads

Chat sends your message to one model and returns its whole reply. It suits a quick answer; for a question that needs claims with sources and quotes, start a research run instead. Each conversation is a thread, and your threads are private to you until you share one (see **Sharing a thread** below).

| Tool | What it does | Costs credits |
|---|---|---|
| `send_message` | Sends a message on the route you choose and returns the reply with its proof. Without a thread id it starts a new thread; with one, it continues that thread | Holds the route's "at most", then charges what the provider billed, rounded up to a whole cent |
| `list_threads` | Lists your threads, newest first, with each one's last model and what its replies were charged, and what all your threads were charged this calendar month (UTC), with how many of this month's costs are not known yet | No |
| `get_thread` | Shows a thread's messages, oldest first, each reply with its proof | No |
| `rename_thread` | Renames one of your threads (its owner only; a thread shared with you can't be renamed) | No |

How a message is charged:

- **The route.** You choose one of the three routes `plan_route` shows: Balanced, Cheapest or Private. Or you choose another model and host from the list `plan_route` shows. Phoenix plans it again when the message is sent, so the model, the host and the price are for exactly what is sent; if the model you chose no longer passes or has no current price, the message is refused and nothing is charged.
- **The hold.** Before the message is sent, the route's "at most" is held from your balance: every token sent plus the longest reply you allow (4,096 tokens unless you ask for another length, up to 8,192).
- **The charge.** When the reply is done, you are charged what the provider billed for it, rounded up to a whole cent, never more than the hold. So the smallest charge is $0.01, even for a reply the provider billed at a fraction of a cent. The rest of the hold returns to your balance.
- **Not personal data.** If Phoenix flagged something that is not personal data, you can say so when you send: `not_personal_data` for the whole message, or `not_personal_data_kinds` for only some kinds (any of `iban`, `card`, `email`, `phone`, `name`, `address`, `organisation`). Phoenix records the kinds you marked with the message, never the words, for that message only; your policy is unchanged. Under Always use Private a mark is ignored.
- **Alerts you were shown.** `alerts_shown` lists the ids of the plan's alerts you saw before sending, so they are recorded and not given again (see [Alerts](#alerts)).
- **The proof.** Every reply has a proof: the model, the host that ran it, what was held and what was charged.
- **A cost not known yet.** If a reply's cost is not known yet (for example, the reply was cut off or the provider answered with an error), its whole hold stays held until the cost arrives. If it has not arrived after 7 days, the hold is charged, and the proof says so. If the provider could not be reached, nothing is charged.
- **Continuing a thread.** The model sees the thread's recent messages as well as your new one, and the price covers the whole conversation sent, not only the new message. So each message in a long thread costs more than the first. In a very long thread, the oldest messages are left out. `plan_route` shows how many earlier messages are carried and their share of each price (see [Earlier messages in the price](#earlier-messages-in-the-price)).

Ask your assistant, for example:

> Use phoenix plan_route for this prompt, then send it with send_message on the Cheapest route: ...

> Continue my last phoenix thread on the Private route: ...

> Use phoenix plan_route for this prompt, then send it with send_message to Mistral Large from the list: ...

> Use phoenix list_threads and show what each thread has cost.

> Use phoenix get_thread on my last thread and show each reply's proof.

## Using Phoenix in your browser

Phoenix also has a web app for chat: https://phoenix.swarm.app/app/

- **Sign in.** Choose **Continue with Google**, with the same Google account you use for the connector: it is the same login, so you see the same balance. A different account starts a new one with a zero balance. You stay signed in when you reload the page, open a link to a thread, or come back from paying. You're signed out after 12 hours without using the web app, after 7 days in any case, or when you choose **Sign out** in the side bar.
- **Home.** Type your prompt. Under it, the **Run plan** shows the three routes, each with its price, model and host. It appears once your prompt is complete (a few words, not ending mid-phrase) and you pause typing, and updates as you change it. It also says whether your prompt holds personal data, as kinds and counts; when it does ("personal data found"), what happens follows your policy (see **Your policy** below): under Suggest Private, the default, Private is preselected. You can mark a kind **Not personal data**, for this message only; your policy is unchanged. The plan's alert, when there is one (see [Alerts](#alerts)), shows in the composer; sending after it, or dismissing it, records it, so it is not shown again. Balanced is selected unless Private is recommended; choose a route and press **Run**. In the web app, a route's "at most" is called the estimate, and the plan says so: "the charge never exceeds the estimate". The reply opens in a new thread.
- **Thread.** Your messages and the replies, oldest first. Under each reply, a line shows what was estimated and what was charged. Choose **proof** on that line, or **Proof** at the top, to open the Proof panel: each reply's model, host, status, estimate and charge, and the thread totals (**Charged**, **Estimated** and **Models**). When the model changes between replies, a handoff line marks it. To continue the thread, type in the box at the bottom; its run plan prices your message with the thread. Beside the model, a line such as "includes 6 earlier messages (≤ $0.31)" says how many earlier messages your message carries and the most they add to the price, which already includes them; each route under **Other models** has its own line. After the first reply, Phoenix gives a new thread a short title that says what you asked, at no charge to you. To rename a thread you own, click its title at the top, type the new name and press Enter (Esc keeps the old one); Phoenix keeps the name you choose and never replaces it.
- **Threads list.** **Recent** in the side bar lists your threads with the last model, the cost and when each was updated. **Search threads** covers titles and messages, yours and the replies, in your threads and threads shared with you. It shows one row per thread, newest match first, with a short piece of the matching message and who wrote it; a shared thread says who shared it. Search is in the web app only. A cost not known yet is marked.
- **Billing and credits.** Open it from your balance at the top or your name in the side bar: your balance, what you've spent this month against your monthly cap (every research run and reply you started, from any app), the per-run cap, and spend by member. **Add credits** (also at the top of Home) pays through Stripe's own page, and you come back to Billing with the credits added. It's a real card payment; Stripe may show and charge it in your own currency, and the credits are the dollar amount you chose. As the workspace's owner you can change your monthly cap; runs and replies stop at it, and it resets on the 1st (UTC).
- **Sharing a thread.** In a thread, choose **Share**. Type someone's email address and choose **Share**: they can read the whole thread, including earlier prompts, and prompt in it. A thread can be shared with up to 10 people, and you can share with up to 20 people a day. They get an email from noreply@mg.swarm.app with a link to the thread, and must sign in with that email address to open it. Under **General access**, **Anyone with the link** gives you a link to copy instead: anyone who signs in with it can join, until it expires (7 days), you choose **Reset link** (which makes a new link and turns the old one off) or you switch back to **Private**. To remove someone, set their role to **Remove**. Threads shared with you are under **Shared with me** in the threads list; to leave one, open **Share** in it and choose **Leave thread**.
- **Who pays in a shared thread.** Each person pays for their own prompts from their own balance, never the owner's. So someone you share with needs credits of their own to prompt: with a zero balance they can read the thread, but their prompts are refused until they add credits. Each reply's line says whose balance it was charged to.
- **Your policy.** Choose your name in the side bar, then **Settings**, to set what happens when personal data is found (Just tell me, Suggest Private or Always use Private; see [Personal data](#personal-data)), which rule set Private stands for, and which kinds count. Changes apply to your next message. Offers from your own choices (see [Alerts](#alerts)) also appear here, to confirm or keep checking.
- **Dark mode and phone.** The web app follows your device's light or dark setting, and works on a phone, where the Proof panel opens from the bottom of the screen.

The web app is its own connection, on the same account, balance and monthly cap.

## Limits and money

- Checking models against templates or rules is free: each account can make up to 100 catalogue checks per day (the count resets at 00:00 UTC).

- When a run starts, its budget is held from your balance. When it ends, you pay only what it spent, and the rest is returned to your balance.
- Per-run limit: a single run can hold at most $10.
- Your monthly cap ($50 to start) counts everything you start, from every app, per calendar month (UTC); each connection also stops at $50 a month.
- In the beta, a run is charged at cost: what its model calls, searches and page fetches cost.
- Chat counts against the same limits as research runs: a message's hold must fit within the per-run limit, and the holds of chat and runs together count against your monthly cap and the connection's own $50. A message that would go past either is refused, with the numbers, and nothing is held.
- A chat message is charged what the provider billed for it, rounded up to a whole cent, never more than its hold.

## Beta terms

- This is a beta. There is no uptime promise, and runs may fail.
- A run that fails to start costs nothing. A run that stops early is charged only what it spent.
- We keep your questions, reports, sources, chat threads and spend for the duration of the beta. Only the Phoenix team can read them, and we delete them on request.
- The $20 beta credit is free, has no cash value and expires when the beta ends.
- Your question and the pages it leads to are processed by these providers: OpenRouter (which routes the model calls), Anthropic, Amazon Bedrock, Google, DeepInfra, CoreWeave and AtlasCloud (which run the models), Exa (search) and Firecrawl (page fetching). Default routing is not limited to EU regions.
- In chat, your messages go through OpenRouter to the host the route you choose picks, which can be any host in Phoenix's model catalogue: a longer list than for research, including OpenAI, Azure, Mistral, Groq and others. Each reply's proof names its host.
- The assistant you connect from (such as Claude Code, the Claude desktop app or Grok) also sees your questions and reports, under its own terms.
- Your account, questions and reports are stored in Frankfurt (Neon, Google Cloud); sign-in is handled by WorkOS.

## Help

Questions or problems: contact the Phoenix team member who invited you.
