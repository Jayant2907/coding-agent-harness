# Running Agents Overnight

Templates and diagrams for running Claude Code (or any coding agent) unattended — with a goal file, a 7-point contract, an orchestrator team, and six git-tracked memory files for human + agent collaboration.

Companion to the Medium series **Running Agents Overnight**.

| Post | Link |
|---|---|
| Part 1 — How I Taught Claude Code to Work the Night Shift | https://medium.com/@gangwanijayant737/how-i-taught-claude-code-to-work-the-night-shift-bb1b555ef734 |
| Part 2 — I Run a Team of AI Agents With One Orchestrator | https://medium.com/@gangwanijayant737/i-run-a-team-of-ai-agents-with-one-orchestrator-heres-what-keeps-it-from-turning-into-chaos-ed59dbf73b12 |
| How Humans and AI Agents Actually Collaborate (Without Meetings) | *(paste after publishing)* |

## Steal this

Copy `templates/` into your repo. Start with **one line**: `Next step (exact):` in `STATUS.md`.

```
templates/
  GOAL.md          Phase-and-checkbox finish line for a /loop run
  CONTRACT.md      Cadence, guardrails, scope, action, budget, stop, report
  STATUS.md        Where things stand + the exact next step
  BACKLOG.md       Findings, labeled by urgency (including "no")
  DECISIONS.md     Why we chose this — so it is never re-argued
  SESSION-LOG.md   One line per session, append-only
  RISKS.md         What could still go wrong
  MEMORY-INDEX.md  Pointers to deeper notes
```

**The rule:** read first, write last. Before you trust Status, glance at what actually changed in the code since it was last updated.

## Diagrams

SVG (GitHub) and PNG (Medium / LinkedIn) live in [`docs/diagrams/`](docs/diagrams/).

## License

Templates and diagrams: use them. Attribution appreciated, not required.
