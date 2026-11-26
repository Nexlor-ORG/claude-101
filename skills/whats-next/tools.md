# Finding and connecting their tools

You cannot sign a founder into anything. They click Connect and sign in. Your job is to
find the right connector, recommend where to start, and check it worked.

There is **no list of supported tools**. Use whatever they have.

## 1. Sort their tools into three groups

Take the tools they named in the interview, plus what you can already see is connected.
For each one, check whether Claude has a connector for it (use your connector directory or
search tool if you have one). Show the result in three short groups:

- **Already connected**
- **Ready to connect** — show the Connect option for each (use your connector suggestion
  tool if you have one; otherwise tell them: Settings → Connectors → find it → Connect)
- **No connector yet**

A tool that is connected but expired, or switched off for this conversation, is not a
failure. Say so and show how to reconnect or switch it on.

## 2. Recommend where to start

Recommend the money tools first: their accounting, and their shop if they sell online.
The dashboard's numbers (cash, profit, how fast customers pay, the goal's progress) come
from these; email and calendar alone give only a list of replies and meetings. Say it in
one line: "To get to 15 schools, start with Xero. Gmail can come next." Getting to a first
dashboard in this sitting matters more than connecting everything.

**You recommend; they decide.** If they want to connect more, help them connect more. If
they have no accounting or shop connector, carry on with what they have and say what the
dashboard will show once one is connected.

## 3. Prove each connection straight away

After they connect a tool, make one real read and tell them what you see in one line:

> Connected. I can see 43 invoices, 6 of them overdue.

When the tool is the one a goal is counted in, read the goal's number today as well and
confirm it with them: "Xero shows 11 schools with an active contract. Is that right?" Save
it as that goal's `now`. If they say it is wrong, ask what you are missing; never take a
number they type in its place. If counting needs a rule only they know, see "A goal you
cannot count yet" in `measures.md`.

When the tool is their accounting or shop, save its name as `SETUP.moneyTool`.

Then say in one line what you can see in that tool, and that nothing will be changed or
sent without their say-so. If your line looks wrong to them, believe them and look again.

If a tool they just connected is not available to you in this conversation, say: "That
worked. Start a new chat and say 'continue my What's next setup', and I'll pick up from
here." Their setup is saved in the dashboard, so nothing is lost.

## 4. Use every connected tool

Once the recommended tools are connected, look at **everything** the founder has
connected, not only what they named. Each tool that holds this business's customers,
deals, work, events or meetings gets plays (`plays.md`). Tell the founder in one line which
tools the dashboard reads and which it leaves out, with the reason, so nothing is skipped
silently.

When you read the accounting tool for the first time, check two things with the founder:

- **Old debts.** If some money owed is more than 90 days late, name the customers and ask:
  "Do you still expect to be paid by them?" Any they have written off go into
  `SETUP.writtenOff`, and are left out of money owed and every chase list. Suggest they
  ask their bookkeeper to write it off in the books too.
- **Currencies.** If invoices are in more than one currency, say you will show everything
  in their home currency, at the rate on each invoice.

## 5. The "not yet" list

For tools with no connector:

- Offer a workaround if there is one, such as an export to a spreadsheet you can read.
- Give them a ready-made message for the Claude 101 community, for example: "I use Payfast
  and there's no connector for it yet." Do not post it for them.
- Record them in `SETUP.tools.notYet`.

## What to record

In `SETUP.tools`: `connected` (names exactly as Claude shows them), `next` (ready to
connect, not yet connected, most useful first) and `notYet`.

**Connector names.** The name a play reads with, and the name in the page's declaration, is
the connector's display name, not its tool prefix: a tool listed to you as
`mcp__apollo_io__search` belongs to the connector "Apollo io" or "Apollo.io". Check it in the
connector list or directory rather than guessing; a wrong name fails without a clear error.
