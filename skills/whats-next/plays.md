# Plays: what the dashboard can notice

A **play** is one thing the dashboard knows how to notice in a founder's tools and suggest
they do. "Three of your biggest customers have gone quiet" is a play. So is "two people
have waited more than two days for a reply".

Plays are run in two places from one description:

- **You run them now**, in this conversation, on the founder's real data.
- **The dashboard runs them** every time it opens.

So a play is written once, as a small piece of JavaScript in the dashboard page that you
can also read and follow by hand. When the page opens, nothing is decided by Claude: the
plays and the ranking are fixed, which is why the dashboard is instant and says the same
thing twice.

## Ready-made plays

`dashboard-template.html` ships with three, in its `PLAYS` list:

| Play | Reads | Notices |
|---|---|---|
| `gmail-waiting-on-you` | Gmail | People who have waited more than two days for a reply |
| `calendar-prepare` | Google Calendar | Meetings in the next two days with one or two outside people |
| `calendar-follow-up` | Google Calendar and Gmail | A meeting last week with one or two outside people, and no email since |

Keep the ones whose tools this founder has connected and delete the rest. They are also
your worked examples for writing new ones. They have run on two founders' accounts, but
connectors change: prove each one on this founder's data (step 4 below) before keeping it.
Guests who declined never count. Teammates are found from the founder's own entry on each
event (`ownDomain`), not the calendar's name. A meeting with more than two outside people is
delivery (a training, a webinar), and a repeating meeting is a routine check-in: neither is a
sale to prepare for or follow up.

## Writing plays for any other tool

Do this for **every connected tool that holds this business's customers, deals, work,
events or meetings**, not only the ones the founder named or you recommended. A tool you
skip is one line in your summary to the founder, with the reason ("Canva: nothing in it
needs you this week"). Aim for one to three plays a tool. The founder should not see the
work; tell them only what you found.

- **Other businesses.** A CRM or task tool may also hold work for other companies or
  clients the founder serves. Ask once which spaces, lists or pipelines belong to this
  business, and read only those.
- **Text answers.** Some connectors answer in text, not JSON. `read` then returns the text;
  parse it in the play, and prove the parse on the real answer.
- **Find lists wherever they are.** Connectors nest their lists differently (Zoho Books puts
  invoices a level deeper than Xero). Use `ctx.findList(answer, "invoices")` rather than a
  fixed path, so a play still works when the shape shifts.
- **Read little.** Ask for the fields and page size you need, and narrow by date. A whole
  calendar or invoice list can run past 100,000 characters.
- **The founder's own work.** In a task tool, read only the founder's own tasks for this
  business. Skip tasks named for someone else (a client's action logged against the founder)
  so overdue counts are honest.
- **Values you can't see.** If some records lack the number a play adds up (deals with no
  value), say how many in the fact ("2 more quiet deals have no value in Attio, so they
  aren't in the total"), never leave them out silently.

1. **Read what the tool offers.** Look at its tools and their descriptions. Use only tools
   that read. A play never changes, sends or creates anything.
2. **Make a real request** on the founder's data and look at exactly what comes back: the
   field names, how dates and money are written, what an empty answer looks like. Never
   guess a field name.
3. **Write the play** in the format below.
4. **Prove it.** Follow your own `find` by hand against the real answer you just got. Keep
   the play only if the request worked and the answer had the shape you coded for. A play
   that correctly finds nothing today (invoices exist, none overdue) is proven: keep it.
   A play you could not prove is left out.

Ideas, by kind of tool:

| Kind of tool | Worth noticing | Label |
|---|---|---|
| Accounting | Invoices overdue; a top customer with no invoice lately | `get-paid`, `keep-customers` |
| Online shop | Orders not yet sent; best customers who have stopped ordering | `deliver-work`, `keep-customers` |
| CRM or sales | Open deals with no contact in 14 days; quotes about to expire | `win-customers` |
| Projects or tasks | The founder's own overdue tasks for this business | `deliver-work` |
| Events or webinars | An event in the next three weeks with seats to fill | `win-customers` |
| Meeting notes or recordings | Something the founder promised in a meeting and has not sent | `keep-customers` |
| Community platform | New members who have not been welcomed; members about to lapse | `keep-customers` |
| Email, calendar | See the ready-made plays | |

## The format

```js
{
  id: "xero-overdue-invoices",              // unique, lowercase, tool first
  what: "Invoices more than 30 days overdue.",   // one plain sentence
  helps: ["get-paid"],                      // labels from interview.md
  tools: [["Xero", "get_aged_receivables"]],     // every [connector name, tool name] it reads
  clear: "Xero: no invoices overdue",           // shown under "Also checked" when it finds nothing
  async find(read, ctx) {                   // ctx.now is a Date; ctx.setup is SETUP
    const r = await read("Xero", "get_aged_receivables", {});
    const late = /* ...pick out what matters from the real shape you saw... */;
    if (!late.length) return null;          // nothing to do: return null
    return {
      line: `Chase ${late.length} overdue invoices`,   // starts with a verb, under 60 characters
      fact: `R48,200 outstanding.`,                    // the one fact that makes it concrete
      urgent: true,                                    // from dates: due, expiring, going stale
      doHere: true,                                    // Claude can draft this on the page
      doButton: "Draft reminders here",                // four or five words
      prompt: "Task: Chase 4 overdue invoices, R48,200 in total.\n\nFind: the overdue invoices in Xero and the billing contact for each.\n\nWrite: one polite reminder per customer, listing its invoices and total. Signed Thandi."
    };
  }
}
```

Rules for what `find` returns:

- **One item per play**, or `null`. If there are several of the same thing, summarise them
  in one line and put the count in the fact.
- **`line` says what to do**, not why, how or who decided. It starts with a verb.
- **`fact` is one fact from their data.** Never a number you estimated.
- **`urgent` comes from a date in the data**, never from your opinion. **`due`**
  (`YYYY-MM-DD`) is the date that makes it urgent, when there is one: equal items are shown
  in due-date order.
- **`clear`** says what the play checked and how far it looked, for the "Also checked,
  nothing needed" line: "Gmail, Primary inbox, last 14 days: no one is waiting on a reply
  from you". Then a founder can tell "nothing found" from "not looked".
- **Owners.** When the founder has a team, each item shows who owns it: `SETUP.owners`
  maps a play's `id` to a first name, set with the founder at the weekly review. Plays do
  not decide owners.
- **`prompt` is the full request**, so Claude in a new chat (or on the page) needs no
  answers first. Write it in the founder's voice, in this shape:
  `Task: …` (what, with names, amounts, dates and why it matters) `\n\nFind: …` (which tool
  to look in for what) `\n\nWrite: …` (what to write, the tone, the length, "signed
  [first name]"), or `Give me: …` for a brief or a report. The page adds `SETUP.about` in
  front and the closing instruction (no questions first, list assumptions, never send).
- **`doHere: true`** when Claude can do the whole job on the page by reading the founder's
  tools and drafting: emails, a post, a brief. Then `doButton` names it ("Draft the 3
  follow-ups here"). Leave it out for bigger work that needs a full chat (research, a plan),
  and give a `button` instead. **`kind: "brief"`** for a brief or report rather than drafts.
- **`goal`** (optional) is the tag saying which goal it moves. Leave it out and the page
  works it out from `helps` and each goal's `helps`.
- **Names only.** Put a person's or customer's name in `line`, `fact` or `prompt`; never
  paste the text of an email, note or document into them. Pass names through `clean()`.
- **`link`** may replace `prompt` when the right next step is simply to open something.
- The connector name in `read(...)` and in `tools` is the connector's name exactly as Claude
  shows it, and the tool name is exact too.
- A play may read more than one tool (see `calendar-follow-up`). Keep it to a handful of
  requests.

## How the dashboard ranks them

You do not rank; the page does, with the Eisenhower Matrix:

| | The item is | The dashboard |
|---|---|---|
| Urgent and important | `urgent`, and `helps` something in the founder's `focus` | Shows it first |
| Important, not urgent | helps their `focus` | Shows it next |
| Urgent, not important | `urgent` only | Offers it under "Needs attention" as work Claude can take |
| Neither | | Never shows it |

Equal items go in `due` order, then play order. The top three show under "This week"; the
rest wait behind "1 more this week", never silently cut.

So the labels in `helps` and the founder's `focus` do the ranking. Choose them honestly.
