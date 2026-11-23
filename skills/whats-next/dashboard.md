# Building and publishing the dashboard

The dashboard is one page, built from `dashboard-template.html` in this folder. A founder
has exactly one, at one link, for good. Every later change updates that same page.

## Build the page

Start from the template and change only three things. Everything under "4. THE PAGE"
stays exactly as it is.

1. **`SETUP`** — replace the demo with this founder's setup:
   - remove `demo: true`
   - every field listed in `interview.md` under "What to write down", from the interview
     and the tools step (`tools.md`); leave `accent` and `accentLight` out if they have no
     brand colour
   - `goal.unit` (the word after the number: "schools", "a month") and `goal.money`
     (`true` when the goal is an amount of money)
   - `snapshot` — see below
2. **`PLAYS`** — keep the ready-made plays whose tools are connected, delete the others,
   and add the plays you wrote and proved (`plays.md`). With no tools connected yet, leave
   it as an empty list.
3. **`MEASURES`** — the numbers you wrote and proved (`measures.md`), including the goal
   measure when the goal is counted in a connected tool. Empty until a money tool is
   connected; the page then shows a "Connect" tile in their place.

Keep the page's title as "What's next".

## The snapshot

The snapshot is what the dashboard shows before live data arrives, and whenever live data
is not available. You make it by running every play and measure yourself, now:

- For each play, make the same requests it makes, follow its `find` by hand, and write the
  item it returns into `snapshot.items`, adding `id` (the play's id), `play` (the play's
  id) and `helps` (the play's labels).
- For each measure, do the same and write its result into `snapshot.measures`, adding
  `id`, `group`, `label` and `helps` from the measure. The goal measure's result goes into
  `SETUP.goal` instead (`now`, `ratePerMonth`, `key`).
- Plays and measures that return nothing add nothing.
- **`snapshot.summary`**: one or two plain sentences on where the business stands, written
  by you from the numbers and the goal, as you would say it to the founder ("You are making
  a profit, but cash is tight because schools take 52 days to pay."). Facts from the
  snapshot only; no advice the actions do not already give. The page shows it after the
  greeting until the next refresh.
- Set `snapshot.takenAt` to the current date and time, with the founder's time zone offset.
- Do not rank or trim. Put in everything the plays found; the page chooses.

Before any tool is connected, the snapshot is empty and the page says "Nothing needs you
right now." That is fine for the first save.

## Publish it

- Publish the page as the founder's own private artifact titled "What's next". If your
  artifact tools have their own guidance for writing or publishing pages, read it first and
  follow it where it differs from this file.
- **Give the page live access to their tools.** Declare connector access for the page (the
  `mcp` capability) listing every connector and tool the plays and measures read: collect
  them from each one's `tools`. Names must be exact. List nothing they do not use.
- If you cannot give the page live access here, publish it anyway. It will show the
  snapshot, and the founder refreshes it by asking you.

## Updating

Read the current page, change `SETUP`, `PLAYS` and `MEASURES`, refresh the snapshot and the
summary, and publish to the same artifact. If they now read a connector or tool they did not
before, update the page's connector access as well. Never create a second dashboard.

## What to tell the founder

Keep it to a few lines:

- Here is your dashboard: the link.
- The first time it opens it will ask whether it may read each tool. Say yes; it only
  reads.
- Each thing has a button. Press it, paste what it copies into Claude, and Claude does the
  work and shows you before anything is sent.
- "Done" clears something for a week. "Not for me" clears it for good.
- Tap "How it's worked out" under any number to see the sum behind it.
- The switch at the top picks light or dark; Auto follows the phone or computer.
- Open it any time, on your phone too. Ask me to add a tool or change your goal whenever
  you like.

Then read out what the three things are right now.
