# The Real Problem With AI Agents Isn't Intelligence. It's Memory.

### *Running Agents Overnight* — Part 3

<!-- FEATURED IMAGE: docs/diagrams/files-are-the-meeting.png -->
<!-- Series: This is Part 3 of Running Agents Overnight. Read Parts 1 and 2 first. -->

In Part 2, I described running Claude Code as an orchestrator with a team of subagents. That handles doing a lot of work in parallel. But it exposes two problems that no amount of model intelligence fixes on its own:

1. Agents forget everything between sessions. Close the window, and today's hard-won context is gone.
2. The moment a second person joins, you have two AI setups that know nothing about each other, quietly undoing each other's work.

The answer to both turned out to be the same, and it's almost boringly simple: a shared, git-tracked filing system that the agents read from and write to. The files are the memory. The files are also the coordination protocol between two humans. Let me explain.

## Memory is a filing system, not a bigger context window

People assume the fix for "the agent forgot" is a longer memory, a bigger context window. It isn't, really. The fix is giving agents a place to write things down that outlives the session — and a habit of reading it first.

We keep a small set of plain files in the repo (so they're versioned and shared, like code):

- A **backlog** of findings. Every bug, gap, or "we should look at this later" gets filed as a numbered entry with what it is, where it lives, and how to reproduce it. Nothing an agent discovers is ever lost, even if we don't fix it today.
- An **epics** file for the big, multi-week efforts that are bigger than any one task.
- A **status** file whose most important line is literally "Next step (exact)." This is how an agent picks up work: it doesn't guess where we left off, it reads the status file and does the next named action.
- A **session log** — a one-line-per-session diary of what actually happened.
- A **memory index** — short pointers to the deeper notes, so a fresh agent can get oriented in seconds instead of re-reading everything.

The workflow becomes a ritual: **read status first, do the work, write status last.** An agent starting cold reads the status file, learns the exact next step, checks that the code actually matches what the status claims, does the work, then updates the status and files anything new. The "memory" isn't in the model. It's in the repo. Any agent, any session, any day can rehydrate from it.

<!-- UPLOAD IMAGE: files-are-the-meeting.png -->
<!-- Caption: The Files Are the Meeting -->
<!-- Alt: Shared git-tracked files acting as memory and coordination between two people and their agent teams. -->

This also quietly fixes a classic failure: an agent that finds a second bug while fixing the first one. Without a filing habit, that second bug evaporates. With it, the agent stops, files it, and moves on — so it's still there next week when we choose to deal with it.

## Not every finding is equal — the taxonomy that keeps a backlog usable

A backlog only stays useful if you can tell, at a glance, which of the (eventually hundreds of) filed issues need attention *today*. We settled on a small set of labels, and the discipline is applying them honestly rather than skipping the unglamorous ones:

- **Fix now** — blocks the current phase of work
- **Fix later, on a named trigger** — "deal with this when we're back working on this area," not just "someday"
- **Nice-to-have** — real, but only worth doing if time allows
- **Explicitly dismissed** — a reason is recorded, even when the answer is "we're not doing this"
- **Done** — closed, kept for the audit trail, not deleted

The one that surprised me is how much value sits in "explicitly dismissed, with a reason." The default instinct is to just delete a finding you've decided not to act on — and months later someone rediscovers the same thing and re-litigates it from zero, because the reasoning left with the finding. Writing down *why not* is cheaper than re-arguing it later.

## Memory can go stale — verify it before you trust it

Here's the failure mode that taught me the most. A status file can keep confidently reporting "this area is fully covered" long after that stopped being true — and every agent that reads it and moves on is doing exactly what it was told, while quietly reporting on a version of the product that no longer exists.

A real example from my own project: a testing checklist for one feature area had sat at roughly a dozen items for weeks, all reported passing. When someone finally checked how much the underlying code had moved since that checklist was last honestly written, dozens of commits and several entirely new capabilities had shipped in that exact area — none of them named anywhere in the checklist. Three separate sessions had read the file, seen "all green," and moved on. All three were technically correct and all three were wrong.

The fix wasn't "trust the file less." It was adding one mechanical check before trusting it at all: before starting work in an area, diff what's actually changed in the code since the file was last honestly updated, and treat anything that shipped but isn't named anywhere as a finding in its own right — not a footnote to file "later." A memory file is a *claim* about reality, and claims need to be checked against reality occasionally, not taken as reality itself.

That reshapes the ritual from Part 1 slightly: read status first, spot-check that status against what's actually on disk, *then* do the work.

## Skills: teaching the team a procedure once

Some things we do the same way every time — how we review a change, how we prepare a release, how we verify a fix end-to-end. Instead of re-explaining those every session, they get promoted to **skills**: named, reusable procedures the agents can invoke. A skill is basically a checklist with muscle memory. The first time you catch yourself explaining the same workflow twice, that's the signal to turn it into a skill. It's how the team's know-how compounds instead of resetting.

## The punchline: two people, two orchestrators, one shared memory

Here's the part I find genuinely powerful. My collaborator and I don't sit in the same session. Each of us has our own orchestrator. Mine runs my agents; theirs runs theirs. We're often working on the same product, sometimes near the same code.

We almost never need to interrupt each other to coordinate, because both orchestrators read and write the same shared files. When my orchestrator finds and files an issue, it's not telling me — it's writing to the shared backlog. Tomorrow, my teammate's orchestrator reads that same backlog and knows about it. When their side finishes a big piece and updates the status and session log, my side picks it up on the next run.

Mechanically, this only works because not every file needs the same collision strategy. The backlog and the session log are both **append-only** — two people can each add a new line at the same moment without stepping on each other, so they stay single shared files. The status file is different: it holds *one* current position, and two people overwriting the same "here's where things stand" file is exactly the collision we're trying to avoid. So the moment a second person joins, that one status file becomes a short index pointing at one status file per person — my current state, their current state — read together, written separately.

The files are the meeting. The status file is the standup. The backlog is the shared to-do. The session log is the handoff note. Two humans and their agent teams stay in sync asynchronously, through a filing system in git, instead of through constant real-time chatter. That's the difference between two people separately using AI, and two people running coordinated AI teams that don't collide.

And worktrees, from Part 2, are what make that collision-free even at the code level: separate private workspaces per agent, one clean shared branch everyone merges into, one shared memory everyone reads.

## The lessons, distilled

If you want to try this, these are the things that actually made it work:

- **Separate the manager from the workers.** One orchestrator that plans and verifies; many agents that do.
- **Give every agent a private worktree.** Parallelism without isolation is just corruption.
- **Make review a law, not a nicety** — independent, source-checked, and applied even to the orchestrator.
- **Treat files as memory.** Backlog, epics, status, session log, memory index — versioned in git so they survive sessions and travel between people.
- **Give every finding a real label, including "no."** Fix-now, fix-later-on-a-trigger, nice-to-have, dismissed-with-a-reason, done — anything less loses either urgency or history.
- **Read status first, write status last — but verify status before you trust it.** A file can go stale exactly when it matters most; check it against what's actually on disk before building on it.
- **Promote repeated procedures to skills.** Explain it twice, then never again.
- **Keep the human at the forks.** The system scales the doing; you keep the deciding.

We didn't set out to build a "framework." We just kept hitting the same walls — forgetting, colliding, over-trusting, two people getting in each other's way — and the fixes kept being the same shape: isolate, review, and write it down where everyone can read it.

That's the whole thing across this series: a loop with a goal and a contract (Part 1), a manager and a team with private workspaces and mandatory review (Part 2), and a shared memory made of plain files (Part 3). Simple parts. It's the discipline of using them together that turns a fast intern into a team you can actually ship with.

---

*Copy the six files from [GitHub — running-agents-overnight](#). Companion post: how each file earns its place.*

**Tags:** Artificial Intelligence, Software Engineering, Productivity, Git, Collaboration
