# Running Agents Overnight

Templates and diagrams for running Claude Code (or any coding agent) unattended — with a goal file, a 7-point contract, an orchestrator team, and six git-tracked memory files.

Companion to the Medium series **Running Agents Overnight**.

| Medium post | What it covers |
|---|---|
| Part 1 — *How I Taught Claude Code to Work the Night Shift* | `/loop`, `GOAL.md`, the 7-point contract |
| Part 2 — *I Run a Team of AI Agents With One Orchestrator* | Orchestrator, worktrees, mandatory review |
| Part 3 — *The Real Problem With AI Agents Isn't Intelligence. It's Memory.* | Shared files as memory |
| Companion — *The 6 Files That Let Humans and AI Agents Actually Collaborate* | Steal-this file kit |

*(Paste Medium URLs here after publishing.)*

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

## Medium drafts

Clean, publish-ready copies (teasers stripped, image slots marked) are in [`medium/`](medium/).

## License

Templates and diagrams: use them. Attribution appreciated, not required.
