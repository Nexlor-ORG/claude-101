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
| `calendar-prepare` | Google Calendar | Meetings in the next two days with outside people |
| `calendar-follow-up` | Google Calendar and Gmail | A meeting last week with one or two outside people, and no email since |

Keep the ones whose tools this founder has connected and delete the rest. They are also
your worked examples for writing new ones. They have run on two founders' accounts, but
connectors change: prove each one on this founder's data (step 4 below) before keeping it.
Guests who declined never count, and a meeting with more than two outside people is
treated as delivery (a training, a webinar), not a sale to follow up.

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
  async find(read, ctx) {                   // ctx.now is a Date; ctx.setup is SETUP
    const r = await read("Xero", "get_aged_receivables", {});
    const late = /* ...pick out what matters from the real shape you saw... */;
    if (!late.length) return null;          // nothing to do: return null
    return {
      line: `Chase ${late.length} overdue invoices`,   // starts with a verb, under 60 characters
      fact: `R48,200 outstanding.`,                    // the one fact that makes it concrete
      urgent: true,                                    // from dates: due, expiring, going stale
      button: "Draft reminders",                       // four words or fewer
      prompt: "Draft polite payment reminders for my overdue invoices. Show me each draft before anything is sent."
    };
  }
}
```

Rules for what `find` returns:

- **One item per play**, or `null`. If there are several of the same thing, summarise them
  in one line and put the count in the fact.
- **`line` says what to do**, not why, how or who decided. It starts with a verb.
- **`fact` is one fact from their data.** Never a number you estimated.
- **`urgent` comes from a date in the data**, never from your opinion.
- **Owners.** When the founder has a team, each item shows who owns it: `SETUP.owners`
  maps a play's `id` to a first name, set with the founder at the weekly review. Plays do
  not decide owners.
- **`prompt` is what the founder will hand back to Claude.** Write it as their request, and
  end with "Show me … before anything is sent" whenever it would send or change something.
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
| Urgent, not important | `urgent` only | Offers it under "Claude can take this off your hands" |
| Neither | | Never shows it |

So the labels in `helps` and the founder's `focus` do the ranking. Choose them honestly.
