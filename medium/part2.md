# I Run a Team of AI Agents With One Orchestrator. Here's What Keeps It From Turning Into Chaos.

### *Running Agents Overnight* — Part 2

<!-- FEATURED IMAGE: docs/diagrams/orchestration-loop.png -->
<!-- Series: This is Part 2 of Running Agents Overnight. Read Part 1 first. -->

Most people use an AI coding agent the way you'd use a very fast intern: give it one task, watch it work, take the result. That's genuinely useful. But it hits a wall the moment your work looks like real software work — many things happening at once, across days, with correctness that actually matters, and a teammate touching the same codebase.

So my collaborator and I stopped using our agent as a single worker. We started running one instance as an **orchestrator** — a manager that almost never writes code itself. It breaks the work down, hands each piece to a subagent, waits for them to report back, and then does the one thing that makes the whole thing safe: it reviews everything before anything ships.

Here's the model, in plain terms.

## The orchestrator doesn't do the work. It runs the work.

The orchestrator's job is decomposition, delegation, verification, and integration. On a normal afternoon it might be running four or five subagents at once:

- a **research agent** reading across the codebase to answer "how does X actually work today?"
- two **build agents**, each implementing a separate feature or fix
- a **review agent** tearing apart what a build agent just produced

Each subagent gets its own tailored prompt — think of it as a profile: who you are, exactly what to do, how to prove it worked, and what to do when finished. A research agent's profile says "read, don't change, report findings." A build agent's says "implement this, test it live, commit, report the diff." A review agent's says "try to break this; assume it's wrong until proven right."

The orchestrator kicks them off, keeps working on other things, and collects each one as it reports back. Some tasks fan out one problem into several agents — three different agents attacking the same bug from different angles, then comparing notes. Others run several unrelated tasks in parallel — a UI tweak, a data-export fix, and a business-rule fix, all in flight at once. The orchestrator is the only one holding the whole board.

<!-- UPLOAD IMAGE: orchestration-loop.png -->
<!-- Caption: The Orchestration Loop -->
<!-- Alt: An orchestrator decomposes work, delegates to specialized subagents, collects reports, and reviews before anything merges. -->

## The non-obvious part: agents need their own private workspace

The first time you run two agents editing code at the same time, you learn a painful lesson: they overwrite each other. One agent's half-finished change lands in the middle of another's, and now you can't tell whose work is whose.

The fix is **git worktrees** — and if you take one practical thing from this post, take this. A worktree is a separate, private copy of the same repository, on its own branch. Each build agent gets its own worktree. They can all edit "the same files" at the same time without ever colliding, because each one is really editing its own isolated copy. When an agent is done, its branch gets reviewed and merged in cleanly. No stepped-on toes, no mystery changes.

This one decision is what turns "a few agents I babysit one at a time" into a team that actually runs in parallel.

## Review is not optional. It's the whole point.

Here's the rule we never break: nothing an agent produces is trusted until an independent review says so. Not the tests passing. Not a green checkmark. An actual review — a separate agent, and then the orchestrator, reading the real source and checking the real behavior — before anything merges.

Why so strict? Because agents are confident even when they're wrong. A build agent will hand you a beautiful summary that says "verified, all good," and it'll be 90% right and 10% quietly broken. The review layer exists to catch that 10%.

And it catches the orchestrator too. In one recent session, my orchestrator flagged what looked like a data problem — duplicate-looking records piling up. The review pass proved the orchestrator itself had misread the data: those weren't duplicates, they were intentional history. No code got changed on a false alarm. That only happened because "review before you trust" applies to everyone, including the manager.

## What one loop actually looks like

A real, small example, start to finish:

1. I ask a plain-English question: "this date-cutoff logic looks wrong for one of our edge cases."
2. The orchestrator investigates (a read-only research pass) and finds the root cause — the same setting was being interpreted three different ways in three parts of the code.
3. It writes a precise profile and hands a build agent the fix, in its own worktree.
4. The agent implements it, tests it against the live system, and reports back with the diff and evidence.
5. The orchestrator reviews it against the source, runs the change on the real app to watch the behavior, confirms it, and merges.
6. It files the finding so the reasoning is never lost, and updates the shared status so a teammate's setup will know about it tomorrow.

I stayed in the loop the whole time — I made the judgment calls at the forks, and at one point I told it to stop mid-task and it stopped immediately. The orchestrator scales the doing. It never takes the deciding away from me.

## Coming up

That's the model: one manager, many specialized agents, isolated workspaces, and review as a law rather than a nicety. But none of this survives if the agents forget everything the moment a session ends, or if a second person joins and the two setups quietly undo each other's work. **Part 3** is the memory-and-collaboration problem — solved, surprisingly, with something low-tech: shared files.

---

*Worktree + review habits pair with the templates in [GitHub — running-agents-overnight](#).*

**Tags:** Artificial Intelligence, Software Engineering, Claude, Productivity, Git
