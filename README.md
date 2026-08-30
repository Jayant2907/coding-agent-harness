# coding-agent-harness

A **coding agent harness** for humans and AI agents to collaborate, develop, and test in the same codebase — without overwriting each other or losing context between sessions.

Built for: Claude Code, Cursor, Codex, and similar coding agents. Works for one person or two people each running their own agent team.

## Why this exists

Coding agents are fast. Teams still break in the same three places:

1. **No finish line** — the agent keeps “improving” forever
2. **No isolation** — two agents (or two people) collide in the same working tree
3. **No memory** — close the session and yesterday’s context is gone

This harness is a small, git-tracked system that fixes those without a new platform: a goal + contract for unattended work, private workspaces for parallel agents, and six shared files for collaboration.

## What’s in the box

```text
coding-agent-harness/
├── templates/           # Copy these into your project
│   ├── GOAL.md          # What “done” looks like (phases + checkboxes)
│   ├── CONTRACT.md      # 7-question fence before any overnight /loop
│   ├── STATUS.md        # Exact next step + where things stand
│   ├── BACKLOG.md       # Findings, labeled (including “dismissed”)
│   ├── DECISIONS.md     # Why we chose X (so it is never re-argued)
│   ├── SESSION-LOG.md   # Append-only handoff diary
│   ├── RISKS.md         # Known landmines + workarounds
│   └── MEMORY-INDEX.md  # Pointers so a fresh agent orients fast
├── docs/diagrams/       # SVG + PNG explainers
└── examples/            # Sample /loop prompts
```

## Quick start (5 minutes)

1. Copy everything in `templates/` into your repo (root or a `docs/agent/` folder).
2. Fill **one line** in `STATUS.md`:

   ```md
   Next step (exact): …
   ```

   Specific enough that a stranger — or a fresh agent with zero memory — could do it without asking.
3. Before any unattended run, fill `CONTRACT.md` (cadence, guardrails, scope, action, budget, stop, report).
4. Tell your agent:

   > Read `STATUS.md` first. Do only the next exact step. Verify it. Update `STATUS.md` and append `SESSION-LOG.md` last. File anything new in `BACKLOG.md`.

**The rule:** read first, write last — and spot-check Status against the real code before you trust it.

## How two people collaborate

Each person can run their own orchestrator / agent session. You stay in sync through git, not meetings:

| File | Job | Collision rule |
|---|---|---|
| **Status** | Standup + exact next step | Split per person once two people are active |
| **Backlog** | Shared to-do / findings | Append-only |
| **Decisions** | Why we chose this | Append when a real choice is made |
| **Session log** | Handoff note | Append-only |
| **Risks** | Known failure modes | Update as discovered |
| **Memory index** | Map to deeper notes | Update occasionally |

One finishes a session → writes status + session log. The other opens the same files tomorrow and continues. No “did you already fix that?” tax.

## How parallel agents stay safe

1. **One orchestrator** plans, delegates, and reviews — it almost never writes feature code itself.
2. **Each build agent gets a private git worktree** (own branch) so they can edit “the same files” without colliding.
3. **Review is a law** — nothing merges until an independent check against real source and behavior.
4. **Files are memory** — discoveries go to `BACKLOG.md`; position lives in `STATUS.md`.

## Unattended / overnight runs

Use `GOAL.md` + `CONTRACT.md` with your agent’s loop (e.g. Claude Code `/loop`):

- Goal file = phases and checkboxes (tick only when **verified**)
- Contract = the fence: what it may never do, where it may edit, when to stop and wait for a human

See `examples/loop-prompts.md` for starter prompts.

## Diagrams

| Diagram | File |
|---|---|
| Loop tick cycle | [`docs/diagrams/loop-tick-cycle.png`](docs/diagrams/loop-tick-cycle.png) |
| 7-point contract | [`docs/diagrams/seven-point-contract.png`](docs/diagrams/seven-point-contract.png) |
| Orchestration loop | [`docs/diagrams/orchestration-loop.png`](docs/diagrams/orchestration-loop.png) |
| Files are the meeting | [`docs/diagrams/files-are-the-meeting.png`](docs/diagrams/files-are-the-meeting.png) |
| Six files at a glance | [`docs/diagrams/six-files-at-a-glance.png`](docs/diagrams/six-files-at-a-glance.png) |
| Read-first, write-last | [`docs/diagrams/read-first-write-last.png`](docs/diagrams/read-first-write-last.png) |

SVG sources are in the same folder.

## Topics this repo is for

`coding-agent-harness` · multi-agent orchestration · git worktrees · shared agent memory · human–agent collaboration · Claude Code `/loop` · overnight agents · pair development with AI

## Read the write-ups

| Post | Link |
|---|---|
| Part 1 — `/loop`, goal file, overnight contract | https://medium.com/@gangwanijayant737/how-i-taught-claude-code-to-work-the-night-shift-bb1b555ef734 |
| Part 2 — Orchestrator, worktrees, mandatory review | https://medium.com/@gangwanijayant737/i-run-a-team-of-ai-agents-with-one-orchestrator-heres-what-keeps-it-from-turning-into-chaos-ed59dbf73b12 |
| How humans and AI agents collaborate (without meetings) | *(add after publishing)* |

## License

Templates and diagrams: use them. Attribution appreciated, not required.
