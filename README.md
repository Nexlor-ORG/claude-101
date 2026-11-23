# What's next

A Claude plugin from the Claude 101 community, by [Nexlor](https://nexlor.co).

Claude interviews you about your business, helps you connect the tools you already use,
and gives you a live dashboard that answers one question: **what's next?** It shows up to
three things you can do to move your business toward your goal, each with a button that
hands the job to Claude.

*Independent training, not affiliated with Anthropic.*

## Your data stays with you

This plugin sends nothing about you or your business to Nexlor or anyone else. The
interview, your connected tools and your dashboard all stay in your own Claude account.
The dashboard only reads from your tools. Nothing is changed or sent without your say-so.

## Install

You need the Claude desktop app on a paid plan.

1. Open **Customize → Plugins**.
2. Choose **Add**, then add this marketplace: `Nexlor-ORG/claude-101`
3. Install **What's next**.

In Claude Code:

```
/plugin install whats-next --marketplace Nexlor-ORG/claude-101
```

## Use

Start a new chat and say:

> Set up my What's next dashboard

Claude asks a few short questions (about ten minutes), helps you connect one or two tools,
and gives you a link to your dashboard. Come back any time and say "add my Shopify to my
dashboard", "my goal has changed" or "refresh my dashboard".

## Status: 0.1, in testing

Works today:
- The interview, the tool finding and connecting steps, and the dashboard page.
- Ready-made checks for Gmail and Google Calendar. For any other connected tool, Claude
  writes and proves checks on your own data during setup.

Known limits:
- "Done" and "Not for me" are remembered per device.
- A button copies a request for you to paste into Claude; it does not start Claude itself.
- Not yet tested on a brand-new Pro account from start to finish.

## What is in here

```
.claude-plugin/            plugin and marketplace manifests
skills/whats-next/
  SKILL.md                 where Claude starts
  interview.md             the ten-minute interview
  tools.md                 finding and connecting tools
  plays.md                 what the dashboard can notice, and how new checks are written
  dashboard.md             building and publishing the dashboard
  dashboard-template.html  the dashboard page (open it in a browser to see a demo)
```
