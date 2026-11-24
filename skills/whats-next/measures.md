# Measures: the numbers on the dashboard

A **measure** is one number under Business health or Growth: cash in the bank, how fast
customers pay, profit on each sale. Each one comes with a verdict (Healthy, Watch or Act
now), one line that says what it means, and "How it's worked out" with the sum behind it.
That is how a founder learns what the number is and trusts that nothing was invented.

Measures work like plays (`plays.md`): written once as JavaScript in the dashboard's
`MEASURES` list, run by you now to make the snapshot, and run by the page every time it
opens. No measure is ready-made: accounting and shop connectors differ too much. You write
each one from what the founder's own tool returns, and keep only what you can prove.

## Which numbers, by how the business makes money

Read `SETUP.model`. Business health is the same for every model; Growth depends on it.
Show at most five in each group. Use the founder's `customerWord` in every label: "How
fast schools pay", "Cost to win a café".

**Business health** (from the accounting tool):

| `id` | Label | Worked out as | Healthy / Watch / Act now |
|---|---|---|---|
| `cash` | Cash in the bank | Bank balance today ÷ average monthly running costs over the last 3 months, in months | 3 months or more / 1 to 3 / under 1 |
| `debtor-days` | How fast [customers] pay | What customers owe ÷ last month's sales × 30, in days. Only for `invoices` and `services` | Within `SETUP.terms` + 7 days / up to `terms` + 15 / more |
| `gross-margin` | Profit on each sale | (Sales − cost of sales) ÷ sales, last full month | Steady or rising / fell 2 points or more in 3 months / fell 5 points or more |
| `net-profit` | Profit after all costs | Sales − every cost, last full month, before tax | Profit / under 5% of sales / a loss |
| `break-even` | Sales needed to cover costs | Monthly running costs ÷ gross margin | Sales 10% or more above it / within 10% / below it |
| `stock-days` | Stock on the shelf | Stock value ÷ cost of sales per day. Only for `online-shop` and `walk-in` | Under 45 days / 45 to 90 / over 90; a fresh product uses the founder's own number |

**Growth** (from accounting, shop or CRM):

| Model | Measures |
|---|---|
| `invoices` | Cost to win a [customer], profit from a [customer] over its life, quotes that win, [customers] that stay |
| `online-shop` | Cost to win a customer, profit from a customer over time, visitors who buy, average order, customers who order again |
| `walk-in` | Average sale, customers who come back (only if the till records them) |
| `services` | Work booked ahead, profit per project, [clients] who come back |
| `subscription` | Revenue each month, customers who leave, cost to win a customer |

Verdicts for Growth:

- **Profit from a customer over its life** compared with **cost to win one**: 3 times or
  more is Healthy, 1 to 3 is Watch, under 1 is Act now. Say the ratio in the note.
- **Everything else** is judged against the founder's own last three months: steady or
  better is Healthy, worse for three months running is Watch. Never compare a founder to
  an industry average you cannot cite.

## Rules

- **Never estimate.** A measure the data cannot give is left out, not guessed. Five honest
  numbers beat nine shaky ones.
- **Books behind** (`SETUP.books.current` is not `true`): still show the numbers, but every
  `note` ends "Books up to <month>." and nothing from the accounting tool is Act now.
- **Cost of sales and running costs** come from how the profit and loss report groups its
  accounts. If it does not separate them, ask the founder once which accounts are the cost
  of making a sale, and write the answer into the measure.
- **Marketing spend** is the accounts whose names say advertising, marketing or promotion.
  If there are none, leave out every measure that needs it.
- **`how` shows the real sum** with the real numbers: "R212,000 ÷ R104,000". Plain words,
  no formulas, no jargon. Say the thresholds in the same sentence.
- **`note` says what it means in one line**, with the comparison that matters: "Up from 38
  days in August. R552,000 is waiting to be paid."
- **`alert`** is required when the verdict can be Watch or Act now: the number as a plain
  sentence ("Schools take 52 days to pay you"). It heads the number's card under Needs
  attention. Only Watch and Act now numbers appear there; the rest wait behind "See all
  numbers".
- **`helps`** uses the focus labels from `interview.md`. A measure and a play that share a
  label are linked: when the measure is Act now, its card offers that play's work.
- **Money** is written in the founder's currency, whole units, thousands with commas:
  "R212,000".
- Only reads. A measure never changes anything in a tool.

## The goal

If the goal is counted in a connected tool, write one more measure with `group: "goal"`.
It returns `now` (the number today), and when there is history, `ratePerMonth` (how much
it has moved each month, averaged over the last three to six months). For a money goal it may also return `perCustomer` (the average monthly
value of one customer), which gives "One more client takes you to …". The page works out
the pace, "Behind pace" or "On pace", and the progress bar from these and `SETUP.goal`.
Without a `ratePerMonth` the page shows the pace needed and no verdict.

## The format

```js
{
  id: "debtor-days",
  group: "health",                       // "health", "growth" or "goal"
  label: "How fast schools pay",
  helps: ["get-paid"],
  tools: [["Xero", "get_aged_receivables"], ["Xero", "get_profit_and_loss"]],
  async find(read, ctx) {                // ctx.now is a Date; ctx.setup is SETUP
    const owed = await read("Xero", "get_aged_receivables", {});
    const pl = await read("Xero", "get_profit_and_loss", { /* the period, as the tool asks */ });
    const total = /* ...from the real shape you saw... */, sales = /* ... */;
    if (!sales) return null;             // the data is not there: leave the measure out
    const days = Math.round(total / sales * 30), terms = ctx.setup.terms || 30;
    return {
      value: `${days} days`,
      tone: days <= terms + 7 ? "good" : days <= terms + 15 ? "watch" : "act",
      alert: `Schools take ${days} days to pay you`,
      note: `${money(total)} is waiting to be paid.`,
      how: `What customers owe you, divided by last month's sales, times 30 days: ${money(total)} ÷ ${money(sales)} × 30. Your terms are ${terms} days.`,
      period: "October · from Xero"      // the first measure in a group sets the group's line
    };
  }
}
```

`money(n)` is available in the page for writing rand amounts.

## Proving a measure

Do this for each one, as for plays:

1. Read what the tool offers, and make the real request on the founder's data.
2. Look at the exact shape that comes back. Never guess a field name.
3. Write the measure, then follow its `find` by hand on the answer you got and check the
   number makes sense: cash is not negative unless they are overdrawn, a margin is between
   −100% and 100%, days are not in the thousands.
4. Read the number back to the founder in one line before you keep it: "Your cash covers
   about 2 months of running costs. Does that sound right?" If they say no, find out why
   before you keep it.
5. Keep it only if all of that holds. Leave out anything you could not prove.

Put the proven measures in `MEASURES`, and their results in `snapshot.measures` (each with
its `id`, `group`, `label` and `helps` added).
