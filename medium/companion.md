# The 6 Files That Let Humans and AI Agents Actually Collaborate

### A companion piece to *Running Agents Overnight* — steal this for your own team

<!-- FEATURED IMAGE: docs/diagrams/six-files-at-a-glance.png -->

Most teams don't have a memory problem because they lack tools. They have one because whatever they've learned lives in the worst possible places: someone's head, a Slack thread from three weeks ago, a comment on a ticket that's since closed. Add AI agents into the mix and it gets worse — every session starts from zero unless someone hand-feeds it context, every single time.

The fix we landed on is almost embarrassingly simple: six plain markdown files, checked into git, that both people and AI agents read first and write last. No tool, no service, no plugin — just files. Here's what's in them and why each one earns its place.

<!-- UPLOAD IMAGE: six-files-at-a-glance.png -->
<!-- Caption: The Six Files, at a Glance -->
<!-- Alt: Six governance files and the one question each answers: Status, Backlog, Decisions, Session log, Risks, Memory index. -->

## The six files

| File | What it answers | Written by | How often |
|---|---|---|---|
| **Status** | Where do things stand, and what's the *exact* next step? | Whoever just finished working | Every session |
| **Backlog** | What's broken or unfinished, and how urgent is it? | Anyone who finds something | As discovered |
| **Decisions** | Why did we choose this, and what did we reject? | Whoever made the call | When a real choice is made |
| **Session log** | What actually happened, session by session? | Everyone, append-only | Every session |
| **Risks / known issues** | What could still go wrong, and what's the workaround? | Anyone who spots one | As discovered |
| **Memory index** | Where's the deeper context, so I don't re-read everything? | Whoever writes the deep note | Occasionally |

Five minutes reading these before you start replaces an hour of "wait, why did we do it this way" and "didn't we already find this bug."

## The one rule that makes them work

**Read first, write last.** Every session — human or AI — opens with the status file, not a re-briefing from a person. It ends by updating that file, appending to the session log, and filing anything new to the backlog. Skip either half and the files quietly rot into fiction.

One caveat: **a status file can lie by omission.** If nobody checks it against the actual code, it will happily keep reporting "all good" long after that stopped being true. The fix is cheap — glance at what's actually changed since it was last written, before you trust it — but skipping that glance is the single most common way this system quietly fails.

<!-- UPLOAD IMAGE: read-first-write-last.png -->
<!-- Caption: The Read-First, Write-Last Ritual -->
<!-- Alt: A flowchart: start a session, read Status, Backlog, and Decisions, verify status against the actual code, do the work, then write updates back before the next session. -->

## Why this is actually a collaboration tool

The magic isn't the files individually — it's what they let you skip. Two people, or two people each running their own AI agent, don't need a standup, a handoff call, or a shared Slack thread to stay in sync. One finishes a session and writes down what happened and what's next. The other opens the same files the next day and picks up exactly there — no re-explaining, no "wait, did you already fix that?"

- The **backlog** is the shared to-do list, so a finding never depends on someone remembering to mention it out loud.
- The **decisions log** stops the same argument from happening twice, months apart.
- The **status file** is the standup that never needs scheduling.
- The **session log** is a handoff note nobody has to write specially, because it's already a habit.

Not every file needs the same collision rule, either. Anything append-only — backlog, session log — can be shared by any number of people at once; two people can each add a new line without stepping on each other. Anything that holds *one current position* — status — needs to split per person once more than one person is active, so nobody overwrites someone else's "here's where I am."

## If you only do one thing

Start with a status file that has exactly one non-negotiable line: **"Next step (exact)."** Not "keep working on the thing" — the literal next action, specific enough that a stranger, or a fresh AI session with zero memory, could do it without asking a single question. Everything else on this list earns its place over time; that one line pays for itself on day one.

---

*Blank templates for all six files (plus `GOAL.md` and the overnight contract): [GitHub — running-agents-overnight](#).*

**Tags:** Productivity, Software Engineering, Artificial Intelligence, Documentation, Collaboration
