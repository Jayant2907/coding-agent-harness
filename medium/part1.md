# How I Taught Claude Code to Work the Night Shift

### *Running Agents Overnight* (and whenever else you're not at the desk) — Part 1: `/loop`, the goal file, and the contract that keeps it honest

<!-- FEATURED IMAGE: docs/diagrams/loop-tick-cycle.png -->

I used to treat my coding agent like an intern I had to hover over. Every few minutes: "keep going," "yes, now do the next part," "wait, don't touch that file." The moment I looked away, it either stalled waiting for me, or it "helpfully" did five things I never asked for.

Then I found two small habits that changed how I work with Claude Code entirely: **`/loop`**, and a lightweight technique I now just call **"the goal file."** Together, they let an agent keep working for hours — sometimes overnight — and hand me something safe and reviewable in the morning, instead of a mess.

This is Part 1 of a series on running Claude Code unattended. Today: the two primitives, and the "contract" I write before every overnight run so it doesn't go off the rails.

## The first primitive: `/loop`

`/loop` does one simple thing: it re-runs a prompt on a cadence — every 30 seconds, every 5 minutes, or "whenever the model thinks is reasonable" — until you stop it or it hits a condition you set.

That's it. No magic. A babysitting instruction, repeated.

Some plain examples:

- `/loop 30s check if the build finished; if it failed, read the error and fix it`
- `/loop run the test suite; stop once two runs in a row are clean`
- `/loop read today's task list, do the next item, update the list`

The unlock isn't the repetition — it's that each "tick" of the loop can *read where it left off* and *pick up from there*. Which brings me to the second habit.

## The second primitive: the goal file

Here's the part most people miss: you don't need a special feature to give an agent a target. You just need a **short file that states, in plain language, what "done" looks like** — and you tell the agent to check it, and tick it off, every single tick.

This is also where "big" tasks stop being scary. A whole feature is too large for one tick to swallow safely — so the goal file's real job is to break it into phases, and each phase into small, checkable steps. I call mine `GOAL.md`:

```
## Goal
Ship a complete notification system: in-app bell, email digests, and per-user preferences.

### Phase 1 — Data layer
- [ ] Notification table + migration
- [ ] Read/unread state tracking

### Phase 2 — Backend
- [ ] Create-notification endpoint
- [ ] Mark-as-read endpoint
- [ ] Nightly email-digest job

### Phase 3 — Frontend
- [ ] Notification bell + dropdown
- [ ] Preferences page (per-type email on/off)

### Phase 4 — Polish
- [ ] One event never fires two notifications
- [ ] End-to-end test covering all three channels
```

Notice what this does to a genuinely big feature: it's no longer one intimidating blob called "add notifications." It's eleven small, independently checkable steps, grouped into four phases with a natural order. Every tick of the loop reads this file, picks the smallest unchecked box, does *only* that, and ticks it only once it's *verified* — not merely attempted. A vague "keep working on notifications" becomes something both the agent and I can objectively track, phase by phase, overnight.

That's the whole trick behind Part 1: **`/loop` gives you repetition; a goal file gives that repetition a stopping point.**

<!-- UPLOAD IMAGE: loop-tick-cycle.png -->
<!-- Caption: The Loop Tick Cycle -->
<!-- Alt: A four-step cycle: read the state, do the smallest step, verify it worked, report and update state, then repeat. A branch shows parking and reporting to a human when a stop condition is met. -->

## Why "just let it loop" is a bad idea

Here's what I discovered the hard way, early on: a loop with no rules is a liability, not a convenience. Left alone, an agent will happily:

- Restart a server you didn't want touched
- Wander outside the one folder you meant it to change
- Burn through your token budget on a task that should've taken five minutes
- Or just... keep "improving" things forever, because nobody told it when to stop

None of that is the agent being reckless. It's doing exactly what a loop does: repeating, without a fence around it. **The fence is on you to build.**

## The contract: seven questions, answered once, obeyed every tick

Before I let anything run unattended — especially overnight — I write a short "contract" at the top of the loop prompt. It's just answers to seven questions. I fill these in fresh for every project, but the questions never change:

1. **Cadence** — *When does a tick fire?* Fixed interval, or let the model self-pace? (e.g., "every 3 minutes" vs. "as soon as the last step finishes")
2. **Guardrails** — *What can never happen, no matter what?* (e.g., "never restart a shared server," "never force-push," "never touch the payments folder")
3. **Scope** — *Where is it allowed to make changes?* A branch, a folder, a set of files — named explicitly, not implied.
4. **Action** — *What does one tick actually do?* The smallest unit of work, followed immediately by a way to check it worked (a test, a build, a read-only query).
5. **Budget** — *What's the ceiling?* How many helper agents per tick, how many tokens, how many retries before giving up on one approach.
6. **Stop conditions** — *When does it park and wait for me?* All goal-file boxes checked; three failed attempts at the same step in a row; or it needs something only I can approve (a restart, a real answer, a judgment call).
7. **Report** — *What must every tick tell me?* What it did, how it verified that, and what's next — written down, not just implied by a green checkmark.

<!-- UPLOAD IMAGE: seven-point-contract.png -->
<!-- Caption: Before You Walk Away: 7 Questions -->
<!-- Alt: Seven questions to answer before an unattended run: cadence, guardrails, scope, action, budget, stop conditions, and reporting. -->

Answer those seven once, and you can hand the loop a task and walk away — because it now knows what it's allowed to do, how far it can go, and exactly when to stop and wait for you instead of guessing.

## Putting it together: a small worked example

Say I want an overnight run that builds that notification system. My contract might read:

> **Cadence:** self-paced, no fixed timer. **Guardrails:** never touch the email-sending credentials or run a destructive migration; if a step needs a live server or a real email to go out, stop and ask. **Scope:** only files under `/notifications/**` and the shared UI shell it plugs into, branch `feature/notifications` only. **Action:** one unchecked box from `GOAL.md` per tick, in phase order; verify with that step's test or a build check. **Budget:** at most one helper agent per tick. **Stop:** every phase in `GOAL.md` checked, or three failed attempts on the same box. **Report:** box completed, how it was verified, next box.

That's maybe eight lines of setup. In exchange, I've woken up more than once to find a whole phase done — say, the entire data layer and backend, cleanly separated into small commits, each backed by a real check — instead of one giant, unreviewable diff, or a half-built feature with the frontend expecting an API that was never finished.

## What this actually buys you

The honest version of what changed for me: I stopped thinking of `/loop` as "let the AI run wild for a while" and started thinking of it as **delegation with a written brief** — the same brief I'd give a junior engineer working a night shift I couldn't personally supervise. Clear cadence, clear no-go zones, one task at a time, a way to verify it, a budget, a stopping point, and a report waiting for me in the morning.

That's the whole idea. Two primitives — `/loop` and a goal file — plus one habit: write the seven-point contract before you walk away, not after something goes wrong.

## Coming up

**Part 2** is about what happens when one loop isn't enough — how I coordinate *several* agents working on different pieces of the same project at once, without them stepping on each other (the "orchestrator" pattern). **Part 3** gets into the memory files and reusable skills that let an agent stay coherent across days, not just one overnight run.

If you've tried running Claude Code unattended and hit a wall — or a 2 a.m. surprise — I'd genuinely like to hear about it in the comments.

---

*Templates for `GOAL.md` and the 7-point contract: [GitHub — running-agents-overnight](#) — paste the repo URL after you publish it.*

**Tags:** Artificial Intelligence, Software Engineering, Claude, Productivity, Programming
