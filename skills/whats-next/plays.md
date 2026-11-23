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

`dashboard-template.html` ships with four, in its `PLAYS` list. They were checked against
real data from these connectors on 2026-11-23:

| Play | Reads | Notices |
|---|---|---|
| `gmail-waiting-on-you` | Gmail | People who have waited more than two days for a reply |
| `gmail-no-reply-to-you` | Gmail | Emails the founder sent that nobody answered |
| `calendar-prepare` | Google Calendar | Meetings in the next two days with outside people |
| `calendar-follow-up` | Google Calendar and Gmail | A meeting last week with no email since |

Keep the ones whose tools this founder has connected and delete the rest. They are also
your worked examples for writing new ones. (The calendar plays read attendees in Google's
usual format, which was not present in the sample checked; confirm it in step 2 below.)

## Writing plays for any other tool

Do this for every connected tool that has no ready-made plays. Aim for one to three plays a
tool. The founder should not see this work; tell them only what you found.

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
| CRM or sales | Deals with no activity; quotes about to expire | `win-customers` |
| Projects or tasks | Work for a customer that is overdue | `deliver-work` |
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
      ctx: ["All more than 60 days late", "From 3 schools"],   // up to three short facts: "The numbers behind it"
      moves: "Cash",                                   // optional; else taken from `helps`
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
- **`ctx` is up to three more facts** that show why it matters, each a few words. They
  appear under "The numbers behind it" when the item is in the lead. Say "Your goal" in
  `moves` when the item moves the founder's goal directly.
- **`urgent` comes from a date in the data**, never from your opinion.
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
