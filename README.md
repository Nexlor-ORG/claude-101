# What's next

A Claude plugin from the Claude 101 community, by [Nexlor](https://nexlor.co).

Claude interviews you about your business, helps you connect the tools you already use,
and gives you a live dashboard that answers one question: **what's next?** It shows up to
three things you can do to move your business toward your goal. Claude can draft each one
right on the page, and your weekly review happens there too.

*Independent training, not affiliated with Anthropic.*

## Your data stays with you

This plugin sends nothing about you or your business to Nexlor or anyone else. The
interview, your connected tools and your dashboard all stay in your own Claude account.
The dashboard reads from your tools and can save drafts to your Gmail. It never sends
anything: you check and send.

## Install

You need the Claude desktop app on a paid plan. You do not need a GitHub account.

1. In Claude, open **Customize**, then **Plugins**.
2. Choose **Add**, then **Add marketplace**.
3. Paste `Nexlor-ORG/claude-101` and confirm.
4. Find **What's next** in your plugin list and install it. It is not listed under
   **Discover**, which only shows Anthropic's directory.
5. Start a new chat and type `/`, then pick **whats-next** from the list.

If Claude asks you to connect GitHub, the name was typed differently: it must be exactly
`Nexlor-ORG/claude-101`. This repository is public and needs no GitHub sign-in.

**If adding the marketplace does not work** (for example, a work account where it is
switched off): download
[whats-next.zip](https://github.com/Nexlor-ORG/claude-101/releases/latest/download/whats-next.zip),
then in **Customize → Plugins** choose **Add → Upload plugin** and pick the file. A plugin
added this way does not update itself; download the file again for a new version.

In Claude Code:

```
/plugin install whats-next --marketplace Nexlor-ORG/claude-101
```

## Use

Start a new chat, type `/` and pick **whats-next**, or say:

> Set up my What's next dashboard

Picking it from the `/` list makes sure this plugin answers, even if you have other
dashboard tools installed.

Claude asks a few short questions (about ten minutes), helps you connect one or two tools,
and gives you a link to your dashboard. Once a week, press **Weekly review** on the page
(about five minutes). Come back any time and say "add my Shopify to my dashboard", "my goal
has changed", "start my What's next from scratch" or "refresh my dashboard".

## Status: 0.4, in testing

Works today:
- The interview, the tool finding and connecting steps, and the dashboard page.
- One or two goals, each item tagged with the goal it moves.
- "Draft it here": Claude reads your tools and writes the emails on the page; you save them
  to Gmail drafts and send them yourself. Every other button shows and copies a full request
  for a new chat.
- The weekly review on the page: a few questions, then a new plan and ideas for your goal.
- Ready-made checks for Gmail and Google Calendar. For any other connected tool, Claude
  writes and proves checks on your own data during setup.

Known limits:
- "Done" and "Not for me" are remembered per device.
- Drafting and the review on the page use your own Claude usage, and need the page to be
  allowed to use Claude the first time.
- The drafting and the on-page review have been tested with stand-in connectors, not yet on a
  real account.
- The numbers under Business health and Growth are written by Claude during setup from your
  own accounting or shop data. They have not yet been tested against a real Xero, QuickBooks
  or Shopify account.
- Not yet tested on a brand-new Pro account from start to finish.

## What is in here

```
.claude-plugin/            plugin and marketplace manifests
skills/whats-next/
  SKILL.md                 where Claude starts
  interview.md             the ten-minute interview
  tools.md                 finding and connecting tools
  plays.md                 what the dashboard can notice, and how new checks are written
  measures.md              the numbers on the dashboard, and how Claude proves them
  dashboard.md             building and publishing the dashboard
  dashboard-template.html  the dashboard page (open it in a browser to see a demo)
```

## Licence

MIT. See [LICENSE](LICENSE).

## Releasing a new version

Bump `version` in `.claude-plugin/plugin.json`, push, then attach a fresh zip for the
upload route:

```
git archive --format=zip -o whats-next.zip HEAD -- .claude-plugin/plugin.json skills README.md LICENSE
gh release create v<version> whats-next.zip --title "What's next <version>" --notes "<what changed>"
```
