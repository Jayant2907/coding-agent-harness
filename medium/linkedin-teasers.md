# LinkedIn teasers — post after each Medium URL is live

## Part 1

I used to babysit my coding agent like an intern — "keep going," "no wait, not that," every few minutes. Then I found two small habits that let it work an entire night shift on its own and hand me something safe to review in the morning.

Part 1 of my new series, *Running Agents Overnight*, breaks down:

🔁 `/loop` — the simplest way to keep an agent working without you in the room
🎯 The "goal file" trick — how to give a loop an actual finish line
📜 The 7-question "contract" I write before any unattended run — cadence, guardrails, scope, action, budget, stop conditions, and reporting

No jargon. Just what I actually discovered the hard way, with a worked example.

Read Part 1 on Medium → [link]

Part 2 (running multiple agents together) and Part 3 (memory + skills) coming soon.

Attach: docs/diagrams/loop-tick-cycle.png

## Part 2

Most people use an AI coding agent like a fast intern: one task, watch it, take the result. That breaks the moment real work shows up — many things at once, across days, with a teammate in the same codebase.

Part 2 of *Running Agents Overnight* covers how I run mine as a **team** instead:

🧭 One orchestrator that plans, delegates, and (almost) never codes itself
🗂️ Every agent gets its own private git worktree — no more agents overwriting each other
🔍 Review is a law, not a nicety — it even caught my orchestrator being wrong once

Read Part 2 on Medium → [link]

Part 3 (how the whole thing remembers anything, and how two people collaborate through it) coming soon.

Attach: docs/diagrams/orchestration-loop.png

## Part 3

The real problem with AI coding agents isn't intelligence. It's that they forget everything the moment you close the window — and it gets worse the moment a second person joins.

Part 3 of *Running Agents Overnight* covers the fix, and it's almost boringly low-tech:

🗃️ Memory as a filing system, not a bigger context window — a backlog, a status file, a session log, all versioned in git
🔁 The ritual: read status first, do the work, write status last
🤝 The real payoff — two people, two separate orchestrators, one shared memory, staying in sync without a single meeting

Read Part 3 on Medium → [link]

That wraps the series — thanks for following along. Thoughts, disagreements, or "we do it differently" stories welcome in the comments.

Attach: docs/diagrams/files-are-the-meeting.png

## Companion

Most teams don't have a memory problem because they lack tools — they have one because whatever they've learned lives in someone's head, an old Slack thread, or a closed ticket. Add AI agents and it gets worse: every session starts from zero.

The fix is six plain markdown files, checked into git, that people and AI agents both read first and write last:

📌 Status — where things stand, and the exact next step
🗂️ Backlog — what's broken, tagged by urgency
🧭 Decisions — why we chose this, so it's never re-argued
📓 Session log — what actually happened, append-only
⚠️ Risks / known issues — what could still go wrong
🔎 Memory index — pointers to deeper context

One rule makes it work: read first, write last — and verify the file against reality before you trust it. Full breakdown + templates → [link]

Attach: docs/diagrams/six-files-at-a-glance.png
