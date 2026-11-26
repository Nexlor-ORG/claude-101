# Building and publishing the dashboard

The dashboard is one page, built from `dashboard-template.html` in this folder. A founder
has exactly one, at one link, for good. Every later change updates that same page.

## Build the page

Start from the template and change only three things. Everything under "4. THE PAGE"
stays exactly as it is, and so do the `data-own` marks on the page's own elements: the page
uses them to republish itself after a weekly review.

1. **`SETUP`** — the JSON in `<script type="application/json" id="setup">`. Replace the demo
   with this founder's setup, as valid JSON (no comments), writing each `<` as `<`:
   - leave out `"demo": true`; `"version": 4`
   - every field in `interview.md` under "What to write down", from the interview and the
     tools step (`tools.md`); leave `accent` and `accentLight` out if they have no brand
     colour
   - `about`: one or two sentences in their voice, starting "I'm [name] and I run …", with
     what they sell and the goals and today's numbers. Every prompt the page hands to Claude
     starts with it, so a new chat never has to ask who they are
   - `goals`: one or two, each with `unit`, `money`, `helps` and `countedBy` (`interview.md`)
   - `plan` (`objective`, `reviewedAt`) and `ideas` (`ideasFrom`) from the first plan;
     `owners` empty until a weekly review assigns them
   - `moneyTool`: the connected accounting or shop tool's name, or `null`. With `null` the
     page shows one card asking to connect it
   - `missing`: numbers you could not show and why, `[{label, why}]`, in plain words ("Your
     bank accounts are in Zoho's separate banking connector. Connect it and cash appears
     here."). They show as "Not shown yet" under "See all numbers"
   - `reads`: any extra `[connector, tool]` pairs Claude may read while it drafts on the page,
     beyond those the plays and measures read (for example `["Gmail", "get_thread"]`). Reads
     only
   - `snapshot` — see below
2. **`PLAYS`** — keep the ready-made plays whose tools are connected, delete the others,
   and add the plays you wrote and proved (`plays.md`). With no tools connected yet, leave
   it as an empty list.
3. **`MEASURES`** — the numbers you wrote and proved (`measures.md`), including a goal
   measure for each goal counted in a connected tool.

Keep the page's title as "What's next". Setups from before version 4 had one `goal`; the
page still reads them, but write `goals` whenever you update one.

## The snapshot

The snapshot is what the dashboard shows before live data arrives, and whenever live data
is not available. You make it by running every play and measure yourself, now:

- For each play, make the same requests it makes, follow its `find` by hand, and write the
  item it returns into `snapshot.items`, adding `id` (the play's id), `play` (the play's
  id) and `helps` (the play's labels). A play that finds nothing adds its `clear` line to
  `snapshot.clear` instead.
- For each measure, do the same and write its result into `snapshot.measures`, adding
  `id`, `group`, `label` and `helps` from the measure. A goal measure's result goes into
  that goal in `SETUP.goals` instead (`now`, `ratePerMonth`, `perCustomer`).
- **`snapshot.summary`**: one or two plain sentences on where the business stands, written
  by you from the numbers and the goals, as you would say it to the founder ("You are making
  a profit, but cash is tight because schools take 52 days to pay."). Facts from the
  snapshot only. The page shows it after the greeting until the next refresh.
- Set `snapshot.takenAt` to the current date and time, with the founder's time zone offset.
- Do not rank or trim. Put in everything the plays found; the page chooses.

Before any tool is connected, the snapshot is empty and the page says "Nothing needs you
this week." That is fine for the first save.

## Publish it

- Publish the page as the founder's own private artifact titled "What's next". If your
  artifact tools have their own guidance for writing or publishing pages, read it first and
  follow it where it differs from this file.
- **Declare what the page may do** (your artifact tools' capability declaration; a new page
  is published first, then declared):
  - `mcp`: every connector and tool the plays and measures read (collect them from each
    one's `tools`), the extra `reads`, and, when Gmail is connected, Gmail's
    `create_draft`, so "Save to Gmail drafts" works. Names must be exact. List nothing else.
  - `sample`: so Claude can draft on the page ("Do it here") and run the weekly review.
  - `artifact`: so a saved weekly review can update the page.
- If you cannot declare these here, publish anyway. The page shows the snapshot, every button
  copies a full-context prompt instead, and the founder refreshes it by asking you.

## Updating

Read the current page, change `SETUP`, `PLAYS` and `MEASURES`, refresh the snapshot and the
summary, and publish to the same artifact. Keep anything a weekly review saved (`plan`,
`ideas`, goal numbers and `askedAt`, `owners`). If the page now reads a connector or tool it
did not before, update its declaration first. Never create a second dashboard.

## What to tell the founder

Keep it to a few lines:

- Here is your dashboard: the link.
- The first time it opens it asks whether it may read each tool and use Claude. Say yes; it
  reads, drafts and saves drafts, and never sends.
- "This week" is up to three things, each tagged with the goal it moves. "Draft it here" has
  Claude write the emails on the page; "Save to Gmail drafts" puts them in Gmail to check and
  send. "Copy prompt" shows and copies the full request, for a new chat.
- "Done" clears something for a week. "Not for me" clears it for good. "1 more this week"
  shows what didn't fit.
- "Needs attention" shows only the numbers that need you. "See all numbers" shows the rest,
  "How it's worked out" shows the sum behind each, and "Not shown yet" says what's missing.
- Once a week, press "Weekly review" on the page. It asks two or three questions and updates
  your plan and ideas.
- Open it any time, on your phone too. Ask me to add a tool or change your goal whenever you
  like.

Then read out what the three things are right now.
