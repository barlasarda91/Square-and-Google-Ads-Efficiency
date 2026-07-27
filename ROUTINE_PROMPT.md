# Boxx Ads Routine

Repo-as-memory scaffolding for the weekly Boxx Coffee analysis routine.

**This file is for humans and is not part of the routine's inputs.** The routine reads `CLAUDE.md`
and works from there. If anything here ever contradicts `CLAUDE.md`, `reference/`, or
`playbooks/`, those win and this file is out of date.

Each run is a fresh Claude Code session with no memory and no access to the claude.ai project, so
everything it needs lives here as files, and it writes its state back here. That is what makes
week two smarter than week one. File-by-file description is in `CLAUDE.md`.

## Setup

**1. Push this to a private GitHub repo.** Any name.

**2. Connectors must be claude.ai account connectors.** Most common failure. A routine can only use
connectors attached to your claude.ai account, not MCP servers added locally with `claude mcp add`.
Go to claude.ai, Settings, Connectors and confirm **Square**, **Google Ads**, and **Gmail** are
there and connected.

**3. Create the routine** at claude.ai/code/routines, or the Desktop app, or `/schedule` in the CLI.

- Name: `Boxx Weekly Analysis`
- Instructions: paste `ROUTINE_PROMPT.md`. Nothing else.
- Model: pick a capable one, it runs every week.
- Repositories: attach this repo.
- Environment: Default, trusted network.
- Connectors tab: remove everything except Square, Google Ads, Gmail.
- Permissions tab: **enable unrestricted branch pushes.** Routines default to pushing a
  `claude/`-prefixed branch. If state lands on a branch instead of `main`, every later run reads
  stale state and the memory design fails with no visible error. If you cannot enable it, the
  routine pushes a branch and puts `ACTION REQUIRED: merge branch` as the first line of the email,
  and you merge weekly.
- Trigger: one weekly Schedule trigger, Thursday morning. One is enough. The biweekly ads sweep is
  decided inside the run from `state.json`, not by a second trigger.

**4. Test with Run now before trusting the schedule.** A green status only means the session exited
cleanly. Open the transcript and confirm it read `CLAUDE.md`, ran the bootstrap backfill on both
`square_daily.csv` and `ads_daily.csv`, produced `dashboard.html` and `checklist.md`, sent the
email, and committed **to `main`**. Verify the branch specifically. That is the one failure that
breaks everything else quietly.

## Ongoing

To change budgets, the trigger table, the ladder, or account facts, edit the file in `reference/`.
Never edit the routine prompt. It should not need to change again.

Read-only on Google Ads and Square, always. The routine recommends, you execute.
