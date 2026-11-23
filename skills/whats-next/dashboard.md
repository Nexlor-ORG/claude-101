# Building and publishing the dashboard

The dashboard is one page, built from `dashboard-template.html` in this folder. A founder
has exactly one, at one link, for good. Every later change updates that same page.

## Build the page

Start from the template and change only two things. Everything under "3. THE PAGE" stays
exactly as it is.

1. **`SETUP`** — replace the demo with this founder's setup:
   - remove `demo: true`
   - `founder`, `business`, `goal`, `focus`, `heard`, `tools`, `accent` from the interview
     (`interview.md`) and the tools step (`tools.md`); leave `accent` out if they have no
     brand colour
   - `snapshot` — see below
2. **`PLAYS`** — keep the ready-made plays whose tools are connected, delete the others,
   and add the plays you wrote and proved (`plays.md`). With no tools connected yet, leave
   it as an empty list.

Keep the page's title as "What's next".

## The snapshot

The snapshot is what the dashboard shows before live data arrives, and whenever live data
is not available. You make it by running every play yourself, now:

- For each play, make the same requests it makes, follow its `find` by hand, and write the
  item it returns into `snapshot.items`, adding `id` (the play's id), `play` (the play's
  id) and `helps` (the play's labels).
- Plays that return nothing add nothing.
- Set `snapshot.takenAt` to the current date and time, with the founder's time zone offset.
- Do not rank or trim. Put in everything the plays found; the page chooses.

Before any tool is connected, the snapshot is empty and the page says "Nothing needs you
right now." That is fine for the first save.

## Publish it

- Publish the page as the founder's own private artifact titled "What's next". If your
  artifact tools have their own guidance for writing or publishing pages, read it first and
  follow it where it differs from this file.
- **Give the page live access to their tools.** Declare connector access for the page (the
  `mcp` capability) listing every connector and tool the plays read: collect them from each
  play's `tools`. Names must be exact. List nothing the plays do not use.
- If you cannot give the page live access here, publish it anyway. It will show the
  snapshot, and the founder refreshes it by asking you.

## Updating

Read the current page, change `SETUP` and `PLAYS`, refresh the snapshot, and publish to the
same artifact. If the plays now read a connector or tool they did not before, update the
page's connector access as well. Never create a second dashboard.

## What to tell the founder

Keep it to a few lines:

- Here is your dashboard: the link.
- The first time it opens it will ask whether it may read each tool. Say yes; it only
  reads.
- Each thing has a button. Press it, paste what it copies into Claude, and Claude does the
  work and shows you before anything is sent.
- "Done" clears something for a week. "Not for me" clears it for good.
- Open it any time, on your phone too. Ask me to add a tool or change your goal whenever
  you like.

Then read out what the three things are right now.
